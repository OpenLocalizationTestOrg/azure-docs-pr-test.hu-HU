---
title: "aaaAzure felhő rendszerhéj (előzetes verzió) – áttekintés |} Microsoft Docs"
description: "Hello Azure Cloud rendszerhéj áttekintése."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 45c6c85b167a90947a333f44a9186e2c01b4fa7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a><span data-ttu-id="f8a88-103">Azure-felhőbe rendszerhéj (előzetes verzió) áttekintése</span><span class="sxs-lookup"><span data-stu-id="f8a88-103">Overview of Azure Cloud Shell (Preview)</span></span>
<span data-ttu-id="f8a88-104">Azure Cloud rendszerhéjjal egy interaktív, a böngésző által elérhető rendszerhéj Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f8a88-104">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span>

![](media/overview-pic.png)

## <a name="features"></a><span data-ttu-id="f8a88-105">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f8a88-105">Features</span></span>
### <a name="browser-based-shell-experience"></a><span data-ttu-id="f8a88-106">Böngészőalapú felület élmény</span><span class="sxs-lookup"><span data-stu-id="f8a88-106">Browser-based shell experience</span></span>
<span data-ttu-id="f8a88-107">A felhő rendszerhéj lehetővé teszi, hogy hozzáférési tooa böngészőalapú parancssori felületén beépített szem előtt az Azure felügyeleti feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="f8a88-107">Cloud Shell enables access tooa browser-based command-line experience built with Azure management tasks in mind.</span></span> <span data-ttu-id="f8a88-108">Éljen az olyan felhő rendszerhéj toowork untethered oly módon, csak hello felhő biztosíthat a helyi gépről.</span><span class="sxs-lookup"><span data-stu-id="f8a88-108">Leverage Cloud Shell toowork untethered from a local machine in a way only hello cloud can provide.</span></span>

### <a name="pre-configured-azure-workstation"></a><span data-ttu-id="f8a88-109">Előre konfigurált Azure munkaállomás</span><span class="sxs-lookup"><span data-stu-id="f8a88-109">Pre-configured Azure workstation</span></span>
<span data-ttu-id="f8a88-110">Felhő rendszerhéj előre előre telepített népszerű parancssori eszközökkel, és a nyelv támogatja, így gyorsabban működnek.</span><span class="sxs-lookup"><span data-stu-id="f8a88-110">Cloud Shell comes pre-installed with popular command-line tools and language support so you can work faster.</span></span>

[<span data-ttu-id="f8a88-111">Hello teljes tooling listájának megtekintése Azure Cloud rendszerhéj itt.</span><span class="sxs-lookup"><span data-stu-id="f8a88-111">View hello full tooling list for Azure Cloud Shell here.</span></span>](features.md#tools)

### <a name="automatic-authentication"></a><span data-ttu-id="f8a88-112">Automatikus hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="f8a88-112">Automatic authentication</span></span>
<span data-ttu-id="f8a88-113">Felhő rendszerhéj biztonságosan hitelesíti automatikusan minden munkameneten keresztül hello Azure CLI 2.0 azonnali hozzáférési tooyour erőforrások.</span><span class="sxs-lookup"><span data-stu-id="f8a88-113">Cloud Shell securely authenticates automatically on each session for instant access tooyour resources through hello Azure CLI 2.0.</span></span>

### <a name="connect-your-azure-file-storage"></a><span data-ttu-id="f8a88-114">Csatlakozás az Azure File storage</span><span class="sxs-lookup"><span data-stu-id="f8a88-114">Connect your Azure File storage</span></span>
<span data-ttu-id="f8a88-115">Felhő rendszerhéj gépek ideiglenes, és emiatt igényel az egy fájl Azure fájlmegosztás toobe csatlakoztatva, mint a `clouddrive` toopersist $Home címtárat.</span><span class="sxs-lookup"><span data-stu-id="f8a88-115">Cloud Shell machines are temporary and as a result require an Azure file share toobe mounted as `clouddrive` toopersist your $Home directory.</span></span>
<span data-ttu-id="f8a88-116">Első indítsa el a felhő rendszerhéj kérni fogja a toocreate egy erőforráscsoport, a tárfiók, és fájlmegosztás az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="f8a88-116">On first launch Cloud Shell prompts toocreate a resource group, storage account, and file share on your behalf.</span></span> <span data-ttu-id="f8a88-117">Ez egy egyszeri lépés, és lesz automatikusan hozzárendelve minden munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="f8a88-117">This is a one-time step and will be automatically attached for all sessions.</span></span> 

#### <a name="create-new-storage"></a><span data-ttu-id="f8a88-118">Új tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8a88-118">Create new storage</span></span>
![](media/basic-storage.png)

<span data-ttu-id="f8a88-119">A helyileg redundáns tárolás (LRS) fiók az Azure fájlmegosztások alapértelmezett 5 GB-os lemezképet tartalmazó az Ön nevében hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="f8a88-119">A locally-redundant storage (LRS) account can be created on your behalf with an Azure file share containing a default 5-GB disk image.</span></span> <span data-ttu-id="f8a88-120">hello fájlmegosztás csatlakoztatja `clouddrive` fájl megosztása hello lemezképet való együttműködéshez használt toosync alatt, és a $Home címtárban maradnak.</span><span class="sxs-lookup"><span data-stu-id="f8a88-120">hello file share mounts as `clouddrive` for file share interaction with hello disk image being used toosync and persist your $Home directory.</span></span> <span data-ttu-id="f8a88-121">Rendszeres tárolási költségek vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="f8a88-121">Regular storage costs apply.</span></span>

<span data-ttu-id="f8a88-122">Három erőforrások hozza létre az Ön nevében:</span><span class="sxs-lookup"><span data-stu-id="f8a88-122">Three resources will be created on your behalf:</span></span>
1. <span data-ttu-id="f8a88-123">Az erőforráscsoport neve:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="f8a88-123">Resource Group named: `cloud-shell-storage-<region>`</span></span>
2. <span data-ttu-id="f8a88-124">A Tárfiók neve:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="f8a88-124">Storage Account named: `cs<uniqueGuid>`</span></span>
3. <span data-ttu-id="f8a88-125">A fájlmegosztás neve:`cs-<user>-<domain>-com-uniqueGuid`</span><span class="sxs-lookup"><span data-stu-id="f8a88-125">File Share named: `cs-<user>-<domain>-com-uniqueGuid`</span></span>

> [!Note]
> <span data-ttu-id="f8a88-126">Például az SSH-kulcsok a $Home könyvtárban található összes fájl megmaradnak, az a felhasználó lemezképet, a csatlakoztatott fájl megosztáson található.</span><span class="sxs-lookup"><span data-stu-id="f8a88-126">All files in your $Home directory such as SSH keys are persisted in your user disk image stored in your mounted file share.</span></span> <span data-ttu-id="f8a88-127">Alkalmazza az ajánlott eljárásokat, ha a fájlok mentése a $Home címtár és a csatlakoztatott fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="f8a88-127">Apply best practices when saving files in your $Home directory and mounted file share.</span></span>

#### <a name="use-existing-resources"></a><span data-ttu-id="f8a88-128">Létező erőforrásokból</span><span class="sxs-lookup"><span data-stu-id="f8a88-128">Use existing resources</span></span>
![](media/advanced-storage.png)

<span data-ttu-id="f8a88-129">Speciális beállítás így Ön tooassociate meglévő erőforrások tooCloud rendszerhéj is biztosítja.</span><span class="sxs-lookup"><span data-stu-id="f8a88-129">An advanced option is also provided allowing you tooassociate existing resources tooCloud Shell.</span></span> <span data-ttu-id="f8a88-130">Hello tárolási telepítő kérdéshez megjelenésekor kattintson a "Speciális megjelenítése beállítások" tooselect további beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f8a88-130">When presented with hello storage setup prompt, click "Show advanced settings" tooselect additional options.</span></span> <span data-ttu-id="f8a88-131">A hozzárendelt felhő rendszerhéj pedig a helyi/globálisan-redundancia tárfiókok legördülő listák Megnyílásának szűrve.</span><span class="sxs-lookup"><span data-stu-id="f8a88-131">Dropdowns are filtered for your assigned Cloud Shell region and locally/globally-redundant storage accounts.</span></span>

<span data-ttu-id="f8a88-132">[További információk a felhő rendszerhéj tároló fájlmegosztások frissítése, és a feltöltése/letöltése fájlokat.] (megőrzése-rendszerhéj-storage.md)</span><span class="sxs-lookup"><span data-stu-id="f8a88-132">[Learn about Cloud Shell storage, updating file shares, and uploading/downloading files.] (persisting-shell-storage.md)</span></span>

## <a name="concepts"></a><span data-ttu-id="f8a88-133">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="f8a88-133">Concepts</span></span>
* <span data-ttu-id="f8a88-134">Felhő rendszerhéj egy munkamenetenként, egyes felhasználók számára a megadott ideiglenes gépen fut</span><span class="sxs-lookup"><span data-stu-id="f8a88-134">Cloud Shell runs on a temporary machine provided on a per-session, per-user basis</span></span>
* <span data-ttu-id="f8a88-135">Interaktív tevékenység nélkül 20 perc elteltével időtúllépést felhő rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="f8a88-135">Cloud Shell times out after 20 minutes without interactive activity</span></span>
* <span data-ttu-id="f8a88-136">Felhő rendszerhéj csak és csatolt fájlmegosztás érhető el</span><span class="sxs-lookup"><span data-stu-id="f8a88-136">Cloud Shell can only be accessed with a file share attached</span></span>
* <span data-ttu-id="f8a88-137">Felhő rendszerhéj hozzá van rendelve egy gép felhasználónként</span><span class="sxs-lookup"><span data-stu-id="f8a88-137">Cloud Shell is assigned one machine per user account</span></span>
* <span data-ttu-id="f8a88-138">Linux felhasználói engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="f8a88-138">Permissions are set as a regular Linux user</span></span>

[<span data-ttu-id="f8a88-139">További információ az összes felhő Shell szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f8a88-139">Learn more about all Cloud Shell features.</span></span>](features.md)

## <a name="examples"></a><span data-ttu-id="f8a88-140">Példák</span><span class="sxs-lookup"><span data-stu-id="f8a88-140">Examples</span></span>
* <span data-ttu-id="f8a88-141">Hozzon létre vagy parancsfájlok tooautomate Azure felügyeleti szerkesztése</span><span class="sxs-lookup"><span data-stu-id="f8a88-141">Create or edit scripts tooautomate Azure management</span></span>
* <span data-ttu-id="f8a88-142">Egyidejűleg az Azure-portál és az Azure CLI 2.0-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="f8a88-142">Simultaneously manage resources via Azure portal and Azure CLI 2.0</span></span>
* <span data-ttu-id="f8a88-143">Az Azure CLI 2.0 test-Drive</span><span class="sxs-lookup"><span data-stu-id="f8a88-143">Test-drive Azure CLI 2.0</span></span>

[<span data-ttu-id="f8a88-144">Próbálja ki hello felhő rendszerhéj gyors üzembe helyezés, a példákat.</span><span class="sxs-lookup"><span data-stu-id="f8a88-144">Try out all these examples at hello Cloud Shell quickstart.</span></span>](quickstart.md)

## <a name="pricing"></a><span data-ttu-id="f8a88-145">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="f8a88-145">Pricing</span></span>
<span data-ttu-id="f8a88-146">Felhő rendszerhéj futtató hello gépen szabad, olyan előfeltételt egy csatlakoztatott Azure fájl a könyvtárban van toopersist a $Home.</span><span class="sxs-lookup"><span data-stu-id="f8a88-146">hello machine hosting Cloud Shell is free, with a pre-requisite of a mounted Azure file share toopersist your $Home directory.</span></span> <span data-ttu-id="f8a88-147">Rendszeres tárolási költségek vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="f8a88-147">Regular storage costs apply.</span></span>

## <a name="supported-browsers"></a><span data-ttu-id="f8a88-148">Támogatott böngészők</span><span class="sxs-lookup"><span data-stu-id="f8a88-148">Supported browsers</span></span>
<span data-ttu-id="f8a88-149">Felhő rendszerhéj Chrome, biztonsági és Safari ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f8a88-149">Cloud Shell is recommended for Chrome, Edge, and Safari.</span></span> <span data-ttu-id="f8a88-150">Felhő rendszerhéj Chrome, Firefox, Safari, Internet Explorer vagy Edge esetén támogatott, amíg a felhő rendszerhéj tulajdonos toospecific böngésző beállításait.</span><span class="sxs-lookup"><span data-stu-id="f8a88-150">While Cloud Shell is supported for Chrome, Firefox, Safari, IE, and Edge, Cloud Shell is subject toospecific browser settings.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f8a88-151">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="f8a88-151">Troubleshooting</span></span>
1. <span data-ttu-id="f8a88-152">Egy Azure Active Directory-előfizetés használata esetén nem lehet létrehozni a tárolási esedékes tooError: 400 DisallowedOperation.</span><span class="sxs-lookup"><span data-stu-id="f8a88-152">When using an Azure Active Directory subscription, I cannot create storage due tooError: 400 DisallowedOperation.</span></span> <span data-ttu-id="f8a88-153">tooresolve, használja a képes a tárolási erőforrások létrehozása Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="f8a88-153">tooresolve this, please use an Azure subscription capable of creating storage resources.</span></span> <span data-ttu-id="f8a88-154">AD-előfizetés nem képes toocreate Azure-erőforrások vannak.</span><span class="sxs-lookup"><span data-stu-id="f8a88-154">AD subscriptions are not able toocreate Azure resources.</span></span>

<span data-ttu-id="f8a88-155">Adott ismert korlátozásai, látogasson el [felhő rendszerhéj korlátai](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="f8a88-155">For specific known limitations, visit [limitations of Cloud Shell](limitations.md).</span></span>
