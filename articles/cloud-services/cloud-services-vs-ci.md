---
title: "A Visual Studio Online szolgáltatások folyamatos kézbesítési felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan állíthat be Azure felhőalapú alkalmazásokat folyamatos kézbesítési diagnosztika biztonságitár-kulcs a szolgáltatás konfigurációs fájlok mentése nélkül"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 7e70f92d4d1333ca6cbac5876e5ccbc763bd915c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-to-azure-using-visual-studio-online"></a><span data-ttu-id="1564a-103">Securely Cloud Services diagnosztika tárolási kulcs mentése és a telepítő folyamatos integrációt és a használatával a Visual Studio Online telepítési</span><span class="sxs-lookup"><span data-stu-id="1564a-103">Securely Save Cloud Services Diagnostics Storage Key and Setup Continuous Integration and Deployment to Azure using Visual Studio Online</span></span>
 <span data-ttu-id="1564a-104">Általános gyakorlat, nyissa meg a forrás-projektek ma is.</span><span class="sxs-lookup"><span data-stu-id="1564a-104">It is a common practice to open source projects nowadays.</span></span> <span data-ttu-id="1564a-105">Alkalmazás titkos kulcsok menteni a konfigurációs fájlok már nem biztonságos gyakorlat, a biztonsági rések ki vannak téve a titkos kulcsok a nyilvános forráskódú vezérlők küldik.</span><span class="sxs-lookup"><span data-stu-id="1564a-105">Saving application secrets in configuration files is no longer safe practice as security vulnerabilities are exposed from secrets being leaked from public source controls.</span></span> <span data-ttu-id="1564a-106">Titkos kulcs tárolása, az egyszerű szöveges fájlban folyamatos integrációt-feldolgozási folyamat nem biztonságos vagy óta lemezképfájl-kiszolgálókhoz lehet megosztott erőforrások a felhőalapú környezetben.</span><span class="sxs-lookup"><span data-stu-id="1564a-106">Storing secret as plaintext in a file in a Continuous Integration pipeline is not secure either since build servers could be shared resources on the Cloud environment.</span></span> <span data-ttu-id="1564a-107">Ez a cikk azt ismerteti, hogyan Visual Studio és a Visual Studio Online csökkenti a biztonsági aggályokon fejlesztési és folyamatos integrációt folyamat során.</span><span class="sxs-lookup"><span data-stu-id="1564a-107">This article explains how Visual Studio and Visual Studio Online mitigates the security concerns during development and Continuous Integration process.</span></span>

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a><span data-ttu-id="1564a-108">Távolítsa el a diagnosztika tárolási kulcs titkos projekt konfigurációs fájlban</span><span class="sxs-lookup"><span data-stu-id="1564a-108">Remove Diagnostics Storage Key Secret in Project Configuration File</span></span>
<span data-ttu-id="1564a-109">Cloud Services diagnosztika bővítmény diagnosztikai eredmények mentése az Azure storage igényel.</span><span class="sxs-lookup"><span data-stu-id="1564a-109">Cloud Services diagnostics extension requires Azure storage for saving diagnostics results.</span></span> <span data-ttu-id="1564a-110">Korábban a tárolási kapcsolati karakterlánc lett megadva a Felhőszolgáltatások konfigurációs (.cscfg) fájlokat, és sikerült beadni a verziókövetési rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="1564a-110">Formerly the storage connection string is specified in the Cloud Services configuration (.cscfg) files and could be checked in to source control.</span></span> <span data-ttu-id="1564a-111">A a legutóbb kiadott Azure SDK változtattuk a viselkedés csak tárolásához egy részleges kapcsolati karakterlánc szerepét a jogkivonatot a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="1564a-111">In the latest Azure SDK release we changed the behavior to only store a partial connection string with the key replaced by a token.</span></span> <span data-ttu-id="1564a-112">A következő lépések bemutatják, hogyan működik, az új Felhőszolgáltatás eszközt használunk erre:</span><span class="sxs-lookup"><span data-stu-id="1564a-112">The following steps describe how the new Cloud Services tooling works:</span></span>

### <a name="1-open-the-role-designer"></a><span data-ttu-id="1564a-113">1. Nyissa meg a szerepkör-Tervező</span><span class="sxs-lookup"><span data-stu-id="1564a-113">1. Open the Role designer</span></span>
* <span data-ttu-id="1564a-114">Kattintson duplán vagy felhőalapú szolgáltatások szerepkör szerepkör designer megnyitásához kattintson a jobb gombbal</span><span class="sxs-lookup"><span data-stu-id="1564a-114">Double click or right click on a Cloud Services role to open Role designer</span></span>

![Nyissa meg a szerepkör-Tervező][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a><span data-ttu-id="1564a-116">2. A diagnosztika című szakaszban, egy új jelölőnégyzet "ne távolítsa el a projektből titkos kulcs" kerül.</span><span class="sxs-lookup"><span data-stu-id="1564a-116">2. Under diagnostics section, a new check box “Don’t remove storage key secret from project” is added</span></span>
* <span data-ttu-id="1564a-117">A helyi storage emulator használ, ha a jelölőnégyzet le van tiltva, mert nincs titkos kulcsot a helyi kapcsolati karakterlánc, amely UseDevelopmentStorage kezeléséhez = true.</span><span class="sxs-lookup"><span data-stu-id="1564a-117">If you are using the local storage emulator, this checkbox is disabled because there is no secret to manage for the local connection string, which is UseDevelopmentStorage=true.</span></span>

![Nincs titkos helyi storage emulator kapcsolati karakterlánc][1]

* <span data-ttu-id="1564a-119">Új projekt létrehozásakor alapértelmezés szerint a jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="1564a-119">If you are creating a new project, by default this checkbox is unchecked.</span></span> <span data-ttu-id="1564a-120">Ennek eredményeképp a tárolási fő szakasz a kiválasztott tárolási kapcsolati karakterlánc jogkivonatok lecserélni.</span><span class="sxs-lookup"><span data-stu-id="1564a-120">This results in the storage key section of the selected storage connection string being replaced with a token.</span></span> <span data-ttu-id="1564a-121">A token értékét fogja található az aktuális felhasználó AppData központi mappában, például: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span><span class="sxs-lookup"><span data-stu-id="1564a-121">The value of the token will be found under the current user’s AppData Roaming folder, for example: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span></span>

> <span data-ttu-id="1564a-122">Vegye figyelembe, hogy a user\AppData mappa hozzáférést által a felhasználói bejelentkezés és tekinthető fejlesztési titkos kulcsokat tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="1564a-122">Note that the user\AppData folder is Access Controlled by user sign-in and is considered a secure place to store development secrets.</span></span>
> 
> 

![Biztonságitár-kulcs mentett felhasználói profil mappájába][2]

### <a name="3-select-a-diagnostics-storage-account"></a><span data-ttu-id="1564a-124">3. Válassza ki a diagnosztikai tárfiók</span><span class="sxs-lookup"><span data-stu-id="1564a-124">3. Select a diagnostics storage account</span></span>
* <span data-ttu-id="1564a-125">Válasszon egy tárfiókot a párbeszédpanelen kattintva elindítja a "..."</span><span class="sxs-lookup"><span data-stu-id="1564a-125">Select a storage account from the dialog launched by clicking the “…”</span></span> <span data-ttu-id="1564a-126">gombra.</span><span class="sxs-lookup"><span data-stu-id="1564a-126">button.</span></span> <span data-ttu-id="1564a-127">Figyelje meg, hogyan generált tárolási kapcsolati karakterlánc nem lesz a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="1564a-127">Notice how the storage connection string generated will not have the storage account key.</span></span>
* <span data-ttu-id="1564a-128">Például: megadni: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)</span><span class="sxs-lookup"><span data-stu-id="1564a-128">For example: DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(*clouddiagstrg.key*)</span></span>

### <a name="4----debugging-the-project"></a><span data-ttu-id="1564a-129">4.    A projekt hibakeresési</span><span class="sxs-lookup"><span data-stu-id="1564a-129">4.    Debugging the project</span></span>
* <span data-ttu-id="1564a-130">F5 billentyűt a Visual Studio a hibakeresés elindításához.</span><span class="sxs-lookup"><span data-stu-id="1564a-130">F5 to start debugging in Visual Studio.</span></span> <span data-ttu-id="1564a-131">Minden előtt a azonos módon működnek.</span><span class="sxs-lookup"><span data-stu-id="1564a-131">Everything should work in the same way as before.</span></span>
  <span data-ttu-id="1564a-132">![Helyileg a hibakeresés elindításához][3]</span><span class="sxs-lookup"><span data-stu-id="1564a-132">![Start debugging locally][3]</span></span>

### <a name="5-publish-project-from-visual-studio"></a><span data-ttu-id="1564a-133">5. A Visual Studio projekt közzététele</span><span class="sxs-lookup"><span data-stu-id="1564a-133">5. Publish project from Visual Studio</span></span>
* <span data-ttu-id="1564a-134">Indítsa el a közzététel párbeszédpanel, és folytassa a bejelentkezési utasításokat a applicaion közzététele az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="1564a-134">Launch the publish dialog and proceed with sign-in instructions to publish the applicaion to Azure.</span></span>

### <a name="6-additional-information"></a><span data-ttu-id="1564a-135">6. További információ</span><span class="sxs-lookup"><span data-stu-id="1564a-135">6. Additional information</span></span>
> <span data-ttu-id="1564a-136">Megjegyzés: A beállítások panelen a szerepkör-tervezőben marad, mert a lépést.</span><span class="sxs-lookup"><span data-stu-id="1564a-136">Note: The Settings panel in the role designer will stay as it is for now.</span></span> <span data-ttu-id="1564a-137">Ha azt szeretné, a titkos felügyeleti szolgáltatás használatához diagnosztikai, nyissa meg a konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="1564a-137">If you want to use the secret management feature for diagnostics, go to the Configurations tab.</span></span>
> 
> 

![Beállítások hozzáadása][4]

> <span data-ttu-id="1564a-139">Megjegyzés: Ha engedélyezve van, az Application Insights kulcs tárolja egyszerű szövegként.</span><span class="sxs-lookup"><span data-stu-id="1564a-139">Note: If enabled, the Application Insights key will be stored as plain text.</span></span> <span data-ttu-id="1564a-140">A kulcs csak szolgál feltöltés tartalmát, a bizalmas adatok lesz feltörésének kockázata.</span><span class="sxs-lookup"><span data-stu-id="1564a-140">The key is only used for upload contents so no sensitive data will be at risk being compromised.</span></span>
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a><span data-ttu-id="1564a-141">Hozza létre és közzététele a felhőalapú szolgáltatások a projekt Visual Studio online Feladatsablonok</span><span class="sxs-lookup"><span data-stu-id="1564a-141">Build and Publish a Cloud Services Project using Visual Studio online Task Templates</span></span>
* <span data-ttu-id="1564a-142">A következő lépések a Visual Studio online feladatainak Felhőszolgáltatások projekt folyamatos integráció telepítője mutatja be:</span><span class="sxs-lookup"><span data-stu-id="1564a-142">The following steps illustrates how to setup Continuous Integration for Cloud Services project using Visual Studio online tasks:</span></span>
  ### <a name="1----obtain-a-vso-account"></a><span data-ttu-id="1564a-143">1.    Egy VSO fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="1564a-143">1.    Obtain a VSO account</span></span>
* <span data-ttu-id="1564a-144">[A Visual Studio Online-fiók létrehozása] [ Create Visual Studio Online account] Ha még nem rendelkezik már</span><span class="sxs-lookup"><span data-stu-id="1564a-144">[Create Visual Studio Online account][Create Visual Studio Online account] if you don’t have one already</span></span>
* <span data-ttu-id="1564a-145">[Hozzon létre csapatprojekt] [ Create team project] a Visual Studio online-fiók</span><span class="sxs-lookup"><span data-stu-id="1564a-145">[Create team project][Create team project] in your Visual Studio online account</span></span>

### <a name="2----setup-source-control-in-visual-studio"></a><span data-ttu-id="1564a-146">2.    A Visual Studio Verziókövetés beállítása</span><span class="sxs-lookup"><span data-stu-id="1564a-146">2.    Setup source control in Visual Studio</span></span>
* <span data-ttu-id="1564a-147">A csapatprojekt kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="1564a-147">Connect to a team project</span></span>

![Csapatprojekt kapcsolódni][5]

![Válassza ki a csapatprojekt való kapcsolódáshoz][6]

* <span data-ttu-id="1564a-150">Adja hozzá a projekthez a verziókövetési rendszerrel</span><span class="sxs-lookup"><span data-stu-id="1564a-150">Add your project to source control</span></span>

![A verziókövetési projekt hozzáadása][7]

![A vezérlő mappa projekt leképezése][8]

* <span data-ttu-id="1564a-153">Ellenőrizze a projekt csapata Intézőből</span><span class="sxs-lookup"><span data-stu-id="1564a-153">Check in your project from Team Explorer</span></span>

![A verziókövetési be][9]

### <a name="3----configure-build-process"></a><span data-ttu-id="1564a-155">3.    Konfigurálja a felépítési folyamat</span><span class="sxs-lookup"><span data-stu-id="1564a-155">3.    Configure Build process</span></span>
* <span data-ttu-id="1564a-156">Keresse meg a csapatprojekt és egy új felépítési folyamat sablonok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1564a-156">Browse to your team project and add a new build process Templates</span></span>

![Adja hozzá egy új build][10]

* <span data-ttu-id="1564a-158">Jelölje be összeállítási feladat</span><span class="sxs-lookup"><span data-stu-id="1564a-158">Select build task</span></span>

![Egy összeállítási feladat hozzáadása][11]

![Válassza ki a Visual Studio Build sablon][12]

* <span data-ttu-id="1564a-161">Összeállítási feladat bemenet szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="1564a-161">Edit build task input.</span></span> <span data-ttu-id="1564a-162">A build paraméterek az igényeknek megfelelően adjon testreszabása</span><span class="sxs-lookup"><span data-stu-id="1564a-162">Please customize the build parameters according to your need</span></span>

![Összeállítási feladat konfigurálása][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* <span data-ttu-id="1564a-164">Build változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1564a-164">Configure build variables</span></span>

![Build változók konfigurálása][14]

* <span data-ttu-id="1564a-166">Adjon hozzá egy feladatot build eldobási feltöltése</span><span class="sxs-lookup"><span data-stu-id="1564a-166">Add a task to upload build drop</span></span>

![Válassza a build eldobási tevékenység közzététele][15]

![Konfigurálása build eldobási tevékenység közzététele][16]

* <span data-ttu-id="1564a-169">Futtassa a build</span><span class="sxs-lookup"><span data-stu-id="1564a-169">Run the build</span></span>

![Új várólista létrehozása][17]

![Build összegzésének megtekintése][18]

* <span data-ttu-id="1564a-172">Ha a build sikeres látni fogja a hasonló alatt eredményt</span><span class="sxs-lookup"><span data-stu-id="1564a-172">if the build is successful you will see a result similar to below</span></span>

![Build eredménye][19]

### <a name="4----configure-release-process"></a><span data-ttu-id="1564a-174">4.    Kiadás folyamat beállítása</span><span class="sxs-lookup"><span data-stu-id="1564a-174">4.    Configure Release process</span></span>
* <span data-ttu-id="1564a-175">Hozzon létre egy új kiadás</span><span class="sxs-lookup"><span data-stu-id="1564a-175">Create a new release</span></span>

![Hozzon létre új kiadás][20]

* <span data-ttu-id="1564a-177">Válassza ki az Azure felhőalapú szolgáltatásokhoz központi telepítési feladatot</span><span class="sxs-lookup"><span data-stu-id="1564a-177">Select the Azure Cloud Services Deployment task</span></span>

![Válassza ki a szolgáltatástelepítési feladat Azure Cloud Services csomag][21]

* <span data-ttu-id="1564a-179">A tárfiók kulcsa nem ellenőrzése verziókövetési rendszerrel, igazolnia kell a titkos kulcsot diagnosztika bővítmények beállítása.</span><span class="sxs-lookup"><span data-stu-id="1564a-179">As the storage account key is not checked in to source control, we need to specify the secret key for setting diagnostics extensions.</span></span> <span data-ttu-id="1564a-180">Bontsa ki a **speciális beállítások új szolgáltatás létrehozása** szakaszt, és szerkesztheti a **diagnosztikai Tárfiók kulcsait** bemeneti paraméter.</span><span class="sxs-lookup"><span data-stu-id="1564a-180">Expand the **Advanced Options for Creating New Service** section and edit the **Diagnostics Storage Account Keys** parameter input.</span></span> <span data-ttu-id="1564a-181">A bemeneti időt vesz igénybe, a kulcs-érték pár formátumban több sornyi **[RoleName]:$(StorageAccountKey)**</span><span class="sxs-lookup"><span data-stu-id="1564a-181">This input takes in multiple lines of key value pair in the format of **[RoleName]:$(StorageAccountKey)**</span></span>

> <span data-ttu-id="1564a-182">Megjegyzés: a diagnosztikai tárfiók ugyanahhoz az előfizetéshez, mint ahol teszi közzé a Cloud Services alkalmazás alatt áll, ha azt nem kell megadnia a kulcsot a telepítési tevékenység bemeneti; a központi telepítés programozott módon beszerzi a tárolással kapcsolatos az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="1564a-182">Note: if your diagnostics storage account is under the same subscription as where you will publish the Cloud Services application, you don't have to enter the key in the deployment task input; the deployment will programmatically obtain the storage information from your subscription</span></span>
> 
> 

![Cloud Services telepítési feladat konfigurálása][22]

* <span data-ttu-id="1564a-184">Használjon titkos hozhat létre. változók tárolási kulcsok mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="1564a-184">Use secret build variables to save storage Keys.</span></span> <span data-ttu-id="1564a-185">Maszkolás titkos kulcsként változó kattintson a jobb oldalán a változók bemeneti zárolási ikonra</span><span class="sxs-lookup"><span data-stu-id="1564a-185">To mask a variable as secret click on the lock icon on the right side of the Variables input</span></span>

![Mentési tároló kulcsok a titkos kulcs változók létrehozása][23]

* <span data-ttu-id="1564a-187">Egy kiadási létrehozása és központi telepítése a projekthez az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="1564a-187">Create a release and deploy your project to Azure</span></span>

![Hozzon létre új kiadás][24]

## <a name="next-steps"></a><span data-ttu-id="1564a-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1564a-189">Next steps</span></span>
<span data-ttu-id="1564a-190">Az Azure Cloud Services diagnosztika bővítmények beállításával kapcsolatos további tudnivalókért tekintse meg [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése][Enable diagnostics in Azure Cloud Services using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="1564a-190">To learn more about setting diagnostics extensions for Azure Cloud Services, please see [Enable diagnostics in Azure Cloud Services using PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span></span>

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png
