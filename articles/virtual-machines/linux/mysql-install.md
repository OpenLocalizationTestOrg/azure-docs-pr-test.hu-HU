---
title: "mentése a Linux virtuális gép az Azure-ban MySQL aaaSet |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello MySQL verem Linux virtuális gépre (Ubuntu vagy RedHat termékcsalád operációs rendszer) az Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: e47d0de7f0eb5bb873ad20e4bc35f1b5f8d33bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-mysql-on-azure"></a>Hogyan tooinstall MySQL az Azure-on
Ebből a cikkből megtudhatja, hogyan tooinstall és MySQL konfigurálása a Linux operációs rendszert futtató Azure virtuális géphez.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>MySQL telepítése a virtuális gépen
> [!NOTE]
> Már rendelkeznie kell egy Microsoft Azure virtuális gépen, Linuxot futtató rendelés toocomplete Ez az oktatóanyag. Tekintse meg a [Azure Linux virtuális gép oktatóanyag](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate és a Linux virtuális gépet `mysqlnode` hello virtuális gép nevét, és `azureuser` felhasználóként a folytatás előtt.
> 
> 

Ebben az esetben 3306 portot használni hello MySQL port.  

Csatlakozás toohello putty segítségével létrehozott Linux virtuális Géphez. Ha ez hello Azure Linux virtuális gép első használatakor, lásd: hogyan kapcsolódnak az toouse putty a Linux virtuális gép tooa [Itt](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tárház csomag tooinstall MySQL5.6 használjuk példaként, ebben a cikkben. MySQL5.6 rendelkezik-e további fokozása mint MySQL5.5 teljesítményét.  További információ [Itt](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-tooinstall-mysql56-on-ubuntu"></a>Hogyan tooinstall az Ubuntu MySQL5.6
Használjuk Linux virtuális gép az Azure-ból Ubuntu itt.

* 1. lépés: Telepítse a MySQL Server 5.6 túl kapcsoló`root` felhasználó:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Mysql-kiszolgáló 5.6 telepítése:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    A telepítés során megjelenik egy párbeszédpanel ablak poping tooask fel akkor tooset MySQL gyökér szintű jelszavát az alábbi, és kell itt beállította az hello jelszavát.
  
    ![Kép](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Bemeneti hello jelszó újra tooconfirm.

    ![Kép](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* 2. lépés: Bejelentkezés MySQL-kiszolgáló
  
    MySQL-kiszolgáló telepítésének befejeződése után MySQL-szolgáltatás automatikusan elindul. MySQL kiszolgáló jelentkezhet `root` felhasználó.
    Hello alatt parancs toologin és a bemeneti jelszó használata.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* 3. lépés: A MySQL-szolgáltatást futtató hello kezelése
  
    a MySQL-szolgáltatás állapotának beolvasása
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) a MySQL-szolgáltatás elindítása
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) MySQL-szolgáltatás leállítása
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) hello MySQL-szolgáltatás újraindítása
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-tooinstall-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Hogyan tooinstall MySQL Red Hat operációsrendszer-család például a CentOS, Oracle Linux
Használjuk Linux virtuális gép CentOS vagy Oracle Linux itt.

* 1. lépés: Hello MySQL Yum tárház kapcsoló túl hozzáadása`root` felhasználó:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Töltse le és telepítse hello MySQL kiadási csomag:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* 2. lépés: Szerkesztés alatt fájl tooenable hello MySQL tárháza hello MySQL5.6 csomag letöltése.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    A fájl toobelow minden értékének frissítése:
  
        \# *Enable toouse MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* 3. lépés: A telepítés MySQL MySQL tárházból MySQL telepítése:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    MySQL RPM-csomag és az összes kapcsolódó csomag lesz telepítve.
* 4. lépés: A MySQL-szolgáltatást futtató hello kezelése
  
    (a) hello MySQL kiszolgáló hello szolgáltatás állapotának ellenőrzése:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) ellenőrizze, hogy fut-e hello alapértelmezett port a MySQL-kiszolgáló:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) hello MySQL kiszolgáló indítása:

           #[root@mysqlnode ~]#service mysqld start

    (d) hello MySQL kiszolgáló leállítása:

           #[root@mysqlnode ~]#service mysqld stop

    e MySQL toostart be, ha a rendszer hello állítja:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-tooinstall-mysql-on-suse-linux"></a>Hogyan tooinstall MySQL SUSE Linux rendszeren
Használjuk Linux virtuális gép, OpenSUSE itt.

* 1. lépés: Töltse le és telepítse a MySQL-kiszolgáló
  
    Váltás túl`root` felhasználó keresztül alábbi parancsot:  
  
           #sudo su -
  
    Töltse le és telepítse a MySQL-csomagot:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* 2. lépés: A MySQL-szolgáltatást futtató hello kezelése
  
    (a) hello MySQL server hello állapotának ellenőrzése:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) ellenőrizze, hogy hello hello MySQL kiszolgáló alapértelmezett port:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) hello MySQL kiszolgáló indítása:

           #[root@mysqlnode ~]# rcmysql start

    (d) hello MySQL kiszolgáló leállítása:

           #[root@mysqlnode ~]# rcmysql stop

    e MySQL toostart be, ha a rendszer hello állítja:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Következő lépés
További használati és MySQL kapcsolatos információk keresése [Itt](https://www.mysql.com/).

