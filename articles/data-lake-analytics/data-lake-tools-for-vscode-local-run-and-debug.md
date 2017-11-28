---
title: "Az Azure Data Lake Tools: U-SQL helyi futtatásával és a Visual Studio Code helyi hibakeresési |} Microsoft Docs"
description: "Útmutató: Azure Data Lake Tools használandó Visual Studio Code helyi Futtatás és helyi hibakeresési."
Keywords: "VScode, az Azure Data Lake Tools, a helyi futtatáskor, helyi hibakeresése a helyi debug preview tárolófájl feltölteni. tárolási elérési útja"
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
ms.openlocfilehash: 367e4ba792f83d6ee246208306e4c09b69cb49ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a><span data-ttu-id="12f70-104">U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12f70-104">U-SQL local run and local debug with Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12f70-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="12f70-105">Prerequisites</span></span>
<span data-ttu-id="12f70-106">Győződjön meg arról, hogy már működik a következő előfeltételek vonatkoznak a ezek az eljárások megkezdése előtt:</span><span class="sxs-lookup"><span data-stu-id="12f70-106">Make sure you have the following prerequisites in place before you start these procedures:</span></span>
- <span data-ttu-id="12f70-107">Az Azure Data Lake Visual Studio Code eszköz.</span><span class="sxs-lookup"><span data-stu-id="12f70-107">Azure Data Lake Tool for Visual Studio Code.</span></span> <span data-ttu-id="12f70-108">Útmutatásért lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="12f70-108">For instructions, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="12f70-109">C# a Visual Studio Code (Ha el kíván végezni egy U-SQL helyi hibakeresési).</span><span class="sxs-lookup"><span data-stu-id="12f70-109">C# for Visual Studio Code (if you want to perform a U-SQL local debug).</span></span>

   ![Telepítse C# a Data Lake Tools for Visual Studio kód](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > <span data-ttu-id="12f70-111">A U-SQL helyi futtatása és a hibakeresési szolgáltatások jelenleg csak Windows felhasználók támogatására.</span><span class="sxs-lookup"><span data-stu-id="12f70-111">The U-SQL local run and debug features currently only support Windows users.</span></span> 


## <a name="set-up-the-u-sql-local-run-environment"></a><span data-ttu-id="12f70-112">A U-SQL helyi futtatási környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="12f70-112">Set up the U-SQL local run environment</span></span>

1. <span data-ttu-id="12f70-113">Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, és írja be a **ADL: letöltése LocalRun függőségi** letöltése a csomagok.</span><span class="sxs-lookup"><span data-stu-id="12f70-113">Select Ctrl+Shift+P to open the command palette, and then enter **ADL: Download LocalRun Dependency** to download the packages.</span></span>  

   ![Töltse le az ADL LocalRun függőségi csomagok](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. <span data-ttu-id="12f70-115">Keresse meg a függőségi csomagok látható elérési útjáról a **kimeneti** ablaktáblán, majd telepítse BuildTools és Win10SDK 10240.</span><span class="sxs-lookup"><span data-stu-id="12f70-115">Locate the dependency packages from the path shown in the **Output** pane, and then install BuildTools and Win10SDK 10240.</span></span> <span data-ttu-id="12f70-116">Íme egy példa elérési útja:</span><span class="sxs-lookup"><span data-stu-id="12f70-116">Here is an example path:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  <span data-ttu-id="12f70-117">![Keresse meg a függőségi csomagok](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span><span class="sxs-lookup"><span data-stu-id="12f70-117">![Locate the dependency packages](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span></span>

   <span data-ttu-id="12f70-118">a.</span><span class="sxs-lookup"><span data-stu-id="12f70-118">a.</span></span> <span data-ttu-id="12f70-119">BuildTools telepítéséhez kövesse a varázsló utasításait.</span><span class="sxs-lookup"><span data-stu-id="12f70-119">To install BuildTools, follow the wizard instructions.</span></span>   

  ![BuildTools telepítése](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   <span data-ttu-id="12f70-121">b.</span><span class="sxs-lookup"><span data-stu-id="12f70-121">b.</span></span> <span data-ttu-id="12f70-122">Win10SDK 10240 telepítéséhez kövesse a varázsló utasításait.</span><span class="sxs-lookup"><span data-stu-id="12f70-122">To install Win10SDK 10240, follow the wizard instructions.</span></span>  

  ![Telepítse a Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. <span data-ttu-id="12f70-124">A környezeti változó beállítása.</span><span class="sxs-lookup"><span data-stu-id="12f70-124">Set up the environment variable.</span></span> <span data-ttu-id="12f70-125">Állítsa be a **SCOPE_CPP_SDK** környezeti változót:</span><span class="sxs-lookup"><span data-stu-id="12f70-125">Set the **SCOPE_CPP_SDK** environment variable to:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. <span data-ttu-id="12f70-126">Az operációs rendszer, győződjön meg arról, hogy a környezetiváltozó-beállításainak érvénybe léptetéséhez indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="12f70-126">Restart the OS to make sure that the environment variable settings take effect.</span></span>  

   ![Győződjön meg arról, a SCOPE_CPP_SDK környezeti változó telepítve van](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a><span data-ttu-id="12f70-128">Indítsa el a helyi Futtatás szolgáltatást, és küldje el a U-SQL helyi fiók feladat</span><span class="sxs-lookup"><span data-stu-id="12f70-128">Start the local run service and submit the U-SQL job to a local account</span></span> 
<span data-ttu-id="12f70-129">Az első felhasználó kéri töltse le az ADL: letöltése LocalRun függőségi csomagok, ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="12f70-129">For the first-time user, you are prompted to download the ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
1. <span data-ttu-id="12f70-130">Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, és írja be a **ADL: helyi futtatása szolgáltatás indítása**.</span><span class="sxs-lookup"><span data-stu-id="12f70-130">Select Ctrl+Shift+P to open the command palette, and then enter **ADL: Start Local Run Service**.</span></span>
2. <span data-ttu-id="12f70-131">Válassza ki **elfogadás** először a Microsoft szoftverlicenc-szerződést fogadásához.</span><span class="sxs-lookup"><span data-stu-id="12f70-131">Select **Accept** to accept the Microsoft Software License Terms for the first time.</span></span> 

   ![A Microsoft szoftverlicenc-feltételek elfogadása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. <span data-ttu-id="12f70-133">Az cmd-konzol megnyitása.</span><span class="sxs-lookup"><span data-stu-id="12f70-133">The cmd console opens.</span></span> <span data-ttu-id="12f70-134">A felhasználók első alkalommal meg kell adnia **3**, és keresse meg a bemeneti és kimeneti a helyi mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="12f70-134">For first-time users, you need to enter **3**, and then locate the local folder path for your data input and output.</span></span> <span data-ttu-id="12f70-135">Egyéb lehetőségek használhatja az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="12f70-135">For other options, you can use the default values.</span></span> 

   ![A Data Lake Tools for Visual Studio Code helyi futtatási cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. <span data-ttu-id="12f70-137">Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, adja meg **ADL: feladat elküldése**, majd válassza ki **helyi** elküldeni a feladatot a helyi fiókba.</span><span class="sxs-lookup"><span data-stu-id="12f70-137">Select Ctrl+Shift+P to open the command palette, enter **ADL: Submit Job**, and then select **Local** to submit the job to your local account.</span></span>

   ![A Data Lake Tools for Visual Studio Code helyi kiválasztása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. <span data-ttu-id="12f70-139">A feladat elküldése után megtekintheti a küldésének részleteit.</span><span class="sxs-lookup"><span data-stu-id="12f70-139">After you submit the job, you can view the submission details.</span></span> <span data-ttu-id="12f70-140">A küldése részleteinek kiválasztása nézetre **jobUrl** a a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="12f70-140">To view the submission details select **jobUrl** in the **Output** window.</span></span> <span data-ttu-id="12f70-141">A feladatállapot küldésének a cmd-konzolról is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="12f70-141">You can also view the job submission status from the cmd console.</span></span> <span data-ttu-id="12f70-142">Adja meg **7** a cmd-konzolon, ha szeretné tudni, hogy több feladat részleteit.</span><span class="sxs-lookup"><span data-stu-id="12f70-142">Enter **7** in the cmd console if you want to know more job details.</span></span>

   <span data-ttu-id="12f70-143">![A Data Lake Visual Studio Code helyi futtatáskor a kimeneti eszközei](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools for Visual Studio Code helyi futtatáskor a cmd állapota](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span><span class="sxs-lookup"><span data-stu-id="12f70-143">![Data Lake Tools for Visual Studio Code local run output](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
![Data Lake Tools for Visual Studio Code local run cmd status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span></span> 


## <a name="start-a-local-debug-for-the-u-sql-job"></a><span data-ttu-id="12f70-144">Egy helyi hibakeresési a U-SQL-feladat indítása</span><span class="sxs-lookup"><span data-stu-id="12f70-144">Start a local debug for the U-SQL job</span></span>  
<span data-ttu-id="12f70-145">Az első felhasználó kéri töltse le az ADL: letöltése LocalRun függőségi csomagok, ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="12f70-145">For the first-time user, you are prompted to download the ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
  
1. <span data-ttu-id="12f70-146">Válassza ki a Ctrl + Shift + P nyissa meg a parancs palettát, és írja be a **ADL: helyi futtatása szolgáltatás indítása**.</span><span class="sxs-lookup"><span data-stu-id="12f70-146">Select Ctrl+Shift+P to open the command palette, and then enter **ADL: Start Local Run Service**.</span></span> <span data-ttu-id="12f70-147">Az cmd-konzol megnyitása.</span><span class="sxs-lookup"><span data-stu-id="12f70-147">The cmd console opens.</span></span> <span data-ttu-id="12f70-148">Győződjön meg arról, hogy a **DataRoot** van beállítva.</span><span class="sxs-lookup"><span data-stu-id="12f70-148">Make sure that the **DataRoot** is set.</span></span>
3. <span data-ttu-id="12f70-149">A C# háttérkód állítható be töréspont.</span><span class="sxs-lookup"><span data-stu-id="12f70-149">Set a breakpoint in your C# code-behind.</span></span>
4. <span data-ttu-id="12f70-150">Vissza a parancsprogram-szerkesztő, válassza ki a Ctrl + Shift + P a parancs konzol megnyitásához, és írja be **helyi hibakeresése** a helyi hibakeresési szolgáltatás elindításához.</span><span class="sxs-lookup"><span data-stu-id="12f70-150">Back in the script editor, select Ctrl+Shift+P to open the command console, and then enter **Local Debug** to start your local debug service.</span></span>

![A Data Lake Tools for Visual Studio Code helyi hibakeresési eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a><span data-ttu-id="12f70-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12f70-152">Next steps</span></span>
- <span data-ttu-id="12f70-153">Azure Data Lake Tools használatával a Visual Studio Code, lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="12f70-153">For using Azure Data Lake Tools for Visual Studio Code, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="12f70-154">A Data Lake Analytics elindított adatainak lekérése, lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="12f70-154">For getting started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="12f70-155">A Data Lake Tools for Visual Studio információ: [oktatóanyag: fejlesztése U-SQL-parancsfájlok Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="12f70-155">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="12f70-156">A szerelvények fejlesztésével információkért lásd: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="12f70-156">For the information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>
