---
title: "U-SQL feladatok hibakeresése |} Microsoft Docs"
description: "Útmutató egy U-SQL Visual Studio használatával sikertelen csúcspont hibakereséséhez."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a><span data-ttu-id="7e337-103">Felhasználó által definiált C# programkódja sikertelen U-SQL feladatok hibakeresése</span><span class="sxs-lookup"><span data-stu-id="7e337-103">Debug user-defined C# code for failed U-SQL jobs</span></span>

<span data-ttu-id="7e337-104">U-SQL egy bővíthetőségi modell használatával C#, így a kód hozzáadása a Funkciók, például egy egyéni készülék vagy nyomáscsökkentő írhat biztosít.</span><span class="sxs-lookup"><span data-stu-id="7e337-104">U-SQL provides an extensibility model using C#, so you can write your code to add functionality such as a custom extractor or reducer.</span></span> <span data-ttu-id="7e337-105">További tudnivalókért lásd: [U-SQL programozástámogatási útmutató](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span><span class="sxs-lookup"><span data-stu-id="7e337-105">To learn more, see [U-SQL programmability guide](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span></span> <span data-ttu-id="7e337-106">A gyakorlatban kódok hibakeresés esetleg, és a big data-rendszereket csupán korlátozott futási hibakeresési információk, például a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="7e337-106">In practice any code may need debugging, and big data systems may only provide limited runtime debugging information such as log files.</span></span>

<span data-ttu-id="7e337-107">Az Azure Data Lake Tools for Visual Studio nevű funkcióval rendelkezik **csúcspont Debug nem sikerült**, amellyel klónozza a felhőből sikertelen feladat a helyi számítógépen a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="7e337-107">Azure Data Lake Tools for Visual Studio provides a feature called **Failed Vertex Debug**, which lets you clone a failed job from the cloud to your local machine for debugging.</span></span> <span data-ttu-id="7e337-108">A helyi másolat a teljes felhőkörnyezetben, beleértve a bemeneti adatok és a felhasználói kód rögzíti.</span><span class="sxs-lookup"><span data-stu-id="7e337-108">The local clone captures the entire cloud environment, including any input data and user code.</span></span>

<span data-ttu-id="7e337-109">A következő videó bemutatja a csúcspont Debug nem sikerült Azure Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e337-109">The following video demonstrates Failed Vertex Debug in Azure Data Lake Tools for Visual Studio.</span></span>

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> <span data-ttu-id="7e337-110">A Visual Studio a következő két frissítésekre van szüksége, ha még nincs telepítve: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) és a [Universal C futásidejű for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span><span class="sxs-lookup"><span data-stu-id="7e337-110">Visual Studio requires the following two updates, if they are not already installed: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) and the [Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span></span>

## <a name="download-failed-vertex-to-local-machine"></a><span data-ttu-id="7e337-111">Nem sikerült letölteni a csúcspont helyi géphez</span><span class="sxs-lookup"><span data-stu-id="7e337-111">Download failed vertex to local machine</span></span>

<span data-ttu-id="7e337-112">Amikor megnyit sikertelen feladat a Azure Data Lake Tools for Visual Studio, egy sárga értesítési sáv részletes hibaüzeneteket a hiba lapon látható.</span><span class="sxs-lookup"><span data-stu-id="7e337-112">When you open a failed job in Azure Data Lake Tools for Visual Studio, you see a yellow alert bar with detailed error messages in the error tab.</span></span>

1. <span data-ttu-id="7e337-113">Kattintson a **letöltése** letölteni a szükséges erőforrások és a bemeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="7e337-113">Click **Download** to download all the required resources and input streams.</span></span> <span data-ttu-id="7e337-114">Kattintson a letöltés indul el, ha **újra**.</span><span class="sxs-lookup"><span data-stu-id="7e337-114">If the download doesn't complete, click **Retry**.</span></span>

2. <span data-ttu-id="7e337-115">Kattintson a **nyitott** helyi hibakeresési környezetben létrehozásához a letöltés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="7e337-115">Click **Open** after the download completes to generate a local debugging environment.</span></span> <span data-ttu-id="7e337-116">Az automatikusan létrehozott és megnyitott egy új Visual Studio-példány és a hibakeresési megoldás.</span><span class="sxs-lookup"><span data-stu-id="7e337-116">A new Visual Studio instance with a debugging solution is automatically created and opened.</span></span>

![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio letöltése csúcspont](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

<span data-ttu-id="7e337-118">Feladatok közé tartozhat a háttérkód forrásfájlok vagy a regisztrált szerelvényeket, és két különböző hibakeresési forgatókönyvek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7e337-118">Jobs may include code-behind source files or registered assemblies, and these two types have different debugging scenarios.</span></span>

- [<span data-ttu-id="7e337-119">Sikertelen feladat a háttérkód hibakeresése</span><span class="sxs-lookup"><span data-stu-id="7e337-119">Debug a failed job with code-behind</span></span>](#debug-job-failed-with-code-behind)
- [<span data-ttu-id="7e337-120">Sikertelen feladat a hibakereséshez szerelvények</span><span class="sxs-lookup"><span data-stu-id="7e337-120">Debug a failed job with assemblies</span></span>](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a><span data-ttu-id="7e337-121">Feladat sikertelen volt a háttérkód hibakeresése</span><span class="sxs-lookup"><span data-stu-id="7e337-121">Debug job failed with code-behind</span></span>

<span data-ttu-id="7e337-122">Ha egy U-SQL-feladat sikertelen lesz, és a feladat tartalmazza a felhasználói kód (általában nevű `Script.usql.cs` U-SQL projekt), hogy forráskód importálta-e a hibakeresési megoldás.</span><span class="sxs-lookup"><span data-stu-id="7e337-122">If a U-SQL job fails, and the job includes user code (typically named `Script.usql.cs` in a U-SQL project), that source code is imported into the debugging solution.</span></span>  <span data-ttu-id="7e337-123">Innen a Visual Studio hibakereső eszközöket (figyelési, változókat, stb.) a probléma elhárításához is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7e337-123">From there you can use the Visual Studio debugging tools (watch, variables, etc.) to troubleshoot the problem.</span></span>

> [!NOTE]
> <span data-ttu-id="7e337-124">Mielőtt hibakeresés, ügyeljen arra, hogy ellenőrizze **közös nyelvi futtatókörnyezet kivételek** a kivétel beállítások ablakban (**Ctrl + Alt + E**).</span><span class="sxs-lookup"><span data-stu-id="7e337-124">Before debugging, be sure to check **Common Language Runtime Exceptions** in the Exception Settings window (**Ctrl + Alt + E**).</span></span>

![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio-beállítás](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. <span data-ttu-id="7e337-126">Nyomja le az **F5** a háttérkód kód futtatására.</span><span class="sxs-lookup"><span data-stu-id="7e337-126">Press **F5** to run the code-behind code.</span></span> <span data-ttu-id="7e337-127">Akkor fog futni, amíg egy kivétel miatt leállt.</span><span class="sxs-lookup"><span data-stu-id="7e337-127">It will run until it is stopped by an exception.</span></span>

2. <span data-ttu-id="7e337-128">Nyissa meg a `ADLTool_Codebehind.usql.cs` fájlt, és állítson be töréspontokat, majd nyomja le az **F5** a kód lépésenkénti hibakereséséhez.</span><span class="sxs-lookup"><span data-stu-id="7e337-128">Open the `ADLTool_Codebehind.usql.cs` file and set breakpoints, then press **F5** to debug the code step by step.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési kivétel](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a><span data-ttu-id="7e337-130">Feladat sikertelen volt a szerelvények hibakeresését</span><span class="sxs-lookup"><span data-stu-id="7e337-130">Debug job failed with assemblies</span></span>

<span data-ttu-id="7e337-131">Ha regisztrált szerelvényeket a U-SQL parancsfájlt használja, a rendszer automatikusan a forráskód nem lehet lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="7e337-131">If you use registered assemblies in your U-SQL script, the system can't get the source code automatically.</span></span> <span data-ttu-id="7e337-132">Ebben az esetben adja hozzá manuálisan a szerelvények forráskódfájl a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="7e337-132">In this case, manually add the assemblies' source code files to the solution.</span></span>

### <a name="configure-the-solution"></a><span data-ttu-id="7e337-133">A megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e337-133">Configure the solution</span></span>

1. <span data-ttu-id="7e337-134">Kattintson a jobb gombbal **megoldás "VertexDebug" > vegye fel > létező projekt...**  keresése a szerelvények forráskódot, és adja hozzá a projekt hibakeresési megoldás.</span><span class="sxs-lookup"><span data-stu-id="7e337-134">Right-click **Solution 'VertexDebug' > Add > Existing Project...** to find the assemblies' source code and add the project to the debugging solution.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési projekt hozzáadása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. <span data-ttu-id="7e337-136">Kattintson a jobb gombbal **LocalVertexHost > Tulajdonságok** a megoldás, és másolja a **munkakönyvtár** elérési útja.</span><span class="sxs-lookup"><span data-stu-id="7e337-136">Right-click **LocalVertexHost > Properties** in the solution and copy the **Working Directory** path.</span></span>

3. <span data-ttu-id="7e337-137">Kattintson a jobb gombbal **forrás kód szerelvényprojektet > Tulajdonságok**, jelölje be a **Build** bal oldali lapon, és illessze be a másolt elérési útjára **kimeneti > elérési utat a kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="7e337-137">Right-Click **assembly source code project > Properties**, select the **Build** tab at left, and paste the copied path as **Output > Output path**.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési pdb elérési útjának beállítása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. <span data-ttu-id="7e337-139">Nyomja le az **Ctrl + Alt + E**, ellenőrizze **közös nyelvi futtatókörnyezet kivételek** kivétel beállításai ablakban.</span><span class="sxs-lookup"><span data-stu-id="7e337-139">Press **Ctrl + Alt + E**, check **Common Language Runtime Exceptions** in Exception Settings window.</span></span>

### <a name="start-debug"></a><span data-ttu-id="7e337-140">Indítsa el a hibakeresési</span><span class="sxs-lookup"><span data-stu-id="7e337-140">Start debug</span></span>

1. <span data-ttu-id="7e337-141">Kattintson a jobb gombbal **szerelvény forráskód Projekt > újraépítése** kimeneti .pdb fájlt a `LocalVertexHost` munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="7e337-141">Right-click **assembly source code project > Rebuild** to output .pdb files to the `LocalVertexHost` working directory.</span></span>

2. <span data-ttu-id="7e337-142">Nyomja le az **F5** és a projekt fog futni, amíg egy kivétel miatt leállt.</span><span class="sxs-lookup"><span data-stu-id="7e337-142">Press **F5** and the project will run until it is stopped by an exception.</span></span> <span data-ttu-id="7e337-143">A következő figyelmeztető üzenet, amely biztonságosan figyelmen kívül hagyhatja jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="7e337-143">You may see the following warning message, which you can safely ignore.</span></span> <span data-ttu-id="7e337-144">Azt is eltarthat egy percet a hibakeresési képernyő eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7e337-144">It can take up to a minute to get to the debug screen.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio figyelmeztetés](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. <span data-ttu-id="7e337-146">Nyissa meg a forráskódot, és állítson be töréspontokat, majd nyomja le az **F5** a kód lépésenkénti hibakereséséhez.</span><span class="sxs-lookup"><span data-stu-id="7e337-146">Open your source code and set breakpoints, then press **F5** to debug the code step by step.</span></span>

<span data-ttu-id="7e337-147">A Visual Studio hibakereső eszközöket (figyelési, változókat, stb.) a probléma elhárításához is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7e337-147">You can also use the Visual Studio debugging tools (watch, variables, etc.) to troubleshoot the problem.</span></span>

> [!NOTE]
> <span data-ttu-id="7e337-148">A szerelvény forráskód projekt újraépítése frissített .pdb fájlok létrehozásához a kód módosítása után minden alkalommal.</span><span class="sxs-lookup"><span data-stu-id="7e337-148">Rebuild the assembly source code project each time after you modify the code to generate updated .pdb files.</span></span>

<span data-ttu-id="7e337-149">Hibakeresési információ után a projekt sikeresen befejeződik a kimeneti ablakban látható a következő üzenetet:</span><span class="sxs-lookup"><span data-stu-id="7e337-149">After debugging, if the project completes successfully the output window shows the following message:</span></span>

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Az Azure Data Lake Analytics U-SQL hibakeresési sikeres](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a><span data-ttu-id="7e337-151">Küldje el újra a feladatot</span><span class="sxs-lookup"><span data-stu-id="7e337-151">Resubmit the job</span></span>

<span data-ttu-id="7e337-152">Ha befejezte a hibakeresést, küldje el újra a sikertelen feladatot.</span><span class="sxs-lookup"><span data-stu-id="7e337-152">Once you have completed debugging, resubmit the failed job.</span></span>

1. <span data-ttu-id="7e337-153">A feladatok a háttérkód megoldásokkal, másolja a C#-kódban a háttérkód forrásfájl (általában `Script.usql.cs`).</span><span class="sxs-lookup"><span data-stu-id="7e337-153">For jobs with code-behind solutions, copy your C# code into the code-behind source file (typically `Script.usql.cs`).</span></span>
2. <span data-ttu-id="7e337-154">A szerelvények feladatok a frissített .dll szerelvényeket a ADLA-adatbázisba regisztrálása:</span><span class="sxs-lookup"><span data-stu-id="7e337-154">For jobs with assemblies, register the updated .dll assemblies into your ADLA database:</span></span>
    1. <span data-ttu-id="7e337-155">A Server Explorer vagy a Cloud Explorerben bontsa ki a **ADLA fiók > adatbázisok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="7e337-155">From Server Explorer or Cloud Explorer, expand the **ADLA account > Databases** node.</span></span>
    2. <span data-ttu-id="7e337-156">Kattintson a jobb gombbal **szerelvények** , és regisztrálja az új .dll szerelvényeket a ADLA adatbázissal: ![Azure Data Lake Analytics U-SQL hibakeresési szerelvény regisztrálása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span><span class="sxs-lookup"><span data-stu-id="7e337-156">Right-click **Assemblies** and register your new .dll assemblies with the ADLA database: ![Azure Data Lake Analytics U-SQL debug register assembly](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span></span>
3. <span data-ttu-id="7e337-157">Küldje el újra a feladatot.</span><span class="sxs-lookup"><span data-stu-id="7e337-157">Resubmit your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e337-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e337-158">Next steps</span></span>

- [<span data-ttu-id="7e337-159">U-SQL programozástámogatási útmutató</span><span class="sxs-lookup"><span data-stu-id="7e337-159">U-SQL programmability guide</span></span>](data-lake-analytics-u-sql-programmability-guide.md)
- [<span data-ttu-id="7e337-160">Azure Data Lake Analytics-feladatok U-SQL-felhasználó által definiált operátorok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="7e337-160">Develop U-SQL User-defined operators for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [<span data-ttu-id="7e337-161">Oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="7e337-161">Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
