---
title: aaaUse Azure Automation tootrigger egy feladat |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Azure Automation indítására, a StorSimple adatkezelő feladatok (magán előnézetben)"
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
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 0d9d6e5e6d52ed27ca759ed7e949b5f5dfd319c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-automation-tootrigger-a-job-private-preview"></a><span data-ttu-id="3920b-103">Azure Automation tootrigger egy feladat (magán előnézetben) használata</span><span class="sxs-lookup"><span data-stu-id="3920b-103">Use Azure Automation tootrigger a job (Private Preview)</span></span>

<span data-ttu-id="3920b-104">Ez a cikk ismerteti, hogyan toouse Azure Automation tootrigger egy StorSimple adatkezelő feladat.</span><span class="sxs-lookup"><span data-stu-id="3920b-104">This articles describes how toouse Azure Automation tootrigger a StorSimple Data Manager job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3920b-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3920b-105">Prerequisites</span></span>

<span data-ttu-id="3920b-106">Mielőtt elkezdené, győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="3920b-106">Before you begin, ensure that you have:</span></span>

*   <span data-ttu-id="3920b-107">Az Azure Powershell telepítése.</span><span class="sxs-lookup"><span data-stu-id="3920b-107">Azure Powershell installed.</span></span> <span data-ttu-id="3920b-108">[Töltse le az Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="3920b-108">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="3920b-109">Konfigurációs beállítások tooinitialize hello Data Transformation feladat (utasításokat tooobtain ezek a beállítások tartoznak ide).</span><span class="sxs-lookup"><span data-stu-id="3920b-109">Configuration settings tooinitialize hello Data Transformation job (instructions tooobtain these settings are included here).</span></span>
*   <span data-ttu-id="3920b-110">A feladat definíciójához erőforráscsoporton belül egy hibrid adatforrás, melyhez a megfelelően konfigurált.</span><span class="sxs-lookup"><span data-stu-id="3920b-110">A job definition that has been correctly configured in a Hybrid Data Resource within a resource group.</span></span>
*   <span data-ttu-id="3920b-111">Töltse le `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) hello github-tárházban a fájlt.</span><span class="sxs-lookup"><span data-stu-id="3920b-111">Download `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) file from hello github repository.</span></span>
*   <span data-ttu-id="3920b-112">Töltse le `Get-ConfigurationParams.ps1` [parancsfájl](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) a hello github-tárházban.</span><span class="sxs-lookup"><span data-stu-id="3920b-112">Download `Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) from hello github repository.</span></span>
*   <span data-ttu-id="3920b-113">Töltse le `Trigger-DataTransformation-Job.ps1` [parancsfájl](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) a hello github-tárházban.</span><span class="sxs-lookup"><span data-stu-id="3920b-113">Download `Trigger-DataTransformation-Job.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) from hello github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="3920b-114">Lépésről lépésre</span><span class="sxs-lookup"><span data-stu-id="3920b-114">Step-by-step</span></span>

### <a name="get-azure-active-directory-permissions-for-hello-automation-job-toorun-hello-job-definition"></a><span data-ttu-id="3920b-115">Azure Active Directory-engedélyek hello automation feladat toorun hello feladat definíciójának beolvasása</span><span class="sxs-lookup"><span data-stu-id="3920b-115">Get Azure Active Directory permissions for hello automation job toorun hello job definition</span></span>

1. <span data-ttu-id="3920b-116">tooretrieve hello konfigurációs paramétereket az Active Directory hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3920b-116">tooretrieve hello configuration parameters for Active Directory, do hello following steps:</span></span>

    1. <span data-ttu-id="3920b-117">Nyissa meg a Windows PowerShell a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="3920b-117">Open Windows PowerShell in your local machine.</span></span> <span data-ttu-id="3920b-118">Győződjön meg arról, hogy [Azure PowerShell](https://azure.microsoft.com/downloads/) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3920b-118">Ensure that [Azure PowerShell](https://azure.microsoft.com/downloads/) is installed.</span></span>
    1. <span data-ttu-id="3920b-119">Futtassa a hello `Get-ConfigurationParams.ps1` parancsfájl (a fent letöltött hello mappában).</span><span class="sxs-lookup"><span data-stu-id="3920b-119">Run hello `Get-ConfigurationParams.ps1` script (in hello folder you downloaded above).</span></span> <span data-ttu-id="3920b-120">Írja be a következő parancs hello PowerShell-ablakban hello:</span><span class="sxs-lookup"><span data-stu-id="3920b-120">Type hello following command in hello PowerShell window:</span></span>

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        <span data-ttu-id="3920b-121">hello ActiveDirectoryKey egy jelszót, amely későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="3920b-121">hello ActiveDirectoryKey is a password that you use later.</span></span> <span data-ttu-id="3920b-122">Adjon meg egy tetszőleges jelszót.</span><span class="sxs-lookup"><span data-stu-id="3920b-122">Enter a password of your choice.</span></span> <span data-ttu-id="3920b-123">Alkalmazásnév karakterlánc lehet.</span><span class="sxs-lookup"><span data-stu-id="3920b-123">AppName can be any string.</span></span>

2. <span data-ttu-id="3920b-124">A parancsfájl kimenete a következő értékeket kell használni a hello automatizálási runbook indítására hello.</span><span class="sxs-lookup"><span data-stu-id="3920b-124">This script outputs hello following values that should be used while triggering hello automation runbook.</span></span> <span data-ttu-id="3920b-125">Jegyezze fel ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3920b-125">Make a note of these values.</span></span>

    - <span data-ttu-id="3920b-126">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="3920b-126">Client ID</span></span>
    - <span data-ttu-id="3920b-127">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="3920b-127">Tenant ID</span></span>
    - <span data-ttu-id="3920b-128">Active Directory-kulcs (azonos a fent megadott egyik hello)</span><span class="sxs-lookup"><span data-stu-id="3920b-128">Active Directory key (same as hello one entered above)</span></span>
    - <span data-ttu-id="3920b-129">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="3920b-129">Subscription ID</span></span>

### <a name="set-up-hello-automation-account"></a><span data-ttu-id="3920b-130">Hello Automation-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="3920b-130">Set up hello Automation Account</span></span>

1. <span data-ttu-id="3920b-131">Jelentkezzen be tooAzure, és nyissa meg az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="3920b-131">Log on tooAzure and open your Automation account.</span></span>
2. <span data-ttu-id="3920b-132">Kattintson a **eszközök** csempe tooopen hello eszközök listája.</span><span class="sxs-lookup"><span data-stu-id="3920b-132">Click **Assets** tile tooopen hello list of assets.</span></span>
3. <span data-ttu-id="3920b-133">Kattintson a **modulok** csempe tooopen hello listájához.</span><span class="sxs-lookup"><span data-stu-id="3920b-133">Click **Modules** tile tooopen hello list of modules.</span></span>
4. <span data-ttu-id="3920b-134">Kattintson a **+ a modul hozzá lesz adva** gombra és hello Hozzáadás modul panel nincs elindítva.</span><span class="sxs-lookup"><span data-stu-id="3920b-134">Click **+ Add a module** button and hello Add module blade is launched.</span></span>

    ![Automation-fiók beállításai](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. <span data-ttu-id="3920b-136">Miután kiválasztotta a hello `DataTransformationApp.zip` fájlt a helyi számítógépről, kattintson **OK** tooimport hello modul.</span><span class="sxs-lookup"><span data-stu-id="3920b-136">After you have selected hello `DataTransformationApp.zip` file from your local computer, click **OK** tooimport hello module.</span></span>

   <span data-ttu-id="3920b-137">Azure Automation importálására a modul tooyour fiókot, ha hello modullal kapcsolatos metaadatok bontja ki.</span><span class="sxs-lookup"><span data-stu-id="3920b-137">When Azure Automation imports a module tooyour account, it extracts metadata about hello module.</span></span> <span data-ttu-id="3920b-138">Ez a művelet eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="3920b-138">This operation may take a couple of minutes.</span></span>

   ![Automation-fiók beállításai](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. <span data-ttu-id="3920b-140">Értesítést kaphat, hogy hello modul telepítése és egy másik értesítés hello folyamat be nem fejeződik.</span><span class="sxs-lookup"><span data-stu-id="3920b-140">You receive a notification that hello module is being deployed and another notification when hello process is complete.</span></span>  <span data-ttu-id="3920b-141">Ellenőrizheti a hello állapota **modulok** csempére.</span><span class="sxs-lookup"><span data-stu-id="3920b-141">You can also check hello status in **Modules** tile.</span></span>

### <a name="tooimport-hello-runbook-that-triggers-hello-job-definition"></a><span data-ttu-id="3920b-142">hello feladatdefiníció kiváltó tooimport hello runbook</span><span class="sxs-lookup"><span data-stu-id="3920b-142">tooimport hello runbook that triggers hello job definition</span></span>

1. <span data-ttu-id="3920b-143">Hello Azure-portálon nyissa meg az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="3920b-143">In hello Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="3920b-144">Kattintson a **Runbookok** csempe tooopen hello forgatókönyvek listája.</span><span class="sxs-lookup"><span data-stu-id="3920b-144">Click **Runbooks** tile tooopen hello list of runbooks.</span></span>
3. <span data-ttu-id="3920b-145">Kattintson a **+ Hozzáadás runbook** , majd **meglévő forgatókönyv importálása**.</span><span class="sxs-lookup"><span data-stu-id="3920b-145">Click **+ Add a runbook** and then **Import an existing runbook**.</span></span>

   ![Meglévő forgatókönyv importálása](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. <span data-ttu-id="3920b-147">Kattintson a **Runbook fájl** és select hello fájl tooimport `Trigger-DataTransformation-Job.ps1`.</span><span class="sxs-lookup"><span data-stu-id="3920b-147">Click **Runbook file** and select hello file tooimport `Trigger-DataTransformation-Job.ps1`.</span></span>
5. <span data-ttu-id="3920b-148">Kattintson a **létrehozása** tooimport hello runbook.</span><span class="sxs-lookup"><span data-stu-id="3920b-148">Click **Create** tooimport hello runbook.</span></span> <span data-ttu-id="3920b-149">hello új runbook megjelenik hello runbookokat hello Automation-fiók esetében.</span><span class="sxs-lookup"><span data-stu-id="3920b-149">hello new runbook appears in hello list of runbooks for hello Automation account.</span></span>
7. <span data-ttu-id="3920b-150">Kattintson a **eseményindító-DataTransformation-feladat** runbookot, majd **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="3920b-150">Click **Trigger-DataTransformation-Job** runbook and then click **Edit**.</span></span>
8. <span data-ttu-id="3920b-151">Kattintson a **közzététel** , majd **Igen** során a rendszer megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="3920b-151">Click **Publish** and then **Yes** when prompted for confirmation.</span></span>


### <a name="toorun-hello-runbook"></a><span data-ttu-id="3920b-152">toorun hello runbook:</span><span class="sxs-lookup"><span data-stu-id="3920b-152">toorun hello runbook:</span></span>
1. <span data-ttu-id="3920b-153">Hello Azure-portálon nyissa meg az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="3920b-153">In hello Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="3920b-154">Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.</span><span class="sxs-lookup"><span data-stu-id="3920b-154">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>
3. <span data-ttu-id="3920b-155">Kattintson a **eseményindító-DataTransformation-feladat**.</span><span class="sxs-lookup"><span data-stu-id="3920b-155">Click **Trigger-DataTransformation-Job**.</span></span>
4. <span data-ttu-id="3920b-156">Kattintson a **Start** toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="3920b-156">Click **Start** toostart hello runbook.</span></span>

   ![Forgatókönyv indítása](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. <span data-ttu-id="3920b-158">A hello **indítsa el a runbook** panelen adja meg az összes hello paramétereket.</span><span class="sxs-lookup"><span data-stu-id="3920b-158">In hello **Start runbook** blade, enter all hello parameters.</span></span> <span data-ttu-id="3920b-159">Kattintson a **OK** toosubmit hello Data Transformation feladat.</span><span class="sxs-lookup"><span data-stu-id="3920b-159">Click **OK** toosubmit hello Data Transformation job.</span></span>

   ![Forgatókönyv indítása](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a><span data-ttu-id="3920b-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3920b-161">Next steps</span></span>

<span data-ttu-id="3920b-162">[StorSimple adatokat kezelő felhasználói felületén tootransform használja az adatok](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="3920b-162">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
