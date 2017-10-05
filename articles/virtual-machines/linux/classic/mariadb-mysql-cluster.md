---
title: "Egy MariaDB (MySQL) fürt futtatása az Azure-on |} Microsoft Docs"
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
ms.openlocfilehash: 53e9bf18b26338212411ea7c4f260eb308486738
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a><span data-ttu-id="15ac2-103">MariaDB (MySQL) fürt: Azure útmutató</span><span class="sxs-lookup"><span data-stu-id="15ac2-103">MariaDB (MySQL) cluster: Azure tutorial</span></span>
> [!IMPORTANT]
> <span data-ttu-id="15ac2-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="15ac2-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="15ac2-105">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="15ac2-105">This article covers the classic deployment model.</span></span> <span data-ttu-id="15ac2-106">A Microsoft azt javasolja, hogy az új telepítések esetén az Azure Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="15ac2-106">Microsoft recommends that most new deployments use the Azure Resource Manager model.</span></span>

> [!NOTE]
> <span data-ttu-id="15ac2-107">MariaDB vállalati fürt most nem áll rendelkezésre az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="15ac2-107">MariaDB Enterprise cluster is now available in the Azure Marketplace.</span></span> <span data-ttu-id="15ac2-108">Az új ajánlat automatikusan telepíteni fogja az Azure Resource Manager MariaDB Galera fürt.</span><span class="sxs-lookup"><span data-stu-id="15ac2-108">The new offering will automatically deploy a MariaDB Galera cluster on Azure Resource Manager.</span></span> <span data-ttu-id="15ac2-109">Az új ajánlat használjon [Azure piactér](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span><span class="sxs-lookup"><span data-stu-id="15ac2-109">You should use the new offering from [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span></span>
>
>

<span data-ttu-id="15ac2-110">Ez a cikk bemutatja, hogyan hozzon létre egy többszörös főkiszolgáló [Galera](http://galeracluster.com/products/) fürtben [MariaDBs](https://mariadb.org/en/about/) (nagy teljesítményű, méretezhető és megbízható Esőcsepp helyettesíti a MySQL) működését a magas rendelkezésre állású környezet az Azure-on virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="15ac2-110">This article shows you how to create a multi-Master [Galera](http://galeracluster.com/products/) cluster of [MariaDBs](https://mariadb.org/en/about/) (a robust, scalable, and reliable drop-in replacement for MySQL) to work in a highly available environment on Azure virtual machines.</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="15ac2-111">Az architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="15ac2-111">Architecture overview</span></span>
<span data-ttu-id="15ac2-112">Ez a cikk ismerteti, hogyan lehet elvégezni az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15ac2-112">This article describes how to complete the following steps:</span></span>

- <span data-ttu-id="15ac2-113">Hozzon létre egy három csomópontos fürtre.</span><span class="sxs-lookup"><span data-stu-id="15ac2-113">Create a three-node cluster.</span></span>
- <span data-ttu-id="15ac2-114">Az adatlemezek külön származó az operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="15ac2-114">Separate the data disks from the OS disk.</span></span>
- <span data-ttu-id="15ac2-115">Hozzon létre az adatlemezek RAID-0/csíkozott beállítás IOPS növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="15ac2-115">Create the data disks in RAID-0/striped setting to increase IOPS.</span></span>
- <span data-ttu-id="15ac2-116">Használja az Azure terheléselosztó a három csomópontok a terhelés kiegyenlítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="15ac2-116">Use Azure Load Balancer to balance the load for the three nodes.</span></span>
- <span data-ttu-id="15ac2-117">Ismétlődő munkahelyi minimalizálása érdekében hozzon létre egy Virtuálisgép-lemezkép MariaDB + Galera tartalmazó és a virtuális gépek más fürt létrehozására használható.</span><span class="sxs-lookup"><span data-stu-id="15ac2-117">To minimize repetitive work, create a VM image that contains MariaDB + Galera and use it to create the other cluster VMs.</span></span>

![Rendszer-architektúra](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> <span data-ttu-id="15ac2-119">Ez a témakör használja a [Azure CLI](../../../cli-install-nodejs.md) eszközök, ezért ügyeljen arra, hogy letöltheti a fájlokat, és csatlakoztassa őket az Azure-előfizetéshez a utasításainak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="15ac2-119">This topic uses the [Azure CLI](../../../cli-install-nodejs.md) tools, so make sure to download them and connect them to your Azure subscription according to the instructions.</span></span> <span data-ttu-id="15ac2-120">Ha egy hivatkozást az az Azure parancssori felület parancsai van szüksége, tekintse meg a [Azure CLI parancsdokumentációja](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="15ac2-120">If you need a reference to the commands available in the Azure CLI, see the [Azure CLI command reference](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="15ac2-121">Is kell [hitelesítéshez SSH-kulcs létrehozása] és jegyezze fel a .pem fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="15ac2-121">You will also need to [create an SSH key for authentication] and make note of the .pem file location.</span></span>
>
>

## <a name="create-the-template"></a><span data-ttu-id="15ac2-122">A sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="15ac2-122">Create the template</span></span>
### <a name="infrastructure"></a><span data-ttu-id="15ac2-123">Infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="15ac2-123">Infrastructure</span></span>
1. <span data-ttu-id="15ac2-124">Ahhoz, hogy az erőforrások együtt affinitáscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="15ac2-124">Create an affinity group to hold the resources together.</span></span>

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. <span data-ttu-id="15ac2-125">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="15ac2-125">Create a virtual network.</span></span>

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. <span data-ttu-id="15ac2-126">Hozzon létre egy tárfiókot, a lemezek.</span><span class="sxs-lookup"><span data-stu-id="15ac2-126">Create a storage account to host all our disks.</span></span> <span data-ttu-id="15ac2-127">Legfeljebb 40 terhelésnek kitett lemezek nem szabad elhelyezése elérte-e a 20 000 IOPS fiók méretkorlátot elkerülése érdekében ugyanazt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="15ac2-127">You shouldn't place more than 40 heavily used disks on the same storage account to avoid hitting the 20,000 IOPS storage account limit.</span></span> <span data-ttu-id="15ac2-128">Ebben az esetben Ön alatt ezt a határértéket, így mindent az egyszerűség kedvéért ugyanazt a fiókot fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="15ac2-128">In this case, you're well below that limit, so you'll store everything on the same account for simplicity.</span></span>

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. <span data-ttu-id="15ac2-129">Keresse meg a CentOS 7 virtuális gép lemezképe.</span><span class="sxs-lookup"><span data-stu-id="15ac2-129">Find the name of the CentOS 7 virtual machine image.</span></span>

        azure vm image list | findstr CentOS
   <span data-ttu-id="15ac2-130">A kimenet lesz hasonlót `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span><span class="sxs-lookup"><span data-stu-id="15ac2-130">The output will be something like `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span></span>

   <span data-ttu-id="15ac2-131">Használja ezt a nevet a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="15ac2-131">Use that name in the following step.</span></span>
5. <span data-ttu-id="15ac2-132">A Virtuálisgép-sablon létrehozásához, és cserélje le /path/to/key.pem az elérési út a generált .pem SSH-kulcs tárolására.</span><span class="sxs-lookup"><span data-stu-id="15ac2-132">Create the VM template and replace /path/to/key.pem with the path where you stored the generated .pem SSH key.</span></span>

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. <span data-ttu-id="15ac2-133">Négy 500 GB-os adatlemezt csatolni a virtuális Gépet a RAID-konfigurációban való használat céljából.</span><span class="sxs-lookup"><span data-stu-id="15ac2-133">Attach four 500-GB data disks to the VM for use in the RAID configuration.</span></span>

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. <span data-ttu-id="15ac2-134">Jelentkezzen be a sablon virtuális Gépet, amely létrehozta a következő mariadbhatemplate.cloudapp.net:22 az SSH segítségével, és a titkos kulcs használatával csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="15ac2-134">Use SSH to sign in to the template VM that you created at mariadbhatemplate.cloudapp.net:22, and connect by using your private key.</span></span>

### <a name="software"></a><span data-ttu-id="15ac2-135">Szoftver</span><span class="sxs-lookup"><span data-stu-id="15ac2-135">Software</span></span>
1. <span data-ttu-id="15ac2-136">A legfelső szintű beolvasása.</span><span class="sxs-lookup"><span data-stu-id="15ac2-136">Get the root.</span></span>

        sudo su

2. <span data-ttu-id="15ac2-137">A RAID telepítése:</span><span class="sxs-lookup"><span data-stu-id="15ac2-137">Install RAID support:</span></span>

    <span data-ttu-id="15ac2-138">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-138">a.</span></span> <span data-ttu-id="15ac2-139">Telepítse a mdadm.</span><span class="sxs-lookup"><span data-stu-id="15ac2-139">Install mdadm.</span></span>

              yum install mdadm

    <span data-ttu-id="15ac2-140">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-140">b.</span></span> <span data-ttu-id="15ac2-141">Hozza létre a RAID0/paritásos konfigurációt EXT4 fájlrendszerrel.</span><span class="sxs-lookup"><span data-stu-id="15ac2-141">Create the RAID0/stripe configuration with an EXT4 file system.</span></span>

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    <span data-ttu-id="15ac2-142">c.</span><span class="sxs-lookup"><span data-stu-id="15ac2-142">c.</span></span> <span data-ttu-id="15ac2-143">Hozzon létre a csatlakoztatási pont könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="15ac2-143">Create the mount point directory.</span></span>

              mkdir /mnt/data
    <span data-ttu-id="15ac2-144">d.</span><span class="sxs-lookup"><span data-stu-id="15ac2-144">d.</span></span> <span data-ttu-id="15ac2-145">Az újonnan létrehozott RAID eszköz UUID beolvasása.</span><span class="sxs-lookup"><span data-stu-id="15ac2-145">Retrieve the UUID of the newly created RAID device.</span></span>

              blkid | grep /dev/md0
    <span data-ttu-id="15ac2-146">e.</span><span class="sxs-lookup"><span data-stu-id="15ac2-146">e.</span></span> <span data-ttu-id="15ac2-147">/Etc/fstab szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="15ac2-147">Edit /etc/fstab.</span></span>

              vi /etc/fstab
    <span data-ttu-id="15ac2-148">f.</span><span class="sxs-lookup"><span data-stu-id="15ac2-148">f.</span></span> <span data-ttu-id="15ac2-149">Adja hozzá az eszközt, hogy engedélyezze az automatikus csatlakoztatását a újraindításkor UUID cseréje a során kapott érték az előző **blkid** parancsot.</span><span class="sxs-lookup"><span data-stu-id="15ac2-149">Add the device to enable auto mounting on reboot, replacing the UUID with the value obtained from the previous **blkid** command.</span></span>

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    <span data-ttu-id="15ac2-150">g.</span><span class="sxs-lookup"><span data-stu-id="15ac2-150">g.</span></span> <span data-ttu-id="15ac2-151">Csatlakoztassa az új partíciót.</span><span class="sxs-lookup"><span data-stu-id="15ac2-151">Mount the new partition.</span></span>

              mount /mnt/data

3. <span data-ttu-id="15ac2-152">Telepítse a MariaDB.</span><span class="sxs-lookup"><span data-stu-id="15ac2-152">Install MariaDB.</span></span>

    <span data-ttu-id="15ac2-153">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-153">a.</span></span> <span data-ttu-id="15ac2-154">Hozza létre a MariaDB.repo fájlt.</span><span class="sxs-lookup"><span data-stu-id="15ac2-154">Create the MariaDB.repo file.</span></span>

                vi /etc/yum.repos.d/MariaDB.repo

    <span data-ttu-id="15ac2-155">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-155">b.</span></span> <span data-ttu-id="15ac2-156">Töltse ki a tárházban fájl a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="15ac2-156">Fill the repo file with the following content:</span></span>

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    <span data-ttu-id="15ac2-157">c.</span><span class="sxs-lookup"><span data-stu-id="15ac2-157">c.</span></span> <span data-ttu-id="15ac2-158">Az ütközések elkerülése érdekében távolítsa el a meglévő utótag és mariadb-függvénytárak.</span><span class="sxs-lookup"><span data-stu-id="15ac2-158">To avoid conflicts, remove existing postfix and mariadb-libs.</span></span>

           yum remove postfix mariadb-libs-*
    <span data-ttu-id="15ac2-159">d.</span><span class="sxs-lookup"><span data-stu-id="15ac2-159">d.</span></span> <span data-ttu-id="15ac2-160">Telepítési MariaDB Galera.</span><span class="sxs-lookup"><span data-stu-id="15ac2-160">Install MariaDB with Galera.</span></span>

           yum install MariaDB-Galera-server MariaDB-client galera

4. <span data-ttu-id="15ac2-161">Helyezze át a MySQL adatkönyvtár a RAID-blokk eszköz.</span><span class="sxs-lookup"><span data-stu-id="15ac2-161">Move the MySQL data directory to the RAID block device.</span></span>

    <span data-ttu-id="15ac2-162">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-162">a.</span></span> <span data-ttu-id="15ac2-163">Az aktuális MySQL könyvtár átmásolja az új helyre, és távolítsa el a régi könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="15ac2-163">Copy the current MySQL directory into its new location and remove the old directory.</span></span>

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    <span data-ttu-id="15ac2-164">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-164">b.</span></span> <span data-ttu-id="15ac2-165">Ennek megfelelően állítsa be az új könyvtár engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="15ac2-165">Set permissions for the new directory accordingly.</span></span>

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    <span data-ttu-id="15ac2-166">c.</span><span class="sxs-lookup"><span data-stu-id="15ac2-166">c.</span></span> <span data-ttu-id="15ac2-167">Hozzon létre egy symlink mutat, a régi könyvtár az új helyre a RAID-partíción.</span><span class="sxs-lookup"><span data-stu-id="15ac2-167">Create a symlink that points the old directory to the new location on the RAID partition.</span></span>

           ln -s /mnt/data/mysql /var/lib/mysql

5. <span data-ttu-id="15ac2-168">Mivel [SELinux a fürt működését gátolja](http://galeracluster.com/documentation-webpages/configuration.html#selinux), le kell tiltani az aktuális munkamenetre szükséges.</span><span class="sxs-lookup"><span data-stu-id="15ac2-168">Because [SELinux interferes with the cluster operations](http://galeracluster.com/documentation-webpages/configuration.html#selinux), it is necessary to disable it for the current session.</span></span> <span data-ttu-id="15ac2-169">Szerkesztés `/etc/selinux/config` le kell tiltani a későbbi újraindítások.</span><span class="sxs-lookup"><span data-stu-id="15ac2-169">Edit `/etc/selinux/config` to disable it for subsequent restarts.</span></span>

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
6. <span data-ttu-id="15ac2-170">Ellenőrizze a MySQL futtatása.</span><span class="sxs-lookup"><span data-stu-id="15ac2-170">Validate MySQL runs.</span></span>

   <span data-ttu-id="15ac2-171">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-171">a.</span></span> <span data-ttu-id="15ac2-172">Indítsa el a MySQL.</span><span class="sxs-lookup"><span data-stu-id="15ac2-172">Start MySQL.</span></span>

           service mysql start
   <span data-ttu-id="15ac2-173">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-173">b.</span></span> <span data-ttu-id="15ac2-174">A MySQL-telepítés biztonságos, a legfelső szintű jelszó beállítását, távolítsa el a névtelen felhasználók számára, hogy tiltsa le a távoli legfelső szintű bejelentkezést és a test-adatbázis eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="15ac2-174">Secure the MySQL installation, set the root password, remove anonymous users to disable remote root login, and remove the test database.</span></span>

           mysql_secure_installation
   <span data-ttu-id="15ac2-175">c.</span><span class="sxs-lookup"><span data-stu-id="15ac2-175">c.</span></span> <span data-ttu-id="15ac2-176">Felhasználó létrehozása az adatbázis, a fürt működését, és szükség esetén az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="15ac2-176">Create a user on the database for cluster operations, and optionally for your applications.</span></span>

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   <span data-ttu-id="15ac2-177">d.</span><span class="sxs-lookup"><span data-stu-id="15ac2-177">d.</span></span> <span data-ttu-id="15ac2-178">Állítsa le a MySQL.</span><span class="sxs-lookup"><span data-stu-id="15ac2-178">Stop MySQL.</span></span>

            service mysql stop
7. <span data-ttu-id="15ac2-179">Hozzon létre egy konfigurációs helyőrző.</span><span class="sxs-lookup"><span data-stu-id="15ac2-179">Create a configuration placeholder.</span></span>

   <span data-ttu-id="15ac2-180">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-180">a.</span></span> <span data-ttu-id="15ac2-181">A fürtbeállítások helyőrzője létrehozása a MySQL konfigurációjának a szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="15ac2-181">Edit the MySQL configuration to create a placeholder for the cluster settings.</span></span> <span data-ttu-id="15ac2-182">Cserélje le a  **`<Variables>`**  vagy állítsa vissza a most.</span><span class="sxs-lookup"><span data-stu-id="15ac2-182">Do not replace the **`<Variables>`** or uncomment now.</span></span> <span data-ttu-id="15ac2-183">Amely akkor után hozzon létre egy virtuális Gépet a sablon alapján történik.</span><span class="sxs-lookup"><span data-stu-id="15ac2-183">That will happen after you create a VM from this template.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="15ac2-184">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-184">b.</span></span> <span data-ttu-id="15ac2-185">Szerkessze a  **[galera]**  szakaszt, és törölje a jelölést.</span><span class="sxs-lookup"><span data-stu-id="15ac2-185">Edit the **[galera]** section and clear it out.</span></span>

   <span data-ttu-id="15ac2-186">c.</span><span class="sxs-lookup"><span data-stu-id="15ac2-186">c.</span></span> <span data-ttu-id="15ac2-187">Szerkessze a **[mariadb]** szakasz.</span><span class="sxs-lookup"><span data-stu-id="15ac2-187">Edit the **[mariadb]** section.</span></span>

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server
8. <span data-ttu-id="15ac2-188">Nyissa meg a szükséges portok a tűzfalon keresztül FirewallD CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="15ac2-188">Open required ports on the firewall by using FirewallD on CentOS 7.</span></span>

   * <span data-ttu-id="15ac2-189">MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="15ac2-189">MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span></span>
   * <span data-ttu-id="15ac2-190">GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="15ac2-190">GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span></span>
   * <span data-ttu-id="15ac2-191">GALERA ITT:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="15ac2-191">GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span></span>
   * <span data-ttu-id="15ac2-192">RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="15ac2-192">RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span></span>
   * <span data-ttu-id="15ac2-193">Töltse be újra a tűzfalon:`firewall-cmd --reload`</span><span class="sxs-lookup"><span data-stu-id="15ac2-193">Reload the firewall: `firewall-cmd --reload`</span></span>

9. <span data-ttu-id="15ac2-194">A rendszer a teljesítmény optimalizálása.</span><span class="sxs-lookup"><span data-stu-id="15ac2-194">Optimize the system for performance.</span></span> <span data-ttu-id="15ac2-195">További információkért lásd: [teljesítményének hangolása stratégia](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="15ac2-195">For more information, see [performance tuning strategy](optimize-mysql.md).</span></span>

   <span data-ttu-id="15ac2-196">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-196">a.</span></span> <span data-ttu-id="15ac2-197">A MySQL-konfigurációs fájl újból szerkeszteni.</span><span class="sxs-lookup"><span data-stu-id="15ac2-197">Edit the MySQL configuration file again.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="15ac2-198">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-198">b.</span></span> <span data-ttu-id="15ac2-199">Szerkessze a **[mariadb]** szakaszt, és hozzáfűzése a következőket:</span><span class="sxs-lookup"><span data-stu-id="15ac2-199">Edit the **[mariadb]** section and append the following content:</span></span>

   > [!NOTE]
   > <span data-ttu-id="15ac2-200">Azt javasoljuk, hogy innodb\_puffer\_pool_size a virtuális gép memória 70 százalék.</span><span class="sxs-lookup"><span data-stu-id="15ac2-200">We recommend that innodb\_buffer\_pool_size is 70 percent of your VM's memory.</span></span> <span data-ttu-id="15ac2-201">Ebben a példában beállítása 2.45 GB a közepes 3.5-ös GB RAM-MAL rendelkező Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="15ac2-201">In this example, it has been set at 2.45 GB for the medium Azure VM with 3.5 GB of RAM.</span></span>
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. <span data-ttu-id="15ac2-202">Állítsa le a MySQL, futtatását indítási elkerülése érdekében a fürt megszakítása, ha a csomópont hozzáadása a MySQL-szolgáltatás letiltása, és a gép kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="15ac2-202">Stop MySQL, disable MySQL service from running on startup to avoid disrupting the cluster when adding a node, and deprovision the machine.</span></span>

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. <span data-ttu-id="15ac2-203">Rögzítse a virtuális Gépet a portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="15ac2-203">Capture the VM through the portal.</span></span> <span data-ttu-id="15ac2-204">(Jelenleg [adja ki az Azure CLI-eszközei #1268](https://github.com/Azure/azure-xplat-cli/issues/1268) ismerteti azt a tényt, hogy az Azure CLI-eszközök által rögzített lemezképeket nem rögzíthető lemezkép a mellékelt adatok lemezek.)</span><span class="sxs-lookup"><span data-stu-id="15ac2-204">(Currently, [issue #1268 in the Azure CLI tools](https://github.com/Azure/azure-xplat-cli/issues/1268) describes the fact that images captured by the Azure CLI tools do not capture the attached data disks.)</span></span>

    <span data-ttu-id="15ac2-205">a.</span><span class="sxs-lookup"><span data-stu-id="15ac2-205">a.</span></span> <span data-ttu-id="15ac2-206">Állítsa le a gépet a portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="15ac2-206">Shut down the machine through the portal.</span></span>

    <span data-ttu-id="15ac2-207">b.</span><span class="sxs-lookup"><span data-stu-id="15ac2-207">b.</span></span> <span data-ttu-id="15ac2-208">Kattintson a **rögzítése** , és adja meg a lemezkép neve, ahogyan **mariadb-galera-lemezkép**.</span><span class="sxs-lookup"><span data-stu-id="15ac2-208">Click **Capture** and specify the image name as **mariadb-galera-image**.</span></span> <span data-ttu-id="15ac2-209">Adjon meg egy leírást, és ellenőrizze a "Rendelkezik futtattam a waagent."</span><span class="sxs-lookup"><span data-stu-id="15ac2-209">Provide a description and check "I have run waagent."</span></span>
      
      ![A virtuális gép rögzítése](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a><span data-ttu-id="15ac2-211">A fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="15ac2-211">Create the cluster</span></span>
<span data-ttu-id="15ac2-212">Hozzon létre három virtuális gépek sablonnal létrehozott, és ezután konfigurálása és a fürt elindításához.</span><span class="sxs-lookup"><span data-stu-id="15ac2-212">Create three VMs with the template you created, and then configure and start the cluster.</span></span>

1. <span data-ttu-id="15ac2-213">Az első CentOS 7 virtuális gép létrehozása a mariadb-galera-lemezkép lemezképet létrehozni, a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="15ac2-213">Create the first CentOS 7 VM from the mariadb-galera-image image you created, providing the following information:</span></span>

 - <span data-ttu-id="15ac2-214">Virtuális hálózati név: mariadbvnet</span><span class="sxs-lookup"><span data-stu-id="15ac2-214">Virtual network name: mariadbvnet</span></span>
 - <span data-ttu-id="15ac2-215">Alhálózati: mariadb</span><span class="sxs-lookup"><span data-stu-id="15ac2-215">Subnet: mariadb</span></span>
 - <span data-ttu-id="15ac2-216">Méret gépen: Közepes</span><span class="sxs-lookup"><span data-stu-id="15ac2-216">Machine size: medium</span></span>
 - <span data-ttu-id="15ac2-217">Felhőszolgáltatás neve: mariadbha (vagy mariadbha.cloudapp.net elérni kívánt nevet)</span><span class="sxs-lookup"><span data-stu-id="15ac2-217">Cloud service name: mariadbha (or whatever name you want to be accessed through mariadbha.cloudapp.net)</span></span>
 - <span data-ttu-id="15ac2-218">Számítógépnév: mariadb1</span><span class="sxs-lookup"><span data-stu-id="15ac2-218">Machine name: mariadb1</span></span>
 - <span data-ttu-id="15ac2-219">Felhasználónév: azureuser</span><span class="sxs-lookup"><span data-stu-id="15ac2-219">Username: azureuser</span></span>
 - <span data-ttu-id="15ac2-220">SSH-elérést: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="15ac2-220">SSH access: enabled</span></span>
 - <span data-ttu-id="15ac2-221">Átadja a SSH tanúsítvány .pem fájlt, és /path/to/key.pem cseréje az elérési út a generált .pem SSH-kulcs tárolására.</span><span class="sxs-lookup"><span data-stu-id="15ac2-221">Passing the SSH certificate .pem file and replacing /path/to/key.pem with the path where you stored the generated .pem SSH key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="15ac2-222">A következő parancsokat az átláthatóság többsoros vannak osztva, de egyes egy sorba kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="15ac2-222">The following commands are split over multiple lines for clarity, but you should enter each as one line.</span></span>
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
2. <span data-ttu-id="15ac2-223">Hozzon létre két további virtuális gépek csatlakoztatja őket a mariadbha felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="15ac2-223">Create two more virtual machines by connecting them to the mariadbha cloud service.</span></span> <span data-ttu-id="15ac2-224">Módosítsa a virtuális gép nevét és az SSH-port nem ütközik más virtuális gépek ugyanazt a felhőszolgáltatásban található egy egyedi portot.</span><span class="sxs-lookup"><span data-stu-id="15ac2-224">Change the VM name and the SSH port to a unique port not conflicting with other VMs in the same cloud service.</span></span>

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
  <span data-ttu-id="15ac2-225">A MariaDB3:</span><span class="sxs-lookup"><span data-stu-id="15ac2-225">For MariaDB3:</span></span>

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
3. <span data-ttu-id="15ac2-226">A három virtuális gépek mindegyikének a belső IP-cím lekérése a következő lépésben lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="15ac2-226">You will need to get the internal IP address of each of the three VMs for the next step:</span></span>

    ![IP-cím beolvasása](./media/mariadb-mysql-cluster/IP.png)
4. <span data-ttu-id="15ac2-228">SSH használatával jelentkezzen be a három virtuális gépeket, majd szerkessze a konfigurációs fájlt a hozzájuk.</span><span class="sxs-lookup"><span data-stu-id="15ac2-228">Use SSH to sign in to the three VMs and edit the configuration file on each of them.</span></span>

        sudo vi /etc/my.cnf.d/server.cnf

    <span data-ttu-id="15ac2-229">Állítsa vissza  **`wsrep_cluster_name`**  és  **`wsrep_cluster_address`**  eltávolításával a  **#**  a sor elején.</span><span class="sxs-lookup"><span data-stu-id="15ac2-229">Uncomment **`wsrep_cluster_name`** and **`wsrep_cluster_address`** by removing the **#** at the beginning of the line.</span></span>
    <span data-ttu-id="15ac2-230">Továbbá cserélje le  **`<ServerIP>`**  a  **`wsrep_node_address`**  és  **`<NodeName>`**  a  **`wsrep_node_name`**  rendelkező a A virtuális gép IP cím és nevet, illetve kódrészig, és azokat a sorokat.</span><span class="sxs-lookup"><span data-stu-id="15ac2-230">Additionally, replace **`<ServerIP>`** in **`wsrep_node_address`** and **`<NodeName>`** in **`wsrep_node_name`** with the VM's IP address and name, respectively, and uncomment those lines as well.</span></span>
5. <span data-ttu-id="15ac2-231">Indítsa el a fürt a MariaDB1, és hagyja, hogy az indítási parancsot.</span><span class="sxs-lookup"><span data-stu-id="15ac2-231">Start the cluster on MariaDB1 and let it run at startup.</span></span>

        sudo service mysql bootstrap
        chkconfig mysql on
6. <span data-ttu-id="15ac2-232">Indítsa el a MySQL MariaDB2 és MariaDB3, és hagyja, hogy az indítási parancsot.</span><span class="sxs-lookup"><span data-stu-id="15ac2-232">Start MySQL on MariaDB2 and MariaDB3 and let it run at startup.</span></span>

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a><span data-ttu-id="15ac2-233">A fürt terheléselosztás</span><span class="sxs-lookup"><span data-stu-id="15ac2-233">Load balance the cluster</span></span>
<span data-ttu-id="15ac2-234">A fürtözött virtuális gépek létrehozásakor hozzá azokat a clusteravset annak érdekében, hogy a különböző tartalék és a frissítési tartományok fel, és egyszerre csak a soha nem nem karbantartás, minden számítógépen, hogy Azure nevű rendelkezésre állási csoportok.</span><span class="sxs-lookup"><span data-stu-id="15ac2-234">When you created the clustered VMs, you added them into an availability set called clusteravset to ensure that they were put on different fault and update domains and that Azure never does maintenance on all machines at once.</span></span> <span data-ttu-id="15ac2-235">Ez a konfiguráció megfelel az Azure szolgáltatásiszint-szerződéssel (SLA) támogatott.</span><span class="sxs-lookup"><span data-stu-id="15ac2-235">This configuration meets the requirements to be supported by the Azure service level agreement (SLA).</span></span>

<span data-ttu-id="15ac2-236">Azure Load Balancer segítségével kiegyenlítheti vonatkozó a három csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="15ac2-236">Now use Azure Load Balancer to balance requests between the three nodes.</span></span>

<span data-ttu-id="15ac2-237">A következő parancsokat a számítógépen az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="15ac2-237">Run the following commands on your machine by using the Azure CLI.</span></span>

<span data-ttu-id="15ac2-238">A parancs paraméterei struktúra a következő:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span><span class="sxs-lookup"><span data-stu-id="15ac2-238">The command parameters structure is: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span></span>

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

<span data-ttu-id="15ac2-239">A parancssori felület a load balancer mintavétel gyakoriságát 15 másodpercre állítja, elképzelhető, hogy egy kicsit túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="15ac2-239">The CLI sets the load balancer probe interval to 15 seconds, which might be a bit too long.</span></span> <span data-ttu-id="15ac2-240">A portál módosítása **végpontok** bármely, a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="15ac2-240">Change it in the portal under **Endpoints** for any of the VMs.</span></span>

![Végpont szerkesztése](./media/mariadb-mysql-cluster/Endpoint.PNG)

<span data-ttu-id="15ac2-242">Válassza ki **konfigurálja újra az elosztott terhelésű készlet**.</span><span class="sxs-lookup"><span data-stu-id="15ac2-242">Select **Reconfigure the Load-Balanced Set**.</span></span>

![Konfigurálja újra az elosztott terhelésű készlet](./media/mariadb-mysql-cluster/Endpoint2.PNG)

<span data-ttu-id="15ac2-244">Változás **mintavételi időköze** 5 másodperc és a változtatások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="15ac2-244">Change **Probe Interval** to 5 seconds and save your changes.</span></span>

![Mintavételi időköz módosítása](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a><span data-ttu-id="15ac2-246">A fürt ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="15ac2-246">Validate the cluster</span></span>
<span data-ttu-id="15ac2-247">A rögzített munkát.</span><span class="sxs-lookup"><span data-stu-id="15ac2-247">The hard work is done.</span></span> <span data-ttu-id="15ac2-248">A fürt elérhető kell, hogy most a `mariadbha.cloudapp.net:3306`, amely találatok, a terheléselosztó és útvonal terhelésigényét a három virtuális gépek közötti hatékony és problémamentesen.</span><span class="sxs-lookup"><span data-stu-id="15ac2-248">The cluster should be now accessible at `mariadbha.cloudapp.net:3306`, which hits the load balancer and route requests between the three VMs smoothly and efficiently.</span></span>

<span data-ttu-id="15ac2-249">A kedvenc MySQL-ügyfél segítségével csatlakoztassa, vagy csatlakoztassa a virtuális gépek ellenőrzése, hogy működik-e a fürt egyik.</span><span class="sxs-lookup"><span data-stu-id="15ac2-249">Use your favorite MySQL client to connect, or connect from one of the VMs to verify that this cluster is working.</span></span>

     mysql -u cluster -h mariadbha.cloudapp.net -p

<span data-ttu-id="15ac2-250">Ezután hozzon létre egy adatbázist, és feltöltheti olyan néhány adat.</span><span class="sxs-lookup"><span data-stu-id="15ac2-250">Then create a database and populate it with some data.</span></span>

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

<span data-ttu-id="15ac2-251">A létrehozott adatbázis a következő táblázatot ad vissza:</span><span class="sxs-lookup"><span data-stu-id="15ac2-251">The database you created returns the following table:</span></span>

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="15ac2-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15ac2-252">Next steps</span></span>
<span data-ttu-id="15ac2-253">Ebben a cikkben egy három csomópontos MariaDB létrehozott + Galera magas rendelkezésre állású fürt Azure virtuális gépen futó CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="15ac2-253">In this article, you created a three-node MariaDB + Galera highly available cluster on Azure virtual machines running CentOS 7.</span></span> <span data-ttu-id="15ac2-254">A virtuális gépek az elosztott terhelésű Azure terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="15ac2-254">The VMs are load balanced with Azure Load Balancer.</span></span>

<span data-ttu-id="15ac2-255">Érdemes megtekinteni [MySQL-fürt Linux rendszeren másik módja](mysql-cluster.md) és módjai [optimalizálása és MySQL az Azure Linux virtuális gépeken futó teljesítménytesztelési](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="15ac2-255">You might want to look at [another way to cluster MySQL on Linux](mysql-cluster.md) and ways to [optimize and test MySQL performance on Azure Linux VMs](optimize-mysql.md).</span></span>

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating the template]:#creating-the-template
[Creating the cluster]:#creating-the-cluster
[Load balancing the cluster]:#load-balancing-the-cluster
[Validating the cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
<span data-ttu-id="15ac2-256">[Galera]:http://galeracluster.com/products/</span><span class="sxs-lookup"><span data-stu-id="15ac2-256">[Galera]:http://galeracluster.com/products/</span></span>
[MariaDBs]:https://mariadb.org/en/about/
<span data-ttu-id="15ac2-257">[hitelesítéshez SSH-kulcs létrehozása]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/</span><span class="sxs-lookup"><span data-stu-id="15ac2-257">[create an SSH key for authentication]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/</span></span>
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
