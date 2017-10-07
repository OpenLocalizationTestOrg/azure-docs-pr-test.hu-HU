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
# <a name="use-hello-net-sdk-tooinitiate-data-transformation-private-preview"></a>Használjon .net SDK hello tooinitiate adatok átalakítása (magán előnézetben)

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti, hogyan használhatja a hello adatok átalakítása szolgáltatás hello StorSimple adatkezelő szolgáltatás tootransform StorSimple eszközön tárolt adatok belül. hello átalakított adatok majd fel más Azure-szolgáltatásokkal hello felhőben. hello cikk is van olyan forgatókönyv toohelp egy minta .NET console application tooinitiate adatok átalakítási feladat létrehozása, és nyomon követheti azt befejezésére.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, győződjön meg arról, hogy:
*   A rendszer a Visual Studio 2012, 2013, 2015-öt vagy telepített 2017.
*   Az Azure Powershell telepítése. [Töltse le az Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfigurációs beállítások tooinitialize hello Data Transformation feladat (utasításokat tooobtain ezek a beállítások tartoznak ide).
*   A feladat definíciójához erőforráscsoporton belül egy hibrid adatforrás, melyhez a megfelelően konfigurált.
*   Összes szükséges hello dll-fájl. Ezek a DLL fájlok letöltését hello [GitHub-tárházban](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[parancsfájl](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) a hello github-tárházban.

## <a name="step-by-step"></a>Lépésről lépésre

Hajtsa végre a következő lépéseket toouse .NET toolaunch egy átalakítási feladatot hello.

1. tooretrieve hello konfigurációs paramétert is – hello a következő lépéseket:
    1. Töltse le a hello `Get-ConfigurationParams.ps1` parancsfájlból hello github tárház a `C:\DataTransformation` helyét.
    1. Futtassa a hello `Get-ConfigurationParams.ps1` hello github-tárházban parancsfájlt. Írja be a következő parancs hello:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        ActiveDirectoryKey és AppName átadhatók bármely hello tartozó értékeket.


2. Ez a parancsfájl kimenete a következő értékek hello:
    * Ügyfél-azonosító
    * Bérlőazonosító
    * Active Directory-kulcs (azonos a fent megadott egyik hello)
    * Előfizetés azonosítója

3. Visual Studio 2012 használ, 2013 vagy 2015, hozzon létre egy C# .NET konzolalkalmazást.

    1. Indítsa el **Visual Studio 2012 2013 vagy2015/**.
    1. Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.
    2. Bontsa ki a **Sablonok** lehetőséget, és válassza a **Visual C#** lehetőséget.
    3. Válassza ki **Konzolalkalmazás** jobb hello projekttípusok hello listája.
    4. Adja meg **DataTransformationApp** a hello **neve**.
    5. Válassza ki **C:\DataTransformation** a hello **hely**.
    6. Kattintson a **OK** toocreate hello projekt.

4.  Ezután adja hozzá az összes DLL-fájl szerepel hello [DLL-ek](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) mappában **hivatkozások** létrehozott hello projektben. toodownload hello dll-fájlok, a következő hello:

    1. A Visual Studióban nyissa meg túl**Nézet > Megoldáskezelőben**.
    1. Kattintson a hello toohello balra nyíl adatok átalakítása alkalmazás projekt. Kattintson a **hivatkozások** , és kattintson a jobb gombbal túl**hivatkozás hozzáadása**.
    2. Tallózás toohello hello csomagok mappa helyét, válassza ki az összes hello dll-fájl, kattintson a **Hozzáadás**, és kattintson a **OK**.

5. Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl (Program.cs) hello projektben.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. a következő kód hello inicializálja hello az átalakítási feladat példánya. Adja hozzá ezt a hello **fő metódus**. Cserélje le a konfigurációs paraméterek, a korábban beszerzett hello értékét. Beépülő modul hello értékének **erőforráscsoport-név** és **hibrid adatok erőforrásnév**. Hello **erőforráscsoport-név** egyike hello tároló hello hibrid adatforrás, melyhez a mely hello feladatdefiníció lett konfigurálva.

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

7. Hello futtassa mely hello a feladat definíciójához kell toobe paraméterek megadása

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (VAGY)

    Ha futásidőben toochange hello feladat definition paramétereit, majd adja hozzá a következő kód hello:

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

8. Hello inicializálás után adja hozzá a következő kód tootrigger hello feladatdefiníció adatok átalakítása feladatot hello. Beépülő modul hello megfelelő **feladatdefiníció nevét**.

    ```
    // Trigger a job, retrieve hello jobId and hello retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. Ez a feladat fel a gyökérkönyvtárban hello hello StorSimple kötet toohello megadott tárolóban található egyező hello fájlokat. Amikor egy fájl feltöltése, egy üzenet megszakad a hello sorban (a hello hello tárolóként ugyanazt a tárfiókot) hello az azonos név hello feladat definíciófrissítések. Ez az üzenet egy eseményindító tooinitiate további hello fájl feldolgozása használható.

10. Miután hello feladat lett elindítva, vegye fel a hello kód tootrack hello feladat befejezését követően.

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


## <a name="next-steps"></a>Következő lépések

[StorSimple adatokat kezelő felhasználói felületén tootransform használja az adatok](storsimple-data-manager-ui.md).
