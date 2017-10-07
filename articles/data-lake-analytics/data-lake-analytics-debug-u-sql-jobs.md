---
title: aaaDebug U-SQL feladatok |} Microsoft Docs
description: "Ismerje meg, hogyan nem sikerült a toodebug egy U-SQL Visual Studio használatával csúcspont."
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
ms.openlocfilehash: 092bffa1a59ed91c5837402d0276447480b923fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a><span data-ttu-id="78d5d-103">Felhasználó által definiált C# programkódja sikertelen U-SQL feladatok hibakeresése</span><span class="sxs-lookup"><span data-stu-id="78d5d-103">Debug user-defined C# code for failed U-SQL jobs</span></span>

<span data-ttu-id="78d5d-104">U-SQL egy bővíthetőségi modell használatával C#, így a kód tooadd funkciókat, például egy egyéni készülék vagy nyomáscsökkentő írhat biztosít.</span><span class="sxs-lookup"><span data-stu-id="78d5d-104">U-SQL provides an extensibility model using C#, so you can write your code tooadd functionality such as a custom extractor or reducer.</span></span> <span data-ttu-id="78d5d-105">több, lásd: toolearn [U-SQL programozástámogatási útmutató](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span><span class="sxs-lookup"><span data-stu-id="78d5d-105">toolearn more, see [U-SQL programmability guide](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf).</span></span> <span data-ttu-id="78d5d-106">A gyakorlatban kódok hibakeresés esetleg, és a big data-rendszereket csupán korlátozott futási hibakeresési információk, például a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="78d5d-106">In practice any code may need debugging, and big data systems may only provide limited runtime debugging information such as log files.</span></span>

<span data-ttu-id="78d5d-107">Az Azure Data Lake Tools for Visual Studio nevű funkcióval rendelkezik **csúcspont Debug nem sikerült**, amely lehetővé teszi a sikertelen feladat a hello felhő tooyour helyi számítógépről hibakeresési klónozza.</span><span class="sxs-lookup"><span data-stu-id="78d5d-107">Azure Data Lake Tools for Visual Studio provides a feature called **Failed Vertex Debug**, which lets you clone a failed job from hello cloud tooyour local machine for debugging.</span></span> <span data-ttu-id="78d5d-108">helyi másolat hello hello teljes felhőkörnyezetben, beleértve a bemeneti adatok és a felhasználói kód rögzíti.</span><span class="sxs-lookup"><span data-stu-id="78d5d-108">hello local clone captures hello entire cloud environment, including any input data and user code.</span></span>

<span data-ttu-id="78d5d-109">hello következő videó bemutatja csúcspont Debug nem sikerült Azure Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78d5d-109">hello following video demonstrates Failed Vertex Debug in Azure Data Lake Tools for Visual Studio.</span></span>

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> <span data-ttu-id="78d5d-110">A Visual Studio a következő két frissítések hello van szükség, ha még nincs telepítve: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) és a [Universal C futásidejű for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span><span class="sxs-lookup"><span data-stu-id="78d5d-110">Visual Studio requires hello following two updates, if they are not already installed: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) and the [Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410).</span></span>

## <a name="download-failed-vertex-toolocal-machine"></a><span data-ttu-id="78d5d-111">Nem sikerült letölteni a csúcspont toolocal gép</span><span class="sxs-lookup"><span data-stu-id="78d5d-111">Download failed vertex toolocal machine</span></span>

<span data-ttu-id="78d5d-112">Amikor megnyit sikertelen feladat a Azure Data Lake Tools for Visual Studio, egy sárga értesítési sáv hello hiba lapon a részletes hibaüzenetek látható.</span><span class="sxs-lookup"><span data-stu-id="78d5d-112">When you open a failed job in Azure Data Lake Tools for Visual Studio, you see a yellow alert bar with detailed error messages in hello error tab.</span></span>

1. <span data-ttu-id="78d5d-113">Kattintson a **letöltése** összes hello toodownload szükséges erőforrások és a bemeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="78d5d-113">Click **Download** toodownload all hello required resources and input streams.</span></span> <span data-ttu-id="78d5d-114">Ha hello letöltési indul el, kattintson a **újra**.</span><span class="sxs-lookup"><span data-stu-id="78d5d-114">If hello download doesn't complete, click **Retry**.</span></span>

2. <span data-ttu-id="78d5d-115">Kattintson a **nyitott** hello letöltés toogenerate helyi hibakeresési környezetben befejezése után.</span><span class="sxs-lookup"><span data-stu-id="78d5d-115">Click **Open** after hello download completes toogenerate a local debugging environment.</span></span> <span data-ttu-id="78d5d-116">Az automatikusan létrehozott és megnyitott egy új Visual Studio-példány és a hibakeresési megoldás.</span><span class="sxs-lookup"><span data-stu-id="78d5d-116">A new Visual Studio instance with a debugging solution is automatically created and opened.</span></span>

![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio letöltése csúcspont](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

<span data-ttu-id="78d5d-118">Feladatok közé tartozhat a háttérkód forrásfájlok vagy a regisztrált szerelvényeket, és két különböző hibakeresési forgatókönyvek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="78d5d-118">Jobs may include code-behind source files or registered assemblies, and these two types have different debugging scenarios.</span></span>

- [<span data-ttu-id="78d5d-119">Sikertelen feladat a háttérkód hibakeresése</span><span class="sxs-lookup"><span data-stu-id="78d5d-119">Debug a failed job with code-behind</span></span>](#debug-job-failed-with-code-behind)
- [<span data-ttu-id="78d5d-120">Sikertelen feladat a hibakereséshez szerelvények</span><span class="sxs-lookup"><span data-stu-id="78d5d-120">Debug a failed job with assemblies</span></span>](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a><span data-ttu-id="78d5d-121">Feladat sikertelen volt a háttérkód hibakeresése</span><span class="sxs-lookup"><span data-stu-id="78d5d-121">Debug job failed with code-behind</span></span>

<span data-ttu-id="78d5d-122">Ha egy U-SQL-feladat sikertelen lesz, és hello feladat tartalmazza a felhasználói kód (általában nevű `Script.usql.cs` U-SQL projekt), hogy forráskód megoldás hibakeresés hello importálta-e.</span><span class="sxs-lookup"><span data-stu-id="78d5d-122">If a U-SQL job fails, and hello job includes user code (typically named `Script.usql.cs` in a U-SQL project), that source code is imported into hello debugging solution.</span></span>  <span data-ttu-id="78d5d-123">Innen hello Visual Studio hibakeresési eszközök (figyelési, változók stb.) tootroubleshoot hello probléma is használhatja.</span><span class="sxs-lookup"><span data-stu-id="78d5d-123">From there you can use hello Visual Studio debugging tools (watch, variables, etc.) tootroubleshoot hello problem.</span></span>

> [!NOTE]
> <span data-ttu-id="78d5d-124">Előtt hibakeresés, lehet, hogy toocheck **közös nyelvi futtatókörnyezet kivételek** hello kivétel beállításai ablakban (**Ctrl + Alt + E**).</span><span class="sxs-lookup"><span data-stu-id="78d5d-124">Before debugging, be sure toocheck **Common Language Runtime Exceptions** in hello Exception Settings window (**Ctrl + Alt + E**).</span></span>

![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio-beállítás](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. <span data-ttu-id="78d5d-126">Nyomja le az **F5** toorun hello háttérkód kódot.</span><span class="sxs-lookup"><span data-stu-id="78d5d-126">Press **F5** toorun hello code-behind code.</span></span> <span data-ttu-id="78d5d-127">Akkor fog futni, amíg egy kivétel miatt leállt.</span><span class="sxs-lookup"><span data-stu-id="78d5d-127">It will run until it is stopped by an exception.</span></span>

2. <span data-ttu-id="78d5d-128">Nyissa meg hello `ADLTool_Codebehind.usql.cs` fájlt, és állítson be töréspontokat, majd nyomja le az **F5** toodebug hello kód lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="78d5d-128">Open hello `ADLTool_Codebehind.usql.cs` file and set breakpoints, then press **F5** toodebug hello code step by step.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési kivétel](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a><span data-ttu-id="78d5d-130">Feladat sikertelen volt a szerelvények hibakeresését</span><span class="sxs-lookup"><span data-stu-id="78d5d-130">Debug job failed with assemblies</span></span>

<span data-ttu-id="78d5d-131">Ha regisztrált szerelvényeket a U-SQL parancsfájlt használja, hello rendszer automatikusan hello forráskód nem lehet lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="78d5d-131">If you use registered assemblies in your U-SQL script, hello system can't get hello source code automatically.</span></span> <span data-ttu-id="78d5d-132">Ebben az esetben manuálisan adja hozzá hello szerelvények forrás kód fájlok toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="78d5d-132">In this case, manually add hello assemblies' source code files toohello solution.</span></span>

### <a name="configure-hello-solution"></a><span data-ttu-id="78d5d-133">Hello megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78d5d-133">Configure hello solution</span></span>

1. <span data-ttu-id="78d5d-134">Kattintson a jobb gombbal **megoldás "VertexDebug" > vegye fel > létező projekt...**  toofind forráskód szerelvényeket hello, és adja hozzá a hello projekt toohello megoldás hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="78d5d-134">Right-click **Solution 'VertexDebug' > Add > Existing Project...** toofind hello assemblies' source code and add hello project toohello debugging solution.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési projekt hozzáadása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. <span data-ttu-id="78d5d-136">Kattintson a jobb gombbal **LocalVertexHost > Tulajdonságok** a hello megoldás, és másolja hello **munkakönyvtár** elérési útja.</span><span class="sxs-lookup"><span data-stu-id="78d5d-136">Right-click **LocalVertexHost > Properties** in hello solution and copy hello **Working Directory** path.</span></span>

3. <span data-ttu-id="78d5d-137">Kattintson a jobb gombbal **forrás kód szerelvényprojektet > Tulajdonságok**, jelölje be hello **Build** bal oldali lapon, és illessze be a másolt hello elérési útjára **kimeneti > elérési utat a kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="78d5d-137">Right-Click **assembly source code project > Properties**, select hello **Build** tab at left, and paste hello copied path as **Output > Output path**.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési pdb elérési útjának beállítása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. <span data-ttu-id="78d5d-139">Nyomja le az **Ctrl + Alt + E**, ellenőrizze **közös nyelvi futtatókörnyezet kivételek** kivétel beállításai ablakban.</span><span class="sxs-lookup"><span data-stu-id="78d5d-139">Press **Ctrl + Alt + E**, check **Common Language Runtime Exceptions** in Exception Settings window.</span></span>

### <a name="start-debug"></a><span data-ttu-id="78d5d-140">Indítsa el a hibakeresési</span><span class="sxs-lookup"><span data-stu-id="78d5d-140">Start debug</span></span>

1. <span data-ttu-id="78d5d-141">Kattintson a jobb gombbal **szerelvény forráskód Projekt > újraépítése** toooutput .pdb fájlok toohello `LocalVertexHost` munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="78d5d-141">Right-click **assembly source code project > Rebuild** toooutput .pdb files toohello `LocalVertexHost` working directory.</span></span>

2. <span data-ttu-id="78d5d-142">Nyomja le az **F5** és hello projekt fog futni, amíg egy kivétel miatt leállt.</span><span class="sxs-lookup"><span data-stu-id="78d5d-142">Press **F5** and hello project will run until it is stopped by an exception.</span></span> <span data-ttu-id="78d5d-143">A következő figyelmeztető üzenet, amely biztonságosan figyelmen kívül hagyhatja hello jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="78d5d-143">You may see hello following warning message, which you can safely ignore.</span></span> <span data-ttu-id="78d5d-144">Tooa perces tooget toohello hibakeresési képernyő is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="78d5d-144">It can take up tooa minute tooget toohello debug screen.</span></span>

    ![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio figyelmeztetés](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. <span data-ttu-id="78d5d-146">Nyissa meg a forráskódot, és állítson be töréspontokat, majd nyomja le az **F5** toodebug hello kód lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="78d5d-146">Open your source code and set breakpoints, then press **F5** toodebug hello code step by step.</span></span>

<span data-ttu-id="78d5d-147">Hello Visual Studio hibakeresési eszközök (figyelési, változók stb.) tootroubleshoot hello probléma is használható.</span><span class="sxs-lookup"><span data-stu-id="78d5d-147">You can also use hello Visual Studio debugging tools (watch, variables, etc.) tootroubleshoot hello problem.</span></span>

> [!NOTE]
> <span data-ttu-id="78d5d-148">Hello szerelvény forrás kód projekt újraépítéséhez hello kód frissítése toogenerate .pdb fájlok módosítása után minden alkalommal.</span><span class="sxs-lookup"><span data-stu-id="78d5d-148">Rebuild hello assembly source code project each time after you modify hello code toogenerate updated .pdb files.</span></span>

<span data-ttu-id="78d5d-149">Hibakeresés, miután hello projekt fejeződik hello kimeneti ablakban látható a következő üzenet hello:</span><span class="sxs-lookup"><span data-stu-id="78d5d-149">After debugging, if hello project completes successfully hello output window shows hello following message:</span></span>

```
hello Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Az Azure Data Lake Analytics U-SQL hibakeresési sikeres](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-hello-job"></a><span data-ttu-id="78d5d-151">Küldje el újra a hello feladat</span><span class="sxs-lookup"><span data-stu-id="78d5d-151">Resubmit hello job</span></span>

<span data-ttu-id="78d5d-152">Ha befejezte a hibakeresést, küldje el újra a hello sikertelen feladat adatait.</span><span class="sxs-lookup"><span data-stu-id="78d5d-152">Once you have completed debugging, resubmit hello failed job.</span></span>

1. <span data-ttu-id="78d5d-153">A feladatok a háttérkód megoldásokkal, másolja a C#-kódban hello háttérkód forrás fájlba (általában `Script.usql.cs`).</span><span class="sxs-lookup"><span data-stu-id="78d5d-153">For jobs with code-behind solutions, copy your C# code into hello code-behind source file (typically `Script.usql.cs`).</span></span>
2. <span data-ttu-id="78d5d-154">A feladatokat a szerelvényeket frissített hello .dll szerelvényeket a ADLA adatbázisba regisztrálása:</span><span class="sxs-lookup"><span data-stu-id="78d5d-154">For jobs with assemblies, register hello updated .dll assemblies into your ADLA database:</span></span>
    1. <span data-ttu-id="78d5d-155">A Server Explorer vagy a Cloud Explorerben bontsa ki a hello **ADLA fiók > adatbázisok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="78d5d-155">From Server Explorer or Cloud Explorer, expand hello **ADLA account > Databases** node.</span></span>
    2. <span data-ttu-id="78d5d-156">Kattintson a jobb gombbal **szerelvények** , és regisztrálja az új .dll szerelvények hello ADLA adatbázissal: ![Azure Data Lake Analytics U-SQL hibakeresési szerelvény regisztrálása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span><span class="sxs-lookup"><span data-stu-id="78d5d-156">Right-click **Assemblies** and register your new .dll assemblies with hello ADLA database: ![Azure Data Lake Analytics U-SQL debug register assembly](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)</span></span>
3. <span data-ttu-id="78d5d-157">Küldje el újra a feladatot.</span><span class="sxs-lookup"><span data-stu-id="78d5d-157">Resubmit your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78d5d-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78d5d-158">Next steps</span></span>

- [<span data-ttu-id="78d5d-159">U-SQL programozástámogatási útmutató</span><span class="sxs-lookup"><span data-stu-id="78d5d-159">U-SQL programmability guide</span></span>](data-lake-analytics-u-sql-programmability-guide.md)
- [<span data-ttu-id="78d5d-160">Azure Data Lake Analytics-feladatok U-SQL-felhasználó által definiált operátorok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="78d5d-160">Develop U-SQL User-defined operators for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [<span data-ttu-id="78d5d-161">Oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="78d5d-161">Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
