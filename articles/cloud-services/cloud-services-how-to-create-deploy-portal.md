---
title: "aaaHow toocreate és egy felhőalapú szolgáltatás üzembe helyezése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és központi telepítése egy felhőalapú szolgáltatás hello Azure-portál használatával."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: dc8b81a594f3514e662c49c9a46a33da8a551f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a><span data-ttu-id="be2c4-103">Hogyan toocreate és egy felhőalapú szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="be2c4-103">How toocreate and deploy a cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="be2c4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="be2c4-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="be2c4-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="be2c4-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
>
>

<span data-ttu-id="be2c4-106">hello Azure-portálon két lehetőséget biztosít az Ön toocreate és egy felhőalapú szolgáltatás üzembe helyezése: *Gyorslétrehozás* és *egyéni létrehozás*.</span><span class="sxs-lookup"><span data-stu-id="be2c4-106">hello Azure portal provides two ways for you toocreate and deploy a cloud service: *Quick Create* and *Custom Create*.</span></span>

<span data-ttu-id="be2c4-107">Ez a cikk azt ismerteti, hogyan toouse hello gyors létrehozás módszerrel toocreate új felhőalapú szolgáltatás, és ezután **feltöltése** tooupload és az Azure cloud service csomag telepítését.</span><span class="sxs-lookup"><span data-stu-id="be2c4-107">This article explains how toouse hello Quick Create method toocreate a new cloud service and then use **Upload** tooupload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="be2c4-108">Ha ezt a módszert használja, hello Azure-portálon teszi követelményeinek befejezése menet elérhető kényelmes hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="be2c4-108">When you use this method, hello Azure portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="be2c4-109">Ha készen áll a felhőalapú szolgáltatás létrehozásakor toodeploy, mind hello teheti ugyanannyi időt vesz igénybe, használja a egyéni létrehozás.</span><span class="sxs-lookup"><span data-stu-id="be2c4-109">If you're ready toodeploy your cloud service when you create it, you can do both at hello same time using Custom Create.</span></span>

> [!NOTE]
> <span data-ttu-id="be2c4-110">Ha azt tervezi, toopublish a felhőszolgáltatás a Visual Studio Team Services (VSTS), használja a Gyorslétrehozás, és VSTS közzététel hello Azure gyors üzembe helyezési vagy hello irányítópultról majd beállítása.</span><span class="sxs-lookup"><span data-stu-id="be2c4-110">If you plan toopublish your cloud service from Visual Studio Team Services (VSTS), use Quick Create, and then set up VSTS publishing from hello Azure Quickstart or hello dashboard.</span></span> <span data-ttu-id="be2c4-111">További információkért lásd: [folyamatos kézbesítési tooAzure használatával Visual Studio Team Services által][TFSTutorialForCloudService], vagy tekintse át a súgóban talál hello **gyors üzembe helyezés** lap.</span><span class="sxs-lookup"><span data-stu-id="be2c4-111">For more information, see [Continuous Delivery tooAzure by Using Visual Studio Team Services][TFSTutorialForCloudService], or see help for hello **Quick Start** page.</span></span>
>
>

## <a name="concepts"></a><span data-ttu-id="be2c4-112">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="be2c4-112">Concepts</span></span>
<span data-ttu-id="be2c4-113">Három kötelező toodeploy egy alkalmazás az Azure-ban felhőszolgáltatásként:</span><span class="sxs-lookup"><span data-stu-id="be2c4-113">Three components are required toodeploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="be2c4-114">**Szolgáltatásdefiníció**</span><span class="sxs-lookup"><span data-stu-id="be2c4-114">**Service Definition**</span></span>  
  <span data-ttu-id="be2c4-115">hello felhő szolgáltatásdefiníciós fájl (.csdef) hello modell, beleértve a szerepkörök hello számának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="be2c4-115">hello cloud service definition file (.csdef) defines hello service model, including hello number of roles.</span></span>
* <span data-ttu-id="be2c4-116">**Szolgáltatás konfigurációja**</span><span class="sxs-lookup"><span data-stu-id="be2c4-116">**Service Configuration**</span></span>  
  <span data-ttu-id="be2c4-117">hello felhőalapú szolgáltatás konfigurációs fájlját (.cscfg) hello felhőalapú szolgáltatás és az adott szerepkörök, beleértve a szerepkörpéldányok számát hello konfigurációs beállításait biztosítja.</span><span class="sxs-lookup"><span data-stu-id="be2c4-117">hello cloud service configuration file (.cscfg) provides configuration settings for hello cloud service and individual roles, including hello number of role instances.</span></span>
* <span data-ttu-id="be2c4-118">**Szolgáltatáscsomag**</span><span class="sxs-lookup"><span data-stu-id="be2c4-118">**Service Package**</span></span>  
  <span data-ttu-id="be2c4-119">hello szolgáltatás csomagba (.cspkg) hello alkalmazáskód és konfigurációk és hello szolgáltatásdefiníciós fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="be2c4-119">hello service package (.cspkg) contains hello application code and configurations and hello service definition file.</span></span>

<span data-ttu-id="be2c4-120">További ezekről, és hogyan toocreate csomag [Itt](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="be2c4-120">You can learn more about these and how toocreate a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="be2c4-121">Az alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="be2c4-121">Prepare your app</span></span>
<span data-ttu-id="be2c4-122">Egy felhőalapú szolgáltatás telepítése előtt létre kell hoznia hello cloud service csomagba (.cspkg) alkalmazáskódjában és egy felhőalapú szolgáltatás konfigurációs fájlját (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="be2c4-122">Before you can deploy a cloud service, you must create hello cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="be2c4-123">hello Azure SDK eszközöket biztosít a szükséges központi telepítés fájlok előkészítése.</span><span class="sxs-lookup"><span data-stu-id="be2c4-123">hello Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="be2c4-124">Hello SDK hello telepítheti [Azure letölti](https://azure.microsoft.com/downloads/) lap, ahol inkább toodevelop hello nyelvi az alkalmazás kódjában.</span><span class="sxs-lookup"><span data-stu-id="be2c4-124">You can install hello SDK from hello [Azure Downloads](https://azure.microsoft.com/downloads/) page, in hello language in which you prefer toodevelop your application code.</span></span>

<span data-ttu-id="be2c4-125">Három cloud service funkciókhoz speciális konfigurációk service-csomag exportálása előtt:</span><span class="sxs-lookup"><span data-stu-id="be2c4-125">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="be2c4-126">Ha azt szeretné, hogy egy felhőszolgáltatás, amely a Secure Sockets Layer (SSL) használ az adatok titkosításához, toodeploy [állítsa be az alkalmazását](cloud-services-configure-ssl-certificate-portal.md#modify) SSL-t.</span><span class="sxs-lookup"><span data-stu-id="be2c4-126">If you want toodeploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate-portal.md#modify) for SSL.</span></span>
* <span data-ttu-id="be2c4-127">Ha azt szeretné, hogy tooconfigure távoli asztali kapcsolatok toorole példányok [hello szerepkörök konfigurálása](cloud-services-role-enable-remote-desktop-new-portal.md) a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="be2c4-127">If you want tooconfigure Remote Desktop connections toorole instances, [configure hello roles](cloud-services-role-enable-remote-desktop-new-portal.md) for Remote Desktop.</span></span>
* <span data-ttu-id="be2c4-128">Ha tooconfigure részletes figyelését a felhőalapú szolgáltatás, engedélyezze az Azure Diagnostics hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="be2c4-128">If you want tooconfigure verbose monitoring for your cloud service, enable Azure Diagnostics for hello cloud service.</span></span> <span data-ttu-id="be2c4-129">*Minimális figyelési* (hello alapértelmezett szint figyelési) hello állomás operációs rendszerek (a virtuális gépek). szerepkörpéldányokra szolgáltatástól összegyűjtött teljesítményszámlálókat használ.</span><span class="sxs-lookup"><span data-stu-id="be2c4-129">*Minimal monitoring* (hello default monitoring level) uses performance counters gathered from hello host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="be2c4-130">*Részletes figyelési* gyűjti a teljesítményadatokat belül hello szerepkör példányok tooenable szorosabb elemzésének kérelem feldolgozása során előforduló problémák alapján további metrikák.</span><span class="sxs-lookup"><span data-stu-id="be2c4-130">*Verbose monitoring* gathers additional metrics based on performance data within hello role instances tooenable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="be2c4-131">Hogyan toofind tooenable Azure Diagnostics, lásd: [engedélyezése az Azure diagnostics](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="be2c4-131">toofind out how tooenable Azure Diagnostics, see [Enabling diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="be2c4-132">toocreate webes szerepkörök vagy feldolgozói szerepkörök példányai egy felhőszolgáltatás, kell [hello service-csomag létrehozása](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="be2c4-132">toocreate a cloud service with deployments of web roles or worker roles, you must [create hello service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="be2c4-133">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="be2c4-133">Before you begin</span></span>
* <span data-ttu-id="be2c4-134">Ha még nem telepítette a hello Azure SDK-t, kattintson a **Azure SDK telepítése** tooopen hello [Azure letöltőoldala](https://azure.microsoft.com/downloads/), majd töltse le az SDK hello hello nyelven, ahol inkább toodevelop a kódot.</span><span class="sxs-lookup"><span data-stu-id="be2c4-134">If you haven't installed hello Azure SDK, click **Install Azure SDK** tooopen hello [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download hello SDK for hello language in which you prefer toodevelop your code.</span></span> <span data-ttu-id="be2c4-135">(Konfigurálnia kell egy lehetőség toodo ezt később.)</span><span class="sxs-lookup"><span data-stu-id="be2c4-135">(You'll have an opportunity toodo this later.)</span></span>
* <span data-ttu-id="be2c4-136">Ha a szerepkörpéldányok szükséges tanúsítvány, hello tanúsítványok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="be2c4-136">If any role instances require a certificate, create hello certificates.</span></span> <span data-ttu-id="be2c4-137">Cloud services és a titkos kulcs egy .pfx fájlba szükséges.</span><span class="sxs-lookup"><span data-stu-id="be2c4-137">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="be2c4-138">Feltöltheti hello tanúsítványok tooAzure létrehozásához és telepítéséhez hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="be2c4-138">You can upload hello certificates tooAzure as you create and deploy hello cloud service.</span></span>

## <a name="create-and-deploy"></a><span data-ttu-id="be2c4-139">Létrehozása és telepítése</span><span class="sxs-lookup"><span data-stu-id="be2c4-139">Create and deploy</span></span>
1. <span data-ttu-id="be2c4-140">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="be2c4-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="be2c4-141">Kattintson a **új > számítási**, majd görgessen lefelé tooand kattintson **Felhőszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="be2c4-141">Click **New > Compute**, and then scroll down tooand click **Cloud Service**.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. <span data-ttu-id="be2c4-143">Az új hello **Felhőszolgáltatás** panelen adjon meg egy értéket a hello **DNS-név**.</span><span class="sxs-lookup"><span data-stu-id="be2c4-143">In hello new **Cloud Service** blade, enter a value for hello **DNS name**.</span></span>
4. <span data-ttu-id="be2c4-144">Hozzon létre egy új **erőforráscsoport** vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="be2c4-144">Create a new **Resource Group** or select an existing one.</span></span>
5. <span data-ttu-id="be2c4-145">Válasszon ki egy **helyet**.</span><span class="sxs-lookup"><span data-stu-id="be2c4-145">Select a **Location**.</span></span>
6. <span data-ttu-id="be2c4-146">Kattintson a **csomag**.</span><span class="sxs-lookup"><span data-stu-id="be2c4-146">Click **Package**.</span></span> <span data-ttu-id="be2c4-147">Ekkor megnyílik hello **a csomag feltöltése** panelen.</span><span class="sxs-lookup"><span data-stu-id="be2c4-147">This will open hello **Upload a package** blade.</span></span> <span data-ttu-id="be2c4-148">Hello kötelező mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="be2c4-148">Fill in hello required fields.</span></span> <span data-ttu-id="be2c4-149">Ha egyetlen példányt tartalmaz, a szerepkörökben, győződjön meg arról **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="be2c4-149">If any of your roles contain a single instance, ensure **Deploy even if one or more roles contain a single instance** is selected.</span></span>
7. <span data-ttu-id="be2c4-150">Győződjön meg arról, hogy **indítsa el a központi telepítés** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="be2c4-150">Make sure that **Start deployment** is selected.</span></span>
8. <span data-ttu-id="be2c4-151">Kattintson a **OK** amely bezárul hello **a csomag feltöltése** panelen.</span><span class="sxs-lookup"><span data-stu-id="be2c4-151">Click **OK** which will close hello **Upload a package** blade.</span></span>
9. <span data-ttu-id="be2c4-152">Ha nem rendelkezik a tanúsítványok tooadd, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="be2c4-152">If you do not have any certificates tooadd, click **Create**.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a><span data-ttu-id="be2c4-154">A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="be2c4-154">Upload a certificate</span></span>
<span data-ttu-id="be2c4-155">Ha a központi telepítési csomag [toouse tanúsítványok konfigurált](cloud-services-configure-ssl-certificate-portal.md#modify), most már feltöltheti hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="be2c4-155">If your deployment package was [configured toouse certificates](cloud-services-configure-ssl-certificate-portal.md#modify), you can upload hello certificate now.</span></span>

1. <span data-ttu-id="be2c4-156">Válassza ki **tanúsítványok**, és a hello **tanúsítványok hozzáadása** panelen válassza ki a hello SSL tanúsítvány .pfx fájlját, és adja meg a hello **jelszó** hello tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="be2c4-156">Select **Certificates**, and on hello **Add certificates** blade, select hello SSL certificate .pfx file, and then provide hello **Password** for hello certificate,</span></span>
2. <span data-ttu-id="be2c4-157">Kattintson a **Attach tanúsítvány**, és kattintson a **OK** a hello **adja hozzá a tanúsítványok** panelen.</span><span class="sxs-lookup"><span data-stu-id="be2c4-157">Click **Attach certificate**, and then click **OK** on hello **Add certificates** blade.</span></span>
3. <span data-ttu-id="be2c4-158">Kattintson a **létrehozása** a hello **Felhőszolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="be2c4-158">Click **Create** on hello **Cloud Service** blade.</span></span> <span data-ttu-id="be2c4-159">Amikor hello telepítési elérte hello **készen** állapotát, folytathatja a következő lépések toohello.</span><span class="sxs-lookup"><span data-stu-id="be2c4-159">When hello deployment has reached hello **Ready** status, you can proceed toohello next steps.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="be2c4-161">Ellenőrizze a telepítés sikeresen befejeződött</span><span class="sxs-lookup"><span data-stu-id="be2c4-161">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="be2c4-162">Kattintson a hello cloud service-példány.</span><span class="sxs-lookup"><span data-stu-id="be2c4-162">Click hello cloud service instance.</span></span>

    <span data-ttu-id="be2c4-163">hello állapot jelenítsen meg, hogy van-e hello szolgáltatás **futtató**.</span><span class="sxs-lookup"><span data-stu-id="be2c4-163">hello status should show that hello service is **Running**.</span></span>
2. <span data-ttu-id="be2c4-164">A **Essentials**, hello kattintson **webhely URL-címe** tooopen a felhőalapú szolgáltatás egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="be2c4-164">Under **Essentials**, click hello **Site URL** tooopen your cloud service in a web browser.</span></span>

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a><span data-ttu-id="be2c4-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be2c4-166">Next steps</span></span>
* <span data-ttu-id="be2c4-167">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="be2c4-167">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="be2c4-168">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="be2c4-168">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="be2c4-169">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="be2c4-169">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="be2c4-170">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="be2c4-170">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
