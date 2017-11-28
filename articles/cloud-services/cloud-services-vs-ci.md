---
title: "a Visual Studio Online szolgáltatások aaaContinuous kézbesítési felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset folyamatos kézbesítését az Azure-bA mentése a felhőalapú alkalmazások diagnosztika tárolási kulcs toohello szolgáltatás konfigurációs fájlok mentése nélkül"
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
ms.openlocfilehash: dc87d049e46daf8b8a26ee4450ebd9b7910f287b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-tooazure-using-visual-studio-online"></a><span data-ttu-id="09cdc-103">Securely mentése Cloud Services diagnosztika Biztonságitár-kulcs és a telepítő folyamatos integrációt és telepítést tooAzure Visual Studio Online használata</span><span class="sxs-lookup"><span data-stu-id="09cdc-103">Securely Save Cloud Services Diagnostics Storage Key and Setup Continuous Integration and Deployment tooAzure using Visual Studio Online</span></span>
 <span data-ttu-id="09cdc-104">Egy általános gyakorlat tooopen forrás projektek ma is.</span><span class="sxs-lookup"><span data-stu-id="09cdc-104">It is a common practice tooopen source projects nowadays.</span></span> <span data-ttu-id="09cdc-105">Alkalmazás titkos kulcsok menteni a konfigurációs fájlok már nem biztonságos gyakorlat, a biztonsági rések ki vannak téve a titkos kulcsok a nyilvános forráskódú vezérlők küldik.</span><span class="sxs-lookup"><span data-stu-id="09cdc-105">Saving application secrets in configuration files is no longer safe practice as security vulnerabilities are exposed from secrets being leaked from public source controls.</span></span> <span data-ttu-id="09cdc-106">Titkos kulcs tárolása, az egyszerű szöveges fájlban folyamatos integrációt-feldolgozási folyamat nem biztonságos vagy óta lemezképfájl-kiszolgálókhoz lehet megosztott erőforrások hello felhőalapú környezetben.</span><span class="sxs-lookup"><span data-stu-id="09cdc-106">Storing secret as plaintext in a file in a Continuous Integration pipeline is not secure either since build servers could be shared resources on hello Cloud environment.</span></span> <span data-ttu-id="09cdc-107">Ez a cikk azt ismerteti, hogyan Visual Studio és a Visual Studio Online csökkenti hello biztonsági problémáit fejlesztési és folyamatos integrációt folyamat során.</span><span class="sxs-lookup"><span data-stu-id="09cdc-107">This article explains how Visual Studio and Visual Studio Online mitigates hello security concerns during development and Continuous Integration process.</span></span>

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a><span data-ttu-id="09cdc-108">Távolítsa el a diagnosztika tárolási kulcs titkos projekt konfigurációs fájlban</span><span class="sxs-lookup"><span data-stu-id="09cdc-108">Remove Diagnostics Storage Key Secret in Project Configuration File</span></span>
<span data-ttu-id="09cdc-109">Cloud Services diagnosztika bővítmény diagnosztikai eredmények mentése az Azure storage igényel.</span><span class="sxs-lookup"><span data-stu-id="09cdc-109">Cloud Services diagnostics extension requires Azure storage for saving diagnostics results.</span></span> <span data-ttu-id="09cdc-110">Korábban hello tárolási kapcsolati karakterlánc lett megadva a hello Felhőszolgáltatások konfigurációs (.cscfg) fájlok, és sikerült beadni toosource vezérlő.</span><span class="sxs-lookup"><span data-stu-id="09cdc-110">Formerly hello storage connection string is specified in hello Cloud Services configuration (.cscfg) files and could be checked in toosource control.</span></span> <span data-ttu-id="09cdc-111">A legfrissebb Azure SDK-verzióban hello változtattuk hello viselkedés tooonly tároló egy részleges kapcsolati karakterlánc szerepét a jogkivonat hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="09cdc-111">In hello latest Azure SDK release we changed hello behavior tooonly store a partial connection string with hello key replaced by a token.</span></span> <span data-ttu-id="09cdc-112">hello lépések azt mutatják be, hello új Felhőszolgáltatás tooling működése:</span><span class="sxs-lookup"><span data-stu-id="09cdc-112">hello following steps describe how hello new Cloud Services tooling works:</span></span>

### <a name="1-open-hello-role-designer"></a><span data-ttu-id="09cdc-113">1. Nyissa meg a hello szerepkör designer</span><span class="sxs-lookup"><span data-stu-id="09cdc-113">1. Open hello Role designer</span></span>
* <span data-ttu-id="09cdc-114">Kattintson duplán, vagy kattintson jobb gombbal a Cloud Services szerepkör tooopen szerepkör jellemzőihez</span><span class="sxs-lookup"><span data-stu-id="09cdc-114">Double click or right click on a Cloud Services role tooopen Role designer</span></span>

![Nyissa meg a szerepkör-Tervező][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a><span data-ttu-id="09cdc-116">2. A diagnosztika című szakaszban, egy új jelölőnégyzet "ne távolítsa el a projektből titkos kulcs" kerül.</span><span class="sxs-lookup"><span data-stu-id="09cdc-116">2. Under diagnostics section, a new check box “Don’t remove storage key secret from project” is added</span></span>
* <span data-ttu-id="09cdc-117">Ha hello helyi storage emulator használata esetén ezt a jelölőnégyzetet le van tiltva, mert nincs titkos toomanage hello helyi kapcsolódási karakterlánc, amely UseDevelopmentStorage = true.</span><span class="sxs-lookup"><span data-stu-id="09cdc-117">If you are using hello local storage emulator, this checkbox is disabled because there is no secret toomanage for hello local connection string, which is UseDevelopmentStorage=true.</span></span>

![Nincs titkos helyi storage emulator kapcsolati karakterlánc][1]

* <span data-ttu-id="09cdc-119">Új projekt létrehozásakor alapértelmezés szerint a jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="09cdc-119">If you are creating a new project, by default this checkbox is unchecked.</span></span> <span data-ttu-id="09cdc-120">Az eredmény hello tárolási kulcs szakaszában hello kiválasztott tárolási kapcsolati karakterlánc jogkivonatok lecserélni.</span><span class="sxs-lookup"><span data-stu-id="09cdc-120">This results in hello storage key section of hello selected storage connection string being replaced with a token.</span></span> <span data-ttu-id="09cdc-121">hello hello token értékének találhatók hello aktuális felhasználó AppData központi mappában, például: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span><span class="sxs-lookup"><span data-stu-id="09cdc-121">hello value of hello token will be found under hello current user’s AppData Roaming folder, for example: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span></span>

> <span data-ttu-id="09cdc-122">Vegye figyelembe a hello user\AppData mappában hozzáférést által a felhasználói bejelentkezés és egy biztonságos hely toostore fejlesztési titkok számít.</span><span class="sxs-lookup"><span data-stu-id="09cdc-122">Note that hello user\AppData folder is Access Controlled by user sign-in and is considered a secure place toostore development secrets.</span></span>
> 
> 

![Biztonságitár-kulcs mentett felhasználói profil mappájába][2]

### <a name="3-select-a-diagnostics-storage-account"></a><span data-ttu-id="09cdc-124">3. Válassza ki a diagnosztikai tárfiók</span><span class="sxs-lookup"><span data-stu-id="09cdc-124">3. Select a diagnostics storage account</span></span>
* <span data-ttu-id="09cdc-125">Válasszon egy tárfiókot hello "..." gombra kattintva elindított hello párbeszédpanelről</span><span class="sxs-lookup"><span data-stu-id="09cdc-125">Select a storage account from hello dialog launched by clicking hello “…”</span></span> <span data-ttu-id="09cdc-126">gombra.</span><span class="sxs-lookup"><span data-stu-id="09cdc-126">button.</span></span> <span data-ttu-id="09cdc-127">Figyelje meg, hogyan hello generált tárolási kapcsolati karakterlánc nem lesz hello tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="09cdc-127">Notice how hello storage connection string generated will not have hello storage account key.</span></span>
* <span data-ttu-id="09cdc-128">Például: megadni: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)</span><span class="sxs-lookup"><span data-stu-id="09cdc-128">For example: DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(*clouddiagstrg.key*)</span></span>

### <a name="4----debugging-hello-project"></a><span data-ttu-id="09cdc-129">4.    Hibakeresési hello projekt</span><span class="sxs-lookup"><span data-stu-id="09cdc-129">4.    Debugging hello project</span></span>
* <span data-ttu-id="09cdc-130">A Visual Studio hibakeresési F5 toostart.</span><span class="sxs-lookup"><span data-stu-id="09cdc-130">F5 toostart debugging in Visual Studio.</span></span> <span data-ttu-id="09cdc-131">Minden kell dolgoznia hello azonos módon mint korábban.</span><span class="sxs-lookup"><span data-stu-id="09cdc-131">Everything should work in hello same way as before.</span></span>
  <span data-ttu-id="09cdc-132">![Helyileg a hibakeresés elindításához][3]</span><span class="sxs-lookup"><span data-stu-id="09cdc-132">![Start debugging locally][3]</span></span>

### <a name="5-publish-project-from-visual-studio"></a><span data-ttu-id="09cdc-133">5. A Visual Studio projekt közzététele</span><span class="sxs-lookup"><span data-stu-id="09cdc-133">5. Publish project from Visual Studio</span></span>
* <span data-ttu-id="09cdc-134">Indítási hello közzétételére párbeszédpanelt, és folytassa a bejelentkezési utasítások toopublish hello applicaion tooAzure.</span><span class="sxs-lookup"><span data-stu-id="09cdc-134">Launch hello publish dialog and proceed with sign-in instructions toopublish hello applicaion tooAzure.</span></span>

### <a name="6-additional-information"></a><span data-ttu-id="09cdc-135">6. További információ</span><span class="sxs-lookup"><span data-stu-id="09cdc-135">6. Additional information</span></span>
> <span data-ttu-id="09cdc-136">Megjegyzés: hello beállítások panel hello szerepkör Designer marad, mert a lépést.</span><span class="sxs-lookup"><span data-stu-id="09cdc-136">Note: hello Settings panel in hello role designer will stay as it is for now.</span></span> <span data-ttu-id="09cdc-137">Ha diagnosztikai toouse hello titkos felügyeleti szolgáltatást, lépjen a toohello konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="09cdc-137">If you want toouse hello secret management feature for diagnostics, go toohello Configurations tab.</span></span>
> 
> 

![Beállítások hozzáadása][4]

> <span data-ttu-id="09cdc-139">Megjegyzés: Ha engedélyezve van, az Application Insights kulcs hello tárolja egyszerű szövegként.</span><span class="sxs-lookup"><span data-stu-id="09cdc-139">Note: If enabled, hello Application Insights key will be stored as plain text.</span></span> <span data-ttu-id="09cdc-140">hello kulcs csak szolgál feltöltés tartalmát, a bizalmas adatok lesz feltörésének kockázata.</span><span class="sxs-lookup"><span data-stu-id="09cdc-140">hello key is only used for upload contents so no sensitive data will be at risk being compromised.</span></span>
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a><span data-ttu-id="09cdc-141">Hozza létre és közzététele a felhőalapú szolgáltatások a projekt Visual Studio online Feladatsablonok</span><span class="sxs-lookup"><span data-stu-id="09cdc-141">Build and Publish a Cloud Services Project using Visual Studio online Task Templates</span></span>
* <span data-ttu-id="09cdc-142">a lépéseket követve hello bemutatja, hogyan toosetup folyamatos integrációt a Felhőszolgáltatások projektre a Visual Studio online feladatok használata:</span><span class="sxs-lookup"><span data-stu-id="09cdc-142">hello following steps illustrates how toosetup Continuous Integration for Cloud Services project using Visual Studio online tasks:</span></span>
  ### <a name="1----obtain-a-vso-account"></a><span data-ttu-id="09cdc-143">1.    Egy VSO fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="09cdc-143">1.    Obtain a VSO account</span></span>
* <span data-ttu-id="09cdc-144">[A Visual Studio Online-fiók létrehozása] [ Create Visual Studio Online account] Ha még nem rendelkezik már</span><span class="sxs-lookup"><span data-stu-id="09cdc-144">[Create Visual Studio Online account][Create Visual Studio Online account] if you don’t have one already</span></span>
* <span data-ttu-id="09cdc-145">[Hozzon létre csapatprojekt] [ Create team project] a Visual Studio online-fiók</span><span class="sxs-lookup"><span data-stu-id="09cdc-145">[Create team project][Create team project] in your Visual Studio online account</span></span>

### <a name="2----setup-source-control-in-visual-studio"></a><span data-ttu-id="09cdc-146">2.    A Visual Studio Verziókövetés beállítása</span><span class="sxs-lookup"><span data-stu-id="09cdc-146">2.    Setup source control in Visual Studio</span></span>
* <span data-ttu-id="09cdc-147">Csatlakozás tooa csapatprojekt</span><span class="sxs-lookup"><span data-stu-id="09cdc-147">Connect tooa team project</span></span>

![Csatlakozás tooteam projekt][5]

![Válassza ki a csapat projekt tooconnect számára][6]

* <span data-ttu-id="09cdc-150">A projekt toosource vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="09cdc-150">Add your project toosource control</span></span>

![Projekt toosource vezérlő hozzáadása][7]

![Térkép vezérlőelem projekt tooa forrásmappa][8]

* <span data-ttu-id="09cdc-153">Ellenőrizze a projekt csapata Intézőből</span><span class="sxs-lookup"><span data-stu-id="09cdc-153">Check in your project from Team Explorer</span></span>

![A projekt toosource vezérlő ellenőrzése][9]

### <a name="3----configure-build-process"></a><span data-ttu-id="09cdc-155">3.    Konfigurálja a felépítési folyamat</span><span class="sxs-lookup"><span data-stu-id="09cdc-155">3.    Configure Build process</span></span>
* <span data-ttu-id="09cdc-156">Keresse meg a tooyour csapatprojekt és egy új felépítési folyamat sablonok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="09cdc-156">Browse tooyour team project and add a new build process Templates</span></span>

![Adja hozzá egy új build][10]

* <span data-ttu-id="09cdc-158">Jelölje be összeállítási feladat</span><span class="sxs-lookup"><span data-stu-id="09cdc-158">Select build task</span></span>

![Egy összeállítási feladat hozzáadása][11]

![Válassza ki a Visual Studio Build sablon][12]

* <span data-ttu-id="09cdc-161">Összeállítási feladat bemenet szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="09cdc-161">Edit build task input.</span></span> <span data-ttu-id="09cdc-162">Adja a hello build szerint tooyour paramétereit testreszabása</span><span class="sxs-lookup"><span data-stu-id="09cdc-162">Please customize hello build parameters according tooyour need</span></span>

![Összeállítási feladat konfigurálása][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* <span data-ttu-id="09cdc-164">Build változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="09cdc-164">Configure build variables</span></span>

![Build változók konfigurálása][14]

* <span data-ttu-id="09cdc-166">Egy feladat tooupload build eldobási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="09cdc-166">Add a task tooupload build drop</span></span>

![Válassza a build eldobási tevékenység közzététele][15]

![Konfigurálása build eldobási tevékenység közzététele][16]

* <span data-ttu-id="09cdc-169">Hello build futtatása</span><span class="sxs-lookup"><span data-stu-id="09cdc-169">Run hello build</span></span>

![Új várólista létrehozása][17]

![Build összegzésének megtekintése][18]

* <span data-ttu-id="09cdc-172">Ha hello build sikeres látni fogja a eredmény hasonló toobelow</span><span class="sxs-lookup"><span data-stu-id="09cdc-172">if hello build is successful you will see a result similar toobelow</span></span>

![Build eredménye][19]

### <a name="4----configure-release-process"></a><span data-ttu-id="09cdc-174">4.    Kiadás folyamat beállítása</span><span class="sxs-lookup"><span data-stu-id="09cdc-174">4.    Configure Release process</span></span>
* <span data-ttu-id="09cdc-175">Hozzon létre egy új kiadás</span><span class="sxs-lookup"><span data-stu-id="09cdc-175">Create a new release</span></span>

![Hozzon létre új kiadás][20]

* <span data-ttu-id="09cdc-177">Válassza ki a hello Azure Cloud Services telepítési feladat</span><span class="sxs-lookup"><span data-stu-id="09cdc-177">Select hello Azure Cloud Services Deployment task</span></span>

![Válassza ki a szolgáltatástelepítési feladat Azure Cloud Services csomag][21]

* <span data-ttu-id="09cdc-179">Hello tárfiók kulcsa nem ellenőrzése toosource vezérlőben, toospecify hello titkos kulcsot kell diagnosztika bővítmény beállításához.</span><span class="sxs-lookup"><span data-stu-id="09cdc-179">As hello storage account key is not checked in toosource control, we need toospecify hello secret key for setting diagnostics extensions.</span></span> <span data-ttu-id="09cdc-180">Bontsa ki a hello **speciális beállítások új szolgáltatás létrehozása** szakaszt, és szerkesztheti a hello **diagnosztikai Tárfiók kulcsait** bemeneti paraméter.</span><span class="sxs-lookup"><span data-stu-id="09cdc-180">Expand hello **Advanced Options for Creating New Service** section and edit hello **Diagnostics Storage Account Keys** parameter input.</span></span> <span data-ttu-id="09cdc-181">A bemeneti időt vesz igénybe, a kulcs-érték pár hello formátumban több sornyi **[RoleName]:$(StorageAccountKey)**</span><span class="sxs-lookup"><span data-stu-id="09cdc-181">This input takes in multiple lines of key value pair in hello format of **[RoleName]:$(StorageAccountKey)**</span></span>

> <span data-ttu-id="09cdc-182">Megjegyzés: a diagnosztikai tárfiók alatt áll hello ugyanahhoz az előfizetéshez, ahol hello Felhőszolgáltatások alkalmazás közzé szeretné tenni, ha nem rendelkezik tooenter hello kulccsal hello telepítési feladat bemeneti; hello telepítési programozott módon szerezze be a hello tárolással kapcsolatos az előfizetésből</span><span class="sxs-lookup"><span data-stu-id="09cdc-182">Note: if your diagnostics storage account is under hello same subscription as where you will publish hello Cloud Services application, you don't have tooenter hello key in hello deployment task input; hello deployment will programmatically obtain hello storage information from your subscription</span></span>
> 
> 

![Cloud Services telepítési feladat konfigurálása][22]

* <span data-ttu-id="09cdc-184">Használjon titkos kulcs létrehozása változók toosave tárolási kulcsok.</span><span class="sxs-lookup"><span data-stu-id="09cdc-184">Use secret build variables toosave storage Keys.</span></span> <span data-ttu-id="09cdc-185">Adjon meg egy változó titkos kulcsként kattintson a hello lakat ikon hello jobb oldalán hello toomask változók</span><span class="sxs-lookup"><span data-stu-id="09cdc-185">toomask a variable as secret click on hello lock icon on hello right side of hello Variables input</span></span>

![Mentési tároló kulcsok a titkos kulcs változók létrehozása][23]

* <span data-ttu-id="09cdc-187">Egy kiadási létrehozása és telepítése a projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="09cdc-187">Create a release and deploy your project tooAzure</span></span>

![Hozzon létre új kiadás][24]

## <a name="next-steps"></a><span data-ttu-id="09cdc-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09cdc-189">Next steps</span></span>
<span data-ttu-id="09cdc-190">További információ az Azure Cloud Services diagnosztika bővítmények beállítása toolearn talál [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése][Enable diagnostics in Azure Cloud Services using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="09cdc-190">toolearn more about setting diagnostics extensions for Azure Cloud Services, please see [Enable diagnostics in Azure Cloud Services using PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span></span>

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
