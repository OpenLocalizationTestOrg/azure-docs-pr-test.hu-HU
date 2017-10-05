---
title: "Használja a Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio |} Microsoft Docs"
description: "Ismerje meg, hogyan használható a Vertex végrehajtási nézetet való vizsgálatához Data Lake Analytics-feladatok."
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
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a><span data-ttu-id="c4b6a-103">A Vertex végrehajtási nézetet, a Data Lake Tools for Visual Studio használata</span><span class="sxs-lookup"><span data-stu-id="c4b6a-103">Use the Vertex Execution View in Data Lake Tools for Visual Studio</span></span>
<span data-ttu-id="c4b6a-104">Ismerje meg, hogyan használható a Vertex végrehajtási nézetet való vizsgálatához Data Lake Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-104">Learn how to use the Vertex Execution View to exam Data Lake Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4b6a-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c4b6a-105">Prerequisites</span></span>

<span data-ttu-id="c4b6a-106">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával vonatkozó általános ismeretekre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-106">You need basic knowledge of using Data Lake Tools for Visual Studio to develop U-SQL script.</span></span>  <span data-ttu-id="c4b6a-107">Lásd: [oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c4b6a-107">See [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>

## <a name="open-the-vertex-execution-view"></a><span data-ttu-id="c4b6a-108">Nyissa meg a Vertex végrehajtási nézetet</span><span class="sxs-lookup"><span data-stu-id="c4b6a-108">Open the Vertex Execution View</span></span>
<span data-ttu-id="c4b6a-109">Nyissa meg egy U-SQL-feladatot: Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-109">Open a U-SQL job in Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="c4b6a-110">Kattintson a **Vertex végrehajtási nézetet** bal alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-110">Click **Vertex Execution View** in the bottom left corner.</span></span> <span data-ttu-id="c4b6a-111">Kérheti profilokat először betöltése, és a hálózati kapcsolattól függően hosszabb időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-111">You may be prompted to load profiles first and it can take some time depending on your network connectivity.</span></span>

![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a><span data-ttu-id="c4b6a-113">Vertex végrehajtási nézetet ismertetése</span><span class="sxs-lookup"><span data-stu-id="c4b6a-113">Understand Vertex Execution View</span></span>
<span data-ttu-id="c4b6a-114">A Vertex végrehajtási nézetet három részből áll:</span><span class="sxs-lookup"><span data-stu-id="c4b6a-114">The Vertex Execution View has three parts:</span></span>

![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

<span data-ttu-id="c4b6a-116">A **csúcspont választó** a bal oldali megadható, hogy a csúcsban jelöl funkciók (például a felső 10 adatokat olvassa el, vagy válasszon fázisa).</span><span class="sxs-lookup"><span data-stu-id="c4b6a-116">The **Vertex selector** on the left lets you select vertices by features (such as top 10 data read, or choose by stage).</span></span> <span data-ttu-id="c4b6a-117">A leggyakrabban használt szűrők egyik tekintse meg a **kritikus úton csúcsban**.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-117">One of the most commonly-used filters is to see the **vertices on critical path**.</span></span> <span data-ttu-id="c4b6a-118">A **kritikus út** a leghosszabb lánc csatlakozik a csúcsban a U-SQL feladatok.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-118">The **Critical path** is the longest chain of vertices of a U-SQL job.</span></span> <span data-ttu-id="c4b6a-119">A kritikus út ismertetése akkor hasznos, a feladatok optimalizálási ellenőrzésével, mely csúcspont a leghosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-119">Understanding the critical path is useful for optimizing your jobs by checking which vertex takes the longest time.</span></span>
  
![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

<span data-ttu-id="c4b6a-121">A felső center ablaktáblán látható a **futási állapota a csúcsban**.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-121">The top center pane shows the **running status of all the vertices**.</span></span>
  
![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

<span data-ttu-id="c4b6a-123">Az alsó középső ablaktáblán minden csúcspont információkat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c4b6a-123">The bottom center pane shows information about each vertex:</span></span>
* <span data-ttu-id="c4b6a-124">Folyamat neve: A csomópont-példány nevét.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-124">Process Name: The name of the vertex instance.</span></span> <span data-ttu-id="c4b6a-125">A StageName különböző részlegein áll |} VertexName |} VertexRunInstance.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-125">It is composed of different parts in StageName|VertexName|VertexRunInstance.</span></span> <span data-ttu-id="c4b6a-126">Például a SV7_Split [62] .v1 csúcspont jelöli, a második futó példány (.v1, 0 kezdődő indexű) szakaszban SV7_Split 62 csúcspont szám.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-126">For example, the SV7_Split[62].v1 vertex stands for the second running instance (.v1, index starting from 0) of Vertex number 62 in Stage SV7_Split.</span></span>
* <span data-ttu-id="c4b6a-127">Összesített adatok olvasás/Written: A történt olvasás/írták a csúcspont.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-127">Total Data Read/Written: The data was read/written by this vertex.</span></span>
* <span data-ttu-id="c4b6a-128">Állapot-és kilépési állapotát: A végső állapotúvá amikor befejeződik a csomópontra.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-128">State/Exit Status: The final status when the vertex is ended.</span></span>
* <span data-ttu-id="c4b6a-129">Kilépési kód/hiba típusa: A hiba a csúcspont sikertelen.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-129">Exit Code/Failure Type: The error when the vertex failed.</span></span>
* <span data-ttu-id="c4b6a-130">Létrehozási ok: A csúcspont létrehozásának okát.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-130">Creation Reason: Why the vertex was created.</span></span>
* <span data-ttu-id="c4b6a-131">Erőforrás késés/folyamat várakozási ideje vagy PN várólista késés: adatok feldolgozására, és ahhoz, hogy a várólistán lévő erőforrások, várja meg a csúcspont igénybe vett idő.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-131">Resource Latency/Process Latency/PN Queue Latency: the time taken for the vertex to wait for resources, to process data, and to stay in the queue.</span></span>
* <span data-ttu-id="c4b6a-132">Folyamat/létrehozó GUID: Az aktuális futó csúcspont vagy létrehozója GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-132">Process/Creator GUID: GUID for the current running vertex or its creator.</span></span>
* <span data-ttu-id="c4b6a-133">Verzió: az N-edik példányát a futó csúcspont (a a rendszer lehet ütemezni csúcspont új példányát, a számos oka lehet, például a feladatátvétel számítási redundancia stb.)</span><span class="sxs-lookup"><span data-stu-id="c4b6a-133">Version: the N-th instance of the running vertex (the system might schedule new instances of a vertex for many reasons, for example failover, compute redundancy, etc.)</span></span>
* <span data-ttu-id="c4b6a-134">Idő létrehozott verzió.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-134">Version Created Time.</span></span>
* <span data-ttu-id="c4b6a-135">Feldolgozni létrehozása kezdési idő és dolgozza várakozik idő és dolgozza kezdési időt és dolgozza Complete idő: a csúcspont folyamat indításakor létrehozása; a csomópont folyamat indításakor a várólistára; az egyes csúcspont folyamat indításakor; Amikor az egyes csúcspont befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c4b6a-135">Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time: when the vertex process starts creation; when the vertex process starts to queue; when the certain vertex process starts; when the certain vertex is completed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4b6a-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4b6a-136">Next steps</span></span>
* <span data-ttu-id="c4b6a-137">A diagnosztikai információk naplózása: [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Az Azure Data Lake Analytics diagnosztikai naplóinak elérése).</span><span class="sxs-lookup"><span data-stu-id="c4b6a-137">To log diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span></span>
* <span data-ttu-id="c4b6a-138">Egy összetettebb lekérdezés megtekintéséhez lásd: [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) (Webhelyek naplóinak elemzése az Azure Data Lake Analytics használatával).</span><span class="sxs-lookup"><span data-stu-id="c4b6a-138">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="c4b6a-139">Feladat részleteinek megtekintése: [használata feladat böngésző és az Azure Data lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="c4b6a-139">To view job details, see [Use Job Browser and Job View for Azure Data lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md)</span></span>
