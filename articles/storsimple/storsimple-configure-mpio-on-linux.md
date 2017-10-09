---
title: "a StorSimple Linux gazdagép MPIO aaaConfigure |} Microsoft Docs"
description: "A csatlakoztatott StorSimple tooa Linux gazdagépen futó CentOS 6.6 az MPIO konfigurálása"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: d9f7e02903243494c909313fb2c33ac690764274
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a><span data-ttu-id="27e6d-103">Az MPIO konfigurálása a StorSimple gazdagépen fut a CentOS</span><span class="sxs-lookup"><span data-stu-id="27e6d-103">Configure MPIO on a StorSimple host running CentOS</span></span>
<span data-ttu-id="27e6d-104">Ez a cikk ismerteti a hello lépéseket szükséges tooconfigure többutas I/O (MPIO) a Centos 6.6 gazdagép-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="27e6d-104">This article explains hello steps required tooconfigure Multipathing IO (MPIO) on your Centos 6.6 host server.</span></span> <span data-ttu-id="27e6d-105">hello gazdagép-kiszolgálón található csatlakoztatott tooyour Microsoft Azure StorSimple eszköz iSCSI-kezdeményezők keresztül magas rendelkezésre állásra.</span><span class="sxs-lookup"><span data-stu-id="27e6d-105">hello host server is connected tooyour Microsoft Azure StorSimple device for high availability via iSCSI initiators.</span></span> <span data-ttu-id="27e6d-106">A részletes hello automatikus felderítés többutas eszközről és hello adott telepítő csak a StorSimple-köteteket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="27e6d-106">It describes in detail hello automatic discovery of multipath devices and hello specific setup only for StorSimple volumes.</span></span>

<span data-ttu-id="27e6d-107">Ez az eljárás akkor alkalmazható tooall hello modelljei a StorSimple 8000 sorozat eszközeire.</span><span class="sxs-lookup"><span data-stu-id="27e6d-107">This procedure is applicable tooall hello models of StorSimple 8000 series devices.</span></span>

> [!NOTE]
> <span data-ttu-id="27e6d-108">Ez az eljárás nem használható a StorSimple virtuális eszköz.</span><span class="sxs-lookup"><span data-stu-id="27e6d-108">This procedure cannot be used for a StorSimple virtual device.</span></span> <span data-ttu-id="27e6d-109">További információkért lásd: hogyan tooconfigure állomáskiszolgáló a virtuális eszköz.</span><span class="sxs-lookup"><span data-stu-id="27e6d-109">For more information, see how tooconfigure host servers for your virtual device.</span></span>
> 
> 

## <a name="about-multipathing"></a><span data-ttu-id="27e6d-110">Többutas kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="27e6d-110">About multipathing</span></span>
<span data-ttu-id="27e6d-111">hello többutas funkció lehetővé teszi tooconfigure a kiszolgáló és a tárolóeszköz közötti több i/o-útvonal.</span><span class="sxs-lookup"><span data-stu-id="27e6d-111">hello multipathing feature allows you tooconfigure multiple I/O paths between a host server and a storage device.</span></span> <span data-ttu-id="27e6d-112">Az i/o-elérési utak, amelyek külön kábelek, a kapcsolók, a hálózati adapterek és a tartományvezérlők fizikai SAN-kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="27e6d-112">These I/O paths are physical SAN connections that can include separate cables, switches, network interfaces, and controllers.</span></span> <span data-ttu-id="27e6d-113">Többutas hello i/o elérési utak, egy új eszközt társított összes összesített hello görbét tooconfigure összesíti.</span><span class="sxs-lookup"><span data-stu-id="27e6d-113">Multipathing aggregates hello I/O paths, tooconfigure a new device that is associated with all of hello aggregated paths.</span></span>

<span data-ttu-id="27e6d-114">hello többutas célja kettős:</span><span class="sxs-lookup"><span data-stu-id="27e6d-114">hello purpose of multipathing is two-fold:</span></span>

* <span data-ttu-id="27e6d-115">**Magas rendelkezésre állású**: hello i/o-elérési utat (például egy kábel, kapcsoló, hálózati adapter vagy tartományvezérlő) bármely elemének meghibásodásakor egy alternatív útvonalat biztosít.</span><span class="sxs-lookup"><span data-stu-id="27e6d-115">**High availability**: It provides an alternate path if any element of hello I/O path (such as a cable, switch, network interface, or controller) fails.</span></span>
* <span data-ttu-id="27e6d-116">**Terheléselosztás**: a tárolóeszköz hello konfigurációjától függően az hello a jobb teljesítmény érdekében terhelések hello i/o-elérési utak a észlelésére, és dinamikusan újraelosztás e terhelések.</span><span class="sxs-lookup"><span data-stu-id="27e6d-116">**Load balancing**: Depending on hello configuration of your storage device, it can improve hello performance by detecting loads on hello I/O paths and dynamically rebalancing those loads.</span></span>

### <a name="about-multipathing-components"></a><span data-ttu-id="27e6d-117">Többutas összetevőivel kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="27e6d-117">About multipathing components</span></span>
<span data-ttu-id="27e6d-118">A Linux többutas áll kernel és felhasználói térben összetevői, az alábbi táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-118">Multipathing in Linux consists of kernel components and user-space components as tabulated below.</span></span>

* <span data-ttu-id="27e6d-119">**Kernel**: hello fő összetevője hello *eszköz-leképező* , amely reroutes i/o és elérési utak és elérési útja csoportok támogatja a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-119">**Kernel**: hello main component is hello *device-mapper* that reroutes I/O and supports failover for paths and path groups.</span></span>

* <span data-ttu-id="27e6d-120">**Felhasználói térben**: ezek a *Többutas-eszközök* , amely multipathed eszközök kezelése modulban a hello eszköz-többutas leképezőmodulja milyen toodo.</span><span class="sxs-lookup"><span data-stu-id="27e6d-120">**User-space**: These are *multipath-tools* that manage multipathed devices by instructing hello device-mapper multipath module what toodo.</span></span> <span data-ttu-id="27e6d-121">hello eszközök foglalják magukban:</span><span class="sxs-lookup"><span data-stu-id="27e6d-121">hello tools consist of:</span></span>
   
   * <span data-ttu-id="27e6d-122">**Többutas**: sorolja fel, és beállítja az multipathed eszközöket.</span><span class="sxs-lookup"><span data-stu-id="27e6d-122">**Multipath**: lists and configures multipathed devices.</span></span>
   * <span data-ttu-id="27e6d-123">**Multipathd**: démon, amely végrehajtja a többutas és a figyelők hello elérési utat.</span><span class="sxs-lookup"><span data-stu-id="27e6d-123">**Multipathd**: daemon that executes multipath and monitors hello paths.</span></span>
   * <span data-ttu-id="27e6d-124">**Devmap-name**: rendelkezik egy jelentéssel bíró eszköznév tooudev devmaps.</span><span class="sxs-lookup"><span data-stu-id="27e6d-124">**Devmap-name**: provides a meaningful device-name tooudev for devmaps.</span></span>
   * <span data-ttu-id="27e6d-125">**Kpartx**: lineáris devmaps toodevice partíciók toomake többutas maps particionálható rendeli.</span><span class="sxs-lookup"><span data-stu-id="27e6d-125">**Kpartx**: maps linear devmaps toodevice partitions toomake multipath maps partitionable.</span></span>
   * <span data-ttu-id="27e6d-126">**MultiPath.conf**: ki, hogy a használt toooverwrite hello beépített konfigurációs tábla többutas démon konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-126">**Multipath.conf**: configuration file for multipath daemon that is used toooverwrite hello built-in configuration table.</span></span>

### <a name="about-hello-multipathconf-configuration-file"></a><span data-ttu-id="27e6d-127">Hello multipath.conf konfigurációs fájllal kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="27e6d-127">About hello multipath.conf configuration file</span></span>
<span data-ttu-id="27e6d-128">hello konfigurációs fájl `/etc/multipath.conf` teszi hello számos többutas szolgáltatások felhasználó által konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-128">hello configuration file `/etc/multipath.conf` makes many of hello multipathing features user-configurable.</span></span> <span data-ttu-id="27e6d-129">Hello `multipath` parancs és hello kernel démon `multipathd` használja a fájlban található információkat.</span><span class="sxs-lookup"><span data-stu-id="27e6d-129">hello `multipath` command and hello kernel daemon `multipathd` use information found in this file.</span></span> <span data-ttu-id="27e6d-130">hello fájl van csak a hello Többutas eszközök hello konfigurálása során megkeresett.</span><span class="sxs-lookup"><span data-stu-id="27e6d-130">hello file is consulted only during hello configuration of hello multipath devices.</span></span> <span data-ttu-id="27e6d-131">Győződjön meg arról, hogy minden módosításai hello futtatása előtt `multipath` parancsot.</span><span class="sxs-lookup"><span data-stu-id="27e6d-131">Make sure that all changes are made before you run hello `multipath` command.</span></span> <span data-ttu-id="27e6d-132">Ha hello fájl módosítása ezután lesz toostop kell és indítsa el újra az hello módosítások tootake hatás multipathd.</span><span class="sxs-lookup"><span data-stu-id="27e6d-132">If you modify hello file afterwards, you will need toostop and start multipathd again for hello changes tootake effect.</span></span>

<span data-ttu-id="27e6d-133">hello multipath.conf öt részből áll:</span><span class="sxs-lookup"><span data-stu-id="27e6d-133">hello multipath.conf has five sections:</span></span>

- <span data-ttu-id="27e6d-134">**Rendszer alapbeállításainak** *(alapértelmezett)*: rendszer alapbeállításainak felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-134">**System level defaults** *(defaults)*: You can override system level defaults.</span></span>
- <span data-ttu-id="27e6d-135">**Eszközök feketelistára teszi** *(blacklist)*: hello eszköz-leképező nem kell vezérelnie eszközök listáját is megadhat.</span><span class="sxs-lookup"><span data-stu-id="27e6d-135">**Blacklisted devices** *(blacklist)*: You can specify hello list of devices that should not be controlled by device-mapper.</span></span>
- <span data-ttu-id="27e6d-136">**Kivételek tiltólistára** *(blacklist_exceptions)*: azonosíthatja, konkrét eszközökhöz toobe Többutas eszközök számít, még akkor is, ha hello blacklist szerepel.</span><span class="sxs-lookup"><span data-stu-id="27e6d-136">**Blacklist exceptions** *(blacklist_exceptions)*: You can identify specific devices toobe treated as multipath devices even if listed in hello blacklist.</span></span>
- <span data-ttu-id="27e6d-137">**Tárolási vezérlő adott beállítások** *(eszközök)*: megadhatja a szállító és a termékkulcsot rendelkező alkalmazott toodevices lesz konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="27e6d-137">**Storage controller specific settings** *(devices)*: You can specify configuration settings that will be applied toodevices that have Vendor and Product information.</span></span>
- <span data-ttu-id="27e6d-138">**Adott eszközbeállítások** *(multipaths)*: Ez a szakasz toofine-hangolási hello konfigurációs beállítások különálló logikaiegység-számok használható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-138">**Device specific settings** *(multipaths)*: You can use this section toofine-tune hello configuration settings for individual LUNs.</span></span>

## <a name="configure-multipathing-on-storsimple-connected-toolinux-host"></a><span data-ttu-id="27e6d-139">Többutas konfigurálása a StorSimple csatlakoztatott tooLinux gazdagépen</span><span class="sxs-lookup"><span data-stu-id="27e6d-139">Configure multipathing on StorSimple connected tooLinux host</span></span>
<span data-ttu-id="27e6d-140">A StorSimple-eszköz csatlakoztatásakor tooa Linux-állomáshoz konfigurálhatja a magas rendelkezésre állású, és a terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="27e6d-140">A StorSimple device connected tooa Linux host can be configured for high availability and load balancing.</span></span> <span data-ttu-id="27e6d-141">Például akkor, ha hello Linux-állomáshoz csatlakoztatott toohello SAN és hello eszköznek két csatolót két csatolót csatlakoztatva toohello SAN úgy, hogy ezek interfészekre hello ugyanazon az alhálózaton, akkor nem lesznek 4 útvonal érhető el.</span><span class="sxs-lookup"><span data-stu-id="27e6d-141">For example, if hello Linux host has two interfaces connected toohello SAN and hello device has two interfaces connected toohello SAN such that these interfaces are on hello same subnet, then there will be 4 paths available.</span></span> <span data-ttu-id="27e6d-142">Azonban ha minden adatillesztő hello eszköz és a gazdagép illesztőn a különböző IP-alhálózat (és nem irányítható), majd csak 2 elérési utak lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="27e6d-142">However, if each DATA interface on hello device and host interface are on a different IP subnet (and not routable), then only 2 paths will be available.</span></span> <span data-ttu-id="27e6d-143">Beállíthatja használjon többutas tooautomatically hello elérhető görbékhez felderítése, válassza ki az adott elérési útján a terheléselosztási algoritmust, csak a StorSimple-köteteket, megadott konfigurációs beállítások alkalmazása engedélyezése és többutas ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="27e6d-143">You can configure multipathing tooautomatically discover all hello available paths, choose a load-balancing algorithm for those paths, apply specific configuration settings for StorSimple-only volumes, and then enable and verify multipathing.</span></span>

<span data-ttu-id="27e6d-144">hello alábbi eljárás leírja, hogy ha két hálózati adapterrel rendelkező StorSimple eszköz tooconfigure többutas csatlakoztatott tooa gazdagépet két hálózati adapterrel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="27e6d-144">hello following procedure describes how tooconfigure multipathing when a StorSimple device with two network interfaces is connected tooa host with two network interfaces.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27e6d-145">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="27e6d-145">Prerequisites</span></span>
<span data-ttu-id="27e6d-146">Ez a szakasz részletesen hello konfigurációs előfeltételeit CentOS kiszolgáló és a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-146">This section details hello configuration prerequisites for CentOS server and your StorSimple device.</span></span>

### <a name="on-centos-host"></a><span data-ttu-id="27e6d-147">A CentOS gazdagépen</span><span class="sxs-lookup"><span data-stu-id="27e6d-147">On CentOS host</span></span>
1. <span data-ttu-id="27e6d-148">Győződjön meg arról, hogy a CentOS gazdagépen engedélyezett 2 hálózati illesztőt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-148">Make sure that your CentOS host has 2 network interfaces enabled.</span></span> <span data-ttu-id="27e6d-149">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-149">Type:</span></span>
   
    `ifconfig`
   
    <span data-ttu-id="27e6d-150">hello következő példa bemutatja, hello kimeneti amikor két hálózati illesztőt (`eth0` és `eth1`) hello gazdagépen találhatók.</span><span class="sxs-lookup"><span data-stu-id="27e6d-150">hello following example shows hello output when two network interfaces (`eth0` and `eth1`) are present on hello host.</span></span>
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. <span data-ttu-id="27e6d-151">Telepítés *iSCSI-kezdeményező-utils* a CentOS kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="27e6d-151">Install *iSCSI-initiator-utils* on your CentOS server.</span></span> <span data-ttu-id="27e6d-152">Hajtsa végre a következő lépéseket tooinstall hello *iSCSI-kezdeményező-utils*.</span><span class="sxs-lookup"><span data-stu-id="27e6d-152">Perform hello following steps tooinstall *iSCSI-initiator-utils*.</span></span>
   
   1. <span data-ttu-id="27e6d-153">Jelentkezzen be `root` a CentOS gazdagép be.</span><span class="sxs-lookup"><span data-stu-id="27e6d-153">Log on as `root` into your CentOS host.</span></span>
   2. <span data-ttu-id="27e6d-154">Telepítse a hello *iSCSI-kezdeményező-utils*.</span><span class="sxs-lookup"><span data-stu-id="27e6d-154">Install hello *iSCSI-initiator-utils*.</span></span> <span data-ttu-id="27e6d-155">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-155">Type:</span></span>
      
       `yum install iscsi-initiator-utils`
   3. <span data-ttu-id="27e6d-156">Hello után *iSCSI-kezdeményező-utils* sikeresen van telepítve, indítsa el a hello iSCSI szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="27e6d-156">After hello *iSCSI-Initiator-utils* is successfully installed, start hello iSCSI service.</span></span> <span data-ttu-id="27e6d-157">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-157">Type:</span></span>
      
       `service iscsid start`
      
       <span data-ttu-id="27e6d-158">Alkalommal `iscsid` előfordulhat, hogy ténylegesen nem start és hello `--force` beállítás szükséges</span><span class="sxs-lookup"><span data-stu-id="27e6d-158">On occasions, `iscsid` may not actually start and hello `--force` option may be needed</span></span>
   4. <span data-ttu-id="27e6d-159">hogy az iSCSI-kezdeményező engedélyezve van-e rendszerindító időszakban használata hello tooensure `chkconfig` tooenable hello szolgáltatás parancsot.</span><span class="sxs-lookup"><span data-stu-id="27e6d-159">tooensure that your iSCSI initiator is enabled during boot time, use hello `chkconfig` command tooenable hello service.</span></span>
      
       `chkconfig iscsi on`
   5. <span data-ttu-id="27e6d-160">tooverify, hogy megfelelően lett, a telepítő hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="27e6d-160">tooverify that that it was properly setup, run hello command:</span></span>
      
       `chkconfig --list | grep iscsi`
      
       <span data-ttu-id="27e6d-161">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="27e6d-161">A sample output is shown below.</span></span>
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       <span data-ttu-id="27e6d-162">A fenti példa hello láthatja, hogy az iSCSI-környezetben működnek rendszerindítás futtatási szinten, 2, 3, 4 és 5.</span><span class="sxs-lookup"><span data-stu-id="27e6d-162">From hello above example, you can see that your iSCSI environment will run on boot time on run levels 2, 3, 4, and 5.</span></span>
3. <span data-ttu-id="27e6d-163">Telepítés *eszköz-leképező-többutas*.</span><span class="sxs-lookup"><span data-stu-id="27e6d-163">Install *device-mapper-multipath*.</span></span> <span data-ttu-id="27e6d-164">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-164">Type:</span></span>
   
    `yum install device-mapper-multipath`
   
    <span data-ttu-id="27e6d-165">hello telepítés indul el.</span><span class="sxs-lookup"><span data-stu-id="27e6d-165">hello installation will start.</span></span> <span data-ttu-id="27e6d-166">Típus **Y** toocontinue, amikor a rendszer megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="27e6d-166">Type **Y** toocontinue when prompted for confirmation.</span></span>

### <a name="on-storsimple-device"></a><span data-ttu-id="27e6d-167">A StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="27e6d-167">On StorSimple device</span></span>
<span data-ttu-id="27e6d-168">A StorSimple eszköz kell rendelkezniük:</span><span class="sxs-lookup"><span data-stu-id="27e6d-168">Your StorSimple device should have:</span></span>

* <span data-ttu-id="27e6d-169">Legalább két csatolót engedélyezni kell az iSCSI.</span><span class="sxs-lookup"><span data-stu-id="27e6d-169">A minimum of two interfaces enabled for iSCSI.</span></span> <span data-ttu-id="27e6d-170">tooverify, hogy két felületet a StorSimple eszköz iSCSI engedélyezve a következő lépéseket a klasszikus Azure portálon, a StorSimple eszköz hello hello hajtsa végre:</span><span class="sxs-lookup"><span data-stu-id="27e6d-170">tooverify that two interfaces are iSCSI-enabled on your StorSimple device, perform hello following steps in hello Azure classic portal for your StorSimple device:</span></span>
  
  1. <span data-ttu-id="27e6d-171">Jelentkezzen be a StorSimple eszköz hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="27e6d-171">Log into hello classic portal for your StorSimple device.</span></span>
  2. <span data-ttu-id="27e6d-172">Jelölje ki azt a StorSimple Manager szolgáltatás **eszközök** , és válassza a hello StorSimple eszközre.</span><span class="sxs-lookup"><span data-stu-id="27e6d-172">Select your StorSimple Manager service, click **Devices** and choose hello specific StorSimple device.</span></span> <span data-ttu-id="27e6d-173">Kattintson a **konfigurálása** és hello hálózati kapcsolat beállításainak ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="27e6d-173">Click **Configure** and verify hello network interface settings.</span></span> <span data-ttu-id="27e6d-174">A képernyőfelvételen két iSCSI-kompatibilis hálózati adapterrel rendelkező alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="27e6d-174">A screenshot with two iSCSI-enabled network interfaces is shown below.</span></span> <span data-ttu-id="27e6d-175">Itt DATA 2 és a DATA 3, mind 10 GbE adapterek iSCSI engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="27e6d-175">Here DATA 2 and DATA 3, both 10 GbE interfaces are enabled for iSCSI.</span></span>
     
      ![Az MPIO StorsSimple DATA 2 config](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![Az MPIO StorSimple adatok 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      <span data-ttu-id="27e6d-178">A hello **konfigurálása** lap</span><span class="sxs-lookup"><span data-stu-id="27e6d-178">In hello **Configure** page</span></span>
     
     1. <span data-ttu-id="27e6d-179">Győződjön meg arról, hogy mindkét hálózati adapterek iSCSI engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="27e6d-179">Ensure that both network interfaces are iSCSI-enabled.</span></span> <span data-ttu-id="27e6d-180">Hello **iSCSI engedélyezve** mezőt állítsa túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="27e6d-180">hello **iSCSI enabled** field should be set too**Yes**.</span></span>
     2. <span data-ttu-id="27e6d-181">Győződjön meg arról, hogy hello hálózati adapternek hello azonos sebességű is 1 gbe-s vagy 10 GbE kell lennie.</span><span class="sxs-lookup"><span data-stu-id="27e6d-181">Ensure that hello network interfaces have hello same speed, both should be 1 GbE or 10 GbE.</span></span>
     3. <span data-ttu-id="27e6d-182">Vegye figyelembe a hello-adapterek iSCSI-kompatibilis hello IPv4-címeket, és mentse hello gazdagépen későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="27e6d-182">Note hello IPv4 addresses of hello iSCSI-enabled interfaces and save for later use on hello host.</span></span>
* <span data-ttu-id="27e6d-183">a StorSimple eszköz iSCSI csatolóinak hello hello CentOS kiszolgálóról elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="27e6d-183">hello iSCSI interfaces on your StorSimple device should be reachable from hello CentOS server.</span></span>
      <span data-ttu-id="27e6d-184">tooverify, tooprovide hello IP-címek az StorSimple iSCSI-kompatibilis hálózati adapterek van szüksége a gazdagép-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="27e6d-184">tooverify this, you need tooprovide hello IP addresses of your StorSimple iSCSI-enabled network interfaces on your host server.</span></span> <span data-ttu-id="27e6d-185">hello használt parancsok és adat2 megfelelő kimenet hello (10.126.162.25) és DATA3 (10.126.162.26) az alábbiakban tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="27e6d-185">hello commands used and hello corresponding output with DATA2 (10.126.162.25) and DATA3 (10.126.162.26) is shown below:</span></span>
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a><span data-ttu-id="27e6d-186">Hardverkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="27e6d-186">Hardware configuration</span></span>
<span data-ttu-id="27e6d-187">Azt javasoljuk, hogy csatlakozni, hello két iSCSI hálózati illesztők külön útvonalak a redundancia érdekében.</span><span class="sxs-lookup"><span data-stu-id="27e6d-187">We recommend that you connect hello two iSCSI network interfaces on separate paths for redundancy.</span></span> <span data-ttu-id="27e6d-188">hello az alábbi ábra hello ajánlott hardverkonfigurációja a magas rendelkezésre állás érdekében és terheléselosztás többutas a CentOS server és a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-188">hello figure below shows hello recommended hardware configuration for high availability and load-balancing multipathing for your CentOS server and StorSimple device.</span></span>

![A StorSimple tooLinux gazdagép MPIO hardverkonfiguráció](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

<span data-ttu-id="27e6d-190">Ahogy az ábra megelőző hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-190">As shown in hello preceding figure:</span></span>

* <span data-ttu-id="27e6d-191">A StorSimple eszköz van egy aktív-passzív konfigurációban két vezérlőkkel.</span><span class="sxs-lookup"><span data-stu-id="27e6d-191">Your StorSimple device is in an active-passive configuration with two controllers.</span></span>
* <span data-ttu-id="27e6d-192">Két SAN kapcsolók csatlakoztatott tooyour eszközvezérlők.</span><span class="sxs-lookup"><span data-stu-id="27e6d-192">Two SAN switches are connected tooyour device controllers.</span></span>
* <span data-ttu-id="27e6d-193">Két iSCSI-kezdeményezők engedélyezve vannak a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-193">Two iSCSI initiators are enabled on your StorSimple device.</span></span>
* <span data-ttu-id="27e6d-194">Két hálózati adapterrel engedélyezve vannak a CentOS állomáson.</span><span class="sxs-lookup"><span data-stu-id="27e6d-194">Two network interfaces are enabled on your CentOS host.</span></span>

<span data-ttu-id="27e6d-195">hello fent konfigurációs eredményez 4 külön útvonalak az eszköz és hello-állomás között, ha hello állomás és az adatok felületek irányítható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-195">hello above configuration will yield 4 separate paths between your device and hello host if hello host and data interfaces are routable.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="27e6d-196">Azt javasoljuk, hogy azt ne keverje 1 gbe-s és a 10 GbE hálózati adaptert használjon a többutas.</span><span class="sxs-lookup"><span data-stu-id="27e6d-196">We recommend that you do not mix 1 GbE and 10 GbE network interfaces for multipathing.</span></span> <span data-ttu-id="27e6d-197">Ha két hálózati adapterrel, akkor mindkét hello felületek hello azonos típusúnak kellene lennie.</span><span class="sxs-lookup"><span data-stu-id="27e6d-197">When using two network interfaces, both hello interfaces should be hello identical type.</span></span>
> * <span data-ttu-id="27e6d-198">A StorSimple eszköz DATA0, adat1, DATA4 és DATA5 1 GbE felületek mivel adat2 és DATA3 10 GbE hálózati adapterrel. |}</span><span class="sxs-lookup"><span data-stu-id="27e6d-198">On your StorSimple device, DATA0, DATA1, DATA4 and DATA5 are 1 GbE interfaces whereas DATA2 and DATA3 are 10 GbE network interfaces.|</span></span>
> 
> 

## <a name="configuration-steps"></a><span data-ttu-id="27e6d-199">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="27e6d-199">Configuration steps</span></span>
<span data-ttu-id="27e6d-200">a többutas hello konfigurációs lépéseket tartalmaz, amely hello elérhető elérési utak hello terheléselosztási algoritmus toouse, többutas engedélyezése, és végül a hello konfigurációjának ellenőrzése megadása automatikus felderítés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="27e6d-200">hello configuration steps for multipathing involve configuring hello available paths for automatic discovery, specifying hello load-balancing algorithm toouse, enabling multipathing and finally verifying hello configuration.</span></span> <span data-ttu-id="27e6d-201">A lépések részletei a következő részekben hello ismertet.</span><span class="sxs-lookup"><span data-stu-id="27e6d-201">Each of these steps is discussed in detail in hello following sections.</span></span>

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a><span data-ttu-id="27e6d-202">1. lépés: Az automatikus felderítést használjon többutas konfigurálása</span><span class="sxs-lookup"><span data-stu-id="27e6d-202">Step 1: Configure multipathing for automatic discovery</span></span>
<span data-ttu-id="27e6d-203">hello többutas által támogatott eszközök automatikusan felderíthető és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="27e6d-203">hello multipath-supported devices can be automatically discovered and configured.</span></span>

1. <span data-ttu-id="27e6d-204">Inicializálni `/etc/multipath.conf` fájlt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-204">Initialize `/etc/multipath.conf` file.</span></span> <span data-ttu-id="27e6d-205">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-205">Type:</span></span>
   
     `mpathconf --enable`
   
    <span data-ttu-id="27e6d-206">hello fent parancs létrehoz egy `sample/etc/multipath.conf` fájlt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-206">hello above command will create a `sample/etc/multipath.conf` file.</span></span>
2. <span data-ttu-id="27e6d-207">A többutas szolgáltatás elindítása.</span><span class="sxs-lookup"><span data-stu-id="27e6d-207">Start multipath service.</span></span> <span data-ttu-id="27e6d-208">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-208">Type:</span></span>
   
    `service multipathd start`
   
    <span data-ttu-id="27e6d-209">A következő kimeneti hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="27e6d-209">You will see hello following output:</span></span>
   
    `Starting multipathd daemon:`
3. <span data-ttu-id="27e6d-210">Multipaths automatikus észleléséhez.</span><span class="sxs-lookup"><span data-stu-id="27e6d-210">Enable automatic discovery of multipaths.</span></span> <span data-ttu-id="27e6d-211">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-211">Type:</span></span>
   
    `mpathconf --find_multipaths y`
   
    <span data-ttu-id="27e6d-212">Hello alapértelmezett szakasza módosítani fogja a `multipath.conf` alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="27e6d-212">This will modify hello defaults section of your `multipath.conf` as shown below:</span></span>
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a><span data-ttu-id="27e6d-213">2. lépés: A StorSimple-köteteket többutas konfigurálása</span><span class="sxs-lookup"><span data-stu-id="27e6d-213">Step 2: Configure multipathing for StorSimple volumes</span></span>
<span data-ttu-id="27e6d-214">Alapértelmezés szerint minden eszköz fekete hello multipath.conf fájlban felsorolt és művelet megkerülését eredményezte.</span><span class="sxs-lookup"><span data-stu-id="27e6d-214">By default, all devices are black listed in hello multipath.conf file and will be bypassed.</span></span> <span data-ttu-id="27e6d-215">Szüksége lesz toocreate blacklist kivételek tooallow többutas kötetek StorSimple eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="27e6d-215">You will need toocreate blacklist exceptions tooallow multipathing for volumes from StorSimple devices.</span></span>

1. <span data-ttu-id="27e6d-216">Hello szerkesztése `/etc/mulitpath.conf` fájlt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-216">Edit hello `/etc/mulitpath.conf` file.</span></span> <span data-ttu-id="27e6d-217">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-217">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="27e6d-218">Keresse meg a hello blacklist_exceptions szakasz hello multipath.conf fájlban.</span><span class="sxs-lookup"><span data-stu-id="27e6d-218">Locate hello blacklist_exceptions section in hello multipath.conf file.</span></span> <span data-ttu-id="27e6d-219">A StorSimple eszköz kell toobe ebben a szakaszban blacklist kivételként szerepel.</span><span class="sxs-lookup"><span data-stu-id="27e6d-219">Your StorSimple device needs toobe listed as a blacklist exception in this section.</span></span> <span data-ttu-id="27e6d-220">Akkor is állítsa vissza megfelelő sort a fájl toomodify azt (használata csak hello adott modell hello eszköz használ) alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="27e6d-220">You can uncomment relevant lines in this file toomodify it as shown below (use only hello specific model of hello device you are using):</span></span>
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a><span data-ttu-id="27e6d-221">3. lépés: Ciklikus multiplexelés többutas konfigurálása</span><span class="sxs-lookup"><span data-stu-id="27e6d-221">Step 3: Configure round-robin multipathing</span></span>
<span data-ttu-id="27e6d-222">A terheléselosztási algoritmust minden hello elérhető multipaths toohello aktív vezérlő egy elosztott terhelésű, a ciklikus multiplexelési módon használja.</span><span class="sxs-lookup"><span data-stu-id="27e6d-222">This load-balancing algorithm uses all hello available multipaths toohello active controller in a balanced, round-robin fashion.</span></span>

1. <span data-ttu-id="27e6d-223">Hello szerkesztése `/etc/multipath.conf` fájlt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-223">Edit hello `/etc/multipath.conf` file.</span></span> <span data-ttu-id="27e6d-224">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-224">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="27e6d-225">A hello `defaults` szakaszban, a set hello `path_grouping_policy` túl`multibus`.</span><span class="sxs-lookup"><span data-stu-id="27e6d-225">Under hello `defaults` section, set hello `path_grouping_policy` too`multibus`.</span></span> <span data-ttu-id="27e6d-226">Hello `path_grouping_policy` hello alapértelmezett elérési út csoportosítási házirend tooapply toounspecified multipaths határozza meg.</span><span class="sxs-lookup"><span data-stu-id="27e6d-226">hello `path_grouping_policy` specifies hello default path grouping policy tooapply toounspecified multipaths.</span></span> <span data-ttu-id="27e6d-227">hello alapértelmezett szakasz jelenik meg, mint a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="27e6d-227">hello defaults section will look as shown below.</span></span>
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> <span data-ttu-id="27e6d-228">a leggyakrabban használt értékek hello `path_grouping_policy` tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="27e6d-228">hello most common values of `path_grouping_policy` include:</span></span>
> 
> * <span data-ttu-id="27e6d-229">feladatátvételi prioritási csoportonként 1 elérési útja =</span><span class="sxs-lookup"><span data-stu-id="27e6d-229">failover = 1 path per priority group</span></span>
> * <span data-ttu-id="27e6d-230">multibus = 1 prioritás csoportban lévő összes érvényes elérési utat</span><span class="sxs-lookup"><span data-stu-id="27e6d-230">multibus = all valid paths in 1 priority group</span></span>
> 
> 

### <a name="step-4-enable-multipathing"></a><span data-ttu-id="27e6d-231">4. lépés: Engedélyezze többutas</span><span class="sxs-lookup"><span data-stu-id="27e6d-231">Step 4: Enable multipathing</span></span>
1. <span data-ttu-id="27e6d-232">Indítsa újra a hello `multipathd` démon.</span><span class="sxs-lookup"><span data-stu-id="27e6d-232">Restart hello `multipathd` daemon.</span></span> <span data-ttu-id="27e6d-233">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-233">Type:</span></span>
   
    `service multipathd restart`
2. <span data-ttu-id="27e6d-234">hello kimeneti lesz a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="27e6d-234">hello output will be as shown below:</span></span>
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a><span data-ttu-id="27e6d-235">5. lépés: Ellenőrizze, hogy többutas</span><span class="sxs-lookup"><span data-stu-id="27e6d-235">Step 5: Verify multipathing</span></span>
1. <span data-ttu-id="27e6d-236">Először győződjön meg arról, hogy az iSCSI-kapcsolatot létesíteni az alábbiak szerint hello StorSimple eszköz:</span><span class="sxs-lookup"><span data-stu-id="27e6d-236">First make sure that iSCSI connection is established with hello StorSimple device as follows:</span></span>
   
   <span data-ttu-id="27e6d-237">a.</span><span class="sxs-lookup"><span data-stu-id="27e6d-237">a.</span></span> <span data-ttu-id="27e6d-238">A StorSimple eszköz felderítése.</span><span class="sxs-lookup"><span data-stu-id="27e6d-238">Discover your StorSimple device.</span></span> <span data-ttu-id="27e6d-239">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-239">Type:</span></span>
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on hello device>:<iSCSI port on StorSimple device>
    ```
    
    <span data-ttu-id="27e6d-240">Ha IP-címet DATA0 10.126.162.25 és 3260-as port meg van nyitva, a kimenő iSCSI forgalomhoz hello StorSimple eszközön hello kimeneti van alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="27e6d-240">hello output when IP address for DATA0 is 10.126.162.25 and port 3260 is opened on hello StorSimple device for outbound iSCSI traffic is as shown below:</span></span>
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    <span data-ttu-id="27e6d-241">Másolás hello IQN-Nevének a StorSimple eszköz `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, a kimeneti megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="27e6d-241">Copy hello IQN of your StorSimple device, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, from hello preceding output.</span></span>

   <span data-ttu-id="27e6d-242">b.</span><span class="sxs-lookup"><span data-stu-id="27e6d-242">b.</span></span> <span data-ttu-id="27e6d-243">Csatlakoztassa a cél IQN toohello-eszközt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-243">Connect toohello device using target IQN.</span></span> <span data-ttu-id="27e6d-244">hello StorSimple eszköz hello iSCSI-tároló itt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-244">hello StorSimple device is hello iSCSI target here.</span></span> <span data-ttu-id="27e6d-245">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-245">Type:</span></span>

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    <span data-ttu-id="27e6d-246">hello következő példa bemutatja a célkiszolgálóhoz IQN kimeneti a `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span><span class="sxs-lookup"><span data-stu-id="27e6d-246">hello following example shows output with a target IQN of `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span></span> <span data-ttu-id="27e6d-247">hello kimeneti azt jelzi, hogy sikeresen csatlakozott-e be a toohello két iSCSI-kompatibilis hálózati adapterek az eszközön.</span><span class="sxs-lookup"><span data-stu-id="27e6d-247">hello output indicates that you have successfully connected toohello two iSCSI-enabled network interfaces on your device.</span></span>

    ```
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    <span data-ttu-id="27e6d-248">Ha csak egy állomás kapcsolat és két terveket itt látható, akkor szüksége tooenable két hello felülethez gazdagép iSCSI.</span><span class="sxs-lookup"><span data-stu-id="27e6d-248">If you see only one host interface and two paths here, then you need tooenable both hello interfaces on host for iSCSI.</span></span> <span data-ttu-id="27e6d-249">Hajtsa végre az hello [részletes utasításokat Linux dokumentációjának](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span><span class="sxs-lookup"><span data-stu-id="27e6d-249">You can follow hello [detailed instructions in Linux documentation](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span></span>

2. <span data-ttu-id="27e6d-250">A kötet az elérhetőségi toohello CentOS kiszolgáló hello StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="27e6d-250">A volume is exposed toohello CentOS server from hello StorSimple device.</span></span> <span data-ttu-id="27e6d-251">További információkért lásd: [6. lépés: kötet létrehozása](storsimple-deployment-walkthrough.md#step-6-create-a-volume) hello a StorSimple eszközt a klasszikus Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="27e6d-251">For more information, see [Step 6: Create a volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via hello Azure classic portal on your StorSimple device.</span></span>

3. <span data-ttu-id="27e6d-252">Ellenőrizze a hello elérhető elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="27e6d-252">Verify hello available paths.</span></span> <span data-ttu-id="27e6d-253">Típus:</span><span class="sxs-lookup"><span data-stu-id="27e6d-253">Type:</span></span>

      ```
      multipath –l
      ```

      <span data-ttu-id="27e6d-254">a következő példa hello két hálózati adapterrel hello kimenetét mutatja be egy StorSimple eszköz csatlakoztatott tooa egyetlen hálózati adapteren két elérhető elérési útját.</span><span class="sxs-lookup"><span data-stu-id="27e6d-254">hello following example shows hello output for two network interfaces on a StorSimple device connected tooa single host network interface with two available paths.</span></span>

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        hello following example shows hello output for two network interfaces on a StorSimple device connected tootwo host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After hello paths are configured, refer toohello specific instructions on your host operating system (Centos 6.6) toomount and format this volume.

## <a name="troubleshoot-multipathing"></a><span data-ttu-id="27e6d-255">Többutas hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="27e6d-255">Troubleshoot multipathing</span></span>
<span data-ttu-id="27e6d-256">Ez a szakasz néhány hasznos tipp nyújt, ha probléma merül fel többutas konfiguráció során.</span><span class="sxs-lookup"><span data-stu-id="27e6d-256">This section provides some helpful tips if you run into any issues during multipathing configuration.</span></span>

<span data-ttu-id="27e6d-257">Q.</span><span class="sxs-lookup"><span data-stu-id="27e6d-257">Q.</span></span> <span data-ttu-id="27e6d-258">Nem látom hello változásai `multipath.conf` fájl lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="27e6d-258">I do not see hello changes in `multipath.conf` file taking effect.</span></span>

<span data-ttu-id="27e6d-259">A.</span><span class="sxs-lookup"><span data-stu-id="27e6d-259">A.</span></span> <span data-ttu-id="27e6d-260">Ha bármely módosítások toohello végrehajtott `multipath.conf` fájl, akkor toorestart hello többutas szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="27e6d-260">If you have made any changes toohello `multipath.conf` file, you will need toorestart hello multipathing service.</span></span> <span data-ttu-id="27e6d-261">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-261">Type hello following command:</span></span>

    service multipathd restart

<span data-ttu-id="27e6d-262">Q.</span><span class="sxs-lookup"><span data-stu-id="27e6d-262">Q.</span></span> <span data-ttu-id="27e6d-263">Hello StorSimple eszközön két hálózati adapterrel és két hálózati adapterrel hello gazdagépen engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="27e6d-263">I have enabled two network interfaces on hello StorSimple device and two network interfaces on hello host.</span></span> <span data-ttu-id="27e6d-264">Hello elérhető útvonalak felsorolásához a csak két elérési útnak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="27e6d-264">When I list hello available paths, I see only two paths.</span></span> <span data-ttu-id="27e6d-265">Várt toosee négy elérhető elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="27e6d-265">I expected toosee four available paths.</span></span>

<span data-ttu-id="27e6d-266">A.</span><span class="sxs-lookup"><span data-stu-id="27e6d-266">A.</span></span> <span data-ttu-id="27e6d-267">Ellenőrizze, hogy a két hello elérési utak a hello ugyanazon az alhálózaton és irányítható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-267">Make sure that hello two paths are on hello same subnet and routable.</span></span> <span data-ttu-id="27e6d-268">Ha hello hálózati adapterek különböző VLAN-on, és nem irányítható, látni fogja csak két elérési útnak.</span><span class="sxs-lookup"><span data-stu-id="27e6d-268">If hello network interfaces are on different vLANs and not routable, you will see only two paths.</span></span> <span data-ttu-id="27e6d-269">Egyirányú tooverify Ez az toomake meg arról, hogy egy adott hálózati csatoló hello StorSimple eszközön mindkét hello állomás illesztőket érhető el.</span><span class="sxs-lookup"><span data-stu-id="27e6d-269">One way tooverify this is toomake sure that you can reach both hello host interfaces from a network interface on hello StorSimple device.</span></span> <span data-ttu-id="27e6d-270">Szüksége lesz túl[forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) , ez az ellenőrzés csak akkor lehet elvégezni a támogatási munkameneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="27e6d-270">You will need too[contact Microsoft Support](storsimple-contact-microsoft-support.md) as this verification can only be done via a support session.</span></span>

<span data-ttu-id="27e6d-271">Q.</span><span class="sxs-lookup"><span data-stu-id="27e6d-271">Q.</span></span> <span data-ttu-id="27e6d-272">Amikor a rendelkezésre álló útvonalak felsorolásához kimenetet nem látható.</span><span class="sxs-lookup"><span data-stu-id="27e6d-272">When I list available paths, I do not see any output.</span></span>

<span data-ttu-id="27e6d-273">A.</span><span class="sxs-lookup"><span data-stu-id="27e6d-273">A.</span></span> <span data-ttu-id="27e6d-274">Általában nem jelennek meg minden multipathed elérési utak javasol hello többutas démon probléma, amely a legvalószínűbb ok az, hogy van-e bármilyen probléma hello a `multipath.conf` fájlt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-274">Typically, not seeing any multipathed paths suggests a problem with hello multipathing daemon, and it’s most likely that any problem here lies in hello `multipath.conf` file.</span></span>

<span data-ttu-id="27e6d-275">Érdemes ellenőrzése, hogy ténylegesen megjelenik egyes lemezek csatlakoztassa a toohello célként, mivel nincs válasz a hello többutas listaelemek is jelentheti, nem rendelkezik olyan lemezek is lenne.</span><span class="sxs-lookup"><span data-stu-id="27e6d-275">It would also be worth checking that you can actually see some disks after connecting toohello target, as no response from hello multipath listings could also mean you don’t have any disks.</span></span>

* <span data-ttu-id="27e6d-276">A következő parancs toorescan hello SCSI-busz hello használata:</span><span class="sxs-lookup"><span data-stu-id="27e6d-276">Use hello following command toorescan hello SCSI bus:</span></span>
  
    <span data-ttu-id="27e6d-277">`$ rescan-scsi-bus.sh `(a sg3_utils csomag részeként)</span><span class="sxs-lookup"><span data-stu-id="27e6d-277">`$ rescan-scsi-bus.sh `(part of sg3_utils package)</span></span>
* <span data-ttu-id="27e6d-278">Írja be a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-278">Type hello following commands:</span></span>
  
    `$ dmesg | grep sd*`
     
     <span data-ttu-id="27e6d-279">Vagy</span><span class="sxs-lookup"><span data-stu-id="27e6d-279">Or</span></span>
  
    `$ fdisk –l`
  
    <span data-ttu-id="27e6d-280">Ezek visszaadható nemrégiben felvett lemezek adatait.</span><span class="sxs-lookup"><span data-stu-id="27e6d-280">These will return details of recently added disks.</span></span>
* <span data-ttu-id="27e6d-281">toodetermine hogy StorSimple lemez-e használni a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-281">toodetermine whether it is a StorSimple disk, use hello following commands:</span></span>
  
    `cat /sys/block/<DISK>/device/model`
  
    <span data-ttu-id="27e6d-282">Ezzel visszatér a karakterláncot, amely határozza meg, hogy a StorSimple lemez esetén.</span><span class="sxs-lookup"><span data-stu-id="27e6d-282">This will return a string, which will determine if it’s a StorSimple disk.</span></span>

<span data-ttu-id="27e6d-283">A kevésbé valószínű, de lehetséges oka is lehet elavult iscsid azonosítója (PID).</span><span class="sxs-lookup"><span data-stu-id="27e6d-283">A less likely but possible cause could also be stale iscsid pid.</span></span> <span data-ttu-id="27e6d-284">Használja ki a hello iSCSI-munkameneteket a következő parancs toolog hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-284">Use hello following command toolog off from hello iSCSI sessions:</span></span>

    iscsiadm -m node --logout -p <Target_IP>

<span data-ttu-id="27e6d-285">Ismételje meg ezt a parancsot minden hello csatlakoztatott hálózati adapterek hello iSCSI-tároló, amely a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="27e6d-285">Repeat this command for all hello connected network interfaces on hello iSCSI target, which is your StorSimple device.</span></span> <span data-ttu-id="27e6d-286">Miután az összes hello iSCSI-munkameneteket kijelentkezett hello iSCSI cél IQN tooreestablish hello iSCSI-munkamenetet használjon.</span><span class="sxs-lookup"><span data-stu-id="27e6d-286">Once you have logged off from all hello iSCSI sessions, use hello iSCSI target IQN tooreestablish hello iSCSI session.</span></span> <span data-ttu-id="27e6d-287">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-287">Type hello following command:</span></span>

    iscsiadm -m node --login -T <TARGET_IQN>


<span data-ttu-id="27e6d-288">Q.</span><span class="sxs-lookup"><span data-stu-id="27e6d-288">Q.</span></span> <span data-ttu-id="27e6d-289">Nem biztos, ha az eszköz nem szerepel az engedélyezési listán.</span><span class="sxs-lookup"><span data-stu-id="27e6d-289">I am not sure if my device is whitelisted.</span></span>

<span data-ttu-id="27e6d-290">A.</span><span class="sxs-lookup"><span data-stu-id="27e6d-290">A.</span></span> <span data-ttu-id="27e6d-291">tooverify-e az eszköz szerepel az engedélyezési listán, használja a következő hibaelhárítási interaktív parancs hello:</span><span class="sxs-lookup"><span data-stu-id="27e6d-291">tooverify whether your device is whitelisted, use hello following troubleshooting interactive command:</span></span>

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


<span data-ttu-id="27e6d-292">További információ: túl[hibaelhárítási többutas interaktív parancsot használja](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span><span class="sxs-lookup"><span data-stu-id="27e6d-292">For more information, go too[use troubleshooting interactive command for multipathing](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span></span>

## <a name="list-of-useful-commands"></a><span data-ttu-id="27e6d-293">Hasznos parancsok listája</span><span class="sxs-lookup"><span data-stu-id="27e6d-293">List of useful commands</span></span>
| <span data-ttu-id="27e6d-294">Típus</span><span class="sxs-lookup"><span data-stu-id="27e6d-294">Type</span></span> | <span data-ttu-id="27e6d-295">Parancs</span><span class="sxs-lookup"><span data-stu-id="27e6d-295">Command</span></span> | <span data-ttu-id="27e6d-296">Leírás</span><span class="sxs-lookup"><span data-stu-id="27e6d-296">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="27e6d-297">**iSCSI**</span><span class="sxs-lookup"><span data-stu-id="27e6d-297">**iSCSI**</span></span> |`service iscsid start` |<span data-ttu-id="27e6d-298">ISCSI szolgáltatás elindítása</span><span class="sxs-lookup"><span data-stu-id="27e6d-298">Start iSCSI service</span></span> |
| &nbsp; |`service iscsid stop` |<span data-ttu-id="27e6d-299">ISCSI szolgáltatás leállítása</span><span class="sxs-lookup"><span data-stu-id="27e6d-299">Stop iSCSI service</span></span> |
| &nbsp; |`service iscsid restart` |<span data-ttu-id="27e6d-300">ISCSI szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="27e6d-300">Restart iSCSI service</span></span> |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |<span data-ttu-id="27e6d-301">A megadott hello elérhető tárolók felderítése cím</span><span class="sxs-lookup"><span data-stu-id="27e6d-301">Discover available targets on hello specified address</span></span> |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |<span data-ttu-id="27e6d-302">Jelentkezzen be az iSCSI-tároló toohello</span><span class="sxs-lookup"><span data-stu-id="27e6d-302">Log in toohello iSCSI target</span></span> |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |<span data-ttu-id="27e6d-303">Jelentkezzen ki a hello iSCSI-tároló</span><span class="sxs-lookup"><span data-stu-id="27e6d-303">Log out from hello iSCSI target</span></span> |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |<span data-ttu-id="27e6d-304">Nyomtatási iSCSI-kezdeményező neve</span><span class="sxs-lookup"><span data-stu-id="27e6d-304">Print iSCSI initiator name</span></span> |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |<span data-ttu-id="27e6d-305">Hello iSCSI-munkamenetet, és hello gazdagépen felderített kötet hello állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="27e6d-305">Check hello state of hello iSCSI session and volume discovered on hello host</span></span> |
| &nbsp; |`iscsi –m session` |<span data-ttu-id="27e6d-306">Megjeleníti az összes hello iSCSI-munkameneteket hello állomás és hello StorSimple eszköz között</span><span class="sxs-lookup"><span data-stu-id="27e6d-306">Shows all hello iSCSI sessions established between hello host and hello StorSimple device</span></span> |
|  | | |
| <span data-ttu-id="27e6d-307">**Többutas**</span><span class="sxs-lookup"><span data-stu-id="27e6d-307">**Multipathing**</span></span> |`service multipathd start` |<span data-ttu-id="27e6d-308">Indítsa el a többutas démont</span><span class="sxs-lookup"><span data-stu-id="27e6d-308">Start multipath daemon</span></span> |
| &nbsp; |`service multipathd stop` |<span data-ttu-id="27e6d-309">A többutas démon leállítása</span><span class="sxs-lookup"><span data-stu-id="27e6d-309">Stop multipath daemon</span></span> |
| &nbsp; |`service multipathd restart` |<span data-ttu-id="27e6d-310">Indítsa újra a többutas démon</span><span class="sxs-lookup"><span data-stu-id="27e6d-310">Restart multipath daemon</span></span> |
| &nbsp; |`chkconfig multipathd on` </br> <span data-ttu-id="27e6d-311">VAGY</span><span class="sxs-lookup"><span data-stu-id="27e6d-311">OR</span></span> </br> `mpathconf –with_chkconfig y` |<span data-ttu-id="27e6d-312">Engedélyezze a többutas démon toostart rendszerindítás</span><span class="sxs-lookup"><span data-stu-id="27e6d-312">Enable multipath daemon toostart at boot time</span></span> |
| &nbsp; |`multipathd –k` |<span data-ttu-id="27e6d-313">Indítsa el az hello interaktív konzolját a hibaelhárításhoz</span><span class="sxs-lookup"><span data-stu-id="27e6d-313">Start hello interactive console for troubleshooting</span></span> |
| &nbsp; |`multipath –l` |<span data-ttu-id="27e6d-314">Lista többutas kapcsolatok és eszközök</span><span class="sxs-lookup"><span data-stu-id="27e6d-314">List multipath connections and devices</span></span> |
| &nbsp; |`mpathconf --enable` |<span data-ttu-id="27e6d-315">A minta mulitpath.conf fájl létrehozása`/etc/mulitpath.conf`</span><span class="sxs-lookup"><span data-stu-id="27e6d-315">Create a sample mulitpath.conf file in `/etc/mulitpath.conf`</span></span> |
|  | | |

## <a name="next-steps"></a><span data-ttu-id="27e6d-316">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27e6d-316">Next steps</span></span>
<span data-ttu-id="27e6d-317">Konfigurálja az MPIO Linux-gazdagépre, mert a következő CentoS 6.6 dokumentumok toorefer toohello is szükség lehet:</span><span class="sxs-lookup"><span data-stu-id="27e6d-317">As you are configuring MPIO on Linux host, you may also need toorefer toohello following CentoS 6.6 documents:</span></span>

* [<span data-ttu-id="27e6d-318">A CentOS MPIO beállítása</span><span class="sxs-lookup"><span data-stu-id="27e6d-318">Setting up MPIO on CentOS</span></span>](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [<span data-ttu-id="27e6d-319">Linux-képzési útmutató</span><span class="sxs-lookup"><span data-stu-id="27e6d-319">Linux Training Guide</span></span>](http://linux-training.be/files/books/LinuxAdm.pdf)

