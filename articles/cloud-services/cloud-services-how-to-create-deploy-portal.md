---
title: "Hogyan hozhat létre és telepíthet egy felhőalapú szolgáltatás |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet egy felhőalapú szolgáltatás, az Azure portál használatával."
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
ms.openlocfilehash: e5ce666f1d826c7901c9fd5e7fafe6171139c3ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a><span data-ttu-id="c4ee0-103">Hogyan hozhat létre és telepíthet egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c4ee0-103">How to create and deploy a cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c4ee0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c4ee0-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="c4ee0-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="c4ee0-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
>
>

<span data-ttu-id="c4ee0-106">Az Azure portálon két módot biztosít hozhat létre és telepíthet egy felhőalapú szolgáltatás: *Gyorslétrehozás* és *egyéni létrehozás*.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-106">The Azure portal provides two ways for you to create and deploy a cloud service: *Quick Create* and *Custom Create*.</span></span>

<span data-ttu-id="c4ee0-107">Ez a cikk ismerteti, hogyan hozzon létre egy új felhőalapú szolgáltatás, majd a gyors létrehozás módszerrel **feltöltése** frissítése és telepítése az Azure cloud service csomag.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-107">This article explains how to use the Quick Create method to create a new cloud service and then use **Upload** to upload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="c4ee0-108">Ha ezt a módszert használja, az Azure-portálon teszi követelményeinek befejezése menet elérhető kényelmes hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-108">When you use this method, the Azure portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="c4ee0-109">Ha készen áll a felhőalapú szolgáltatás történő létrehozásakor telepítendő, mindkettő egyszerre használja egyéni létrehozás teheti meg.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-109">If you're ready to deploy your cloud service when you create it, you can do both at the same time using Custom Create.</span></span>

> [!NOTE]
> <span data-ttu-id="c4ee0-110">Ha azt tervezi, közzététele a felhőalapú szolgáltatás a Visual Studio Team Services (VSTS), használja a Gyorslétrehozás, és majd VSTS közzététel beállítása az Azure gyors üzembe helyezés vagy az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-110">If you plan to publish your cloud service from Visual Studio Team Services (VSTS), use Quick Create, and then set up VSTS publishing from the Azure Quickstart or the dashboard.</span></span> <span data-ttu-id="c4ee0-111">További információkért lásd: [használatával Visual Studio Team Services Azure folyamatos kézbesítése][TFSTutorialForCloudService], vagy tekintse át a súgóban talál a **gyors üzembe helyezés** lap.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-111">For more information, see [Continuous Delivery to Azure by Using Visual Studio Team Services][TFSTutorialForCloudService], or see help for the **Quick Start** page.</span></span>
>
>

## <a name="concepts"></a><span data-ttu-id="c4ee0-112">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="c4ee0-112">Concepts</span></span>
<span data-ttu-id="c4ee0-113">Három összetevők szükségesek a felhő alapú szolgáltatásként az Azure-ban az alkalmazás központi telepítéséről:</span><span class="sxs-lookup"><span data-stu-id="c4ee0-113">Three components are required to deploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="c4ee0-114">**Szolgáltatásdefiníció**</span><span class="sxs-lookup"><span data-stu-id="c4ee0-114">**Service Definition**</span></span>  
  <span data-ttu-id="c4ee0-115">A felhőalapú szolgáltatás definíciós fájl (.csdef) a szolgáltatásmodellt, beleértve a szerepkörök számának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-115">The cloud service definition file (.csdef) defines the service model, including the number of roles.</span></span>
* <span data-ttu-id="c4ee0-116">**Szolgáltatás konfigurációja**</span><span class="sxs-lookup"><span data-stu-id="c4ee0-116">**Service Configuration**</span></span>  
  <span data-ttu-id="c4ee0-117">A felhőalapú szolgáltatás konfigurációs fájlját (.cscfg) a felhőre vonatkozó konfigurációs beállításokat tartalmazza, szolgáltatás és az egyes szerepkörök, beleértve a szerepkörpéldányok számát.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-117">The cloud service configuration file (.cscfg) provides configuration settings for the cloud service and individual roles, including the number of role instances.</span></span>
* <span data-ttu-id="c4ee0-118">**Szolgáltatáscsomag**</span><span class="sxs-lookup"><span data-stu-id="c4ee0-118">**Service Package**</span></span>  
  <span data-ttu-id="c4ee0-119">A szolgáltatás csomagba (.cspkg) tartalmazza az alkalmazás kódjában és konfigurációkat és a szolgáltatásdefiníciós fájlban.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-119">The service package (.cspkg) contains the application code and configurations and the service definition file.</span></span>

<span data-ttu-id="c4ee0-120">Ezek és csomag létrehozásával kapcsolatos részletesebb [Itt](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-120">You can learn more about these and how to create a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="c4ee0-121">Az alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="c4ee0-121">Prepare your app</span></span>
<span data-ttu-id="c4ee0-122">Egy felhőalapú szolgáltatás telepítése előtt a felhőalapú szolgáltatás csomagba (.cspkg) alkalmazáskódjában és egy felhőalapú szolgáltatás konfigurációs fájlját (.cscfg) kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-122">Before you can deploy a cloud service, you must create the cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="c4ee0-123">Az Azure SDK eszközöket biztosít a szükséges központi telepítés fájlok előkészítése.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-123">The Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="c4ee0-124">Az SDK telepítése a [Azure letölti](https://azure.microsoft.com/downloads/) oldalon, a nyelvet, amelyben az alkalmazás kódjában fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-124">You can install the SDK from the [Azure Downloads](https://azure.microsoft.com/downloads/) page, in the language in which you prefer to develop your application code.</span></span>

<span data-ttu-id="c4ee0-125">Három cloud service funkciókhoz speciális konfigurációk service-csomag exportálása előtt:</span><span class="sxs-lookup"><span data-stu-id="c4ee0-125">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="c4ee0-126">Ha szeretné központilag telepíteni egy felhőszolgáltatás, amely a Secure Sockets Layer (SSL) használ az adatok titkosításához, [állítsa be az alkalmazását](cloud-services-configure-ssl-certificate-portal.md#modify) SSL-t.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-126">If you want to deploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate-portal.md#modify) for SSL.</span></span>
* <span data-ttu-id="c4ee0-127">Ha a távoli asztal kapcsolatokat szerepkörpéldányt beállítani, a konfigurálni kívánt [a szerepkörök konfigurálása](cloud-services-role-enable-remote-desktop-new-portal.md) a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-127">If you want to configure Remote Desktop connections to role instances, [configure the roles](cloud-services-role-enable-remote-desktop-new-portal.md) for Remote Desktop.</span></span>
* <span data-ttu-id="c4ee0-128">Ha konfigurálni szeretné részletes figyelését a felhőalapú szolgáltatás, Azure diagnosztika engedélyezése a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-128">If you want to configure verbose monitoring for your cloud service, enable Azure Diagnostics for the cloud service.</span></span> <span data-ttu-id="c4ee0-129">*Minimális figyelési* (az alapértelmezett figyelési szint) az állomás operációs rendszereket a szerepkörpéldányok (virtuális gépek) szolgáltatástól összegyűjtött teljesítményszámlálókat használ.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-129">*Minimal monitoring* (the default monitoring level) uses performance counters gathered from the host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="c4ee0-130">*Részletes figyelési* gyűjti a teljesítményadatokat belül a szerepkörpéldányok ahhoz, hogy szorosabb kérelem feldolgozása során előforduló problémák elemzése alapján további metrikákat.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-130">*Verbose monitoring* gathers additional metrics based on performance data within the role instances to enable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="c4ee0-131">Azure Diagnostics engedélyezése regisztrációval, lásd: [engedélyezése az Azure diagnostics](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-131">To find out how to enable Azure Diagnostics, see [Enabling diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="c4ee0-132">Felhőalapú szolgáltatás létrehozása a webes szerepkörök vagy feldolgozói szerepkörök példányai, le kell [a service-csomag létrehozása](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-132">To create a cloud service with deployments of web roles or worker roles, you must [create the service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c4ee0-133">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c4ee0-133">Before you begin</span></span>
* <span data-ttu-id="c4ee0-134">Ha még nem telepítette az Azure SDK-t, kattintson a **Azure SDK telepítése** megnyitásához a [Azure letöltőoldala](https://azure.microsoft.com/downloads/), és töltse le az SDK for a nyelvet, amelyben a kód kialakításához.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-134">If you haven't installed the Azure SDK, click **Install Azure SDK** to open the [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download the SDK for the language in which you prefer to develop your code.</span></span> <span data-ttu-id="c4ee0-135">(Összekapcsolta elvégezheti később lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="c4ee0-135">(You'll have an opportunity to do this later.)</span></span>
* <span data-ttu-id="c4ee0-136">Ha bármely szerepkörpéldányokat szükséges tanúsítvány, a tanúsítványok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-136">If any role instances require a certificate, create the certificates.</span></span> <span data-ttu-id="c4ee0-137">Cloud services és a titkos kulcs egy .pfx fájlba szükséges.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-137">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="c4ee0-138">A tanúsítványok feltöltheti az Azure-ba, létrehozásához, és a felhőalapú szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-138">You can upload the certificates to Azure as you create and deploy the cloud service.</span></span>

## <a name="create-and-deploy"></a><span data-ttu-id="c4ee0-139">Létrehozása és telepítése</span><span class="sxs-lookup"><span data-stu-id="c4ee0-139">Create and deploy</span></span>
1. <span data-ttu-id="c4ee0-140">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c4ee0-141">Kattintson a **új > számítási**, és görgessen lefelé, és kattintson a **Felhőszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-141">Click **New > Compute**, and then scroll down to and click **Cloud Service**.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. <span data-ttu-id="c4ee0-143">Az új **Felhőszolgáltatás** panelen adjon meg egy értéket a **DNS-név**.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-143">In the new **Cloud Service** blade, enter a value for the **DNS name**.</span></span>
4. <span data-ttu-id="c4ee0-144">Hozzon létre egy új **erőforráscsoport** vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-144">Create a new **Resource Group** or select an existing one.</span></span>
5. <span data-ttu-id="c4ee0-145">Válasszon ki egy **helyet**.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-145">Select a **Location**.</span></span>
6. <span data-ttu-id="c4ee0-146">Kattintson a **csomag**.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-146">Click **Package**.</span></span> <span data-ttu-id="c4ee0-147">Ekkor megnyílik a **a csomag feltöltése** panelen.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-147">This will open the **Upload a package** blade.</span></span> <span data-ttu-id="c4ee0-148">Töltse ki a kötelező mezőket.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-148">Fill in the required fields.</span></span> <span data-ttu-id="c4ee0-149">Ha egyetlen példányt tartalmaz, a szerepkörökben, győződjön meg arról **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-149">If any of your roles contain a single instance, ensure **Deploy even if one or more roles contain a single instance** is selected.</span></span>
7. <span data-ttu-id="c4ee0-150">Győződjön meg arról, hogy **indítsa el a központi telepítés** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-150">Make sure that **Start deployment** is selected.</span></span>
8. <span data-ttu-id="c4ee0-151">Kattintson a **OK** bezárul, amely a **a csomag feltöltése** panelen.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-151">Click **OK** which will close the **Upload a package** blade.</span></span>
9. <span data-ttu-id="c4ee0-152">Ha nem rendelkezik a tanúsítványok hozzáadása, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-152">If you do not have any certificates to add, click **Create**.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a><span data-ttu-id="c4ee0-154">A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="c4ee0-154">Upload a certificate</span></span>
<span data-ttu-id="c4ee0-155">Ha a központi telepítési csomag [tanúsítványok](cloud-services-configure-ssl-certificate-portal.md#modify), most már feltöltheti a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-155">If your deployment package was [configured to use certificates](cloud-services-configure-ssl-certificate-portal.md#modify), you can upload the certificate now.</span></span>

1. <span data-ttu-id="c4ee0-156">Válassza ki **tanúsítványok**, majd a a **tanúsítványok hozzáadása** panelen válassza ki az SSL tanúsítvány .pfx fájlt, és adja meg a **jelszó** a tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="c4ee0-156">Select **Certificates**, and on the **Add certificates** blade, select the SSL certificate .pfx file, and then provide the **Password** for the certificate,</span></span>
2. <span data-ttu-id="c4ee0-157">Kattintson a **Attach tanúsítvány**, és kattintson a **OK** a a **adja hozzá a tanúsítványok** panelen.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-157">Click **Attach certificate**, and then click **OK** on the **Add certificates** blade.</span></span>
3. <span data-ttu-id="c4ee0-158">Kattintson a **létrehozása** a a **Felhőszolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-158">Click **Create** on the **Cloud Service** blade.</span></span> <span data-ttu-id="c4ee0-159">Ha a központi telepítés elérte a **készen** állapota, továbbléphet a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-159">When the deployment has reached the **Ready** status, you can proceed to the next steps.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="c4ee0-161">Ellenőrizze a telepítés sikeresen befejeződött</span><span class="sxs-lookup"><span data-stu-id="c4ee0-161">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="c4ee0-162">Kattintson a cloud service-példány.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-162">Click the cloud service instance.</span></span>

    <span data-ttu-id="c4ee0-163">Az állapot jelenítsen meg, hogy van-e a szolgáltatás **futtató**.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-163">The status should show that the service is **Running**.</span></span>
2. <span data-ttu-id="c4ee0-164">A **Essentials**, kattintson a **webhely URL-címe** a felhőalapú szolgáltatás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="c4ee0-164">Under **Essentials**, click the **Site URL** to open your cloud service in a web browser.</span></span>

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a><span data-ttu-id="c4ee0-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4ee0-166">Next steps</span></span>
* <span data-ttu-id="c4ee0-167">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-167">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="c4ee0-168">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-168">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="c4ee0-169">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-169">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="c4ee0-170">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4ee0-170">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
