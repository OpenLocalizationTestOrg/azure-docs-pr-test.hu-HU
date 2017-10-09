---
title: "Az Azure Data Lake Tools: U-SQL helyi futtatásával és a Visual Studio Code helyi hibakeresési |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio Code toolocal lefutott, és helyi hibakeresése."
Keywords: "VScode, az Azure Data Lake Tools, a helyi futtatáskor, helyi hibakeresése a helyi debug preview tárolófájl feltöltése toostorage elérési útja"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: fb152f07fe8c4b03dde8fb8e62c7475eccda0578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a><span data-ttu-id="669cd-104">U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="669cd-104">U-SQL local run and local debug with Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="669cd-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="669cd-105">Prerequisites</span></span>
<span data-ttu-id="669cd-106">Győződjön meg arról, hogy ezek az eljárások megkezdése előtt a következő előfeltételek teljesülése hello:</span><span class="sxs-lookup"><span data-stu-id="669cd-106">Make sure you have hello following prerequisites in place before you start these procedures:</span></span>
- <span data-ttu-id="669cd-107">Az Azure Data Lake Visual Studio Code eszköz.</span><span class="sxs-lookup"><span data-stu-id="669cd-107">Azure Data Lake Tool for Visual Studio Code.</span></span> <span data-ttu-id="669cd-108">Útmutatásért lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="669cd-108">For instructions, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="669cd-109">C# a Visual Studio Code (Ha azt szeretné, hogy a helyi hibakeresési tooperform egy U-SQL).</span><span class="sxs-lookup"><span data-stu-id="669cd-109">C# for Visual Studio Code (if you want tooperform a U-SQL local debug).</span></span>

   ![Telepítse C# a Data Lake Tools for Visual Studio kód](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > <span data-ttu-id="669cd-111">U-SQL helyi futtatása hello és hibakeresési szolgáltatások jelenleg csak Windows felhasználók támogatására.</span><span class="sxs-lookup"><span data-stu-id="669cd-111">hello U-SQL local run and debug features currently only support Windows users.</span></span> 


## <a name="set-up-hello-u-sql-local-run-environment"></a><span data-ttu-id="669cd-112">Hello U-SQL helyi futtatási környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="669cd-112">Set up hello U-SQL local run environment</span></span>

1. <span data-ttu-id="669cd-113">Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, és írja be **ADL: letöltése LocalRun függőségi** toodownload hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="669cd-113">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Download LocalRun Dependency** toodownload hello packages.</span></span>  

   ![Hello ADL LocalRun függőségi csomagok letöltése](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. <span data-ttu-id="669cd-115">Keresse meg a hello függőségi csomagok hello látható hello elérési útról **kimeneti** ablaktáblán, majd telepítse BuildTools és Win10SDK 10240.</span><span class="sxs-lookup"><span data-stu-id="669cd-115">Locate hello dependency packages from hello path shown in hello **Output** pane, and then install BuildTools and Win10SDK 10240.</span></span> <span data-ttu-id="669cd-116">Íme egy példa elérési útja:</span><span class="sxs-lookup"><span data-stu-id="669cd-116">Here is an example path:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  <span data-ttu-id="669cd-117">![Keresse meg a hello függőségi csomagok](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span><span class="sxs-lookup"><span data-stu-id="669cd-117">![Locate hello dependency packages](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span></span>

   <span data-ttu-id="669cd-118">a.</span><span class="sxs-lookup"><span data-stu-id="669cd-118">a.</span></span> <span data-ttu-id="669cd-119">tooinstall BuildTools, hello varázsló utasításait követve.</span><span class="sxs-lookup"><span data-stu-id="669cd-119">tooinstall BuildTools, follow hello wizard instructions.</span></span>   

  ![BuildTools telepítése](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   <span data-ttu-id="669cd-121">b.</span><span class="sxs-lookup"><span data-stu-id="669cd-121">b.</span></span> <span data-ttu-id="669cd-122">tooinstall Win10SDK 10240 hello varázsló utasításait követve.</span><span class="sxs-lookup"><span data-stu-id="669cd-122">tooinstall Win10SDK 10240, follow hello wizard instructions.</span></span>  

  ![Telepítse a Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. <span data-ttu-id="669cd-124">Hello környezeti változó beállítása.</span><span class="sxs-lookup"><span data-stu-id="669cd-124">Set up hello environment variable.</span></span> <span data-ttu-id="669cd-125">Set hello **SCOPE_CPP_SDK** környezeti változót:</span><span class="sxs-lookup"><span data-stu-id="669cd-125">Set hello **SCOPE_CPP_SDK** environment variable to:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. <span data-ttu-id="669cd-126">Az operációs rendszer hello toomake meg arról, hogy hello környezetiváltozó-beállításainak érvénybe léptetéséhez indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="669cd-126">Restart hello OS toomake sure that hello environment variable settings take effect.</span></span>  

   ![Győződjön meg arról, hello SCOPE_CPP_SDK környezeti változó telepítve van](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-hello-local-run-service-and-submit-hello-u-sql-job-tooa-local-account"></a><span data-ttu-id="669cd-128">Hello helyi futtatási szolgáltatás elindítása, és küldje el a hello U-SQL projekt tooa helyi fiók</span><span class="sxs-lookup"><span data-stu-id="669cd-128">Start hello local run service and submit hello U-SQL job tooa local account</span></span> 
<span data-ttu-id="669cd-129">Hello első felhasználó esetében áll felszólító toodownload hello ADL: letöltése LocalRun függőségi csomagok, ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="669cd-129">For hello first-time user, you are prompted toodownload hello ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
1. <span data-ttu-id="669cd-130">Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, és írja be **ADL: helyi futtatása szolgáltatás indítása**.</span><span class="sxs-lookup"><span data-stu-id="669cd-130">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Start Local Run Service**.</span></span>
2. <span data-ttu-id="669cd-131">Válassza ki **elfogadás** tooaccept hello Microsoft szoftverlicenc-szerződés hello első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="669cd-131">Select **Accept** tooaccept hello Microsoft Software License Terms for hello first time.</span></span> 

   ![Hello Microsoft szoftverlicenc-feltételek elfogadása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. <span data-ttu-id="669cd-133">hello cmd konzol megnyitása.</span><span class="sxs-lookup"><span data-stu-id="669cd-133">hello cmd console opens.</span></span> <span data-ttu-id="669cd-134">A felhasználók első alkalommal kell tooenter **3**, és keresse meg a bemeneti és kimeneti adatok hello helyi mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="669cd-134">For first-time users, you need tooenter **3**, and then locate hello local folder path for your data input and output.</span></span> <span data-ttu-id="669cd-135">Egyéb lehetőségek hello alapértelmezett értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="669cd-135">For other options, you can use hello default values.</span></span> 

   ![A Data Lake Tools for Visual Studio Code helyi futtatási cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. <span data-ttu-id="669cd-137">Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, adja meg **ADL: feladat elküldése**, majd válassza ki **helyi** toosubmit hello feladat tooyour helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="669cd-137">Select Ctrl+Shift+P tooopen hello command palette, enter **ADL: Submit Job**, and then select **Local** toosubmit hello job tooyour local account.</span></span>

   ![A Data Lake Tools for Visual Studio Code helyi kiválasztása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. <span data-ttu-id="669cd-139">Hello feladat elküldése után megtekintheti a hello küldésének adatokat.</span><span class="sxs-lookup"><span data-stu-id="669cd-139">After you submit hello job, you can view hello submission details.</span></span> <span data-ttu-id="669cd-140">tooview hello küldésének részleteinek kiválasztása **jobUrl** a hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="669cd-140">tooview hello submission details select **jobUrl** in hello **Output** window.</span></span> <span data-ttu-id="669cd-141">Küldésének feladatállapot hello hello cmd konzolról is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="669cd-141">You can also view hello job submission status from hello cmd console.</span></span> <span data-ttu-id="669cd-142">Adja meg **7** hello cmd konzolon Ha azt szeretné, hogy tooknow több feladat részletei.</span><span class="sxs-lookup"><span data-stu-id="669cd-142">Enter **7** in hello cmd console if you want tooknow more job details.</span></span>

   <span data-ttu-id="669cd-143">![A Data Lake Visual Studio Code helyi futtatáskor a kimeneti eszközei](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools for Visual Studio Code helyi futtatáskor a cmd állapota](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span><span class="sxs-lookup"><span data-stu-id="669cd-143">![Data Lake Tools for Visual Studio Code local run output](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
![Data Lake Tools for Visual Studio Code local run cmd status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span></span> 


## <a name="start-a-local-debug-for-hello-u-sql-job"></a><span data-ttu-id="669cd-144">Egy helyi hibakeresési hello U-SQL-feladat indítása</span><span class="sxs-lookup"><span data-stu-id="669cd-144">Start a local debug for hello U-SQL job</span></span>  
<span data-ttu-id="669cd-145">Hello első felhasználó esetében áll felszólító toodownload hello ADL: letöltése LocalRun függőségi csomagok, ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="669cd-145">For hello first-time user, you are prompted toodownload hello ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
  
1. <span data-ttu-id="669cd-146">Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, és írja be **ADL: helyi futtatása szolgáltatás indítása**.</span><span class="sxs-lookup"><span data-stu-id="669cd-146">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Start Local Run Service**.</span></span> <span data-ttu-id="669cd-147">hello cmd konzol megnyitása.</span><span class="sxs-lookup"><span data-stu-id="669cd-147">hello cmd console opens.</span></span> <span data-ttu-id="669cd-148">Győződjön meg arról, hogy hello **DataRoot** van beállítva.</span><span class="sxs-lookup"><span data-stu-id="669cd-148">Make sure that hello **DataRoot** is set.</span></span>
3. <span data-ttu-id="669cd-149">A C# háttérkód állítható be töréspont.</span><span class="sxs-lookup"><span data-stu-id="669cd-149">Set a breakpoint in your C# code-behind.</span></span>
4. <span data-ttu-id="669cd-150">Hello parancsprogram-szerkesztő, jelölje ki Ctrl + Shift + P tooopen hello parancskonzolról, és írja be **helyi hibakeresése** toostart a helyi hibakeresési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="669cd-150">Back in hello script editor, select Ctrl+Shift+P tooopen hello command console, and then enter **Local Debug** toostart your local debug service.</span></span>

![A Data Lake Tools for Visual Studio Code helyi hibakeresési eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a><span data-ttu-id="669cd-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="669cd-152">Next steps</span></span>
- <span data-ttu-id="669cd-153">Azure Data Lake Tools használatával a Visual Studio Code, lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="669cd-153">For using Azure Data Lake Tools for Visual Studio Code, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="669cd-154">A Data Lake Analytics elindított adatainak lekérése, lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="669cd-154">For getting started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="669cd-155">A Data Lake Tools for Visual Studio információ: [oktatóanyag: fejlesztése U-SQL-parancsfájlok Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="669cd-155">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="669cd-156">Hello információ szerelvények fejlesztésével: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="669cd-156">For hello information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>
