---
title: "aaaRun egy MariaDB (MySQL) fürtön, az Azure-on |} Microsoft Docs"
description: "Hozzon létre egy MariaDB + Galera MySQL az Azure virtuális gépek fürtön"
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: f9a4d6c45d76478a8a3526b407c7bbe6aeb40423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL) fürt: Azure útmutató
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus. Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. A Microsoft azt javasolja, hogy az új telepítések esetén hello Azure Resource Manager modellt használja.

> [!NOTE]
> MariaDB vállalati fürt már elérhető a hello Azure piactéren. az új ajánlat hello szolgáltatás automatikusan telepíti az Azure Resource Manager MariaDB Galera fürt. Használja az új ajánlat hello a [Azure piactér](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Ez a cikk bemutatja, hogyan toocreate egy többszörös főkiszolgáló [Galera](http://galeracluster.com/products/) fürtben [MariaDBs](https://mariadb.org/en/about/) (nagy teljesítményű, méretezhető és megbízható Esőcsepp helyettesíti a MySQL) toowork a magas rendelkezésre állású környezet az Azure-on virtuális gépek.

## <a name="architecture-overview"></a>Az architektúra áttekintése
Ez a cikk ismerteti, hogyan toocomplete hello a következő lépéseket:

- Hozzon létre egy három csomópontos fürtre.
- Az operációsrendszer-lemez hello külön hello adatlemezek.
- Hozzon létre hello adatlemezek RAID-0/csíkozott beállítás tooincrease iops-érték.
- Használja az Azure terheléselosztó toobalance hello betöltése hello három csomópontok.
- ismétlődő toominimize működni, hozzon létre egy Virtuálisgép-lemezkép MariaDB + Galera tartalmazó és használják toocreate hello más fürt virtuális gépeket.

![Rendszer-architektúra](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> Ez a témakör használ hello [Azure CLI](../../../cli-install-nodejs.md) eszközök, ezért győződjön meg arról, hogy toodownload őket, és csatlakoztassa őket tooyour Azure-előfizetés függően toohello utasításokat. Ha egy hivatkozás toohello parancsok hello Azure parancssori felület érhető el, lásd: hello [Azure CLI parancsdokumentációja](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Konfigurálnia kell túl[hitelesítéshez SSH-kulcs létrehozása] és jegyezze fel a hello PEM-fájl helyét.
>
>

## <a name="create-hello-template"></a>Hello sablon létrehozása
### <a name="infrastructure"></a>Infrastruktúra
1. Hozzon létre egy affinitáscsoporthoz toohold hello erőforrások együtt.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. Hozzon létre egy virtuális hálózatot.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. Hozzon létre egy tárolási fiók toohost a lemezeket. Legfeljebb 40 terhelésnek kitett lemezek hello ne helyezze azonos tárolási fiók tooavoid szerezze meg a 20 000 hello IOPS fiók méretkorlátot. Ebben az esetben Ön alatt ezt a határértéket, így mindent a fiókot használja az egyszerűség kedvéért hello fogja tárolni.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. Hello neve hello CentOS 7 virtuálisgép-lemezkép található.

        azure vm image list | findstr CentOS
   hello kimenet lesz hasonlót `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Használja ezt a nevet a következő lépés hello.
5. Hello Virtuálisgép-sablon létrehozásához, és cserélje le /path/to/key.pem hello elérési generált hello .pem SSH-kulcs tárolására.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. Rendeljen a négy 500 GB-os adatok lemezek toohello VM hello RAID-konfigurációban való használat céljából.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. SSH toosign toohello sablonban virtuális Gépet, amely létrehozta a következő mariadbhatemplate.cloudapp.net:22 használjon, és a titkos kulcs használatával.

### <a name="software"></a>Szoftver
1. Hello legfelső szintű beolvasása.

        sudo su

2. A RAID telepítése:

    a. Telepítse a mdadm.

              yum install mdadm

    b. Hozzon létre egy EXT4 fájlrendszert hello RAID0/paritásos konfigurációs.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Hozzon létre hello csatlakoztatási pont könyvtárat.

              mkdir /mnt/data
    d. Az újonnan létrehozott hello RAID eszköz UUID hello beolvasása.

              blkid | grep /dev/md0
    e. /Etc/fstab szerkesztése.

              vi /etc/fstab
    f. Adja hozzá az előző hello nyert hello eszköz tooenable automatikus újraindítás hello UUID lecserélését hello érték csatlakoztatni **blkid** parancsot.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Csatlakoztassa a hello új partíciót.

              mount /mnt/data

3. Telepítse a MariaDB.

    a. Hozzon létre hello MariaDB.repo fájlt.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Töltse ki a hello tárház fájlt a tartalom a következő hello:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. tooavoid ütközések, távolítsa el a meglévő utótag és mariadb-függvénytárak.

           yum remove postfix mariadb-libs-*
    d. Telepítési MariaDB Galera.

           yum install MariaDB-Galera-server MariaDB-client galera

4. Helyezze át a hello MySQL adatok directory toohello RAID elérésű eszközt.

    a. Hello aktuális MySQL könyvtár átmásolja az új helyre, és hello régi könyvtár eltávolítása.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Ennek megfelelően állítsa be a hello új könyvtár engedélyeket.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Hozzon létre egy symlink mutat, hello régi directory toohello új helyre a hello RAID-partíció.

           ln -s /mnt/data/mysql /var/lib/mysql

5. Mivel [SELinux hello fürt működését gátolja](http://galeracluster.com/documentation-webpages/configuration.html#selinux), toodisable szükség az aktuális munkamenet hello. Szerkesztés `/etc/selinux/config` toodisable a későbbi újraindítások.

            setenforce 0

            then editing `/etc/selinux/config` tooset `SELINUX=permissive`
6. Ellenőrizze a MySQL futtatása.

   a. Indítsa el a MySQL.

           service mysql start
   b. Hello MySQL telepítési biztonságos, hello gyökér szintű jelszó beállítása, távolítsa el a névtelen felhasználók toodisable távoli legfelső szintű bejelentkezési és hello teszt adatbázis eltávolítása.

           mysql_secure_installation
   c. Felhasználó létrehozása a fürt működését, és szükség esetén az alkalmazások hello adatbázis.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* too'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. Állítsa le a MySQL.

            service mysql stop
7. Hozzon létre egy konfigurációs helyőrző.

   a. Hello MySQL konfigurációs toocreate hello fürtbeállítások helyőrző szerkesztése. Nem váltják ki hello  **`<Variables>`**  vagy állítsa vissza a most. Amely akkor után hozzon létre egy virtuális Gépet a sablon alapján történik.

            vi /etc/my.cnf.d/server.cnf
   b. Hello szerkesztése  **[galera]**  szakaszt, és törölje a jelölést.

   c. Hello szerkesztése **[mariadb]** szakasz.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set too0.0.0.0, hello server listens tooremote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for hello SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set hello node name of this server
8. Nyissa meg a szükséges portok hello tűzfal CentOS 7 FirewallD használatával.

   * MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA ITT:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Töltse be újra hello tűzfal:`firewall-cmd --reload`

9. Hello rendszer teljesítményének optimalizálásához. További információkért lásd: [teljesítményének hangolása stratégia](optimize-mysql.md).

   a. Szerkessze újra hello MySQL konfigurációs fájlt.

            vi /etc/my.cnf.d/server.cnf
   b. Hello szerkesztése **[mariadb]** szakaszt, és a tartalom a következő hello hozzáfűzése:

   > [!NOTE]
   > Azt javasoljuk, hogy innodb\_puffer\_pool_size a virtuális gép memória 70 százalék. Ebben a példában beállítása 2.45 GB hello közepes 3.5-ös GB RAM-MAL rendelkező Azure virtuális gépet.
   >
   >

           innodb_buffer_pool_size = 2508M # hello buffer pool contains buffered data and hello index. This is usually set too70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give hello server more time toorecycle idled connections
           innodb_file_per_table = 1 # Speed up hello table space transmission and optimize hello debris management performance
           innodb_log_buffer_size = 128M # hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit
           innodb_flush_log_at_trx_commit = 2 # hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. Állítsa le a MySQL futtatását indítási tooavoid hello fürt megszakítása, ha a csomópont hozzáadása a MySQL-szolgáltatás letiltása és kiosztásának megszüntetése hello gép.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. Rögzítse a virtuális gép hello hello portálon keresztül. (Jelenleg [#1268 hello Azure CLI-eszközei ki](https://github.com/Azure/azure-xplat-cli/issues/1268) hello tényt, hogy hello Azure CLI-eszközei a rögzített lemezképeket nem rögzíthető lemezkép a csatolt hello adatlemezek ismerteti.)

    a. Állítsa le hello gépet hello portálon keresztül.

    b. Kattintson a **rögzítése** , és adja meg a hello kép neve megegyezik **mariadb-galera-lemezkép**. Adjon meg egy leírást, és ellenőrizze a "Rendelkezik futtattam a waagent."
      
      ![Hello virtuális gép rögzítése](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-hello-cluster"></a>Hello fürt létrehozása
Hozzon létre három virtuális gépek hello sablonnal létrehozott, és ezután konfigurálása és hello fürt elindításához.

1. Hozzon létre első CentOS 7 virtuális gép lemezképből hello mariadb-galera-rendszerkép alapján létrehozott, kezeléséről a következő információ hello hello:

 - Virtuális hálózati név: mariadbvnet
 - Alhálózati: mariadb
 - Méret gépen: Közepes
 - Felhőszolgáltatás neve: mariadbha (vagy bármilyen neve, amelyhez toobe mariadbha.cloudapp.net keresztül érhető el)
 - Számítógépnév: mariadb1
 - Felhasználónév: azureuser
 - SSH-elérést: engedélyezve
 - Sikeres hello SSH tanúsítvány .pem fájl, és /path/to/key.pem lecserélését hello elérési generált hello .pem SSH-kulcs tárolására.

   > [!NOTE]
   > hello következő parancsok el vannak osztva az átláthatóság többsoros, de egyes egy sorba kell megadnia.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
2. Hozzon létre két további virtuális gépek toohello mariadbha felhőszolgáltatás csatlakoztatja őket. Virtuális gép nevének módosítása hello és hello SSH port tooa egyedi port nem ütközik más virtuális gépek a hello ugyanazt a felhőalapú szolgáltatás.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  A MariaDB3:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
3. Tooget hello belső IP-cím hello három virtuális gépek mindegyikének hello a következő lépésben lesz szüksége:

    ![IP-cím beolvasása](./media/mariadb-mysql-cluster/IP.png)
4. SSH toosign toohello három virtuális gép található, és azok konfigurációs fájl hello szerkesztése.

        sudo vi /etc/my.cnf.d/server.cnf

    Állítsa vissza  **`wsrep_cluster_name`**  és  **`wsrep_cluster_address`**  hello eltávolításával  **#**  hello hello sor elején.
    Továbbá cserélje le  **`<ServerIP>`**  a  **`wsrep_node_address`**  és  **`<NodeName>`**  a  **`wsrep_node_name`**  a hello A virtuális gép IP cím és nevet, illetve kódrészig, és azokat a sorokat.
5. Indítsa el a fürt hello a MariaDB1, és hagyja, hogy az indítási parancsot.

        sudo service mysql bootstrap
        chkconfig mysql on
6. Indítsa el a MySQL MariaDB2 és MariaDB3, és hagyja, hogy az indítási parancsot.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-hello-cluster"></a>Load balance hello fürt
Hello fürtözött virtuális gépek létrehozásakor hozzá azokat a clusteravset tooensure volt elhelyezése különböző tartalék és a frissítési tartományok és, hogy Azure soha nem minden gépen karbantartási egyszerre nevű rendelkezésre állási csoport. Ez a konfiguráció megfelel-e toobe hello Azure szolgáltatásiszint-szerződéssel (SLA) által támogatott hello követelményeknek.

Most használja az Azure Load Balancer toobalance kérelmek hello három csomópontok között.

Futtassa a parancsokat a számítógépen a következő hello Azure parancssori felület használatával hello.

hello paraméterek parancsstruktúrát van:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

hello CLI beállítása hello terhelés terheléselosztó mintavételi időköze too15 másodperc, elképzelhető, hogy egy kicsit túl hosszú. Hello portál megváltoztatnia **végpontok** bármely hello virtuális gépeket.

![Végpont szerkesztése](./media/mariadb-mysql-cluster/Endpoint.PNG)

Válassza ki **hitelesítés beállítása Reconfigure hello**.

![Konfigurálja újra a hello-elosztott terhelésű készlet](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Változás **mintavételi időköze** too5 másodperc, és mentse a módosításokat.

![Mintavételi időköz módosítása](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-hello-cluster"></a>Hello fürt ellenőrzése
hello rögzített munkát. hello fürt legyen most már elérhető a `mariadbha.cloudapp.net:3306`, találatok száma a hello terheléselosztó és útvonal-kérelmek között hello három virtuális gépek zökkenőmentesen és hatékonyan.

A kedvenc MySQL ügyfél tooconnect használja, vagy csatlakoztassa egy hello virtuális gépek tooverify, hogy a fürt működik.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Ezután hozzon létre egy adatbázist, és feltöltheti olyan néhány adat.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

hello adatbázis létrehozta a következő táblázat hello adja vissza:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
Ebben a cikkben egy három csomópontos MariaDB létrehozott + Galera magas rendelkezésre állású fürt Azure virtuális gépen futó CentOS 7. hello virtuális gépek az elosztott terhelésű Azure terheléselosztó.

Érdemes toolook: [egy másik módja toocluster Linux MySQL](mysql-cluster.md) és módon túl[optimalizálása és MySQL az Azure Linux virtuális gépeken futó teljesítménytesztelési](optimize-mysql.md).

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating hello template]:#creating-the-template
[Creating hello cluster]:#creating-the-cluster
[Load balancing hello cluster]:#load-balancing-the-cluster
[Validating hello cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[hitelesítéshez SSH-kulcs létrehozása]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in hello Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
