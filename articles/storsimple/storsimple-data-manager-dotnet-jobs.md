---
title: "a Microsoft Azure StorSimple adatkezelő feladatok .NET SDK aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse .NET SDK toolaunch StorSimple adatkezelő feladatai (magán előnézetben)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b07fe64369574c994fd28d42786aa02dca435ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-net-sdk-tooinitiate-data-transformation-private-preview"></a><span data-ttu-id="67552-103">Használjon .net SDK hello tooinitiate adatok átalakítása (magán előnézetben)</span><span class="sxs-lookup"><span data-stu-id="67552-103">Use hello .Net SDK tooinitiate data transformation (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="67552-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="67552-104">Overview</span></span>

<span data-ttu-id="67552-105">Ez a cikk ismerteti, hogyan használhatja a hello adatok átalakítása szolgáltatás hello StorSimple adatkezelő szolgáltatás tootransform StorSimple eszközön tárolt adatok belül.</span><span class="sxs-lookup"><span data-stu-id="67552-105">This article explains how you can use hello data transformation feature within hello StorSimple Data Manager service tootransform StorSimple device data.</span></span> <span data-ttu-id="67552-106">hello átalakított adatok majd fel más Azure-szolgáltatásokkal hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="67552-106">hello transformed data is then consumed by other Azure services in hello cloud.</span></span> <span data-ttu-id="67552-107">hello cikk is van olyan forgatókönyv toohelp egy minta .NET console application tooinitiate adatok átalakítási feladat létrehozása, és nyomon követheti azt befejezésére.</span><span class="sxs-lookup"><span data-stu-id="67552-107">hello article also has a walkthrough toohelp create a sample .NET console application tooinitiate a data transformation job and then track it for completion.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67552-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="67552-108">Prerequisites</span></span>

<span data-ttu-id="67552-109">Mielőtt elkezdené, győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="67552-109">Before you begin, ensure that you have:</span></span>
*   <span data-ttu-id="67552-110">A rendszer a Visual Studio 2012, 2013, 2015-öt vagy telepített 2017.</span><span class="sxs-lookup"><span data-stu-id="67552-110">A system with Visual Studio 2012, 2013, 2015, or 2017 installed.</span></span>
*   <span data-ttu-id="67552-111">Az Azure Powershell telepítése.</span><span class="sxs-lookup"><span data-stu-id="67552-111">Azure Powershell installed.</span></span> <span data-ttu-id="67552-112">[Töltse le az Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="67552-112">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="67552-113">Konfigurációs beállítások tooinitialize hello Data Transformation feladat (utasításokat tooobtain ezek a beállítások tartoznak ide).</span><span class="sxs-lookup"><span data-stu-id="67552-113">Configuration settings tooinitialize hello Data Transformation job (instructions tooobtain these settings are included here).</span></span>
*   <span data-ttu-id="67552-114">A feladat definíciójához erőforráscsoporton belül egy hibrid adatforrás, melyhez a megfelelően konfigurált.</span><span class="sxs-lookup"><span data-stu-id="67552-114">A job definition that has been correctly configured in a Hybrid Data Resource within a Resource Group.</span></span>
*   <span data-ttu-id="67552-115">Összes szükséges hello dll-fájl.</span><span class="sxs-lookup"><span data-stu-id="67552-115">All hello required dlls.</span></span> <span data-ttu-id="67552-116">Ezek a DLL fájlok letöltését hello [GitHub-tárházban](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span><span class="sxs-lookup"><span data-stu-id="67552-116">Download these dlls from hello [GitHub repository](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span></span>
*   <span data-ttu-id="67552-117">`Get-ConfigurationParams.ps1`[parancsfájl](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) a hello github-tárházban.</span><span class="sxs-lookup"><span data-stu-id="67552-117">`Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) from hello github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="67552-118">Lépésről lépésre</span><span class="sxs-lookup"><span data-stu-id="67552-118">Step-by-step</span></span>

<span data-ttu-id="67552-119">Hajtsa végre a következő lépéseket toouse .NET toolaunch egy átalakítási feladatot hello.</span><span class="sxs-lookup"><span data-stu-id="67552-119">Perform hello following steps toouse .NET toolaunch a data transformation job.</span></span>

1. <span data-ttu-id="67552-120">tooretrieve hello konfigurációs paramétert is – hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67552-120">tooretrieve hello configuration parameters, do hello following steps:</span></span>
    1. <span data-ttu-id="67552-121">Töltse le a hello `Get-ConfigurationParams.ps1` parancsfájlból hello github tárház a `C:\DataTransformation` helyét.</span><span class="sxs-lookup"><span data-stu-id="67552-121">Download hello `Get-ConfigurationParams.ps1` from hello github repository script in `C:\DataTransformation` location.</span></span>
    1. <span data-ttu-id="67552-122">Futtassa a hello `Get-ConfigurationParams.ps1` hello github-tárházban parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="67552-122">Run hello `Get-ConfigurationParams.ps1` script from hello github repository.</span></span> <span data-ttu-id="67552-123">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="67552-123">Type hello following command:</span></span>

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        <span data-ttu-id="67552-124">ActiveDirectoryKey és AppName átadhatók bármely hello tartozó értékeket.</span><span class="sxs-lookup"><span data-stu-id="67552-124">You can pass in any values for hello ActiveDirectoryKey and AppName.</span></span>


2. <span data-ttu-id="67552-125">Ez a parancsfájl kimenete a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="67552-125">This script outputs hello following values:</span></span>
    * <span data-ttu-id="67552-126">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="67552-126">Client ID</span></span>
    * <span data-ttu-id="67552-127">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="67552-127">Tenant ID</span></span>
    * <span data-ttu-id="67552-128">Active Directory-kulcs (azonos a fent megadott egyik hello)</span><span class="sxs-lookup"><span data-stu-id="67552-128">Active Directory key (same as hello one entered above)</span></span>
    * <span data-ttu-id="67552-129">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="67552-129">Subscription ID</span></span>

3. <span data-ttu-id="67552-130">Visual Studio 2012 használ, 2013 vagy 2015, hozzon létre egy C# .NET konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67552-130">Using Visual Studio 2012, 2013 or 2015, create a C# .NET console application.</span></span>

    1. <span data-ttu-id="67552-131">Indítsa el **Visual Studio 2012 2013 vagy2015/**.</span><span class="sxs-lookup"><span data-stu-id="67552-131">Launch **Visual Studio 2012/2013/2015**.</span></span>
    1. <span data-ttu-id="67552-132">Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="67552-132">Click **File**, point too**New**, and click **Project**.</span></span>
    2. <span data-ttu-id="67552-133">Bontsa ki a **Sablonok** lehetőséget, és válassza a **Visual C#** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="67552-133">Expand **Templates**, and select **Visual C#**.</span></span>
    3. <span data-ttu-id="67552-134">Válassza ki **Konzolalkalmazás** jobb hello projekttípusok hello listája.</span><span class="sxs-lookup"><span data-stu-id="67552-134">Select **Console Application** from hello list of project types on hello right.</span></span>
    4. <span data-ttu-id="67552-135">Adja meg **DataTransformationApp** a hello **neve**.</span><span class="sxs-lookup"><span data-stu-id="67552-135">Enter **DataTransformationApp** for hello **Name**.</span></span>
    5. <span data-ttu-id="67552-136">Válassza ki **C:\DataTransformation** a hello **hely**.</span><span class="sxs-lookup"><span data-stu-id="67552-136">Select **C:\DataTransformation** for hello **Location**.</span></span>
    6. <span data-ttu-id="67552-137">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="67552-137">Click **OK** toocreate hello project.</span></span>

4.  <span data-ttu-id="67552-138">Ezután adja hozzá az összes DLL-fájl szerepel hello [DLL-ek](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) mappában **hivatkozások** létrehozott hello projektben.</span><span class="sxs-lookup"><span data-stu-id="67552-138">Now, add all DLLs present in hello [dlls](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) folder as **References** in hello project that you created.</span></span> <span data-ttu-id="67552-139">toodownload hello dll-fájlok, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="67552-139">toodownload hello dll files, do hello following:</span></span>

    1. <span data-ttu-id="67552-140">A Visual Studióban nyissa meg túl**Nézet > Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="67552-140">In Visual Studio, go too**View > Solution Explorer**.</span></span>
    1. <span data-ttu-id="67552-141">Kattintson a hello toohello balra nyíl adatok átalakítása alkalmazás projekt.</span><span class="sxs-lookup"><span data-stu-id="67552-141">Click hello arrow toohello left of Data Transformation App project.</span></span> <span data-ttu-id="67552-142">Kattintson a **hivatkozások** , és kattintson a jobb gombbal túl**hivatkozás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="67552-142">Click **References** and then right-click too**Add Reference**.</span></span>
    2. <span data-ttu-id="67552-143">Tallózás toohello hello csomagok mappa helyét, válassza ki az összes hello dll-fájl, kattintson a **Hozzáadás**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="67552-143">Browse toohello location of hello packages folder, select all hello DLLs and click **Add**, and then click **OK**.</span></span>

5. <span data-ttu-id="67552-144">Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl (Program.cs) hello projektben.</span><span class="sxs-lookup"><span data-stu-id="67552-144">Add hello following **using** statements toohello source file (Program.cs) in hello project.</span></span>

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. <span data-ttu-id="67552-145">a következő kód hello inicializálja hello az átalakítási feladat példánya.</span><span class="sxs-lookup"><span data-stu-id="67552-145">hello following code initializes hello data transformation job instance.</span></span> <span data-ttu-id="67552-146">Adja hozzá ezt a hello **fő metódus**.</span><span class="sxs-lookup"><span data-stu-id="67552-146">Add this in hello **Main method**.</span></span> <span data-ttu-id="67552-147">Cserélje le a konfigurációs paraméterek, a korábban beszerzett hello értékét.</span><span class="sxs-lookup"><span data-stu-id="67552-147">Replace hello values of configuration parameters as obtained earlier.</span></span> <span data-ttu-id="67552-148">Beépülő modul hello értékének **erőforráscsoport-név** és **hibrid adatok erőforrásnév**.</span><span class="sxs-lookup"><span data-stu-id="67552-148">Plug in hello values of **Resource Group Name** and **Hybrid Data Resource name**.</span></span> <span data-ttu-id="67552-149">Hello **erőforráscsoport-név** egyike hello tároló hello hibrid adatforrás, melyhez a mely hello feladatdefiníció lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="67552-149">hello **Resource Group Name** is hello one that hosts hello Hybrid Data Resource on which hello job definition was configured.</span></span>

    ```
    // Setup hello configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize hello Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. <span data-ttu-id="67552-150">Hello futtassa mely hello a feladat definíciójához kell toobe paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="67552-150">Specify hello parameters with which hello job definition needs toobe run</span></span>

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    <span data-ttu-id="67552-151">(VAGY)</span><span class="sxs-lookup"><span data-stu-id="67552-151">(OR)</span></span>

    <span data-ttu-id="67552-152">Ha futásidőben toochange hello feladat definition paramétereit, majd adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="67552-152">If you want toochange hello job definition parameters during run time, then add hello following code:</span></span>

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of hello volume on hello StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require hello latest existing backup toobe picked else use TakeNow tootrigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of hello StorSimple device.
        DeviceName = "device-name",
        // Name of hello container in Azure storage where hello files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) toobe applied on files under hello root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of hello volume on StorSimple device on which hello relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. <span data-ttu-id="67552-153">Hello inicializálás után adja hozzá a következő kód tootrigger hello feladatdefiníció adatok átalakítása feladatot hello.</span><span class="sxs-lookup"><span data-stu-id="67552-153">After hello initialization, add hello following code tootrigger a data transformation job on hello job definition.</span></span> <span data-ttu-id="67552-154">Beépülő modul hello megfelelő **feladatdefiníció nevét**.</span><span class="sxs-lookup"><span data-stu-id="67552-154">Plug in hello appropriate **Job Definition Name**.</span></span>

    ```
    // Trigger a job, retrieve hello jobId and hello retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. <span data-ttu-id="67552-155">Ez a feladat fel a gyökérkönyvtárban hello hello StorSimple kötet toohello megadott tárolóban található egyező hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="67552-155">This job uploads hello matched files present under hello root directory on hello StorSimple volume toohello specified container.</span></span> <span data-ttu-id="67552-156">Amikor egy fájl feltöltése, egy üzenet megszakad a hello sorban (a hello hello tárolóként ugyanazt a tárfiókot) hello az azonos név hello feladat definíciófrissítések.</span><span class="sxs-lookup"><span data-stu-id="67552-156">When a file is uploaded, a message is dropped in hello queue (in hello same storage account as hello container) with hello same name as hello job definition.</span></span> <span data-ttu-id="67552-157">Ez az üzenet egy eseményindító tooinitiate további hello fájl feldolgozása használható.</span><span class="sxs-lookup"><span data-stu-id="67552-157">This message can be used as a trigger tooinitiate any further processing of hello file.</span></span>

10. <span data-ttu-id="67552-158">Miután hello feladat lett elindítva, vegye fel a hello kód tootrack hello feladat befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="67552-158">Once hello job has been triggered, add hello following code tootrack hello job for completion.</span></span>

    ```
    Job jobDetails = null;

    // Poll hello job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for hello status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of hello job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // toohold hello console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a><span data-ttu-id="67552-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67552-159">Next steps</span></span>

<span data-ttu-id="67552-160">[StorSimple adatokat kezelő felhasználói felületén tootransform használja az adatok](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="67552-160">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
