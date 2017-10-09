---
title: "a Data Lake Tools for Visual Studio a Vertex végrehajtási nézetet aaaUse hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Vertex végrehajtási nézetet tooexam Data Lake Analytics-feladatok."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: fb54d2af8a32aa27a54ff50a73c1b4903677a21e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a><span data-ttu-id="f98ba-103">Hello Vertex végrehajtási nézetet használja a Data Lake Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f98ba-103">Use hello Vertex Execution View in Data Lake Tools for Visual Studio</span></span>
<span data-ttu-id="f98ba-104">Ismerje meg, hogyan toouse hello Vertex végrehajtási nézetet tooexam Data Lake Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="f98ba-104">Learn how toouse hello Vertex Execution View tooexam Data Lake Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f98ba-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f98ba-105">Prerequisites</span></span>

<span data-ttu-id="f98ba-106">Szükséges alapismeretek adatok Lake Tools for Visual Studio toodevelop U-SQL parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="f98ba-106">You need basic knowledge of using Data Lake Tools for Visual Studio toodevelop U-SQL script.</span></span>  <span data-ttu-id="f98ba-107">Lásd: [oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f98ba-107">See [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>

## <a name="open-hello-vertex-execution-view"></a><span data-ttu-id="f98ba-108">Nyissa meg a hello Vertex végrehajtási nézetet</span><span class="sxs-lookup"><span data-stu-id="f98ba-108">Open hello Vertex Execution View</span></span>
<span data-ttu-id="f98ba-109">Nyissa meg egy U-SQL-feladatot: Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f98ba-109">Open a U-SQL job in Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="f98ba-110">Kattintson a **Vertex végrehajtási nézetet** hello bal alsó sarokban található.</span><span class="sxs-lookup"><span data-stu-id="f98ba-110">Click **Vertex Execution View** in hello bottom left corner.</span></span> <span data-ttu-id="f98ba-111">Először felszólító tooload profilok lehet, és a hálózati kapcsolattól függően hosszabb időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f98ba-111">You may be prompted tooload profiles first and it can take some time depending on your network connectivity.</span></span>

![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a><span data-ttu-id="f98ba-113">Vertex végrehajtási nézetet ismertetése</span><span class="sxs-lookup"><span data-stu-id="f98ba-113">Understand Vertex Execution View</span></span>
<span data-ttu-id="f98ba-114">hello Vertex végrehajtási nézetet három részből áll:</span><span class="sxs-lookup"><span data-stu-id="f98ba-114">hello Vertex Execution View has three parts:</span></span>

![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

<span data-ttu-id="f98ba-116">Hello **csúcspont választó** hello bal lehetővé teszi a csúcsban jelöl funkciók (például a felső 10 adatokat olvassa el, vagy válasszon fázisa).</span><span class="sxs-lookup"><span data-stu-id="f98ba-116">hello **Vertex selector** on hello left lets you select vertices by features (such as top 10 data read, or choose by stage).</span></span> <span data-ttu-id="f98ba-117">Hello leggyakrabban használt szűrők egyik toosee hello **kritikus úton csúcsban**.</span><span class="sxs-lookup"><span data-stu-id="f98ba-117">One of hello most commonly-used filters is toosee hello **vertices on critical path**.</span></span> <span data-ttu-id="f98ba-118">Hello **kritikus út** hello leghosszabb láncában csúcsban egy U-SQL-feladat van.</span><span class="sxs-lookup"><span data-stu-id="f98ba-118">hello **Critical path** is hello longest chain of vertices of a U-SQL job.</span></span> <span data-ttu-id="f98ba-119">Understanding hello kritikus elérési út hasznos, ha a feladatok optimalizálása ellenőrzésével, mely csúcspont hello leghosszabb időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f98ba-119">Understanding hello critical path is useful for optimizing your jobs by checking which vertex takes hello longest time.</span></span>
  
![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

<span data-ttu-id="f98ba-121">hello felső középső ablaktáblán látható hello **futási állapota minden hello csúcsban**.</span><span class="sxs-lookup"><span data-stu-id="f98ba-121">hello top center pane shows hello **running status of all hello vertices**.</span></span>
  
![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

<span data-ttu-id="f98ba-123">hello alsó középső ablaktáblán minden csúcspont információkat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="f98ba-123">hello bottom center pane shows information about each vertex:</span></span>
* <span data-ttu-id="f98ba-124">Folyamat neve: hello hello csúcspont példányának neve.</span><span class="sxs-lookup"><span data-stu-id="f98ba-124">Process Name: hello name of hello vertex instance.</span></span> <span data-ttu-id="f98ba-125">A StageName különböző részlegein áll |} VertexName |} VertexRunInstance.</span><span class="sxs-lookup"><span data-stu-id="f98ba-125">It is composed of different parts in StageName|VertexName|VertexRunInstance.</span></span> <span data-ttu-id="f98ba-126">Például hello SV7_Split [62] .v1 csúcspont hello második futó példány (.v1, 0 kezdődő indexű) jelző csúcspont szám 62 szakasz SV7_Split.</span><span class="sxs-lookup"><span data-stu-id="f98ba-126">For example, hello SV7_Split[62].v1 vertex stands for hello second running instance (.v1, index starting from 0) of Vertex number 62 in Stage SV7_Split.</span></span>
* <span data-ttu-id="f98ba-127">Összesített adatok olvasás/Written: hello történt olvasás/írták a csúcspont.</span><span class="sxs-lookup"><span data-stu-id="f98ba-127">Total Data Read/Written: hello data was read/written by this vertex.</span></span>
* <span data-ttu-id="f98ba-128">Állapot-és kilépési állapotát: hello végső állapotba hello csúcspont véget ér.</span><span class="sxs-lookup"><span data-stu-id="f98ba-128">State/Exit Status: hello final status when hello vertex is ended.</span></span>
* <span data-ttu-id="f98ba-129">Kilépési kód/hiba típusa: hello hiba, ha hello csomópontra nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="f98ba-129">Exit Code/Failure Type: hello error when hello vertex failed.</span></span>
* <span data-ttu-id="f98ba-130">Létrehozási ok: Miért hello csúcspont jött létre.</span><span class="sxs-lookup"><span data-stu-id="f98ba-130">Creation Reason: Why hello vertex was created.</span></span>
* <span data-ttu-id="f98ba-131">Erőforrás késés/folyamat várakozási ideje vagy PN várólista késés: hello idő hello csúcspont toowait erőforrások, tooprocess adatok és toostay hello várólistában.</span><span class="sxs-lookup"><span data-stu-id="f98ba-131">Resource Latency/Process Latency/PN Queue Latency: hello time taken for hello vertex toowait for resources, tooprocess data, and toostay in hello queue.</span></span>
* <span data-ttu-id="f98ba-132">Folyamat/létrehozó GUID: Hello aktuális futó csúcspont vagy létrehozója GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f98ba-132">Process/Creator GUID: GUID for hello current running vertex or its creator.</span></span>
* <span data-ttu-id="f98ba-133">Verzió: hello N-edik példányát futtató csúcspont hello (a hello rendszer lehet ütemezni csúcspont új példányát, a számos oka lehet, például a feladatátvétel számítási redundancia stb.)</span><span class="sxs-lookup"><span data-stu-id="f98ba-133">Version: hello N-th instance of hello running vertex (hello system might schedule new instances of a vertex for many reasons, for example failover, compute redundancy, etc.)</span></span>
* <span data-ttu-id="f98ba-134">Idő létrehozott verzió.</span><span class="sxs-lookup"><span data-stu-id="f98ba-134">Version Created Time.</span></span>
* <span data-ttu-id="f98ba-135">Feldolgozni létrehozása kezdési idő és dolgozza várakozik idő és dolgozza kezdési időt és dolgozza Complete idő: hello csúcspont folyamat indításakor létrehozása; hello csúcspont folyamat indításakor tooqueue; Ha hello bizonyos csúcspont megkezdődése; Ha hello bizonyos csúcspont befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f98ba-135">Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time: when hello vertex process starts creation; when hello vertex process starts tooqueue; when hello certain vertex process starts; when hello certain vertex is completed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f98ba-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f98ba-136">Next steps</span></span>
* <span data-ttu-id="f98ba-137">toolog diagnosztikai információkat, lásd: [diagnosztikai naplók elérése az Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span><span class="sxs-lookup"><span data-stu-id="f98ba-137">toolog diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span></span>
* <span data-ttu-id="f98ba-138">egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="f98ba-138">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="f98ba-139">feladat részletei tooview, lásd: [használata feladat böngésző és az Azure Data lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="f98ba-139">tooview job details, see [Use Job Browser and Job View for Azure Data lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md)</span></span>
