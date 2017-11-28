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
# <a name="mariadb-mysql-cluster-azure-tutorial"></a><span data-ttu-id="9ab14-103">MariaDB (MySQL) fürt: Azure útmutató</span><span class="sxs-lookup"><span data-stu-id="9ab14-103">MariaDB (MySQL) cluster: Azure tutorial</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9ab14-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="9ab14-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="9ab14-105">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="9ab14-105">This article covers hello classic deployment model.</span></span> <span data-ttu-id="9ab14-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Azure Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="9ab14-106">Microsoft recommends that most new deployments use hello Azure Resource Manager model.</span></span>

> [!NOTE]
> <span data-ttu-id="9ab14-107">MariaDB vállalati fürt már elérhető a hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="9ab14-107">MariaDB Enterprise cluster is now available in hello Azure Marketplace.</span></span> <span data-ttu-id="9ab14-108">az új ajánlat hello szolgáltatás automatikusan telepíti az Azure Resource Manager MariaDB Galera fürt.</span><span class="sxs-lookup"><span data-stu-id="9ab14-108">hello new offering will automatically deploy a MariaDB Galera cluster on Azure Resource Manager.</span></span> <span data-ttu-id="9ab14-109">Használja az új ajánlat hello a [Azure piactér](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span><span class="sxs-lookup"><span data-stu-id="9ab14-109">You should use hello new offering from [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span></span>
>
>

<span data-ttu-id="9ab14-110">Ez a cikk bemutatja, hogyan toocreate egy többszörös főkiszolgáló [Galera](http://galeracluster.com/products/) fürtben [MariaDBs](https://mariadb.org/en/about/) (nagy teljesítményű, méretezhető és megbízható Esőcsepp helyettesíti a MySQL) toowork a magas rendelkezésre állású környezet az Azure-on virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9ab14-110">This article shows you how toocreate a multi-Master [Galera](http://galeracluster.com/products/) cluster of [MariaDBs](https://mariadb.org/en/about/) (a robust, scalable, and reliable drop-in replacement for MySQL) toowork in a highly available environment on Azure virtual machines.</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="9ab14-111">Az architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="9ab14-111">Architecture overview</span></span>
<span data-ttu-id="9ab14-112">Ez a cikk ismerteti, hogyan toocomplete hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9ab14-112">This article describes how toocomplete hello following steps:</span></span>

- <span data-ttu-id="9ab14-113">Hozzon létre egy három csomópontos fürtre.</span><span class="sxs-lookup"><span data-stu-id="9ab14-113">Create a three-node cluster.</span></span>
- <span data-ttu-id="9ab14-114">Az operációsrendszer-lemez hello külön hello adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="9ab14-114">Separate hello data disks from hello OS disk.</span></span>
- <span data-ttu-id="9ab14-115">Hozzon létre hello adatlemezek RAID-0/csíkozott beállítás tooincrease iops-érték.</span><span class="sxs-lookup"><span data-stu-id="9ab14-115">Create hello data disks in RAID-0/striped setting tooincrease IOPS.</span></span>
- <span data-ttu-id="9ab14-116">Használja az Azure terheléselosztó toobalance hello betöltése hello három csomópontok.</span><span class="sxs-lookup"><span data-stu-id="9ab14-116">Use Azure Load Balancer toobalance hello load for hello three nodes.</span></span>
- <span data-ttu-id="9ab14-117">ismétlődő toominimize működni, hozzon létre egy Virtuálisgép-lemezkép MariaDB + Galera tartalmazó és használják toocreate hello más fürt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="9ab14-117">toominimize repetitive work, create a VM image that contains MariaDB + Galera and use it toocreate hello other cluster VMs.</span></span>

![Rendszer-architektúra](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> <span data-ttu-id="9ab14-119">Ez a témakör használ hello [Azure CLI](../../../cli-install-nodejs.md) eszközök, ezért győződjön meg arról, hogy toodownload őket, és csatlakoztassa őket tooyour Azure-előfizetés függően toohello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9ab14-119">This topic uses hello [Azure CLI](../../../cli-install-nodejs.md) tools, so make sure toodownload them and connect them tooyour Azure subscription according toohello instructions.</span></span> <span data-ttu-id="9ab14-120">Ha egy hivatkozás toohello parancsok hello Azure parancssori felület érhető el, lásd: hello [Azure CLI parancsdokumentációja](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="9ab14-120">If you need a reference toohello commands available in hello Azure CLI, see hello [Azure CLI command reference](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="9ab14-121">Konfigurálnia kell túl[hitelesítéshez SSH-kulcs létrehozása] és jegyezze fel a hello PEM-fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="9ab14-121">You will also need too[create an SSH key for authentication] and make note of hello .pem file location.</span></span>
>
>

## <a name="create-hello-template"></a><span data-ttu-id="9ab14-122">Hello sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ab14-122">Create hello template</span></span>
### <a name="infrastructure"></a><span data-ttu-id="9ab14-123">Infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="9ab14-123">Infrastructure</span></span>
1. <span data-ttu-id="9ab14-124">Hozzon létre egy affinitáscsoporthoz toohold hello erőforrások együtt.</span><span class="sxs-lookup"><span data-stu-id="9ab14-124">Create an affinity group toohold hello resources together.</span></span>

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. <span data-ttu-id="9ab14-125">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="9ab14-125">Create a virtual network.</span></span>

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. <span data-ttu-id="9ab14-126">Hozzon létre egy tárolási fiók toohost a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="9ab14-126">Create a storage account toohost all our disks.</span></span> <span data-ttu-id="9ab14-127">Legfeljebb 40 terhelésnek kitett lemezek hello ne helyezze azonos tárolási fiók tooavoid szerezze meg a 20 000 hello IOPS fiók méretkorlátot.</span><span class="sxs-lookup"><span data-stu-id="9ab14-127">You shouldn't place more than 40 heavily used disks on hello same storage account tooavoid hitting hello 20,000 IOPS storage account limit.</span></span> <span data-ttu-id="9ab14-128">Ebben az esetben Ön alatt ezt a határértéket, így mindent a fiókot használja az egyszerűség kedvéért hello fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="9ab14-128">In this case, you're well below that limit, so you'll store everything on hello same account for simplicity.</span></span>

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. <span data-ttu-id="9ab14-129">Hello neve hello CentOS 7 virtuálisgép-lemezkép található.</span><span class="sxs-lookup"><span data-stu-id="9ab14-129">Find hello name of hello CentOS 7 virtual machine image.</span></span>

        azure vm image list | findstr CentOS
   <span data-ttu-id="9ab14-130">hello kimenet lesz hasonlót `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span><span class="sxs-lookup"><span data-stu-id="9ab14-130">hello output will be something like `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span></span>

   <span data-ttu-id="9ab14-131">Használja ezt a nevet a következő lépés hello.</span><span class="sxs-lookup"><span data-stu-id="9ab14-131">Use that name in hello following step.</span></span>
5. <span data-ttu-id="9ab14-132">Hello Virtuálisgép-sablon létrehozásához, és cserélje le /path/to/key.pem hello elérési generált hello .pem SSH-kulcs tárolására.</span><span class="sxs-lookup"><span data-stu-id="9ab14-132">Create hello VM template and replace /path/to/key.pem with hello path where you stored hello generated .pem SSH key.</span></span>

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. <span data-ttu-id="9ab14-133">Rendeljen a négy 500 GB-os adatok lemezek toohello VM hello RAID-konfigurációban való használat céljából.</span><span class="sxs-lookup"><span data-stu-id="9ab14-133">Attach four 500-GB data disks toohello VM for use in hello RAID configuration.</span></span>

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. <span data-ttu-id="9ab14-134">SSH toosign toohello sablonban virtuális Gépet, amely létrehozta a következő mariadbhatemplate.cloudapp.net:22 használjon, és a titkos kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="9ab14-134">Use SSH toosign in toohello template VM that you created at mariadbhatemplate.cloudapp.net:22, and connect by using your private key.</span></span>

### <a name="software"></a><span data-ttu-id="9ab14-135">Szoftver</span><span class="sxs-lookup"><span data-stu-id="9ab14-135">Software</span></span>
1. <span data-ttu-id="9ab14-136">Hello legfelső szintű beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9ab14-136">Get hello root.</span></span>

        sudo su

2. <span data-ttu-id="9ab14-137">A RAID telepítése:</span><span class="sxs-lookup"><span data-stu-id="9ab14-137">Install RAID support:</span></span>

    <span data-ttu-id="9ab14-138">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-138">a.</span></span> <span data-ttu-id="9ab14-139">Telepítse a mdadm.</span><span class="sxs-lookup"><span data-stu-id="9ab14-139">Install mdadm.</span></span>

              yum install mdadm

    <span data-ttu-id="9ab14-140">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-140">b.</span></span> <span data-ttu-id="9ab14-141">Hozzon létre egy EXT4 fájlrendszert hello RAID0/paritásos konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="9ab14-141">Create hello RAID0/stripe configuration with an EXT4 file system.</span></span>

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    <span data-ttu-id="9ab14-142">c.</span><span class="sxs-lookup"><span data-stu-id="9ab14-142">c.</span></span> <span data-ttu-id="9ab14-143">Hozzon létre hello csatlakoztatási pont könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="9ab14-143">Create hello mount point directory.</span></span>

              mkdir /mnt/data
    <span data-ttu-id="9ab14-144">d.</span><span class="sxs-lookup"><span data-stu-id="9ab14-144">d.</span></span> <span data-ttu-id="9ab14-145">Az újonnan létrehozott hello RAID eszköz UUID hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9ab14-145">Retrieve hello UUID of hello newly created RAID device.</span></span>

              blkid | grep /dev/md0
    <span data-ttu-id="9ab14-146">e.</span><span class="sxs-lookup"><span data-stu-id="9ab14-146">e.</span></span> <span data-ttu-id="9ab14-147">/Etc/fstab szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="9ab14-147">Edit /etc/fstab.</span></span>

              vi /etc/fstab
    <span data-ttu-id="9ab14-148">f.</span><span class="sxs-lookup"><span data-stu-id="9ab14-148">f.</span></span> <span data-ttu-id="9ab14-149">Adja hozzá az előző hello nyert hello eszköz tooenable automatikus újraindítás hello UUID lecserélését hello érték csatlakoztatni **blkid** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9ab14-149">Add hello device tooenable auto mounting on reboot, replacing hello UUID with hello value obtained from hello previous **blkid** command.</span></span>

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    <span data-ttu-id="9ab14-150">g.</span><span class="sxs-lookup"><span data-stu-id="9ab14-150">g.</span></span> <span data-ttu-id="9ab14-151">Csatlakoztassa a hello új partíciót.</span><span class="sxs-lookup"><span data-stu-id="9ab14-151">Mount hello new partition.</span></span>

              mount /mnt/data

3. <span data-ttu-id="9ab14-152">Telepítse a MariaDB.</span><span class="sxs-lookup"><span data-stu-id="9ab14-152">Install MariaDB.</span></span>

    <span data-ttu-id="9ab14-153">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-153">a.</span></span> <span data-ttu-id="9ab14-154">Hozzon létre hello MariaDB.repo fájlt.</span><span class="sxs-lookup"><span data-stu-id="9ab14-154">Create hello MariaDB.repo file.</span></span>

                vi /etc/yum.repos.d/MariaDB.repo

    <span data-ttu-id="9ab14-155">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-155">b.</span></span> <span data-ttu-id="9ab14-156">Töltse ki a hello tárház fájlt a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9ab14-156">Fill hello repo file with hello following content:</span></span>

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    <span data-ttu-id="9ab14-157">c.</span><span class="sxs-lookup"><span data-stu-id="9ab14-157">c.</span></span> <span data-ttu-id="9ab14-158">tooavoid ütközések, távolítsa el a meglévő utótag és mariadb-függvénytárak.</span><span class="sxs-lookup"><span data-stu-id="9ab14-158">tooavoid conflicts, remove existing postfix and mariadb-libs.</span></span>

           yum remove postfix mariadb-libs-*
    <span data-ttu-id="9ab14-159">d.</span><span class="sxs-lookup"><span data-stu-id="9ab14-159">d.</span></span> <span data-ttu-id="9ab14-160">Telepítési MariaDB Galera.</span><span class="sxs-lookup"><span data-stu-id="9ab14-160">Install MariaDB with Galera.</span></span>

           yum install MariaDB-Galera-server MariaDB-client galera

4. <span data-ttu-id="9ab14-161">Helyezze át a hello MySQL adatok directory toohello RAID elérésű eszközt.</span><span class="sxs-lookup"><span data-stu-id="9ab14-161">Move hello MySQL data directory toohello RAID block device.</span></span>

    <span data-ttu-id="9ab14-162">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-162">a.</span></span> <span data-ttu-id="9ab14-163">Hello aktuális MySQL könyvtár átmásolja az új helyre, és hello régi könyvtár eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="9ab14-163">Copy hello current MySQL directory into its new location and remove hello old directory.</span></span>

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    <span data-ttu-id="9ab14-164">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-164">b.</span></span> <span data-ttu-id="9ab14-165">Ennek megfelelően állítsa be a hello új könyvtár engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="9ab14-165">Set permissions for hello new directory accordingly.</span></span>

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    <span data-ttu-id="9ab14-166">c.</span><span class="sxs-lookup"><span data-stu-id="9ab14-166">c.</span></span> <span data-ttu-id="9ab14-167">Hozzon létre egy symlink mutat, hello régi directory toohello új helyre a hello RAID-partíció.</span><span class="sxs-lookup"><span data-stu-id="9ab14-167">Create a symlink that points hello old directory toohello new location on hello RAID partition.</span></span>

           ln -s /mnt/data/mysql /var/lib/mysql

5. <span data-ttu-id="9ab14-168">Mivel [SELinux hello fürt működését gátolja](http://galeracluster.com/documentation-webpages/configuration.html#selinux), toodisable szükség az aktuális munkamenet hello.</span><span class="sxs-lookup"><span data-stu-id="9ab14-168">Because [SELinux interferes with hello cluster operations](http://galeracluster.com/documentation-webpages/configuration.html#selinux), it is necessary toodisable it for hello current session.</span></span> <span data-ttu-id="9ab14-169">Szerkesztés `/etc/selinux/config` toodisable a későbbi újraindítások.</span><span class="sxs-lookup"><span data-stu-id="9ab14-169">Edit `/etc/selinux/config` toodisable it for subsequent restarts.</span></span>

            setenforce 0

            then editing `/etc/selinux/config` tooset `SELINUX=permissive`
6. <span data-ttu-id="9ab14-170">Ellenőrizze a MySQL futtatása.</span><span class="sxs-lookup"><span data-stu-id="9ab14-170">Validate MySQL runs.</span></span>

   <span data-ttu-id="9ab14-171">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-171">a.</span></span> <span data-ttu-id="9ab14-172">Indítsa el a MySQL.</span><span class="sxs-lookup"><span data-stu-id="9ab14-172">Start MySQL.</span></span>

           service mysql start
   <span data-ttu-id="9ab14-173">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-173">b.</span></span> <span data-ttu-id="9ab14-174">Hello MySQL telepítési biztonságos, hello gyökér szintű jelszó beállítása, távolítsa el a névtelen felhasználók toodisable távoli legfelső szintű bejelentkezési és hello teszt adatbázis eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="9ab14-174">Secure hello MySQL installation, set hello root password, remove anonymous users toodisable remote root login, and remove hello test database.</span></span>

           mysql_secure_installation
   <span data-ttu-id="9ab14-175">c.</span><span class="sxs-lookup"><span data-stu-id="9ab14-175">c.</span></span> <span data-ttu-id="9ab14-176">Felhasználó létrehozása a fürt működését, és szükség esetén az alkalmazások hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9ab14-176">Create a user on hello database for cluster operations, and optionally for your applications.</span></span>

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* too'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   <span data-ttu-id="9ab14-177">d.</span><span class="sxs-lookup"><span data-stu-id="9ab14-177">d.</span></span> <span data-ttu-id="9ab14-178">Állítsa le a MySQL.</span><span class="sxs-lookup"><span data-stu-id="9ab14-178">Stop MySQL.</span></span>

            service mysql stop
7. <span data-ttu-id="9ab14-179">Hozzon létre egy konfigurációs helyőrző.</span><span class="sxs-lookup"><span data-stu-id="9ab14-179">Create a configuration placeholder.</span></span>

   <span data-ttu-id="9ab14-180">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-180">a.</span></span> <span data-ttu-id="9ab14-181">Hello MySQL konfigurációs toocreate hello fürtbeállítások helyőrző szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="9ab14-181">Edit hello MySQL configuration toocreate a placeholder for hello cluster settings.</span></span> <span data-ttu-id="9ab14-182">Nem váltják ki hello  **`<Variables>`**  vagy állítsa vissza a most.</span><span class="sxs-lookup"><span data-stu-id="9ab14-182">Do not replace hello **`<Variables>`** or uncomment now.</span></span> <span data-ttu-id="9ab14-183">Amely akkor után hozzon létre egy virtuális Gépet a sablon alapján történik.</span><span class="sxs-lookup"><span data-stu-id="9ab14-183">That will happen after you create a VM from this template.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="9ab14-184">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-184">b.</span></span> <span data-ttu-id="9ab14-185">Hello szerkesztése  **[galera]**  szakaszt, és törölje a jelölést.</span><span class="sxs-lookup"><span data-stu-id="9ab14-185">Edit hello **[galera]** section and clear it out.</span></span>

   <span data-ttu-id="9ab14-186">c.</span><span class="sxs-lookup"><span data-stu-id="9ab14-186">c.</span></span> <span data-ttu-id="9ab14-187">Hello szerkesztése **[mariadb]** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ab14-187">Edit hello **[mariadb]** section.</span></span>

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
8. <span data-ttu-id="9ab14-188">Nyissa meg a szükséges portok hello tűzfal CentOS 7 FirewallD használatával.</span><span class="sxs-lookup"><span data-stu-id="9ab14-188">Open required ports on hello firewall by using FirewallD on CentOS 7.</span></span>

   * <span data-ttu-id="9ab14-189">MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="9ab14-189">MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span></span>
   * <span data-ttu-id="9ab14-190">GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="9ab14-190">GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span></span>
   * <span data-ttu-id="9ab14-191">GALERA ITT:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="9ab14-191">GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span></span>
   * <span data-ttu-id="9ab14-192">RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="9ab14-192">RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span></span>
   * <span data-ttu-id="9ab14-193">Töltse be újra hello tűzfal:`firewall-cmd --reload`</span><span class="sxs-lookup"><span data-stu-id="9ab14-193">Reload hello firewall: `firewall-cmd --reload`</span></span>

9. <span data-ttu-id="9ab14-194">Hello rendszer teljesítményének optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="9ab14-194">Optimize hello system for performance.</span></span> <span data-ttu-id="9ab14-195">További információkért lásd: [teljesítményének hangolása stratégia](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="9ab14-195">For more information, see [performance tuning strategy](optimize-mysql.md).</span></span>

   <span data-ttu-id="9ab14-196">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-196">a.</span></span> <span data-ttu-id="9ab14-197">Szerkessze újra hello MySQL konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="9ab14-197">Edit hello MySQL configuration file again.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="9ab14-198">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-198">b.</span></span> <span data-ttu-id="9ab14-199">Hello szerkesztése **[mariadb]** szakaszt, és a tartalom a következő hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="9ab14-199">Edit hello **[mariadb]** section and append hello following content:</span></span>

   > [!NOTE]
   > <span data-ttu-id="9ab14-200">Azt javasoljuk, hogy innodb\_puffer\_pool_size a virtuális gép memória 70 százalék.</span><span class="sxs-lookup"><span data-stu-id="9ab14-200">We recommend that innodb\_buffer\_pool_size is 70 percent of your VM's memory.</span></span> <span data-ttu-id="9ab14-201">Ebben a példában beállítása 2.45 GB hello közepes 3.5-ös GB RAM-MAL rendelkező Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9ab14-201">In this example, it has been set at 2.45 GB for hello medium Azure VM with 3.5 GB of RAM.</span></span>
   >
   >

           innodb_buffer_pool_size = 2508M # hello buffer pool contains buffered data and hello index. This is usually set too70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give hello server more time toorecycle idled connections
           innodb_file_per_table = 1 # Speed up hello table space transmission and optimize hello debris management performance
           innodb_log_buffer_size = 128M # hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit
           innodb_flush_log_at_trx_commit = 2 # hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. <span data-ttu-id="9ab14-202">Állítsa le a MySQL futtatását indítási tooavoid hello fürt megszakítása, ha a csomópont hozzáadása a MySQL-szolgáltatás letiltása és kiosztásának megszüntetése hello gép.</span><span class="sxs-lookup"><span data-stu-id="9ab14-202">Stop MySQL, disable MySQL service from running on startup tooavoid disrupting hello cluster when adding a node, and deprovision hello machine.</span></span>

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. <span data-ttu-id="9ab14-203">Rögzítse a virtuális gép hello hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="9ab14-203">Capture hello VM through hello portal.</span></span> <span data-ttu-id="9ab14-204">(Jelenleg [#1268 hello Azure CLI-eszközei ki](https://github.com/Azure/azure-xplat-cli/issues/1268) hello tényt, hogy hello Azure CLI-eszközei a rögzített lemezképeket nem rögzíthető lemezkép a csatolt hello adatlemezek ismerteti.)</span><span class="sxs-lookup"><span data-stu-id="9ab14-204">(Currently, [issue #1268 in hello Azure CLI tools](https://github.com/Azure/azure-xplat-cli/issues/1268) describes hello fact that images captured by hello Azure CLI tools do not capture hello attached data disks.)</span></span>

    <span data-ttu-id="9ab14-205">a.</span><span class="sxs-lookup"><span data-stu-id="9ab14-205">a.</span></span> <span data-ttu-id="9ab14-206">Állítsa le hello gépet hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="9ab14-206">Shut down hello machine through hello portal.</span></span>

    <span data-ttu-id="9ab14-207">b.</span><span class="sxs-lookup"><span data-stu-id="9ab14-207">b.</span></span> <span data-ttu-id="9ab14-208">Kattintson a **rögzítése** , és adja meg a hello kép neve megegyezik **mariadb-galera-lemezkép**.</span><span class="sxs-lookup"><span data-stu-id="9ab14-208">Click **Capture** and specify hello image name as **mariadb-galera-image**.</span></span> <span data-ttu-id="9ab14-209">Adjon meg egy leírást, és ellenőrizze a "Rendelkezik futtattam a waagent."</span><span class="sxs-lookup"><span data-stu-id="9ab14-209">Provide a description and check "I have run waagent."</span></span>
      
      ![Hello virtuális gép rögzítése](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-hello-cluster"></a><span data-ttu-id="9ab14-211">Hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ab14-211">Create hello cluster</span></span>
<span data-ttu-id="9ab14-212">Hozzon létre három virtuális gépek hello sablonnal létrehozott, és ezután konfigurálása és hello fürt elindításához.</span><span class="sxs-lookup"><span data-stu-id="9ab14-212">Create three VMs with hello template you created, and then configure and start hello cluster.</span></span>

1. <span data-ttu-id="9ab14-213">Hozzon létre első CentOS 7 virtuális gép lemezképből hello mariadb-galera-rendszerkép alapján létrehozott, kezeléséről a következő információ hello hello:</span><span class="sxs-lookup"><span data-stu-id="9ab14-213">Create hello first CentOS 7 VM from hello mariadb-galera-image image you created, providing hello following information:</span></span>

 - <span data-ttu-id="9ab14-214">Virtuális hálózati név: mariadbvnet</span><span class="sxs-lookup"><span data-stu-id="9ab14-214">Virtual network name: mariadbvnet</span></span>
 - <span data-ttu-id="9ab14-215">Alhálózati: mariadb</span><span class="sxs-lookup"><span data-stu-id="9ab14-215">Subnet: mariadb</span></span>
 - <span data-ttu-id="9ab14-216">Méret gépen: Közepes</span><span class="sxs-lookup"><span data-stu-id="9ab14-216">Machine size: medium</span></span>
 - <span data-ttu-id="9ab14-217">Felhőszolgáltatás neve: mariadbha (vagy bármilyen neve, amelyhez toobe mariadbha.cloudapp.net keresztül érhető el)</span><span class="sxs-lookup"><span data-stu-id="9ab14-217">Cloud service name: mariadbha (or whatever name you want toobe accessed through mariadbha.cloudapp.net)</span></span>
 - <span data-ttu-id="9ab14-218">Számítógépnév: mariadb1</span><span class="sxs-lookup"><span data-stu-id="9ab14-218">Machine name: mariadb1</span></span>
 - <span data-ttu-id="9ab14-219">Felhasználónév: azureuser</span><span class="sxs-lookup"><span data-stu-id="9ab14-219">Username: azureuser</span></span>
 - <span data-ttu-id="9ab14-220">SSH-elérést: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="9ab14-220">SSH access: enabled</span></span>
 - <span data-ttu-id="9ab14-221">Sikeres hello SSH tanúsítvány .pem fájl, és /path/to/key.pem lecserélését hello elérési generált hello .pem SSH-kulcs tárolására.</span><span class="sxs-lookup"><span data-stu-id="9ab14-221">Passing hello SSH certificate .pem file and replacing /path/to/key.pem with hello path where you stored hello generated .pem SSH key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9ab14-222">hello következő parancsok el vannak osztva az átláthatóság többsoros, de egyes egy sorba kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="9ab14-222">hello following commands are split over multiple lines for clarity, but you should enter each as one line.</span></span>
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
2. <span data-ttu-id="9ab14-223">Hozzon létre két további virtuális gépek toohello mariadbha felhőszolgáltatás csatlakoztatja őket.</span><span class="sxs-lookup"><span data-stu-id="9ab14-223">Create two more virtual machines by connecting them toohello mariadbha cloud service.</span></span> <span data-ttu-id="9ab14-224">Virtuális gép nevének módosítása hello és hello SSH port tooa egyedi port nem ütközik más virtuális gépek a hello ugyanazt a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9ab14-224">Change hello VM name and hello SSH port tooa unique port not conflicting with other VMs in hello same cloud service.</span></span>

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
  <span data-ttu-id="9ab14-225">A MariaDB3:</span><span class="sxs-lookup"><span data-stu-id="9ab14-225">For MariaDB3:</span></span>

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
3. <span data-ttu-id="9ab14-226">Tooget hello belső IP-cím hello három virtuális gépek mindegyikének hello a következő lépésben lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="9ab14-226">You will need tooget hello internal IP address of each of hello three VMs for hello next step:</span></span>

    ![IP-cím beolvasása](./media/mariadb-mysql-cluster/IP.png)
4. <span data-ttu-id="9ab14-228">SSH toosign toohello három virtuális gép található, és azok konfigurációs fájl hello szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="9ab14-228">Use SSH toosign in toohello three VMs and edit hello configuration file on each of them.</span></span>

        sudo vi /etc/my.cnf.d/server.cnf

    <span data-ttu-id="9ab14-229">Állítsa vissza  **`wsrep_cluster_name`**  és  **`wsrep_cluster_address`**  hello eltávolításával  **#**  hello hello sor elején.</span><span class="sxs-lookup"><span data-stu-id="9ab14-229">Uncomment **`wsrep_cluster_name`** and **`wsrep_cluster_address`** by removing hello **#** at hello beginning of hello line.</span></span>
    <span data-ttu-id="9ab14-230">Továbbá cserélje le  **`<ServerIP>`**  a  **`wsrep_node_address`**  és  **`<NodeName>`**  a  **`wsrep_node_name`**  a hello A virtuális gép IP cím és nevet, illetve kódrészig, és azokat a sorokat.</span><span class="sxs-lookup"><span data-stu-id="9ab14-230">Additionally, replace **`<ServerIP>`** in **`wsrep_node_address`** and **`<NodeName>`** in **`wsrep_node_name`** with hello VM's IP address and name, respectively, and uncomment those lines as well.</span></span>
5. <span data-ttu-id="9ab14-231">Indítsa el a fürt hello a MariaDB1, és hagyja, hogy az indítási parancsot.</span><span class="sxs-lookup"><span data-stu-id="9ab14-231">Start hello cluster on MariaDB1 and let it run at startup.</span></span>

        sudo service mysql bootstrap
        chkconfig mysql on
6. <span data-ttu-id="9ab14-232">Indítsa el a MySQL MariaDB2 és MariaDB3, és hagyja, hogy az indítási parancsot.</span><span class="sxs-lookup"><span data-stu-id="9ab14-232">Start MySQL on MariaDB2 and MariaDB3 and let it run at startup.</span></span>

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-hello-cluster"></a><span data-ttu-id="9ab14-233">Load balance hello fürt</span><span class="sxs-lookup"><span data-stu-id="9ab14-233">Load balance hello cluster</span></span>
<span data-ttu-id="9ab14-234">Hello fürtözött virtuális gépek létrehozásakor hozzá azokat a clusteravset tooensure volt elhelyezése különböző tartalék és a frissítési tartományok és, hogy Azure soha nem minden gépen karbantartási egyszerre nevű rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="9ab14-234">When you created hello clustered VMs, you added them into an availability set called clusteravset tooensure that they were put on different fault and update domains and that Azure never does maintenance on all machines at once.</span></span> <span data-ttu-id="9ab14-235">Ez a konfiguráció megfelel-e toobe hello Azure szolgáltatásiszint-szerződéssel (SLA) által támogatott hello követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="9ab14-235">This configuration meets hello requirements toobe supported by hello Azure service level agreement (SLA).</span></span>

<span data-ttu-id="9ab14-236">Most használja az Azure Load Balancer toobalance kérelmek hello három csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="9ab14-236">Now use Azure Load Balancer toobalance requests between hello three nodes.</span></span>

<span data-ttu-id="9ab14-237">Futtassa a parancsokat a számítógépen a következő hello Azure parancssori felület használatával hello.</span><span class="sxs-lookup"><span data-stu-id="9ab14-237">Run hello following commands on your machine by using hello Azure CLI.</span></span>

<span data-ttu-id="9ab14-238">hello paraméterek parancsstruktúrát van:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span><span class="sxs-lookup"><span data-stu-id="9ab14-238">hello command parameters structure is: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span></span>

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

<span data-ttu-id="9ab14-239">hello CLI beállítása hello terhelés terheléselosztó mintavételi időköze too15 másodperc, elképzelhető, hogy egy kicsit túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="9ab14-239">hello CLI sets hello load balancer probe interval too15 seconds, which might be a bit too long.</span></span> <span data-ttu-id="9ab14-240">Hello portál megváltoztatnia **végpontok** bármely hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="9ab14-240">Change it in hello portal under **Endpoints** for any of hello VMs.</span></span>

![Végpont szerkesztése](./media/mariadb-mysql-cluster/Endpoint.PNG)

<span data-ttu-id="9ab14-242">Válassza ki **hitelesítés beállítása Reconfigure hello**.</span><span class="sxs-lookup"><span data-stu-id="9ab14-242">Select **Reconfigure hello Load-Balanced Set**.</span></span>

![Konfigurálja újra a hello-elosztott terhelésű készlet](./media/mariadb-mysql-cluster/Endpoint2.PNG)

<span data-ttu-id="9ab14-244">Változás **mintavételi időköze** too5 másodperc, és mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9ab14-244">Change **Probe Interval** too5 seconds and save your changes.</span></span>

![Mintavételi időköz módosítása](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-hello-cluster"></a><span data-ttu-id="9ab14-246">Hello fürt ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9ab14-246">Validate hello cluster</span></span>
<span data-ttu-id="9ab14-247">hello rögzített munkát.</span><span class="sxs-lookup"><span data-stu-id="9ab14-247">hello hard work is done.</span></span> <span data-ttu-id="9ab14-248">hello fürt legyen most már elérhető a `mariadbha.cloudapp.net:3306`, találatok száma a hello terheléselosztó és útvonal-kérelmek között hello három virtuális gépek zökkenőmentesen és hatékonyan.</span><span class="sxs-lookup"><span data-stu-id="9ab14-248">hello cluster should be now accessible at `mariadbha.cloudapp.net:3306`, which hits hello load balancer and route requests between hello three VMs smoothly and efficiently.</span></span>

<span data-ttu-id="9ab14-249">A kedvenc MySQL ügyfél tooconnect használja, vagy csatlakoztassa egy hello virtuális gépek tooverify, hogy a fürt működik.</span><span class="sxs-lookup"><span data-stu-id="9ab14-249">Use your favorite MySQL client tooconnect, or connect from one of hello VMs tooverify that this cluster is working.</span></span>

     mysql -u cluster -h mariadbha.cloudapp.net -p

<span data-ttu-id="9ab14-250">Ezután hozzon létre egy adatbázist, és feltöltheti olyan néhány adat.</span><span class="sxs-lookup"><span data-stu-id="9ab14-250">Then create a database and populate it with some data.</span></span>

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

<span data-ttu-id="9ab14-251">hello adatbázis létrehozta a következő táblázat hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="9ab14-251">hello database you created returns hello following table:</span></span>

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="9ab14-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ab14-252">Next steps</span></span>
<span data-ttu-id="9ab14-253">Ebben a cikkben egy három csomópontos MariaDB létrehozott + Galera magas rendelkezésre állású fürt Azure virtuális gépen futó CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="9ab14-253">In this article, you created a three-node MariaDB + Galera highly available cluster on Azure virtual machines running CentOS 7.</span></span> <span data-ttu-id="9ab14-254">hello virtuális gépek az elosztott terhelésű Azure terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="9ab14-254">hello VMs are load balanced with Azure Load Balancer.</span></span>

<span data-ttu-id="9ab14-255">Érdemes toolook: [egy másik módja toocluster Linux MySQL](mysql-cluster.md) és módon túl[optimalizálása és MySQL az Azure Linux virtuális gépeken futó teljesítménytesztelési](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="9ab14-255">You might want toolook at [another way toocluster MySQL on Linux](mysql-cluster.md) and ways too[optimize and test MySQL performance on Azure Linux VMs](optimize-mysql.md).</span></span>

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
