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
# <a name="use-hello-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Hello Vertex végrehajtási nézetet használja a Data Lake Tools for Visual Studio
Ismerje meg, hogyan toouse hello Vertex végrehajtási nézetet tooexam Data Lake Analytics-feladatok.

## <a name="prerequisites"></a>Előfeltételek

Szükséges alapismeretek adatok Lake Tools for Visual Studio toodevelop U-SQL parancsfájl használatával.  Lásd: [oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-hello-vertex-execution-view"></a>Nyissa meg a hello Vertex végrehajtási nézetet
Nyissa meg egy U-SQL-feladatot: Data Lake Tools for Visual Studio. Kattintson a **Vertex végrehajtási nézetet** hello bal alsó sarokban található. Először felszólító tooload profilok lehet, és a hálózati kapcsolattól függően hosszabb időt is igénybe vehet.

![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Vertex végrehajtási nézetet ismertetése
hello Vertex végrehajtási nézetet három részből áll:

![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

Hello **csúcspont választó** hello bal lehetővé teszi a csúcsban jelöl funkciók (például a felső 10 adatokat olvassa el, vagy válasszon fázisa). Hello leggyakrabban használt szűrők egyik toosee hello **kritikus úton csúcsban**. Hello **kritikus út** hello leghosszabb láncában csúcsban egy U-SQL-feladat van. Understanding hello kritikus elérési út hasznos, ha a feladatok optimalizálása ellenőrzésével, mely csúcspont hello leghosszabb időt vesz igénybe.
  
![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

hello felső középső ablaktáblán látható hello **futási állapota minden hello csúcsban**.
  
![A Data Lake Analytics eszközök Vertex végrehajtási nézetet](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

hello alsó középső ablaktáblán minden csúcspont információkat jeleníti meg:
* Folyamat neve: hello hello csúcspont példányának neve. A StageName különböző részlegein áll |} VertexName |} VertexRunInstance. Például hello SV7_Split [62] .v1 csúcspont hello második futó példány (.v1, 0 kezdődő indexű) jelző csúcspont szám 62 szakasz SV7_Split.
* Összesített adatok olvasás/Written: hello történt olvasás/írták a csúcspont.
* Állapot-és kilépési állapotát: hello végső állapotba hello csúcspont véget ér.
* Kilépési kód/hiba típusa: hello hiba, ha hello csomópontra nem sikerült.
* Létrehozási ok: Miért hello csúcspont jött létre.
* Erőforrás késés/folyamat várakozási ideje vagy PN várólista késés: hello idő hello csúcspont toowait erőforrások, tooprocess adatok és toostay hello várólistában.
* Folyamat/létrehozó GUID: Hello aktuális futó csúcspont vagy létrehozója GUID azonosítója.
* Verzió: hello N-edik példányát futtató csúcspont hello (a hello rendszer lehet ütemezni csúcspont új példányát, a számos oka lehet, például a feladatátvétel számítási redundancia stb.)
* Idő létrehozott verzió.
* Feldolgozni létrehozása kezdési idő és dolgozza várakozik idő és dolgozza kezdési időt és dolgozza Complete idő: hello csúcspont folyamat indításakor létrehozása; hello csúcspont folyamat indításakor tooqueue; Ha hello bizonyos csúcspont megkezdődése; Ha hello bizonyos csúcspont befejeződött.

## <a name="next-steps"></a>Következő lépések
* toolog diagnosztikai információkat, lásd: [diagnosztikai naplók elérése az Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).
* feladat részletei tooview, lásd: [használata feladat böngésző és az Azure Data lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md)
