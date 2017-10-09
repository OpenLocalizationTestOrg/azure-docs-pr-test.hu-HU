---
title: "Hat frissítés infrastruktúra (RHUI) aaaRed |} Microsoft Docs"
description: "Ismerje meg a Red Hat frissítés infrastruktúra (RHUI) igény Red Hat Enterprise Linux-példányok a Microsoft Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: borisb
ms.openlocfilehash: cc244857104b25e4e61862c518db77e915e137ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a><span data-ttu-id="41379-103">Red Hat frissítési infrastruktúra (RHUI) igény Red Hat Enterprise Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="41379-103">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>
<span data-ttu-id="41379-104">Hello igény Red Hat Enterprise Linux (RHEL) lemezképeket Azure piactéren alapján létrehozott virtuális gépeken regisztrált tooaccess hello Red Hat frissítés infrastruktúra (RHUI) az Azure-ban telepítve.</span><span class="sxs-lookup"><span data-stu-id="41379-104">Virtual machines created from hello on-demand Red Hat Enterprise Linux (RHEL) images available in Azure Marketplace are registered tooaccess hello Red Hat Update Infrastructure (RHUI) deployed in Azure.</span></span>  <span data-ttu-id="41379-105">hello igény szerinti RHEL célpéldánynál hozzáférés tooa regionális yum tárház és képes tooreceive növekményes frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="41379-105">hello on-demand RHEL instances have access tooa regional yum repository and able tooreceive incremental updates.</span></span>

<span data-ttu-id="41379-106">hello yum tárház listájában RHUI kezeli, amely konfigurálva van az RHEL példányának kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="41379-106">hello yum repository list, which is managed by RHUI, is configured in your RHEL instance during provisioning.</span></span> <span data-ttu-id="41379-107">Nem kell toodo további konfiguráció - futtatása `yum update` után RHEL példány készen tooget hello legújabb frissítéseit.</span><span class="sxs-lookup"><span data-stu-id="41379-107">You don't need toodo any additional configuration - run `yum update` after your RHEL instance is ready tooget hello latest updates.</span></span>

> [!NOTE]
> <span data-ttu-id="41379-108">2016 szeptemberétől helyeztünk üzembe egy frissített Azure RHUI és 2017. január azt elindított hello fázisokra bontva történő leállításának régebbi Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="41379-108">In September 2016 we deployed an updated Azure RHUI and in January 2017 we started phased shutdown of hello older Azure RHUI.</span></span> <span data-ttu-id="41379-109">Ha használta hello RHEL képet (vagy a pillanatképek) 2016 szeptemberétől vagy későbbi - valószínűleg intézkedés nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="41379-109">If you have been using hello RHEL images (or their snapshots) from September 2016 or later - likely no action is required.</span></span> <span data-ttu-id="41379-110">Ha azonban még a korábbi pillanatképek/virtuális gépek, a konfigurációjukat kell toobe folyamatos hozzáférést toohello Azure RHUI frissítve.</span><span class="sxs-lookup"><span data-stu-id="41379-110">If, however, you have older snapshots/VMs, their configuration needs toobe updated for uninterrupted access toohello Azure RHUI.</span></span>
> 

## <a name="rhui-azure-infrastructure-update"></a><span data-ttu-id="41379-111">RHUI Azure-infrastruktúra frissítése</span><span class="sxs-lookup"><span data-stu-id="41379-111">RHUI Azure Infrastructure Update</span></span>
<span data-ttu-id="41379-112">2016 szeptemberétől Azure tartozik egy új Red Hat frissítés infrastruktúra (RHUI) kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="41379-112">As of September 2016, Azure has a new set of Red Hat Update Infrastructure (RHUI) servers.</span></span> <span data-ttu-id="41379-113">Ezekre a kiszolgálókra telepített [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) , hogy egy végpontot (rhui-1.microsoft.com) régió függetlenül minden virtuális gép által használható.</span><span class="sxs-lookup"><span data-stu-id="41379-113">These servers are deployed with [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) so that a single endpoint (rhui-1.microsoft.com) can be used by any VM regardless of region.</span></span> <span data-ttu-id="41379-114">hello hello Azure piactér (dátummal 2016 szeptemberétől vagy újabb verzió) pont toohello új Azure RHUI kiszolgálók új RHEL használatalapú fizetés (PAYG) lemezképeit, és minden további műveletet nem igénylő.</span><span class="sxs-lookup"><span data-stu-id="41379-114">hello new RHEL Pay-As-You-Go (PAYG) images in hello Azure Marketplace (versions dated September 2016 or later) point toohello new Azure RHUI servers and do not require any additional action.</span></span>

### <a name="determine-if-action-is-required"></a><span data-ttu-id="41379-115">Megállapítja, hogy szükség-e a művelet</span><span class="sxs-lookup"><span data-stu-id="41379-115">Determine if action is required</span></span>
<span data-ttu-id="41379-116">Ha a csatlakozás az Azure RHEL PAYG virtuális gép tooAzure RHUI problémákat tapasztal, kövesse az alábbi lépéseket</span><span class="sxs-lookup"><span data-stu-id="41379-116">If you are experiencing problems connecting tooAzure RHUI from your Azure RHEL PAYG VM, follow these steps</span></span>
1. <span data-ttu-id="41379-117">Vizsgálja meg a Virtuálisgép-konfiguráció Azure RHUI végpont</span><span class="sxs-lookup"><span data-stu-id="41379-117">Inspect VM configuration for Azure RHUI endpoint</span></span>

    <span data-ttu-id="41379-118">Ellenőrizze, hogy `/etc/yum.repos.d/rh-cloud.repo` fájl túl hivatkozása`rhui-[1-3].microsoft.com` a baseurl a `[rhui-microsoft-azure-rhel*]` hello fájl szakaszában.</span><span class="sxs-lookup"><span data-stu-id="41379-118">Check if `/etc/yum.repos.d/rh-cloud.repo` file contains reference too`rhui-[1-3].microsoft.com` in baseurl of `[rhui-microsoft-azure-rhel*]` section of hello file.</span></span> <span data-ttu-id="41379-119">Ha az - használ hello az új Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="41379-119">If it is - you are using hello new Azure RHUI.</span></span>

    <span data-ttu-id="41379-120">Ha azt tooa helyre mutató a következő mintát hello `mirrorlist.*cds[1-4].cloudapp.net` -hello konfigurációs frissítés szükség.</span><span class="sxs-lookup"><span data-stu-id="41379-120">If it pointing tooa location with hello following pattern `mirrorlist.*cds[1-4].cloudapp.net` - hello configuration update is required.</span></span>

    <span data-ttu-id="41379-121">Ha hello új konfigurációt használ, és továbbra sem sikerül kapcsolódni tooAzure RHUI - fájl a Microsoft vagy a Red Hat támogatási esetet.</span><span class="sxs-lookup"><span data-stu-id="41379-121">If you are using hello new configuration and still cannot connect tooAzure RHUI - file a support case with Microsoft or Red Hat.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41379-122">Hozzáférés tooAzure által szolgáltatott RHUI korlátozott toohello virtuális gépeken belül [Microsoft Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="41379-122">Access tooAzure-hosted RHUI is limited toohello VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
    > 

2. <span data-ttu-id="41379-123">Ha hello régi Azure RHUI marad. Ehhez ellenőrizze, és azt szeretné, hogy tooautomatically frissítés hello konfigurációs, hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="41379-123">If hello old Azure RHUI is still available when you do this check and you would like tooautomatically update hello configuration, execute hello following command:</span></span>

    <span data-ttu-id="41379-124">`sudo yum update RHEL6`vagy `sudo yum update RHEL7` hello RHEL termékcsalád verziójától függően.</span><span class="sxs-lookup"><span data-stu-id="41379-124">`sudo yum update RHEL6` or `sudo yum update RHEL7` depending on hello RHEL family version.</span></span>

3. <span data-ttu-id="41379-125">Ha már nem kapcsolható toohello régi Azure RHUI, hajtsa végre hello manuális lépések hello következő szakaszban leírt.</span><span class="sxs-lookup"><span data-stu-id="41379-125">If you can no longer connect toohello old Azure RHUI, follow hello manual steps described in hello next section.</span></span>

4. <span data-ttu-id="41379-126">Ellenőrizze, hogy tooupdate hello konfigurációs hello forrás lemezkép vagy pillanatkép hatással a virtuális gép át lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="41379-126">Make sure tooupdate hello configuration on hello source image/snapshot affected VM was provisioned from.</span></span>

### <a name="phased-shutdown-of-hello-old-azure-rhui"></a><span data-ttu-id="41379-127">Szakaszos leállása hello régi Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="41379-127">Phased shutdown of hello old Azure RHUI</span></span>
<span data-ttu-id="41379-128">A hello hello leállítás során régi Azure RHUI azt korlátozása elérhető tooit az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="41379-128">During hello shutdown of hello old Azure RHUI we restrict access tooit as follows:</span></span>

1. <span data-ttu-id="41379-129">Az IP-címek tooit már csatlakozó access (ACL) tooset további korlátozásához.</span><span class="sxs-lookup"><span data-stu-id="41379-129">Further restrict access (ACL) tooset of IP addresses that are already connecting tooit.</span></span> <span data-ttu-id="41379-130">Lehetséges mellékhatása: Ha továbbra is használja a régi Azure RHUI hello – az új virtuális gépeket nem lehet képes tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="41379-130">Possible side-effects: if you continue using hello old Azure RHUI - your new VMs may not be able tooconnect tooit.</span></span> <span data-ttu-id="41379-131">RHEL rendelkező virtuális gépek dinamikus IP-címek, amelyek halad át leállítás/felszabadítani/start feladatütemezési jelenhet meg új VI, és ezért is sikerült meghiúsul az tooconnect toohello régi Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="41379-131">RHEL VMs with dynamic IPs that go through shutdown/deallocate/start sequence may receive new IP and hence also could start failing tooconnect toohello old Azure RHUI</span></span>

2. <span data-ttu-id="41379-132">Állítsa le a tükör tartalomkézbesítési kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="41379-132">Shutdown of mirror content delivery servers.</span></span> <span data-ttu-id="41379-133">Lehetséges mellékhatása:, azt több CDSes leállítása jelenhetnek már `yum update` karbantartási idő, amíg hello további időtúllépések terjesztésipont-amikor már nem kapcsolható toohello régi Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="41379-133">Possible side-effects: as we shut down more CDSes you may see longer `yum update` servicing time, more timeouts up until hello point when you can no longer connect toohello old Azure RHUI.</span></span>

### <a name="hello-ips-for-hello-new-rhui-content-delivery-servers-are"></a><span data-ttu-id="41379-134">IP-címek hello hello új RHUI tartalomkézbesítési kiszolgálók vannak</span><span class="sxs-lookup"><span data-stu-id="41379-134">hello IPs for hello new RHUI content delivery servers are</span></span>
<span data-ttu-id="41379-135">Hálózati konfiguráció toofurther használata RHEL PAYG virtuális gépek elérésének korlátozása, győződjön meg arról, hogy a következő IP-címek hello használata engedélyezett `yum update` toowork a hello környezettől függően.</span><span class="sxs-lookup"><span data-stu-id="41379-135">If you are using network configuration toofurther restrict access from RHEL PAYG VMs, make sure hello following IPs are allowed for `yum update` toowork depending on hello environment you are in.</span></span> 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

### <a name="manual-update-procedure-toouse-hello-new-azure-rhui-servers"></a><span data-ttu-id="41379-136">Kézi frissítés eljárás toouse hello új Azure RHUI kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="41379-136">Manual update procedure toouse hello new Azure RHUI servers</span></span>
<span data-ttu-id="41379-137">(A curl) keresztül letöltési hello nyilvános kulcs aláírása</span><span class="sxs-lookup"><span data-stu-id="41379-137">Download (via curl) hello public key signature</span></span>

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

<span data-ttu-id="41379-138">Ellenőrizze a letöltött hello kulcsot</span><span class="sxs-lookup"><span data-stu-id="41379-138">Verify hello downloaded key</span></span>

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="41379-139">Ellenőrizze a hello kimeneti, győződjön meg arról `keyid` és `user ID packet`:</span><span class="sxs-lookup"><span data-stu-id="41379-139">Check hello output, verify `keyid` and `user ID packet`:</span></span>

```bash
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

<span data-ttu-id="41379-140">Hello nyilvános kulcs telepítése</span><span class="sxs-lookup"><span data-stu-id="41379-140">Install hello public key</span></span>

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="41379-141">Töltse le, ellenőrizze és RPM ügyfél telepítése</span><span class="sxs-lookup"><span data-stu-id="41379-141">Download, Verify, and Install Client RPM</span></span>

<span data-ttu-id="41379-142">Letöltés: Az RHEL 6</span><span class="sxs-lookup"><span data-stu-id="41379-142">Download: For RHEL 6</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

<span data-ttu-id="41379-143">Az RHEL 7</span><span class="sxs-lookup"><span data-stu-id="41379-143">For RHEL 7</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

<span data-ttu-id="41379-144">Ellenőrizze a:</span><span class="sxs-lookup"><span data-stu-id="41379-144">Verify:</span></span>

```bash
rpm -Kv azureclient.rpm
```

<span data-ttu-id="41379-145">A kimeneti ellenőrizze, hogy az aláírás hello csomag az OK gombra</span><span class="sxs-lookup"><span data-stu-id="41379-145">Check in output that signature of hello package is OK</span></span>

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

<span data-ttu-id="41379-146">Hello RPM telepítése</span><span class="sxs-lookup"><span data-stu-id="41379-146">Install hello RPM</span></span>

```bash
sudo rpm -U azureclient.rpm
```

<span data-ttu-id="41379-147">Befejezett győződjön meg arról, hogy elérhető Azure RHUI űrlap hello VM</span><span class="sxs-lookup"><span data-stu-id="41379-147">Upon completion, verify that you can access Azure RHUI form hello VM</span></span>

### <a name="all-in-one-script-for-automating-hello-preceding-task"></a><span data-ttu-id="41379-148">A feladat megelőző hello automatizálásához mindent egy parancsfájl</span><span class="sxs-lookup"><span data-stu-id="41379-148">All-in-one script for automating hello preceding task</span></span>
<span data-ttu-id="41379-149">Az érintett virtuális gépek toohello új Azure RHUI kiszolgálók frissítésének szükséges tooautomate hello feladatok a parancsfájl következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="41379-149">Use hello following script as needed tooautomate hello task of updating affected VMs toohello new Azure RHUI servers.</span></span>

```sh
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a><span data-ttu-id="41379-150">RHUI áttekintése</span><span class="sxs-lookup"><span data-stu-id="41379-150">RHUI overview</span></span>
<span data-ttu-id="41379-151">[Red Hat frissítési infrastruktúra](https://access.redhat.com/products/red-hat-update-infrastructure) egy kiválóan méretezhető megoldást toomanage yum tárház tartalmának Red Hat hitelesített szolgáltatók által üzemeltetett Red Hat Enterprise Linux-felhő példányok kínál.</span><span class="sxs-lookup"><span data-stu-id="41379-151">[Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) offers a highly scalable solution toomanage yum repository content for Red Hat Enterprise Linux cloud instances that are hosted by Red Hat-certified cloud providers.</span></span> <span data-ttu-id="41379-152">Hello fölérendelt cellulóz projekt alapján, RHUI lehetővé teszi a szolgáltatók toolocally tükrözött tárház Red Hat által szolgáltatott tartalom hozzon létre egyéni adattárak a saját tartalom, és ezek adattárak elérhető tooa nagy egy elosztott terhelésű végfelhasználók csoport a tartalom kézbesítésével rendszer.</span><span class="sxs-lookup"><span data-stu-id="41379-152">Based on hello upstream Pulp project, RHUI allows cloud providers toolocally mirror Red Hat-hosted repository content, create custom repositories with their own content, and make those repositories available tooa large group of end users through a load-balanced content delivery system.</span></span>

## <a name="regions-where-rhui-is-available"></a><span data-ttu-id="41379-153">Ha rendelkezésre áll-e RHUI régiók</span><span class="sxs-lookup"><span data-stu-id="41379-153">Regions where RHUI is available</span></span>
<span data-ttu-id="41379-154">RHUI minden régióban, ahol elérhetők az RHEL igény szerinti lemezképek érhető el.</span><span class="sxs-lookup"><span data-stu-id="41379-154">RHUI is available in all regions where RHEL on-demand images are available.</span></span> <span data-ttu-id="41379-155">Jelenleg magában foglalja a hello felsorolt összes nyilvános régiókban [Azure állapota irányítópult](https://azure.microsoft.com/status/) Azure Amerikai Egyesült államokbeli kormányzati és a Németországi Azure-régiók lap.</span><span class="sxs-lookup"><span data-stu-id="41379-155">It currently includes all public regions listed on hello [Azure status dashboard](https://azure.microsoft.com/status/) page, Azure US Government and Azure Germany regions.</span></span> <span data-ttu-id="41379-156">Az ár RHUI hozzáférés RHEL igény szerinti lemezképek kiosztott virtuális gépek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41379-156">RHUI access for VMs provisioned from RHEL on-demand images is included in their price.</span></span> <span data-ttu-id="41379-157">További területi/nemzeti felhőalapú rendelkezésre állási, azt bontsa ki az RHEL igény szerinti rendelkezésre állása, a jövőbeli hello lesznek frissítve.</span><span class="sxs-lookup"><span data-stu-id="41379-157">Additional regional/national cloud availability will be updated as we expand RHEL on-demand availability in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="41379-158">Hozzáférés tooAzure által szolgáltatott RHUI korlátozott toohello virtuális gépeken belül [Microsoft Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="41379-158">Access tooAzure-hosted RHUI is limited toohello VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
> 
> 

## <a name="get-updates-from-another-update-repository"></a><span data-ttu-id="41379-159">Egy másik tárházához frissítések beszerzése</span><span class="sxs-lookup"><span data-stu-id="41379-159">Get updates from another update repository</span></span>
<span data-ttu-id="41379-160">Ha (az Azure által üzemeltetett RHUI) helyett egy másik frissítés tárházból tooget frissítésekre van szükség, először meg kell toounregister RHUI a példányok.</span><span class="sxs-lookup"><span data-stu-id="41379-160">If you need tooget updates from a different update repository (instead of Azure-hosted RHUI), first you need toounregister your instances from RHUI.</span></span> <span data-ttu-id="41379-161">Akkor szükséges, hogy toore-nyilvántartás hello kívánt frissítési infrastruktúrával (például a Red Hat műholdas vagy a Red Hat felhasználói portál CDN) őket.</span><span class="sxs-lookup"><span data-stu-id="41379-161">Then you need toore-register them with hello desired update infrastructure (such as Red Hat Satellite or Red Hat Customer Portal CDN).</span></span> <span data-ttu-id="41379-162">Akkor lesz kell megfelelő Red Hat előfizetések ezen szolgáltatások és a regisztrációs [Red Hat felhőalapú férhetnek hozzá az Azure-ban](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="41379-162">You will need appropriate Red Hat subscriptions for these services and registration for [Red Hat Cloud Access in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span></span>

<span data-ttu-id="41379-163">toounregister RHUI és regisztrálja újra tooyour frissítési infrastruktúra kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41379-163">toounregister RHUI and reregister tooyour update infrastructure follow these steps:</span></span>

1. <span data-ttu-id="41379-164">Szerkessze a /etc/yum.repos.d/rh-cloud.repo, és módosítsa az összes `enabled=1` túl`enabled=0`.</span><span class="sxs-lookup"><span data-stu-id="41379-164">Edit /etc/yum.repos.d/rh-cloud.repo and change all `enabled=1` too`enabled=0`.</span></span> <span data-ttu-id="41379-165">Példa:</span><span class="sxs-lookup"><span data-stu-id="41379-165">For example:</span></span>
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. <span data-ttu-id="41379-166">Módosítsuk /etc/yum/pluginconf.d/rhnplugin.conf és `enabled=0` túl`enabled=1`.</span><span class="sxs-lookup"><span data-stu-id="41379-166">Edit /etc/yum/pluginconf.d/rhnplugin.conf and change `enabled=0` too`enabled=1`.</span></span>
3. <span data-ttu-id="41379-167">Hello kívánt infrastruktúrával, például a Red Hat Ügyfélportál majd regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="41379-167">Then register with hello desired infrastructure, such as Red Hat Customer Portal.</span></span> <span data-ttu-id="41379-168">Hajtsa végre a Red Hat megoldás útmutatót az [hogyan tooregister és a rendszer toohello Red Hat Ügyfélportál előfizetés](https://access.redhat.com/solutions/253273).</span><span class="sxs-lookup"><span data-stu-id="41379-168">Follow Red Hat solution guide on [how tooregister and subscribe a system toohello Red Hat Customer Portal](https://access.redhat.com/solutions/253273).</span></span>

> [!NOTE]
> <span data-ttu-id="41379-169">Hozzáférés toohello Azure által üzemeltetett RHUI hello RHEL használatalapú fizetés (PAYG) kép ár tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41379-169">Access toohello Azure-hosted RHUI is included in hello RHEL Pay-As-You-Go (PAYG) image price.</span></span> <span data-ttu-id="41379-170">Regisztrációjának törlése az Azure által üzemeltetett RHUI hello PAYG RHEL virtuális gép nem átalakítása hello virtuális gép VM kerüljön-a-saját-licenc használata (BYOL) típus.</span><span class="sxs-lookup"><span data-stu-id="41379-170">Unregistering a PAYG RHEL VM from hello Azure-hosted RHUI does not convert hello virtual machine into Bring-Your-Own-License (BYOL) type VM.</span></span> <span data-ttu-id="41379-171">Ha regisztrál hello azonos virtuális gép egy másik frissítések-forrással rendelkező, előfordulhat, hogy lehet díjfizetési dupla: először Azure RHEL szoftver díj és hello a Red Hat előfizetés(ek) még egyszer.</span><span class="sxs-lookup"><span data-stu-id="41379-171">If you register hello same VM with another source of updates you may be incurring double charges: first time for Azure RHEL software fee, and hello second time for Red Hat subscription(s).</span></span> 
> 
> <span data-ttu-id="41379-172">Következetesen toouse Azure által üzemeltetett RHUI eltérő frissítési infrastruktúra javasolt létrehozása és telepítése a saját (BYOL-type) lemezképek leírtak [létrehozása és feltöltése Red Hat-alapú virtuális gépet az Azure-](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) cikk.</span><span class="sxs-lookup"><span data-stu-id="41379-172">If you consistently need toouse an update infrastructure other than Azure-hosted RHUI consider creating and deploying your own (BYOL-type) images as described in [Create and Upload Red Hat-based virtual machine for Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article.</span></span>
> 

## <a name="next-steps"></a><span data-ttu-id="41379-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41379-173">Next steps</span></span>
<span data-ttu-id="41379-174">Red Hat Enterprise Linux virtuális gép Azure piactér – használatalapú fizetés lemezképet, és használja ki az Azure által üzemeltetett RHUI toocreate lépjen túl[Azure piactér](https://azure.microsoft.com/marketplace/partners/redhat/).</span><span class="sxs-lookup"><span data-stu-id="41379-174">toocreate a Red Hat Enterprise Linux VM from Azure Marketplace Pay-As-You-Go image and leverage Azure-hosted RHUI go too[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span></span> <span data-ttu-id="41379-175">Fogja tudni toouse `yum update` RHEL betűtípusainak további beállítások elvégzése nélkül.</span><span class="sxs-lookup"><span data-stu-id="41379-175">You will be able toouse `yum update` in your RHEL instance without any additional setup.</span></span>

