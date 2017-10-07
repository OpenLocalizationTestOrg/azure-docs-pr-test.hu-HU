---
title: "az elosztott terhelésű készletek MySQL aaaClusterize |} Microsoft Docs"
description: "Állítson be egy elosztott terhelésű, a magas rendelkezésre állású hello klasszikus üzembe helyezési modellt az Azure-on Linux MySQL-fürt létrehozása"
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
ms.openlocfilehash: 1829fd877c4b0ed177b23a8e3404dbb3db746561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-load-balanced-sets-tooclusterize-mysql-on-linux"></a><span data-ttu-id="0f8a1-103">Elosztott terhelésű készletek tooclusterize MySQL használata Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="0f8a1-103">Use load-balanced sets tooclusterize MySQL on Linux</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0f8a1-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="0f8a1-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="0f8a1-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="0f8a1-107">A [Resource Manager-sablon](https://azure.microsoft.com/documentation/templates/mysql-replication/) érhető el, ha a MySQL-fürt toodeploy van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-107">A [Resource Manager template](https://azure.microsoft.com/documentation/templates/mysql-replication/) is available if you need toodeploy a MySQL cluster.</span></span>

<span data-ttu-id="0f8a1-108">Ez a cikk ismerteti, és hello különböző szempontok elérhető toodeploy magas rendelkezésre állású Linux-alapú szolgáltatásokhoz a Microsoft Azure-böngészést a MySQL-kiszolgáló magas rendelkezésre állás mint egy ismertetése mutatja be.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-108">This article explores and illustrates hello different approaches available toodeploy highly available Linux-based services on Microsoft Azure, exploring MySQL Server high availability as a primer.</span></span> <span data-ttu-id="0f8a1-109">Ez a megközelítés bemutató videó megtalálható [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span><span class="sxs-lookup"><span data-stu-id="0f8a1-109">A video illustrating this approach is available on [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span></span>

<span data-ttu-id="0f8a1-110">Azt fogja szerkezeti osztott, két csomópontos, egyszeri MySQL magas rendelkezésre állású megoldás DRBD, Corosync és támasztja alapján.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-110">We will outline a shared-nothing, two-node, single-master MySQL high availability solution based on DRBD, Corosync, and Pacemaker.</span></span> <span data-ttu-id="0f8a1-111">Csak egy csomópont MySQL fut egyszerre.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-111">Only one node runs MySQL at a time.</span></span> <span data-ttu-id="0f8a1-112">A hello DRBD erőforrás nem is egy korlátozott tooonly-csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-112">Reading and writing from hello DRBD resource is also limited tooonly one node at a time.</span></span>

<span data-ttu-id="0f8a1-113">Nincs szükség az VIP-megoldások HÉ, például mert fogja használni a Microsoft Azure tooprovide ciklikus multiplexelés funkcionalitásra vagy a végpont észlelési, eltávolítás és hello VIP szabályos helyreállítása elosztott terhelésű készletek.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-113">There's no need for a VIP solution like LVS, because you'll use load-balanced sets in Microsoft Azure tooprovide round-robin functionality and endpoint detection, removal, and graceful recovery of hello VIP.</span></span> <span data-ttu-id="0f8a1-114">hello VIP hello felhőalapú szolgáltatás létrehozásakor, a Microsoft Azure által kiosztott globálisan irányíthatóvá teszi IPv4-cím.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-114">hello VIP is a globally routable IPv4 address assigned by Microsoft Azure when you first create hello cloud service.</span></span>

<span data-ttu-id="0f8a1-115">Nincsenek elérhető, mint egy virtuális gép MySQL, beleértve a NBD fürt, Percona, Galera és több köztes megoldások, beleértve a legalább egy másik lehetséges architektúrához [VM raktár számára](http://vmdepot.msopentech.com).</span><span class="sxs-lookup"><span data-stu-id="0f8a1-115">There are other possible architectures for MySQL, including NBD Cluster, Percona, Galera, and several middleware solutions, including at least one available as a VM on [VM Depot](http://vmdepot.msopentech.com).</span></span> <span data-ttu-id="0f8a1-116">Mindaddig, amíg ezek a megoldások képes replikálni, amely a csoportos küldést vagy szórásos és egyedi küldéses, és nem függ a megosztott tárhelytől vagy több hálózati adapterrel, hello forgatókönyvek a Microsoft Azure könnyen toodeploy kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-116">As long as these solutions can replicate on unicast vs. multicast or broadcast and don't rely on shared storage or multiple network interfaces, hello scenarios should be easy toodeploy on Microsoft Azure.</span></span>

<span data-ttu-id="0f8a1-117">Ezek a fürtszolgáltatás architektúrák kiterjeszthető tooother termékek például PostgreSQL és OpenLDAP hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-117">These clustering architectures can be extended tooother products like PostgreSQL and OpenLDAP in a similar fashion.</span></span> <span data-ttu-id="0f8a1-118">Például a megosztás terheléselosztás eljárás sikeresen tesztelték, több főkiszolgálós OpenLDAP, és figyelheti a Channel 9 blogjában.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-118">For example, this load-balancing procedure with shared nothing was successfully tested with multi-master OpenLDAP, and you can watch it on our Channel 9 blog.</span></span>

## <a name="get-ready"></a><span data-ttu-id="0f8a1-119">Felkészülés</span><span class="sxs-lookup"><span data-stu-id="0f8a1-119">Get ready</span></span>
<span data-ttu-id="0f8a1-120">Hello a következőkre lesz szüksége erőforrások és képességek:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-120">You need hello following resources and abilities:</span></span>

  - <span data-ttu-id="0f8a1-121">A Microsoft Azure fiók egy érvényes előfizetéssel, képes toocreate legalább két virtuális gépek (ebben a példában a XS lett megadva)</span><span class="sxs-lookup"><span data-stu-id="0f8a1-121">A Microsoft Azure account with a valid subscription, able toocreate at least two VMs (XS was used in this example)</span></span>
  - <span data-ttu-id="0f8a1-122">A hálózat és alhálózat</span><span class="sxs-lookup"><span data-stu-id="0f8a1-122">A network and a subnet</span></span>
  - <span data-ttu-id="0f8a1-123">Az affinitáscsoportokban</span><span class="sxs-lookup"><span data-stu-id="0f8a1-123">An affinity group</span></span>
  - <span data-ttu-id="0f8a1-124">Rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="0f8a1-124">An availability set</span></span>
  - <span data-ttu-id="0f8a1-125">hello képességét toocreate VHD-k a hello megegyező régióban hello felhőalapú szolgáltatás, és csatolja azokat toohello Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="0f8a1-125">hello ability toocreate VHDs in hello same region as hello cloud service and attach them toohello Linux VMs</span></span>

### <a name="tested-environment"></a><span data-ttu-id="0f8a1-126">Tesztelt környezet</span><span class="sxs-lookup"><span data-stu-id="0f8a1-126">Tested environment</span></span>
* <span data-ttu-id="0f8a1-127">Ubuntu 13.10</span><span class="sxs-lookup"><span data-stu-id="0f8a1-127">Ubuntu 13.10</span></span>
  * <span data-ttu-id="0f8a1-128">DRBD</span><span class="sxs-lookup"><span data-stu-id="0f8a1-128">DRBD</span></span>
  * <span data-ttu-id="0f8a1-129">MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="0f8a1-129">MySQL Server</span></span>
  * <span data-ttu-id="0f8a1-130">Corosync és támasztja</span><span class="sxs-lookup"><span data-stu-id="0f8a1-130">Corosync and Pacemaker</span></span>

### <a name="affinity-group"></a><span data-ttu-id="0f8a1-131">Affinitáscsoportok</span><span class="sxs-lookup"><span data-stu-id="0f8a1-131">Affinity group</span></span>
<span data-ttu-id="0f8a1-132">Ha bejelentkezik a klasszikus Azure-portál toohello hello megoldás affinitáscsoportok létrehozása kiválasztása **beállítások**, és egy affinitáscsoporthoz létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-132">Create an affinity group for hello solution by signing in toohello Azure classic portal, selecting **Settings**, and creating an affinity group.</span></span> <span data-ttu-id="0f8a1-133">A lefoglalt erőforrásokat újabb toothis affinitáscsoport lesz hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-133">Allocated resources created later will be assigned toothis affinity group.</span></span>

### <a name="networks"></a><span data-ttu-id="0f8a1-134">Hálózatok</span><span class="sxs-lookup"><span data-stu-id="0f8a1-134">Networks</span></span>
<span data-ttu-id="0f8a1-135">Egy új hálózatot hoznak létre, és egy alhálózat hello hálózaton belül jön létre.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-135">A new network is created, and a subnet is created inside hello network.</span></span> <span data-ttu-id="0f8a1-136">A példa 10.10.10.0/24 hálózaton belül csak egy /24 alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-136">This example uses a 10.10.10.0/24 network with only one /24 subnet inside.</span></span>

### <a name="virtual-machines"></a><span data-ttu-id="0f8a1-137">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="0f8a1-137">Virtual machines</span></span>
<span data-ttu-id="0f8a1-138">első Ubuntu 13.10 VM Endorsed Ubuntu gyűjtemény lemezkép használatával hozták létre, és nevezik hello `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-138">hello first Ubuntu 13.10 VM is created by using an Endorsed Ubuntu Gallery image and is called `hadb01`.</span></span> <span data-ttu-id="0f8a1-139">Új felhőalapú szolgáltatás hello folyamat, amely a hadb jön létre.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-139">A new cloud service is created in hello process, called hadb.</span></span> <span data-ttu-id="0f8a1-140">Ez a név hello megosztott, elosztott terhelésű ideiglenesek, amely hello szolgáltatást fog rendelkezni, hogy több erőforrást hozzáadják mutatja be.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-140">This name illustrates hello shared, load-balanced nature that hello service will have when more resources are added.</span></span> <span data-ttu-id="0f8a1-141">hello létrehozása `hadb01` Eseménytelen és befejezett hello portál használatával.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-141">hello creation of `hadb01` is uneventful and completed by using hello portal.</span></span> <span data-ttu-id="0f8a1-142">Az SSH a végpont automatikusan létrejön, és hello új hálózati van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-142">An endpoint for SSH is automatically created, and hello new network is selected.</span></span> <span data-ttu-id="0f8a1-143">Most már rendelkezésre állási készlet hello virtuális gépek is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-143">Now you can create an availability set for hello VMs.</span></span>

<span data-ttu-id="0f8a1-144">Létrehozása után hello első virtuális gép létrehozása (technikailag, létrehozásakor hello felhőalapú szolgáltatás), második VM hello `hadb02`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-144">After hello first VM is created (technically, when hello cloud service is created), create hello second VM, `hadb02`.</span></span> <span data-ttu-id="0f8a1-145">Hello második virtuális gép, Ubuntu 13.10 VM hello gyűjteménye a hello portál használatával használni, de egy meglévő felhőszolgáltatáshoz `hadb.cloudapp.net`, egy új létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-145">For hello second VM, use Ubuntu 13.10 VM from hello Gallery by using hello portal, but use an existing cloud service, `hadb.cloudapp.net`, instead of creating a new one.</span></span> <span data-ttu-id="0f8a1-146">hello hálózati és a rendelkezésre állási készlet ki automatikusan.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-146">hello network and availability set should be automatically selected.</span></span> <span data-ttu-id="0f8a1-147">Egy SSH-végpontot rendel, túl.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-147">An SSH endpoint will be created, too.</span></span>

<span data-ttu-id="0f8a1-148">Mindkét virtuális gép létrehozása után tekintse meg az SSH-port hello `hadb01` (22-es TCP) és `hadb02` (Ez automatikusan hozzárendelve az Azure-ban).</span><span class="sxs-lookup"><span data-stu-id="0f8a1-148">After both VMs have been created, take note of hello SSH port for `hadb01` (TCP 22) and `hadb02` (automatically assigned by Azure).</span></span>

### <a name="attached-storage"></a><span data-ttu-id="0f8a1-149">Csatlakoztatott tároló</span><span class="sxs-lookup"><span data-stu-id="0f8a1-149">Attached storage</span></span>
<span data-ttu-id="0f8a1-150">Egy új virtuális lemez tooboth csatolja, és 5 GB-os hello folyamat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-150">Attach a new disk tooboth VMs and create 5-GB disks in hello process.</span></span> <span data-ttu-id="0f8a1-151">hello lemezek tárolt VHD-tároló hello a fő operációsrendszer-lemezek használatban.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-151">hello disks are hosted in hello VHD container in use for your main operating system disks.</span></span> <span data-ttu-id="0f8a1-152">Miután lemezek létrehozni és csatolni, nincs nincs szükség toorestart Linux mert hello kernel hello új eszköz jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-152">After disks are created and attached, there is no need toorestart Linux because hello kernel will see hello new device.</span></span> <span data-ttu-id="0f8a1-153">Ez az eszköz megfelel általában `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-153">This device is usually `/dev/sdc`.</span></span> <span data-ttu-id="0f8a1-154">Ellenőrizze `dmesg` hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-154">Check `dmesg` for hello output.</span></span>

<span data-ttu-id="0f8a1-155">Minden egyes virtuális gépen, hozzon létre egy partíciót használatával `cfdisk` (elsődleges, Linux partíció) írási és olvasási hello új partíciós tábla.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-155">On each VM, create a partition by using `cfdisk` (primary, Linux partition) and write hello new partition table.</span></span> <span data-ttu-id="0f8a1-156">Ne hozzon létre egy fájlrendszer ezt a partíciót.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-156">Do not create a file system on this partition.</span></span>

## <a name="set-up-hello-cluster"></a><span data-ttu-id="0f8a1-157">Hello fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-157">Set up hello cluster</span></span>
<span data-ttu-id="0f8a1-158">Használja a APT tooinstall Corosync támasztja és DRBD mindkét Ubuntu virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-158">Use APT tooinstall Corosync, Pacemaker, and DRBD on both Ubuntu VMs.</span></span> <span data-ttu-id="0f8a1-159">toodo így `apt-get`- ben futtassa hello következő kódot:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-159">toodo so with `apt-get`, run hello following code:</span></span>

    sudo apt-get install corosync pacemaker drbd8-utils.

<span data-ttu-id="0f8a1-160">Ne telepítse a MySQL most.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-160">Do not install MySQL at this time.</span></span> <span data-ttu-id="0f8a1-161">Debian és Ubuntu telepítési parancsfájlokat inicializálja a MySQL adatkönyvtára a `/var/lib/mysql`, de hello directory a rendszer egy DRBD fájlrendszer váltotta fel, mert szükség van-e MySQL tooinstall később.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-161">Debian and Ubuntu installation scripts will initialize a MySQL data directory on `/var/lib/mysql`, but because hello directory will be superseded by a DRBD file system, you need tooinstall MySQL later.</span></span>

<span data-ttu-id="0f8a1-162">Győződjön meg arról (használatával `/sbin/ifconfig`), hogy mindkét virtuális gépeket használ hello 10.10.10.0/24 alhálózati cím és, hogy azok a ping paranccsal elérhető másik név szerint.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-162">Verify (by using `/sbin/ifconfig`) that both VMs are using addresses in hello 10.10.10.0/24 subnet and that they can ping each other by name.</span></span> <span data-ttu-id="0f8a1-163">Is `ssh-keygen` és `ssh-copy-id` toomake meg arról, hogy mindkét virtuális gépek kommunikálhatnak SSH-kapcsolaton keresztül anélkül, hogy a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-163">You can also use `ssh-keygen` and `ssh-copy-id` toomake sure both VMs can communicate via SSH without requiring a password.</span></span>

### <a name="set-up-drbd"></a><span data-ttu-id="0f8a1-164">DRBD beállítása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-164">Set up DRBD</span></span>
<span data-ttu-id="0f8a1-165">Hozzon létre egy DRBD erőforrást, amely használja az alapul szolgáló hello `/dev/sdc1` tooproduce partícióazonosító egy `/dev/drbd1` ext3 fájlrendszerrel formázott és az elsődleges és másodlagos csomópontok használt erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-165">Create a DRBD resource that uses hello underlying `/dev/sdc1` partition tooproduce a `/dev/drbd1` resource that can be formatted by using ext3 and used in both primary and secondary nodes.</span></span>

1. <span data-ttu-id="0f8a1-166">Nyissa meg `/etc/drbd.d/r0.res` és másolási hello erőforrás-definíció következő mindkét virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-166">Open `/etc/drbd.d/r0.res` and copy hello following resource definition on both VMs:</span></span>

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

2. <span data-ttu-id="0f8a1-167">Hello erőforrás inicializálása használatával `drbdadm` mindkét virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-167">Initialize hello resource by using `drbdadm` on both VMs:</span></span>

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. <span data-ttu-id="0f8a1-168">Az elsődleges virtuális gép hello (`hadb01`), hello DRBD erőforrás (elsődleges) tulajdonjogát kényszerítése:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-168">On hello primary VM (`hadb01`), force ownership (primary) of hello DRBD resource:</span></span>

        sudo drbdadm primary --force r0

<span data-ttu-id="0f8a1-169">Ha megvizsgálja/proc/drbd hello tartalmát (`sudo cat /proc/drbd`) mindkét virtuális gépeken, megtekintheti az `Primary/Secondary` a `hadb01` és `Secondary/Primary` a `hadb02`, ezen a ponton hello megoldás konzisztens.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-169">If you examine hello contents of /proc/drbd (`sudo cat /proc/drbd`) on both VMs, you should see `Primary/Secondary` on `hadb01` and `Secondary/Primary` on `hadb02`, consistent with hello solution at this point.</span></span> <span data-ttu-id="0f8a1-170">hello 5 GB-os lemezre, nem kell fizetni toocustomers hello 10.10.10.0/24 hálózaton keresztül szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-170">hello 5-GB disk is synchronized over hello 10.10.10.0/24 network at no charge toocustomers.</span></span>

<span data-ttu-id="0f8a1-171">Hello lemez van szinkronizálása után létrehozhat hello fájlrendszer `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-171">After hello disk is synchronized, you can create hello file system on `hadb01`.</span></span> <span data-ttu-id="0f8a1-172">Tesztelési célokra ext2 használtuk, de a következő kód hello létrehoz egy ext3 fájlrendszert:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-172">For testing purposes, we used ext2, but hello following code will create an ext3 file system:</span></span>

    mkfs.ext3 /dev/drbd1

### <a name="mount-hello-drbd-resource"></a><span data-ttu-id="0f8a1-173">Hello DRBD erőforrás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-173">Mount hello DRBD resource</span></span>
<span data-ttu-id="0f8a1-174">Most már készen áll a toomount hello DRBD erőforrások még `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-174">You're now ready toomount hello DRBD resources on `hadb01`.</span></span> <span data-ttu-id="0f8a1-175">Debian és származékaik használata `/var/lib/mysql` MySQL tartozó adatok könyvtár.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-175">Debian and derivatives use `/var/lib/mysql` as MySQL's data directory.</span></span> <span data-ttu-id="0f8a1-176">Ha még nem telepítette a MySQL, mert hello könyvtár létrehozása, és a csatlakoztatási hello DRBD erőforrás.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-176">Because you haven't installed MySQL, create hello directory and mount hello DRBD resource.</span></span> <span data-ttu-id="0f8a1-177">tooperform ezt a beállítást, futtassa a következő kódot hello `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-177">tooperform this option, run hello following code on `hadb01`:</span></span>

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a><span data-ttu-id="0f8a1-178">MySQL beállítása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-178">Set up MySQL</span></span>
<span data-ttu-id="0f8a1-179">Most már készen áll a tooinstall MySQL még a `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-179">Now you're ready tooinstall MySQL on `hadb01`:</span></span>

    sudo apt-get install mysql-server

<span data-ttu-id="0f8a1-180">A `hadb02`, két lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-180">For `hadb02`, you have two options.</span></span> <span data-ttu-id="0f8a1-181">Mysql-kiszolgáló, amely /var/lib/mysql létrehozása, töltse ki az új adatok könyvtárat, és távolítsa el a hello tartalma is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-181">You can install mysql-server, which will create /var/lib/mysql, fill it with a new data directory, and then remove hello contents.</span></span> <span data-ttu-id="0f8a1-182">tooperform ezt a beállítást, futtassa a következő kódot hello `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-182">tooperform this option, run hello following code on `hadb02`:</span></span>

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

<span data-ttu-id="0f8a1-183">hello második lehetőség toofailover túl`hadb02` és telepítse a mysql-kiszolgáló létezik.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-183">hello second option is toofailover too`hadb02` and then install mysql-server there.</span></span> <span data-ttu-id="0f8a1-184">Telepítési parancsfájlokat megfigyelheti hello példánya, és nem touch.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-184">Installation scripts will notice hello existing installation and won't touch it.</span></span>

<span data-ttu-id="0f8a1-185">Futtatási hello hibakód a következő `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-185">Run hello following code on `hadb01`:</span></span>

    sudo drbdadm secondary –force r0

<span data-ttu-id="0f8a1-186">Futtatási hello hibakód a következő `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-186">Run hello following code on `hadb02`:</span></span>

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

<span data-ttu-id="0f8a1-187">Ha toofailover DRBD most nem kíván, hello első lehetőség bár késései kevésbé elegáns egyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-187">If you don't plan toofailover DRBD now, hello first option is easier although arguably less elegant.</span></span> <span data-ttu-id="0f8a1-188">Beállítása után ez, megkezdheti a MySQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-188">After you set this up, you can start working on your MySQL database.</span></span> <span data-ttu-id="0f8a1-189">Futtatási hello hibakód a következő `hadb02` (vagy bármelyik hello kiszolgálók egyikét aktív, tooDRBD szerint):</span><span class="sxs-lookup"><span data-stu-id="0f8a1-189">Run hello following code on `hadb02` (or whichever one of hello servers is active, according tooDRBD):</span></span>

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* tooroot;

> [!WARNING]
> <span data-ttu-id="0f8a1-190">Az utolsó utasítás gyakorlatilag letiltja az ebben a táblázatban szereplő hello gyökér szintű felhasználó számára hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-190">This last statement effectively disables authentication for hello root user in this table.</span></span> <span data-ttu-id="0f8a1-191">Ez a termelési szintű kell helyettesíteni utasításokat ADNIA és meg adva csak szemléltetési célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-191">This should be replaced by your production-grade GRANT statements and is included only for illustrative purposes.</span></span>

<span data-ttu-id="0f8a1-192">Ha azt szeretné, hogy a külső hello virtuális gépek (Ez az útmutató célja hello) toomake lekérdezések, a MySQL hálózati tooenable is kell.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-192">If you want toomake queries from outside hello VMs (which is hello purpose of this guide), you also need tooenable networking for MySQL.</span></span> <span data-ttu-id="0f8a1-193">Mindkét virtuális gépeken, nyissa meg a `/etc/mysql/my.cnf` , és túl`bind-address`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-193">On both VMs, open `/etc/mysql/my.cnf` and go too`bind-address`.</span></span> <span data-ttu-id="0f8a1-194">Hello címének módosítása a 127.0.0.1 too0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-194">Change hello address from 127.0.0.1 too0.0.0.0.</span></span> <span data-ttu-id="0f8a1-195">Hello fájl mentése után adja ki a `sudo service mysql restart` a jelenlegi elsődleges.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-195">After saving hello file, issue a `sudo service mysql restart` on your current primary.</span></span>

### <a name="create-hello-mysql-load-balanced-set"></a><span data-ttu-id="0f8a1-196">Hello MySQL elosztott terhelésű készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-196">Create hello MySQL load-balanced set</span></span>
<span data-ttu-id="0f8a1-197">Lépjen vissza toohello portal, nyissa meg túl`hadb01`, és válassza a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-197">Go back toohello portal, go too`hadb01`, and choose **Endpoints**.</span></span> <span data-ttu-id="0f8a1-198">egy végpont, toocreate választhat MySQL (TCP 3306) hello legördülő listából, és válassza **hozzon létre új elosztott terhelésű készlet**.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-198">toocreate an endpoint, choose MySQL (TCP 3306) from hello drop-down list and select **Create new load balanced set**.</span></span> <span data-ttu-id="0f8a1-199">Név hello elosztott terhelésű végpont `lb-mysql`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-199">Name hello load-balanced endpoint `lb-mysql`.</span></span> <span data-ttu-id="0f8a1-200">Állítsa be **idő** too5 másodperc, a minimális.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-200">Set **Time** too5 seconds, minimum.</span></span>

<span data-ttu-id="0f8a1-201">Hello végpont létrehozása után nyissa meg túl`hadb02`, válassza a **végpontok**, és hozzon létre egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-201">After you create hello endpoint, go too`hadb02`, choose **Endpoints**, and create an endpoint.</span></span> <span data-ttu-id="0f8a1-202">Válasszon `lb-mysql`, majd válassza ki a MySQL hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-202">Choose `lb-mysql`, and then select MySQL from hello drop-down list.</span></span> <span data-ttu-id="0f8a1-203">Ebben a lépésben hello Azure CLI használhatja.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-203">You can also use hello Azure CLI for this step.</span></span>

<span data-ttu-id="0f8a1-204">Most már rendelkezik minden hello fürt kézi művelet szükséges.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-204">You now have everything you need for manual operation of hello cluster.</span></span>

### <a name="test-hello-load-balanced-set"></a><span data-ttu-id="0f8a1-205">Elosztott terhelésű készlet hello tesztelése</span><span class="sxs-lookup"><span data-stu-id="0f8a1-205">Test hello load-balanced set</span></span>
<span data-ttu-id="0f8a1-206">Tesztek külső gépből hajtható végre a MySQL-ügyfélprogrammal, vagy bizonyos alkalmazások, például egy Azure-webhelyen fut phpMyAdmin használatával.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-206">Tests can be performed from an outside machine by using any MySQL client, or by using certain applications, like phpMyAdmin running as an Azure website.</span></span> <span data-ttu-id="0f8a1-207">Ebben az esetben egy másik Linux mezőben használt MySQL a parancssori eszköz:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-207">In this case, you used MySQL's command-line tool on another Linux box:</span></span>

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a><span data-ttu-id="0f8a1-208">Manuális feladatátvétele</span><span class="sxs-lookup"><span data-stu-id="0f8a1-208">Manually failing over</span></span>
<span data-ttu-id="0f8a1-209">Feladatátvétel szimulálhatja MySQL leállítása, DRBD tartozó elsődleges váltás, és újból elindítani a MySQL.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-209">You can simulate failovers by shutting down MySQL, switching DRBD's primary, and starting MySQL again.</span></span>

<span data-ttu-id="0f8a1-210">tooperform Ez a feladat, futtassa a következő kódot a hadb01 hello:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-210">tooperform this task, run hello following code on hadb01:</span></span>

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

<span data-ttu-id="0f8a1-211">Majd a hadb02:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-211">Then, on hadb02:</span></span>

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

<span data-ttu-id="0f8a1-212">Miután a rendszer átadja manuálisan, megismételheti a távoli lekérdezés, és tökéletesen működnek.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-212">After you fail over manually, you can repeat your remote query and it should work perfectly.</span></span>

## <a name="set-up-corosync"></a><span data-ttu-id="0f8a1-213">Corosync beállítása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-213">Set up Corosync</span></span>
<span data-ttu-id="0f8a1-214">Corosync hello támasztja toowork szükséges alapul szolgáló fürt infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-214">Corosync is hello underlying cluster infrastructure required for Pacemaker toowork.</span></span> <span data-ttu-id="0f8a1-215">A szívverés (és egyéb módszerek, például Ultramonkey) Corosync hello CRM funkciók, valószínűségét, míg támasztja marad több hasonló tooHeartbeat funkciót.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-215">For Heartbeat (and other methodologies like Ultramonkey), Corosync is a split of hello CRM functionalities, while Pacemaker remains more similar tooHeartbeat in functionality.</span></span>

<span data-ttu-id="0f8a1-216">hello fő Corosync az Azure pedig az, hogy egyedi küldésű kommunikációs keresztül Corosync inkább az szórás keresztül csoportos küldés, de a Microsoft Azure-hálózat csak az egyedi küldéses támogatja.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-216">hello main constraint for Corosync on Azure is that Corosync prefers multicast over broadcast over unicast communications, but Microsoft Azure networking only supports unicast.</span></span>

<span data-ttu-id="0f8a1-217">Szerencsére Corosync van egy működő egyedi küldéses módot.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-217">Fortunately, Corosync has a working unicast mode.</span></span> <span data-ttu-id="0f8a1-218">hello csak valós pedig az, hogy minden csomópont nem kommunikál egymással, mert kell toodefine hello csomópontok a konfigurációs fájlokban, beleértve az IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-218">hello only real constraint is that because all nodes are not communicating among themselves, you need toodefine hello nodes in your configuration files, including their IP addresses.</span></span> <span data-ttu-id="0f8a1-219">Az egyedi küldés és a módosítás kötési cím, a csomópont listák és a naplózási könyvtár használhatjuk hello Corosync példa fájlok (Ubuntu használja `/var/log/corosync` hello példa fájlok használata közben `/var/log/cluster`), és kvórum eszközök engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-219">We can use hello Corosync example files for Unicast and change bind address, node lists, and logging directories (Ubuntu uses `/var/log/corosync` while hello example files use `/var/log/cluster`), and enable quorum tools.</span></span>

> [!NOTE]
> <span data-ttu-id="0f8a1-220">Hello következő `transport: udpu` irányelv és hello kézzel definiált mindkét csomópont IP-címet.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-220">Use hello following `transport: udpu` directive and hello manually defined IP addresses for both nodes.</span></span>

<span data-ttu-id="0f8a1-221">Futtatási hello hibakód a következő `/etc/corosync/corosync.conf` mindkét csomópontok:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-221">Run hello following code on `/etc/corosync/corosync.conf` for both nodes:</span></span>

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

<span data-ttu-id="0f8a1-222">Másolja a konfigurációs fájl mindkét virtuális gépeken, és mindkét csomópont Corosync elindításához:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-222">Copy this configuration file on both VMs and start Corosync in both nodes:</span></span>

    sudo service start corosync

<span data-ttu-id="0f8a1-223">Röviddel a hello szolgáltatás elindítása után hello fürt hello aktuális gyűrűben kell létrehozni, és kvórum kell kialakítani.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-223">Shortly after starting hello service, hello cluster should be established in hello current ring, and quorum should be constituted.</span></span> <span data-ttu-id="0f8a1-224">Ez a funkció jelenleg ellenőrizheti a naplók megtekintésével, vagy futtassa a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-224">We can check this functionality by reviewing logs or by running hello following code:</span></span>

    sudo corosync-quorumtool –l

<span data-ttu-id="0f8a1-225">Kimeneti hasonló toohello kép a következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-225">You will see output similar toohello following image:</span></span>

![corosync-quorumtool - l minta kimenet](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a><span data-ttu-id="0f8a1-227">Támasztja beállítása</span><span class="sxs-lookup"><span data-stu-id="0f8a1-227">Set up Pacemaker</span></span>
<span data-ttu-id="0f8a1-228">Támasztja által használt erőforrások a fürt toomonitor hello, adja meg, ha az elsődleges leáll, és ezen erőforrások toosecondaries válthat.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-228">Pacemaker uses hello cluster toomonitor for resources, define when primaries go down, and switch those resources toosecondaries.</span></span> <span data-ttu-id="0f8a1-229">Erőforrások elérhető parancsfájlokkal, illetve LSB (init-bA), parancsfájlok egyéb lehetőségek között definiálhatók.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-229">Resources can be defined from a set of available scripts or from LSB (init-like) scripts, among other choices.</span></span>

<span data-ttu-id="0f8a1-230">Azt szeretnénk, ha támasztja túl "saját" hello DRBD erőforrást, hello csatlakoztatási pontot és hello MySQL-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-230">We want Pacemaker too"own" hello DRBD resource, hello mount point, and hello MySQL service.</span></span> <span data-ttu-id="0f8a1-231">Támasztja kapcsolhatja be és ki DRBD, ha csatlakoztatása és leválasztása, majd indítsa el és leállítani a MySQL hello jobb, ha valami rossz sorrendben fordul elő, ha hello elsődleges, a telepítés befejeződése.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-231">If Pacemaker can turn on and off DRBD, mount and unmount it, and then start and stop MySQL in hello right order when something bad happens with hello primary, setup is complete.</span></span>

<span data-ttu-id="0f8a1-232">Támasztja első telepítésekor a konfiguráció legyen egyszerű, elég, például:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-232">When you first install Pacemaker, your configuration should be simple enough, something like:</span></span>

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. <span data-ttu-id="0f8a1-233">Hello konfigurációjának ellenőrzése futtatásával `sudo crm configure show`.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-233">Check hello configuration by running `sudo crm configure show`.</span></span>
2. <span data-ttu-id="0f8a1-234">Ezután hozzon létre egy fájlt (például `/tmp/cluster.conf`) a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-234">Then create a file (like `/tmp/cluster.conf`) with hello following resources:</span></span>

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

3. <span data-ttu-id="0f8a1-235">Betöltési hello hello konfigurációs fájlból.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-235">Load hello file into hello configuration.</span></span> <span data-ttu-id="0f8a1-236">Csak akkor kell toodo Ez egy csomópontban.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-236">You only need toodo this in one node.</span></span>

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. <span data-ttu-id="0f8a1-237">Győződjön meg arról, hogy támasztja kezdődik-e a mindkét csomópont indítása:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-237">Make sure that Pacemaker starts at boot in both nodes:</span></span>

        sudo update-rc.d pacemaker defaults

5. <span data-ttu-id="0f8a1-238">A `sudo crm_mon –L`, győződjön meg arról, hogy a csomópontok egyikét hello fürt hello főkiszolgálójának és erőforrások hello fut-e.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-238">By using `sudo crm_mon –L`, verify that one of your nodes has become hello master for hello cluster and is running all hello resources.</span></span> <span data-ttu-id="0f8a1-239">Csatlakozási és ps toocheck hello erőforrások futtató is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-239">You can use mount and ps toocheck that hello resources are running.</span></span>

<span data-ttu-id="0f8a1-240">a következő képernyőfelvételen látható hello `crm_mon` egy csomópont leállt (Ctrl + C kiválasztásával kilépési):</span><span class="sxs-lookup"><span data-stu-id="0f8a1-240">hello following screenshot shows `crm_mon` with one node stopped (exit by selecting Ctrl+C):</span></span>

![crm_mon csomópont leállítása](./media/mysql-cluster/image002.png)

<span data-ttu-id="0f8a1-242">Ezen a képernyőfelvételen látható csomópontok, a egy fő és egy alárendelt jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-242">This screenshot shows both nodes, one master and one slave:</span></span>

![crm_mon műveleti főkiszolgáló/alárendelt](./media/mysql-cluster/image003.png)

## <a name="testing"></a><span data-ttu-id="0f8a1-244">Tesztelés</span><span class="sxs-lookup"><span data-stu-id="0f8a1-244">Testing</span></span>
<span data-ttu-id="0f8a1-245">Készen áll az Automatikus feladatátvétel szimuláció.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-245">You're ready for an automatic failover simulation.</span></span> <span data-ttu-id="0f8a1-246">Nincsenek két módon toodo ez: letölthető és rögzített.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-246">There are two ways toodo this: soft and hard.</span></span>

<span data-ttu-id="0f8a1-247">hello enyhe módon hello fürt leállítás függvényt használja: ``crm_standby -U `uname -n` -v on``.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-247">hello soft way uses hello cluster's shutdown function: ``crm_standby -U `uname -n` -v on``.</span></span> <span data-ttu-id="0f8a1-248">Ha ezzel hello fő, hello alárendelt élvez.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-248">If you use this on hello master, hello slave takes over.</span></span> <span data-ttu-id="0f8a1-249">Ne feledje tooset a hátsó toooff.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-249">Remember tooset this back toooff.</span></span> <span data-ttu-id="0f8a1-250">Ha ezt elmulasztja, akkor crm_mon készenléti egy csomópont jelenjen meg.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-250">If you don't, crm_mon will show one node on standby.</span></span>

<span data-ttu-id="0f8a1-251">hello rögzített módon leáll le hello elsődleges virtuális gép (hadb01) hello portálon keresztül, vagy módosítsa úgy a virtuális gép (Ez azt jelenti, hogy halt, leállítási) hello paraméterben megadott futtatási szint hello.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-251">hello hard way is shutting down hello primary VM (hadb01) via hello portal or by changing hello runlevel on hello VM (that is, halt, shutdown).</span></span> <span data-ttu-id="0f8a1-252">Segít Corosync és támasztja által jelzés adott hello fő folyamatos le.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-252">This helps Corosync and Pacemaker by signaling that hello master's going down.</span></span> <span data-ttu-id="0f8a1-253">Ez tesztelheti (hasznos a karbantartási időszakok), de hello forgatókönyv szerint hello VM zárolása is kényszeríthető.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-253">You can test this (useful for maintenance windows), but you can also force hello scenario by freezing hello VM.</span></span>

## <a name="stonith"></a><span data-ttu-id="0f8a1-254">STONITH</span><span class="sxs-lookup"><span data-stu-id="0f8a1-254">STONITH</span></span>
<span data-ttu-id="0f8a1-255">A virtuális gép leállítása hello Azure parancssori felület helyett egy fizikai eszközt vezérlő STONITH parancsfájl keresztül lehetséges tooissue kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-255">It should be possible tooissue a VM shutdown via hello Azure CLI in lieu of a STONITH script that controls a physical device.</span></span> <span data-ttu-id="0f8a1-256">Használhat `/usr/lib/stonith/plugins/external/ssh` talál és hello fürt konfigurációjába STONITH engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-256">You can use `/usr/lib/stonith/plugins/external/ssh` as a base and enable STONITH in hello cluster's configuration.</span></span> <span data-ttu-id="0f8a1-257">Az Azure CLI globálisan kell telepíteni, és a hello közzétételi beállítások és hello fürt felhasználói profil kell betölteni.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-257">Azure CLI should be globally installed, and hello publish settings and profile should be loaded for hello cluster's user.</span></span>

<span data-ttu-id="0f8a1-258">Mintakód hello erőforrás érhető el a [GitHub](https://github.com/bureado/aztonith).</span><span class="sxs-lookup"><span data-stu-id="0f8a1-258">Sample code for hello resource is available on [GitHub](https://github.com/bureado/aztonith).</span></span> <span data-ttu-id="0f8a1-259">Hello fürt konfigurációjának módosítása hello túl a következő hozzáadásával`sudo crm configure`:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-259">Change hello cluster's configuration by adding hello following too`sudo crm configure`:</span></span>

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> <span data-ttu-id="0f8a1-260">hello parancsfájl nem hajtható végre fel/le ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-260">hello script doesn't perform up/down checks.</span></span> <span data-ttu-id="0f8a1-261">hello eredeti SSH erőforrás volt a 15 ping-ellenőrzéseket, de egy Azure virtuális gép helyreállítási idő további változó lehet.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-261">hello original SSH resource had 15 ping checks, but recovery time for an Azure VM might be more variable.</span></span>

## <a name="limitations"></a><span data-ttu-id="0f8a1-262">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="0f8a1-262">Limitations</span></span>
<span data-ttu-id="0f8a1-263">hello a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-263">hello following limitations apply:</span></span>

* <span data-ttu-id="0f8a1-264">hello linbit DRBD erőforrást parancsprogram DRBD kezelő erőforrásként támasztja használja a `drbdadm down` Ha egy csomópont leáll, akkor is, ha hello csomópont csak készenléti van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-264">hello linbit DRBD resource script that manages DRBD as a resource in Pacemaker uses `drbdadm down` when shutting down a node, even if hello node is just going on standby.</span></span> <span data-ttu-id="0f8a1-265">Ennek oka nem ideális hello alárendelt fog nem lehet szinkronizálása hello DRBD erőforrás hello fő lekérdezi az írási műveletek során.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-265">This is not ideal because hello slave will not be synchronizing hello DRBD resource while hello master gets writes.</span></span> <span data-ttu-id="0f8a1-266">Ha hello fő nem graciously sikertelen, a hello alárendelt akár egy régebbi fájlrendszer állapota is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-266">If hello master does not fail graciously, hello slave can take over an older file system state.</span></span> <span data-ttu-id="0f8a1-267">Ez megoldására két lehetséges módja van:</span><span class="sxs-lookup"><span data-stu-id="0f8a1-267">There are two potential ways of solving this:</span></span>
  * <span data-ttu-id="0f8a1-268">Érvényesítési egy `drbdadm up r0` a fürt összes csomópontján keresztül egy helyi (nem clusterized)-figyelő</span><span class="sxs-lookup"><span data-stu-id="0f8a1-268">Enforcing a `drbdadm up r0` in all cluster nodes via a local (not clusterized) watchdog</span></span>
  * <span data-ttu-id="0f8a1-269">Hello linbit DRBD parancsfájl szerkesztése, ügyelve arra, hogy `down` nem hívják meg`/usr/lib/ocf/resource.d/linbit/drbd`</span><span class="sxs-lookup"><span data-stu-id="0f8a1-269">Editing hello linbit DRBD script, making sure that `down` is not called in `/usr/lib/ocf/resource.d/linbit/drbd`</span></span>
* <span data-ttu-id="0f8a1-270">hello terheléselosztónak legalább öt másodpercenként toorespond kell, így az alkalmazások fürttámogató legyen, és jobban tűrik a timeout kell.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-270">hello load balancer needs at least five seconds toorespond, so applications should be cluster-aware and be more tolerant of timeout.</span></span> <span data-ttu-id="0f8a1-271">Egyéb-architektúrák esetén az-app várólisták és a lekérdezés middlewares, például segíthet is.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-271">Other architectures, like in-app queues and query middlewares, can also help.</span></span>
* <span data-ttu-id="0f8a1-272">MySQL hangolása szükséges, amely írás kezelhető ütemben történik tooensure pedig gyorsítótárak kiürített toodisk gyakorisággal lehetséges toominimize memória adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-272">MySQL tuning is necessary tooensure that writing is done at a manageable pace and caches are flushed toodisk as frequently as possible toominimize memory loss.</span></span>
* <span data-ttu-id="0f8a1-273">Az írási teljesítmény függő a virtuális gép virtuális kapcsoló hello kapcsolódnak össze, mivel ez hello mechanizmus DRBD tooreplicate hello eszköz által használt.</span><span class="sxs-lookup"><span data-stu-id="0f8a1-273">Write performance is dependent in VM interconnect in hello virtual switch because this is hello mechanism used by DRBD tooreplicate hello device.</span></span>
