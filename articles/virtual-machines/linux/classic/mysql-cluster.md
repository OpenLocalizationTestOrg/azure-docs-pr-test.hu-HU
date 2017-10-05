---
title: "Az elosztott terhelésű készletek MySQL clusterize |} Microsoft Docs"
description: "Állítson be egy elosztott terhelésű, a magas rendelkezésre állású, a klasszikus üzembe helyezési modellel, az Azure-on Linux MySQL-fürt létrehozása"
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 4eaf86c9ac3e4dc2b51b88383626eda774cab0e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-load-balanced-sets-to-clusterize-mysql-on-linux"></a><span data-ttu-id="5d27d-103">Elosztott terhelésű készletek segítségével clusterize MySQL Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="5d27d-103">Use load-balanced sets to clusterize MySQL on Linux</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5d27d-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="5d27d-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="5d27d-105">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5d27d-105">This article covers using the classic deployment model.</span></span> <span data-ttu-id="5d27d-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="5d27d-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="5d27d-107">A [Resource Manager-sablon](https://azure.microsoft.com/documentation/templates/mysql-replication/) érhető el, ha a MySQL-fürt üzembe kell.</span><span class="sxs-lookup"><span data-stu-id="5d27d-107">A [Resource Manager template](https://azure.microsoft.com/documentation/templates/mysql-replication/) is available if you need to deploy a MySQL cluster.</span></span>

<span data-ttu-id="5d27d-108">Ez a cikk ismerteti, és helyezhetők üzembe a Microsoft Azure-böngészést a MySQL-kiszolgáló magas rendelkezésre állás mint egy ismertetése a magas rendelkezésre állású Linux-alapú szolgáltatások különböző szempontok mutatja be.</span><span class="sxs-lookup"><span data-stu-id="5d27d-108">This article explores and illustrates the different approaches available to deploy highly available Linux-based services on Microsoft Azure, exploring MySQL Server high availability as a primer.</span></span> <span data-ttu-id="5d27d-109">Ez a megközelítés bemutató videó megtalálható [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span><span class="sxs-lookup"><span data-stu-id="5d27d-109">A video illustrating this approach is available on [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span></span>

<span data-ttu-id="5d27d-110">Azt fogja szerkezeti osztott, két csomópontos, egyszeri MySQL magas rendelkezésre állású megoldás DRBD, Corosync és támasztja alapján.</span><span class="sxs-lookup"><span data-stu-id="5d27d-110">We will outline a shared-nothing, two-node, single-master MySQL high availability solution based on DRBD, Corosync, and Pacemaker.</span></span> <span data-ttu-id="5d27d-111">Csak egy csomópont MySQL fut egyszerre.</span><span class="sxs-lookup"><span data-stu-id="5d27d-111">Only one node runs MySQL at a time.</span></span> <span data-ttu-id="5d27d-112">A DRBD erőforrás írásakor vagy olvasásakor korlátozódik is csak egy csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="5d27d-112">Reading and writing from the DRBD resource is also limited to only one node at a time.</span></span>

<span data-ttu-id="5d27d-113">Mert fogjuk elosztott terhelésű készlet a Microsoft Azure-ban ciklikus multiplexelés funkcionalitásra vagy a végpont észlelési, eltávolítás és szabályos helyreállítása a VIP, nincs szükség például HÉ, VIP-megoldások esetén.</span><span class="sxs-lookup"><span data-stu-id="5d27d-113">There's no need for a VIP solution like LVS, because you'll use load-balanced sets in Microsoft Azure to provide round-robin functionality and endpoint detection, removal, and graceful recovery of the VIP.</span></span> <span data-ttu-id="5d27d-114">A VIP nem egy globálisan irányíthatóvá teszi IPv4-cím, a Microsoft Azure által hozzárendelt, amikor először létre kell hoznia a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="5d27d-114">The VIP is a globally routable IPv4 address assigned by Microsoft Azure when you first create the cloud service.</span></span>

<span data-ttu-id="5d27d-115">Nincsenek elérhető, mint egy virtuális gép MySQL, beleértve a NBD fürt, Percona, Galera és több köztes megoldások, beleértve a legalább egy másik lehetséges architektúrához [VM raktár számára](http://vmdepot.msopentech.com).</span><span class="sxs-lookup"><span data-stu-id="5d27d-115">There are other possible architectures for MySQL, including NBD Cluster, Percona, Galera, and several middleware solutions, including at least one available as a VM on [VM Depot](http://vmdepot.msopentech.com).</span></span> <span data-ttu-id="5d27d-116">Mindaddig, amíg ezek a megoldások képes replikálni, amely a csoportos küldést vagy szórásos és egyedi küldéses, és nem függ a megosztott tárhelytől vagy több hálózati adapterrel, a forgatókönyvek egyszerűen telepíthető a Microsoft Azure kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5d27d-116">As long as these solutions can replicate on unicast vs. multicast or broadcast and don't rely on shared storage or multiple network interfaces, the scenarios should be easy to deploy on Microsoft Azure.</span></span>

<span data-ttu-id="5d27d-117">Ezek a fürtszolgáltatás architektúrák kiterjeszthető más termékek, például a PostgreSQL és OpenLDAP hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="5d27d-117">These clustering architectures can be extended to other products like PostgreSQL and OpenLDAP in a similar fashion.</span></span> <span data-ttu-id="5d27d-118">Például a megosztás terheléselosztás eljárás sikeresen tesztelték, több főkiszolgálós OpenLDAP, és figyelheti a Channel 9 blogjában.</span><span class="sxs-lookup"><span data-stu-id="5d27d-118">For example, this load-balancing procedure with shared nothing was successfully tested with multi-master OpenLDAP, and you can watch it on our Channel 9 blog.</span></span>

## <a name="get-ready"></a><span data-ttu-id="5d27d-119">Felkészülés</span><span class="sxs-lookup"><span data-stu-id="5d27d-119">Get ready</span></span>
<span data-ttu-id="5d27d-120">A következő erőforrások és képességek lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5d27d-120">You need the following resources and abilities:</span></span>

  - <span data-ttu-id="5d27d-121">Egy érvényes előfizetéssel, létrehozhat legalább két virtuális gépek Microsoft Azure-fiókra (ebben a példában a XS lett megadva)</span><span class="sxs-lookup"><span data-stu-id="5d27d-121">A Microsoft Azure account with a valid subscription, able to create at least two VMs (XS was used in this example)</span></span>
  - <span data-ttu-id="5d27d-122">A hálózat és alhálózat</span><span class="sxs-lookup"><span data-stu-id="5d27d-122">A network and a subnet</span></span>
  - <span data-ttu-id="5d27d-123">Az affinitáscsoportokban</span><span class="sxs-lookup"><span data-stu-id="5d27d-123">An affinity group</span></span>
  - <span data-ttu-id="5d27d-124">Rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="5d27d-124">An availability set</span></span>
  - <span data-ttu-id="5d27d-125">Virtuális merevlemezek létrehozása ugyanabban a régióban, mint a felhőalapú szolgáltatás, és csatolja őket a Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5d27d-125">The ability to create VHDs in the same region as the cloud service and attach them to the Linux VMs</span></span>

### <a name="tested-environment"></a><span data-ttu-id="5d27d-126">Tesztelt környezet</span><span class="sxs-lookup"><span data-stu-id="5d27d-126">Tested environment</span></span>
* <span data-ttu-id="5d27d-127">Ubuntu 13.10</span><span class="sxs-lookup"><span data-stu-id="5d27d-127">Ubuntu 13.10</span></span>
  * <span data-ttu-id="5d27d-128">DRBD</span><span class="sxs-lookup"><span data-stu-id="5d27d-128">DRBD</span></span>
  * <span data-ttu-id="5d27d-129">MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="5d27d-129">MySQL Server</span></span>
  * <span data-ttu-id="5d27d-130">Corosync és támasztja</span><span class="sxs-lookup"><span data-stu-id="5d27d-130">Corosync and Pacemaker</span></span>

### <a name="affinity-group"></a><span data-ttu-id="5d27d-131">Affinitáscsoportok</span><span class="sxs-lookup"><span data-stu-id="5d27d-131">Affinity group</span></span>
<span data-ttu-id="5d27d-132">A megoldás affinitáscsoportok létrehozása a klasszikus Azure portálra, a bejelentkezéssel kiválasztásával **beállítások**, és egy affinitáscsoporthoz létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5d27d-132">Create an affinity group for the solution by signing in to the Azure classic portal, selecting **Settings**, and creating an affinity group.</span></span> <span data-ttu-id="5d27d-133">Újabb kiosztott erőforrásokat rendeli az affinitáscsoportban.</span><span class="sxs-lookup"><span data-stu-id="5d27d-133">Allocated resources created later will be assigned to this affinity group.</span></span>

### <a name="networks"></a><span data-ttu-id="5d27d-134">Hálózatok</span><span class="sxs-lookup"><span data-stu-id="5d27d-134">Networks</span></span>
<span data-ttu-id="5d27d-135">Egy új hálózatot hoznak létre, és egy alhálózat létre van hozva, a hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="5d27d-135">A new network is created, and a subnet is created inside the network.</span></span> <span data-ttu-id="5d27d-136">A példa 10.10.10.0/24 hálózaton belül csak egy /24 alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="5d27d-136">This example uses a 10.10.10.0/24 network with only one /24 subnet inside.</span></span>

### <a name="virtual-machines"></a><span data-ttu-id="5d27d-137">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="5d27d-137">Virtual machines</span></span>
<span data-ttu-id="5d27d-138">Az első Ubuntu 13.10 VM Endorsed Ubuntu gyűjtemény lemezkép használatával hozták létre, és nevezik `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-138">The first Ubuntu 13.10 VM is created by using an Endorsed Ubuntu Gallery image and is called `hadb01`.</span></span> <span data-ttu-id="5d27d-139">Új felhőszolgáltatás az a folyamat, amely a hadb jön létre.</span><span class="sxs-lookup"><span data-stu-id="5d27d-139">A new cloud service is created in the process, called hadb.</span></span> <span data-ttu-id="5d27d-140">Ez a név azt mutatja be, hogy a szolgáltatás további erőforrások hozzáadásakor megosztott, elosztott terhelésű jellegét.</span><span class="sxs-lookup"><span data-stu-id="5d27d-140">This name illustrates the shared, load-balanced nature that the service will have when more resources are added.</span></span> <span data-ttu-id="5d27d-141">A létrehozását `hadb01` Eseménytelen és kész, a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5d27d-141">The creation of `hadb01` is uneventful and completed by using the portal.</span></span> <span data-ttu-id="5d27d-142">Az SSH a végpont automatikusan létrejön, és az új hálózati van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="5d27d-142">An endpoint for SSH is automatically created, and the new network is selected.</span></span> <span data-ttu-id="5d27d-143">Most egy rendelkezésre állási készletét, a virtuális gépek is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5d27d-143">Now you can create an availability set for the VMs.</span></span>

<span data-ttu-id="5d27d-144">Az első virtuális gép létrehozása (technikailag, amikor a felhőalapú szolgáltatás létrehozása), akkor létre kell hoznia a második virtuális gép `hadb02`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-144">After the first VM is created (technically, when the cloud service is created), create the second VM, `hadb02`.</span></span> <span data-ttu-id="5d27d-145">A második virtuális gép Ubuntu 13.10 virtuális gép a gyűjteményből a portál használatával használni, de egy meglévő felhőszolgáltatáshoz `hadb.cloudapp.net`, egy új létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="5d27d-145">For the second VM, use Ubuntu 13.10 VM from the Gallery by using the portal, but use an existing cloud service, `hadb.cloudapp.net`, instead of creating a new one.</span></span> <span data-ttu-id="5d27d-146">A hálózati és a rendelkezésre állási készlet ki automatikusan.</span><span class="sxs-lookup"><span data-stu-id="5d27d-146">The network and availability set should be automatically selected.</span></span> <span data-ttu-id="5d27d-147">Egy SSH-végpontot rendel, túl.</span><span class="sxs-lookup"><span data-stu-id="5d27d-147">An SSH endpoint will be created, too.</span></span>

<span data-ttu-id="5d27d-148">Mindkét virtuális gép létrehozása után tekintse meg az SSH-port `hadb01` (22-es TCP) és `hadb02` (Ez automatikusan hozzárendelve az Azure-ban).</span><span class="sxs-lookup"><span data-stu-id="5d27d-148">After both VMs have been created, take note of the SSH port for `hadb01` (TCP 22) and `hadb02` (automatically assigned by Azure).</span></span>

### <a name="attached-storage"></a><span data-ttu-id="5d27d-149">Csatlakoztatott tároló</span><span class="sxs-lookup"><span data-stu-id="5d27d-149">Attached storage</span></span>
<span data-ttu-id="5d27d-150">Új lemez csatolása mindkét virtuális gépeket, és 5 GB-os létrehozásához a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="5d27d-150">Attach a new disk to both VMs and create 5-GB disks in the process.</span></span> <span data-ttu-id="5d27d-151">A lemezek a fő operációsrendszer-lemezek használt VHD-tároló tárolt.</span><span class="sxs-lookup"><span data-stu-id="5d27d-151">The disks are hosted in the VHD container in use for your main operating system disks.</span></span> <span data-ttu-id="5d27d-152">Miután lemezek létrehozni és csatolni, nincs szükség Linux újraindítást, mivel a kernel jelenik meg az új eszköz.</span><span class="sxs-lookup"><span data-stu-id="5d27d-152">After disks are created and attached, there is no need to restart Linux because the kernel will see the new device.</span></span> <span data-ttu-id="5d27d-153">Ez az eszköz megfelel általában `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-153">This device is usually `/dev/sdc`.</span></span> <span data-ttu-id="5d27d-154">Ellenőrizze `dmesg` a kimeneti oldal számára.</span><span class="sxs-lookup"><span data-stu-id="5d27d-154">Check `dmesg` for the output.</span></span>

<span data-ttu-id="5d27d-155">Minden egyes virtuális gépen, hozzon létre egy partíciót használatával `cfdisk` (elsődleges, Linux partíció) és az új partíció tábla írása.</span><span class="sxs-lookup"><span data-stu-id="5d27d-155">On each VM, create a partition by using `cfdisk` (primary, Linux partition) and write the new partition table.</span></span> <span data-ttu-id="5d27d-156">Ne hozzon létre egy fájlrendszer ezt a partíciót.</span><span class="sxs-lookup"><span data-stu-id="5d27d-156">Do not create a file system on this partition.</span></span>

## <a name="set-up-the-cluster"></a><span data-ttu-id="5d27d-157">A fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="5d27d-157">Set up the cluster</span></span>
<span data-ttu-id="5d27d-158">APT segítségével telepítse a Corosync támasztja és DRBD mindkét Ubuntu virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="5d27d-158">Use APT to install Corosync, Pacemaker, and DRBD on both Ubuntu VMs.</span></span> <span data-ttu-id="5d27d-159">Ehhez a `apt-get`, futtassa a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="5d27d-159">To do so with `apt-get`, run the following code:</span></span>

    sudo apt-get install corosync pacemaker drbd8-utils.

<span data-ttu-id="5d27d-160">Ne telepítse a MySQL most.</span><span class="sxs-lookup"><span data-stu-id="5d27d-160">Do not install MySQL at this time.</span></span> <span data-ttu-id="5d27d-161">Debian és Ubuntu telepítési parancsfájlokat inicializálja a MySQL adatkönyvtára a `/var/lib/mysql`, de a könyvtár a rendszer egy DRBD fájlrendszer váltotta fel, mert telepítenie kell a MySQL később.</span><span class="sxs-lookup"><span data-stu-id="5d27d-161">Debian and Ubuntu installation scripts will initialize a MySQL data directory on `/var/lib/mysql`, but because the directory will be superseded by a DRBD file system, you need to install MySQL later.</span></span>

<span data-ttu-id="5d27d-162">Győződjön meg arról (használatával `/sbin/ifconfig`), hogy mindkét virtuális gépeket használ a 10.10.10.0/24 alhálózati cím és, hogy azok a ping paranccsal elérhető másik név szerint.</span><span class="sxs-lookup"><span data-stu-id="5d27d-162">Verify (by using `/sbin/ifconfig`) that both VMs are using addresses in the 10.10.10.0/24 subnet and that they can ping each other by name.</span></span> <span data-ttu-id="5d27d-163">Is `ssh-keygen` és `ssh-copy-id` való győződjön meg arról, hogy mindkét virtuális gépek kommunikálhatnak SSH-kapcsolaton keresztül anélkül, hogy a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5d27d-163">You can also use `ssh-keygen` and `ssh-copy-id` to make sure both VMs can communicate via SSH without requiring a password.</span></span>

### <a name="set-up-drbd"></a><span data-ttu-id="5d27d-164">DRBD beállítása</span><span class="sxs-lookup"><span data-stu-id="5d27d-164">Set up DRBD</span></span>
<span data-ttu-id="5d27d-165">Hozzon létre egy DRBD erőforrást, amely használja az alapul szolgáló `/dev/sdc1` előállításához partíciót egy `/dev/drbd1` ext3 fájlrendszerrel formázott és az elsődleges és másodlagos csomópontok használt erőforrást.</span><span class="sxs-lookup"><span data-stu-id="5d27d-165">Create a DRBD resource that uses the underlying `/dev/sdc1` partition to produce a `/dev/drbd1` resource that can be formatted by using ext3 and used in both primary and secondary nodes.</span></span>

1. <span data-ttu-id="5d27d-166">Nyissa meg `/etc/drbd.d/r0.res` , és másolja a következő erőforrás-definíció mindkét virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="5d27d-166">Open `/etc/drbd.d/r0.res` and copy the following resource definition on both VMs:</span></span>

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. <span data-ttu-id="5d27d-167">Az erőforrás inicializálása használatával `drbdadm` mindkét virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="5d27d-167">Initialize the resource by using `drbdadm` on both VMs:</span></span>

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. <span data-ttu-id="5d27d-168">Az elsődleges virtuális gépen (`hadb01`), a DRBD erőforrás (elsődleges) tulajdonjogát kényszerítése:</span><span class="sxs-lookup"><span data-stu-id="5d27d-168">On the primary VM (`hadb01`), force ownership (primary) of the DRBD resource:</span></span>

        sudo drbdadm primary --force r0

<span data-ttu-id="5d27d-169">Ha megvizsgálja a/proc/drbd tartalmát (`sudo cat /proc/drbd`) mindkét virtuális gépeken, megtekintheti az `Primary/Secondary` a `hadb01` és `Secondary/Primary` a `hadb02`, ezen a ponton a megoldás megfelel.</span><span class="sxs-lookup"><span data-stu-id="5d27d-169">If you examine the contents of /proc/drbd (`sudo cat /proc/drbd`) on both VMs, you should see `Primary/Secondary` on `hadb01` and `Secondary/Primary` on `hadb02`, consistent with the solution at this point.</span></span> <span data-ttu-id="5d27d-170">Az 5-GB lemezterület az ügyfél számára díjmentesen a 10.10.10.0/24 hálózaton keresztül szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="5d27d-170">The 5-GB disk is synchronized over the 10.10.10.0/24 network at no charge to customers.</span></span>

<span data-ttu-id="5d27d-171">Miután a szinkronizálás a lemez hozhatja létre a fájlrendszer `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-171">After the disk is synchronized, you can create the file system on `hadb01`.</span></span> <span data-ttu-id="5d27d-172">Tesztelési célokra ext2 használtuk, de az alábbi kód létrehoz egy ext3 fájlrendszert:</span><span class="sxs-lookup"><span data-stu-id="5d27d-172">For testing purposes, we used ext2, but the following code will create an ext3 file system:</span></span>

    mkfs.ext3 /dev/drbd1

### <a name="mount-the-drbd-resource"></a><span data-ttu-id="5d27d-173">Csatlakoztassa a DRBD erőforrás</span><span class="sxs-lookup"><span data-stu-id="5d27d-173">Mount the DRBD resource</span></span>
<span data-ttu-id="5d27d-174">Most már készen áll a DRBD erőforrások csatolható fel a `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-174">You're now ready to mount the DRBD resources on `hadb01`.</span></span> <span data-ttu-id="5d27d-175">Debian és származékaik használata `/var/lib/mysql` MySQL tartozó adatok könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5d27d-175">Debian and derivatives use `/var/lib/mysql` as MySQL's data directory.</span></span> <span data-ttu-id="5d27d-176">Ha még nem telepítette a MySQL, mert a következő könyvtár létrehozásakor, és a csatlakoztatási a DRBD erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5d27d-176">Because you haven't installed MySQL, create the directory and mount the DRBD resource.</span></span> <span data-ttu-id="5d27d-177">Ezt a beállítást, futtassa a következő kódot `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="5d27d-177">To perform this option, run the following code on `hadb01`:</span></span>

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a><span data-ttu-id="5d27d-178">MySQL beállítása</span><span class="sxs-lookup"><span data-stu-id="5d27d-178">Set up MySQL</span></span>
<span data-ttu-id="5d27d-179">Most már készen áll a MySQL telepíthető `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="5d27d-179">Now you're ready to install MySQL on `hadb01`:</span></span>

    sudo apt-get install mysql-server

<span data-ttu-id="5d27d-180">A `hadb02`, két lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="5d27d-180">For `hadb02`, you have two options.</span></span> <span data-ttu-id="5d27d-181">Mysql-kiszolgáló, amely /var/lib/mysql létrehozása, töltse ki az új adatok könyvtárat, és távolítsa el a tartalom telepítése.</span><span class="sxs-lookup"><span data-stu-id="5d27d-181">You can install mysql-server, which will create /var/lib/mysql, fill it with a new data directory, and then remove the contents.</span></span> <span data-ttu-id="5d27d-182">Ezt a beállítást, futtassa a következő kódot `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="5d27d-182">To perform this option, run the following code on `hadb02`:</span></span>

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

<span data-ttu-id="5d27d-183">A második lehetőség van, így `hadb02` és telepítse a mysql-kiszolgáló létezik.</span><span class="sxs-lookup"><span data-stu-id="5d27d-183">The second option is to failover to `hadb02` and then install mysql-server there.</span></span> <span data-ttu-id="5d27d-184">Telepítési parancsfájlokat megfigyelheti a meglévő verziót, és nem touch.</span><span class="sxs-lookup"><span data-stu-id="5d27d-184">Installation scripts will notice the existing installation and won't touch it.</span></span>

<span data-ttu-id="5d27d-185">Futtassa a következő kódot `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="5d27d-185">Run the following code on `hadb01`:</span></span>

    sudo drbdadm secondary –force r0

<span data-ttu-id="5d27d-186">Futtassa a következő kódot `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="5d27d-186">Run the following code on `hadb02`:</span></span>

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

<span data-ttu-id="5d27d-187">Ha nem tervezi feladatátvételi DRBD most, az első lehetőség bár késései kevésbé elegáns egyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="5d27d-187">If you don't plan to failover DRBD now, the first option is easier although arguably less elegant.</span></span> <span data-ttu-id="5d27d-188">Beállítása után ez, megkezdheti a MySQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="5d27d-188">After you set this up, you can start working on your MySQL database.</span></span> <span data-ttu-id="5d27d-189">Futtassa a következő kódot `hadb02` (vagy bármelyik közül a kiszolgálók aktív, DRBD szerint):</span><span class="sxs-lookup"><span data-stu-id="5d27d-189">Run the following code on `hadb02` (or whichever one of the servers is active, according to DRBD):</span></span>

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

> [!WARNING]
> <span data-ttu-id="5d27d-190">Az utolsó utasítás gyakorlatilag letiltja a hitelesítési ebben a táblázatban a gyökér szintű felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="5d27d-190">This last statement effectively disables authentication for the root user in this table.</span></span> <span data-ttu-id="5d27d-191">Ez a termelési szintű kell helyettesíteni utasításokat ADNIA és meg adva csak szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="5d27d-191">This should be replaced by your production-grade GRANT statements and is included only for illustrative purposes.</span></span>

<span data-ttu-id="5d27d-192">Ha engedélyezni szeretné a virtuális gépek (Ez az útmutató célja) kívül a lekérdezések, is lehetővé teszik a MySQL hálózatkezelés kell.</span><span class="sxs-lookup"><span data-stu-id="5d27d-192">If you want to make queries from outside the VMs (which is the purpose of this guide), you also need to enable networking for MySQL.</span></span> <span data-ttu-id="5d27d-193">Mindkét virtuális gépeken, nyissa meg a `/etc/mysql/my.cnf` , és navigáljon `bind-address`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-193">On both VMs, open `/etc/mysql/my.cnf` and go to `bind-address`.</span></span> <span data-ttu-id="5d27d-194">Módosítsa a címet a 127.0.0.1 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="5d27d-194">Change the address from 127.0.0.1 to 0.0.0.0.</span></span> <span data-ttu-id="5d27d-195">A fájl mentése után adja ki a `sudo service mysql restart` a jelenlegi elsődleges.</span><span class="sxs-lookup"><span data-stu-id="5d27d-195">After saving the file, issue a `sudo service mysql restart` on your current primary.</span></span>

### <a name="create-the-mysql-load-balanced-set"></a><span data-ttu-id="5d27d-196">A MySQL-az elosztott terhelésű készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d27d-196">Create the MySQL load-balanced set</span></span>
<span data-ttu-id="5d27d-197">Lépjen vissza a portálra, írja be a `hadb01`, és válassza a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="5d27d-197">Go back to the portal, go to `hadb01`, and choose **Endpoints**.</span></span> <span data-ttu-id="5d27d-198">A végpont létrehozásához válassza MySQL (TCP 3306) a legördülő listából, majd **hozzon létre új elosztott terhelésű készlet**.</span><span class="sxs-lookup"><span data-stu-id="5d27d-198">To create an endpoint, choose MySQL (TCP 3306) from the drop-down list and select **Create new load balanced set**.</span></span> <span data-ttu-id="5d27d-199">Az elosztott terhelésű végpont neve `lb-mysql`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-199">Name the load-balanced endpoint `lb-mysql`.</span></span> <span data-ttu-id="5d27d-200">Állítsa be **idő** minimális 5 másodpercre.</span><span class="sxs-lookup"><span data-stu-id="5d27d-200">Set **Time** to 5 seconds, minimum.</span></span>

<span data-ttu-id="5d27d-201">Miután létrehozta a végpontot, Ugrás `hadb02`, válassza a **végpontok**, és hozzon létre egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="5d27d-201">After you create the endpoint, go to `hadb02`, choose **Endpoints**, and create an endpoint.</span></span> <span data-ttu-id="5d27d-202">Válasszon `lb-mysql`, majd a legördülő listából válassza ki a MySQL.</span><span class="sxs-lookup"><span data-stu-id="5d27d-202">Choose `lb-mysql`, and then select MySQL from the drop-down list.</span></span> <span data-ttu-id="5d27d-203">Ebben a lépésben az Azure parancssori felület is használható.</span><span class="sxs-lookup"><span data-stu-id="5d27d-203">You can also use the Azure CLI for this step.</span></span>

<span data-ttu-id="5d27d-204">Most már rendelkezik minden a fürt kézi művelet szükséges.</span><span class="sxs-lookup"><span data-stu-id="5d27d-204">You now have everything you need for manual operation of the cluster.</span></span>

### <a name="test-the-load-balanced-set"></a><span data-ttu-id="5d27d-205">Az elosztott terhelésű készlet tesztelése</span><span class="sxs-lookup"><span data-stu-id="5d27d-205">Test the load-balanced set</span></span>
<span data-ttu-id="5d27d-206">Tesztek külső gépből hajtható végre a MySQL-ügyfélprogrammal, vagy bizonyos alkalmazások, például egy Azure-webhelyen fut phpMyAdmin használatával.</span><span class="sxs-lookup"><span data-stu-id="5d27d-206">Tests can be performed from an outside machine by using any MySQL client, or by using certain applications, like phpMyAdmin running as an Azure website.</span></span> <span data-ttu-id="5d27d-207">Ebben az esetben egy másik Linux mezőben használt MySQL a parancssori eszköz:</span><span class="sxs-lookup"><span data-stu-id="5d27d-207">In this case, you used MySQL's command-line tool on another Linux box:</span></span>

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a><span data-ttu-id="5d27d-208">Manuális feladatátvétele</span><span class="sxs-lookup"><span data-stu-id="5d27d-208">Manually failing over</span></span>
<span data-ttu-id="5d27d-209">Feladatátvétel szimulálhatja MySQL leállítása, DRBD tartozó elsődleges váltás, és újból elindítani a MySQL.</span><span class="sxs-lookup"><span data-stu-id="5d27d-209">You can simulate failovers by shutting down MySQL, switching DRBD's primary, and starting MySQL again.</span></span>

<span data-ttu-id="5d27d-210">Ez a feladat végrehajtásához futtassa a következő kódot hadb01:</span><span class="sxs-lookup"><span data-stu-id="5d27d-210">To perform this task, run the following code on hadb01:</span></span>

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

<span data-ttu-id="5d27d-211">Majd a hadb02:</span><span class="sxs-lookup"><span data-stu-id="5d27d-211">Then, on hadb02:</span></span>

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

<span data-ttu-id="5d27d-212">Miután a rendszer átadja manuálisan, megismételheti a távoli lekérdezés, és tökéletesen működnek.</span><span class="sxs-lookup"><span data-stu-id="5d27d-212">After you fail over manually, you can repeat your remote query and it should work perfectly.</span></span>

## <a name="set-up-corosync"></a><span data-ttu-id="5d27d-213">Corosync beállítása</span><span class="sxs-lookup"><span data-stu-id="5d27d-213">Set up Corosync</span></span>
<span data-ttu-id="5d27d-214">Corosync támasztja működéséhez szükséges alapul szolgáló fürt infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="5d27d-214">Corosync is the underlying cluster infrastructure required for Pacemaker to work.</span></span> <span data-ttu-id="5d27d-215">A szívverés (és egyéb módszerek, például Ultramonkey) Corosync CRM funkciók valószínűségét, míg a funkció több hasonló szívverés támasztja marad.</span><span class="sxs-lookup"><span data-stu-id="5d27d-215">For Heartbeat (and other methodologies like Ultramonkey), Corosync is a split of the CRM functionalities, while Pacemaker remains more similar to Heartbeat in functionality.</span></span>

<span data-ttu-id="5d27d-216">A fő Corosync az Azure pedig az, hogy egyedi küldésű kommunikációs keresztül Corosync inkább az szórás keresztül csoportos küldés, de a Microsoft Azure-hálózat csak az egyedi küldéses támogatja.</span><span class="sxs-lookup"><span data-stu-id="5d27d-216">The main constraint for Corosync on Azure is that Corosync prefers multicast over broadcast over unicast communications, but Microsoft Azure networking only supports unicast.</span></span>

<span data-ttu-id="5d27d-217">Szerencsére Corosync van egy működő egyedi küldéses módot.</span><span class="sxs-lookup"><span data-stu-id="5d27d-217">Fortunately, Corosync has a working unicast mode.</span></span> <span data-ttu-id="5d27d-218">A csak valós pedig az, hogy minden csomópont nem kommunikál egymással, mert meg kell határoznia a csomópontok a konfigurációs fájlokban, beleértve az IP-címét.</span><span class="sxs-lookup"><span data-stu-id="5d27d-218">The only real constraint is that because all nodes are not communicating among themselves, you need to define the nodes in your configuration files, including their IP addresses.</span></span> <span data-ttu-id="5d27d-219">A Corosync példa fájlokkal azt is megteheti, az egyedi küldés és a módosítás kötési cím, a csomópont listák és a naplózási könyvtár (Ubuntu használ `/var/log/corosync` közben a példa fájlok használata `/var/log/cluster`), és kvórum eszközök engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5d27d-219">We can use the Corosync example files for Unicast and change bind address, node lists, and logging directories (Ubuntu uses `/var/log/corosync` while the example files use `/var/log/cluster`), and enable quorum tools.</span></span>

> [!NOTE]
> <span data-ttu-id="5d27d-220">Használja a következő `transport: udpu` irányelv és a manuálisan megadott IP-címek mindkét csomópont.</span><span class="sxs-lookup"><span data-stu-id="5d27d-220">Use the following `transport: udpu` directive and the manually defined IP addresses for both nodes.</span></span>

<span data-ttu-id="5d27d-221">Futtassa a következő kódot `/etc/corosync/corosync.conf` mindkét csomópontok:</span><span class="sxs-lookup"><span data-stu-id="5d27d-221">Run the following code on `/etc/corosync/corosync.conf` for both nodes:</span></span>

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

<span data-ttu-id="5d27d-222">Másolja a konfigurációs fájl mindkét virtuális gépeken, és mindkét csomópont Corosync elindításához:</span><span class="sxs-lookup"><span data-stu-id="5d27d-222">Copy this configuration file on both VMs and start Corosync in both nodes:</span></span>

    sudo service start corosync

<span data-ttu-id="5d27d-223">Röviddel a szolgáltatás indítása, a fürt kell meghatározni az aktuális gyűrűben, és a kvórum kell kialakítani.</span><span class="sxs-lookup"><span data-stu-id="5d27d-223">Shortly after starting the service, the cluster should be established in the current ring, and quorum should be constituted.</span></span> <span data-ttu-id="5d27d-224">Ez a funkció jelenleg ellenőrizheti a naplók megtekintésével, vagy futtassa a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="5d27d-224">We can check this functionality by reviewing logs or by running the following code:</span></span>

    sudo corosync-quorumtool –l

<span data-ttu-id="5d27d-225">Az alábbi képen hasonló kimenetet fog látni:</span><span class="sxs-lookup"><span data-stu-id="5d27d-225">You will see output similar to the following image:</span></span>

![corosync-quorumtool - l minta kimenet](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a><span data-ttu-id="5d27d-227">Támasztja beállítása</span><span class="sxs-lookup"><span data-stu-id="5d27d-227">Set up Pacemaker</span></span>
<span data-ttu-id="5d27d-228">Támasztja a fürt erőforrások figyelése, határozza meg, ha az elsődleges leáll, és ezeket az erőforrásokat váltani a másodlagos adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="5d27d-228">Pacemaker uses the cluster to monitor for resources, define when primaries go down, and switch those resources to secondaries.</span></span> <span data-ttu-id="5d27d-229">Erőforrások elérhető parancsfájlokkal, illetve LSB (init-bA), parancsfájlok egyéb lehetőségek között definiálhatók.</span><span class="sxs-lookup"><span data-stu-id="5d27d-229">Resources can be defined from a set of available scripts or from LSB (init-like) scripts, among other choices.</span></span>

<span data-ttu-id="5d27d-230">Azt szeretnénk, ha támasztja "tulajdonosa" a DRBD erőforrás, a csatlakoztatási pont és a MySQL-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5d27d-230">We want Pacemaker to "own" the DRBD resource, the mount point, and the MySQL service.</span></span> <span data-ttu-id="5d27d-231">Ha támasztja és DRBD bekapcsolása, csatlakoztatni és leválasztani, és majd elindíthatók és leállíthatók MySQL a megfelelő sorrendben ha valami rossz fordul elő, ha az elsődleges, a telepítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="5d27d-231">If Pacemaker can turn on and off DRBD, mount and unmount it, and then start and stop MySQL in the right order when something bad happens with the primary, setup is complete.</span></span>

<span data-ttu-id="5d27d-232">Támasztja első telepítésekor a konfiguráció legyen egyszerű, elég, például:</span><span class="sxs-lookup"><span data-stu-id="5d27d-232">When you first install Pacemaker, your configuration should be simple enough, something like:</span></span>

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. <span data-ttu-id="5d27d-233">Ellenőrizze a konfigurációs futtatásával `sudo crm configure show`.</span><span class="sxs-lookup"><span data-stu-id="5d27d-233">Check the configuration by running `sudo crm configure show`.</span></span>
2. <span data-ttu-id="5d27d-234">Ezután hozzon létre egy fájlt (például `/tmp/cluster.conf`) az a következőket:</span><span class="sxs-lookup"><span data-stu-id="5d27d-234">Then create a file (like `/tmp/cluster.conf`) with the following resources:</span></span>

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. <span data-ttu-id="5d27d-235">Töltse be a fájlt bővíti a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5d27d-235">Load the file into the configuration.</span></span> <span data-ttu-id="5d27d-236">Csak kell ehhez a egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="5d27d-236">You only need to do this in one node.</span></span>

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. <span data-ttu-id="5d27d-237">Győződjön meg arról, hogy támasztja kezdődik-e a mindkét csomópont indítása:</span><span class="sxs-lookup"><span data-stu-id="5d27d-237">Make sure that Pacemaker starts at boot in both nodes:</span></span>

        sudo update-rc.d pacemaker defaults

5. <span data-ttu-id="5d27d-238">A `sudo crm_mon –L`, győződjön meg arról, hogy a csomópontok közül a master, a fürt és az erőforrások fut-e.</span><span class="sxs-lookup"><span data-stu-id="5d27d-238">By using `sudo crm_mon –L`, verify that one of your nodes has become the master for the cluster and is running all the resources.</span></span> <span data-ttu-id="5d27d-239">Csatlakozási és ps segítségével ellenőrizze, hogy fut-e az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5d27d-239">You can use mount and ps to check that the resources are running.</span></span>

<span data-ttu-id="5d27d-240">Az alábbi képernyőfelvételen látható `crm_mon` egy csomópont leállt (Ctrl + C kiválasztásával kilépési):</span><span class="sxs-lookup"><span data-stu-id="5d27d-240">The following screenshot shows `crm_mon` with one node stopped (exit by selecting Ctrl+C):</span></span>

![crm_mon csomópont leállítása](./media/mysql-cluster/image002.png)

<span data-ttu-id="5d27d-242">Ezen a képernyőfelvételen látható csomópontok, a egy fő és egy alárendelt jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="5d27d-242">This screenshot shows both nodes, one master and one slave:</span></span>

![crm_mon műveleti főkiszolgáló/alárendelt](./media/mysql-cluster/image003.png)

## <a name="testing"></a><span data-ttu-id="5d27d-244">Tesztelés</span><span class="sxs-lookup"><span data-stu-id="5d27d-244">Testing</span></span>
<span data-ttu-id="5d27d-245">Készen áll az Automatikus feladatátvétel szimuláció.</span><span class="sxs-lookup"><span data-stu-id="5d27d-245">You're ready for an automatic failover simulation.</span></span> <span data-ttu-id="5d27d-246">Ehhez két módja van: ideiglenes és rögzített.</span><span class="sxs-lookup"><span data-stu-id="5d27d-246">There are two ways to do this: soft and hard.</span></span>

<span data-ttu-id="5d27d-247">A helyreállítható módja a fürt leállítás függvényt használja: ``crm_standby -U `uname -n` -v on``.</span><span class="sxs-lookup"><span data-stu-id="5d27d-247">The soft way uses the cluster's shutdown function: ``crm_standby -U `uname -n` -v on``.</span></span> <span data-ttu-id="5d27d-248">Ha ez a fő használja, az alárendelt élvez.</span><span class="sxs-lookup"><span data-stu-id="5d27d-248">If you use this on the master, the slave takes over.</span></span> <span data-ttu-id="5d27d-249">Ne felejtse el, állítsa vissza ki.</span><span class="sxs-lookup"><span data-stu-id="5d27d-249">Remember to set this back to off.</span></span> <span data-ttu-id="5d27d-250">Ha ezt elmulasztja, akkor crm_mon készenléti egy csomópont jelenjen meg.</span><span class="sxs-lookup"><span data-stu-id="5d27d-250">If you don't, crm_mon will show one node on standby.</span></span>

<span data-ttu-id="5d27d-251">A rögzített módja az elsődleges virtuális gép (hadb01) leáll, a portálon, vagy ha megváltoztatja a paraméterben megadott futtatási szint a virtuális Gépre (vagyis halt, leállítási).</span><span class="sxs-lookup"><span data-stu-id="5d27d-251">The hard way is shutting down the primary VM (hadb01) via the portal or by changing the runlevel on the VM (that is, halt, shutdown).</span></span> <span data-ttu-id="5d27d-252">Ez segít Corosync és támasztja jelzés által, hogy a fő tartozó is le.</span><span class="sxs-lookup"><span data-stu-id="5d27d-252">This helps Corosync and Pacemaker by signaling that the master's going down.</span></span> <span data-ttu-id="5d27d-253">Ez tesztelheti (hasznos a karbantartási időszakok), de is kényszeríthető a forgatókönyv által a virtuális gép zárolása.</span><span class="sxs-lookup"><span data-stu-id="5d27d-253">You can test this (useful for maintenance windows), but you can also force the scenario by freezing the VM.</span></span>

## <a name="stonith"></a><span data-ttu-id="5d27d-254">STONITH</span><span class="sxs-lookup"><span data-stu-id="5d27d-254">STONITH</span></span>
<span data-ttu-id="5d27d-255">Az Azure parancssori felület helyett egy fizikai eszközt vezérlő STONITH parancsfájl segítségével a virtuális gép leállítása ki kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5d27d-255">It should be possible to issue a VM shutdown via the Azure CLI in lieu of a STONITH script that controls a physical device.</span></span> <span data-ttu-id="5d27d-256">Használhat `/usr/lib/stonith/plugins/external/ssh` kiinduló és a fürt konfigurációjába STONITH engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5d27d-256">You can use `/usr/lib/stonith/plugins/external/ssh` as a base and enable STONITH in the cluster's configuration.</span></span> <span data-ttu-id="5d27d-257">Az Azure CLI globálisan kell telepíteni, és a közzétételi beállítások és a profil a fürt felhasználói kell betölteni.</span><span class="sxs-lookup"><span data-stu-id="5d27d-257">Azure CLI should be globally installed, and the publish settings and profile should be loaded for the cluster's user.</span></span>

<span data-ttu-id="5d27d-258">Példakód az erőforrás érhető el a [GitHub](https://github.com/bureado/aztonith).</span><span class="sxs-lookup"><span data-stu-id="5d27d-258">Sample code for the resource is available on [GitHub](https://github.com/bureado/aztonith).</span></span> <span data-ttu-id="5d27d-259">A fürt konfigurációjának módosítása a következő hozzáadásával `sudo crm configure`:</span><span class="sxs-lookup"><span data-stu-id="5d27d-259">Change the cluster's configuration by adding the following to `sudo crm configure`:</span></span>

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> <span data-ttu-id="5d27d-260">A parancsfájl nem hajtható végre fel/le ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="5d27d-260">The script doesn't perform up/down checks.</span></span> <span data-ttu-id="5d27d-261">Az eredeti SSH erőforrás volt a 15 ping-ellenőrzéseket, de egy Azure virtuális gép helyreállítási idő további változó lehet.</span><span class="sxs-lookup"><span data-stu-id="5d27d-261">The original SSH resource had 15 ping checks, but recovery time for an Azure VM might be more variable.</span></span>

## <a name="limitations"></a><span data-ttu-id="5d27d-262">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="5d27d-262">Limitations</span></span>
<span data-ttu-id="5d27d-263">A következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="5d27d-263">The following limitations apply:</span></span>

* <span data-ttu-id="5d27d-264">A linbit DRBD erőforrás parancsfájl, amely kezeli a DRBD erőforrásként támasztja használja a `drbdadm down` Ha egy csomópont leáll, akkor is, ha a csomópont csak készenléti van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5d27d-264">The linbit DRBD resource script that manages DRBD as a resource in Pacemaker uses `drbdadm down` when shutting down a node, even if the node is just going on standby.</span></span> <span data-ttu-id="5d27d-265">Ennek oka nem ideális megoldás az alárendelt fog nem lehet szinkronizálása a DRBD erőforrás fő lekérdezi az írási műveletek során.</span><span class="sxs-lookup"><span data-stu-id="5d27d-265">This is not ideal because the slave will not be synchronizing the DRBD resource while the master gets writes.</span></span> <span data-ttu-id="5d27d-266">Ha a fő nem graciously sikertelen, az alárendelt átveheti egy régebbi fájlrendszer állapota.</span><span class="sxs-lookup"><span data-stu-id="5d27d-266">If the master does not fail graciously, the slave can take over an older file system state.</span></span> <span data-ttu-id="5d27d-267">Ez megoldására két lehetséges módja van:</span><span class="sxs-lookup"><span data-stu-id="5d27d-267">There are two potential ways of solving this:</span></span>
  * <span data-ttu-id="5d27d-268">Érvényesítési egy `drbdadm up r0` a fürt összes csomópontján keresztül egy helyi (nem clusterized)-figyelő</span><span class="sxs-lookup"><span data-stu-id="5d27d-268">Enforcing a `drbdadm up r0` in all cluster nodes via a local (not clusterized) watchdog</span></span>
  * <span data-ttu-id="5d27d-269">A linbit DRBD parancsfájl, ügyelve arra, hogy szerkesztési `down` nem hívják meg`/usr/lib/ocf/resource.d/linbit/drbd`</span><span class="sxs-lookup"><span data-stu-id="5d27d-269">Editing the linbit DRBD script, making sure that `down` is not called in `/usr/lib/ocf/resource.d/linbit/drbd`</span></span>
* <span data-ttu-id="5d27d-270">A load balancer legalább öt másodpercenként kell válaszolnia, így az alkalmazások fürttámogató legyen, és jobban tűrik a timeout kell.</span><span class="sxs-lookup"><span data-stu-id="5d27d-270">The load balancer needs at least five seconds to respond, so applications should be cluster-aware and be more tolerant of timeout.</span></span> <span data-ttu-id="5d27d-271">Egyéb-architektúrák esetén az-app várólisták és a lekérdezés middlewares, például segíthet is.</span><span class="sxs-lookup"><span data-stu-id="5d27d-271">Other architectures, like in-app queues and query middlewares, can also help.</span></span>
* <span data-ttu-id="5d27d-272">MySQL hangolása győződjön meg arról, hogy írás kezelhető ütemben történik, és gyorsítótárak tartalmát lemezre memória veszteség minimalizálása érdekében a lehető gyakran szükség.</span><span class="sxs-lookup"><span data-stu-id="5d27d-272">MySQL tuning is necessary to ensure that writing is done at a manageable pace and caches are flushed to disk as frequently as possible to minimize memory loss.</span></span>
* <span data-ttu-id="5d27d-273">Az írási teljesítmény függő a virtuális gép kapcsolódnak össze a virtuális kapcsolón, mert ez a mechanizmus DRBD használja replikálja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="5d27d-273">Write performance is dependent in VM interconnect in the virtual switch because this is the mechanism used by DRBD to replicate the device.</span></span>
