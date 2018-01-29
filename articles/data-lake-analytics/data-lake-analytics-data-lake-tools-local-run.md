---
title: "Tesztelése és hibakeresése a U-SQL feladatok helyi futtatáskor és az Azure Data Lake U-SQL SDK segítségével |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Data Lake Tools for Visual Studio és az Azure Data Lake U-SQL SDK tesztelése és hibakeresése a U-SQL feladatok a helyi munkaállomáson."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: 771a96df5cc66bac46e7144785be8cc072b57b31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a>Tesztelése és hibakeresése a U-SQL feladatok használatával helyi futtatásához és az Azure Data Lake U-SQL-SDK

Használhatja az Azure Data Lake Tools for Visual Studio és az Azure Data Lake U-SQL SDK szolgáltatásokat a U-SQL-feladatok munkaállomáson való futtatására, pontosan úgy, ahogyan azt az Azure Data Lake szolgáltatásban is megteheti. Ez a két, helyi futtatású szolgáltatás időt takarít meg a U-SQL feladatok tesztelése és hibakeresése során.

## <a name="understand-the-data-root-folder-and-the-file-path"></a>Az adatok gyökérmappa és a fájl elérési útja

Helyi futtatás, mind a U-SQL SDK szükséges adatok gyökérmappájában. Az adatok-gyökérmappája "helyi tároló" a helyi számítási fiókhoz. Megegyezik a Data Lake Analytics-fiók az Azure Data Lake Store-fiókjába. Váltás másik adatok-gyökérmappának az csakúgy, mint egy másik store-fiók vált. Ha azt szeretné, más adatok legfelső szintű mappák általában megosztott adatokhoz való hozzáférést, abszolút elérési utakat kell használnia a parancsfájlban. Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) a megosztott adatok az adatok-gyökere alatt.

Az adatok gyökérmappa a következőkre használható:

- Tárolni a metaadatokat, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket.
- Keresse meg a bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL is meg van adva. Relatív elérési utak használata megkönnyíti a U-SQL projekt telepítése az Azure-bA.

U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat. A relatív elérési út a megadott adatok-gyökérmappa elérési útja viszonyítva. Azt javasoljuk, hogy használjon "/" az elérési út elválasztó annak a parancsfájlokat a kiszolgálóoldali kompatibilis. Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat. Ezekben a példákban C:\LocalRunDataRoot az adatok-gyökérmappájába.

|Relatív elérési útja|Abszolút elérési útja|
|-------------|-------------|
|/ABC/DEF/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Futtassa a Visual Studio helyi használata

A Data Lake Tools for Visual Studio a Visual Studio U-SQL helyi futtatási élményt nyújt. Ez a funkció használatával a következő műveletek végezhetők el:

- Helyileg, egy U-SQL-parancsfájl futtatása C#-szerelvények együtt.
- A hibakeresési egy C# szerelvény helyileg.
- Létrehozása, megtekintése és a Server Explorer U-SQL katalógusok (helyi adatbázisokat, szerelvényeket, sémákat és táblák) törlése. A helyi katalógus is is tájékozódhat a Server Explorer.

    ![A Data Lake Tools for Visual Studio helyi futtatási helyi katalógus](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

A Data Lake Tools telepítő mappát hoz létre C:\LocalRunRoot használandó alapértelmezett adatok-gyökérmappájába. Az alapértelmezett helyi futtatási párhuzamossági: 1.

### <a name="to-configure-local-run-in-visual-studio"></a>A Visual Studio helyi Futtatás konfigurálása

1. Nyissa meg a Visual Studiót.
2. Nyissa meg **Server Explorer**.
3. Bontsa ki a **Azure** > **a Data Lake Analytics**.
4. Kattintson a **Data Lake** menüben, majd kattintson **lehetőségek és beállítások**.
5. A bal oldali fában bontsa ki a **Azure Data Lake**, majd bontsa ki a **általános**.

    ![A Data Lake Tools for Visual Studio helyi futtatási beállítások konfigurálása](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

A Visual Studio U-SQL projekt végrehajtása helyi futtatásához szükség. Ez a kijelző nem azonos a U-SQL-parancsfájlok futtatásakor, az Azure-ból.

### <a name="to-run-a-u-sql-script-locally"></a>U-SQL parancsfájl futtatása helyben
1. A Visual Studio eszközből a U-SQL projekt megnyitása.   
2. Kattintson a jobb gombbal a Solution Explorer U-SQL parancsfájl, és kattintson **parancsfájl elküldése**.
3. Válassza ki **(helyi)** a Analytics-fiók helyileg futtassa a parancsfájlt.
Is kattinthat a **(helyi)** parancsfájl ablak tetején fiókra, majd **Submit** (vagy használja a Ctrl + F5 billentyűparancsot).

    ![Data Lake Tools for Visual Studio helyi futtatási küldés feladatok](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Parancsfájlok és C#-szerelvények helyi hibakeresése

A hibakeresési C#-szerelvények elküldené és regisztrálná őket az Azure Data Lake Analytics szolgáltatás nélkül. Töréspontokat állíthat be a fájl mögötti kódban és a hivatkozott C#-projektben is.

#### <a name="to-debug-local-code-in-code-behind-file"></a>Helyi kódok hibakeresése fájl mögötti kódban

1. Állítson be töréspontokat a fájl mögötti kódban.
2. Nyomja le az F5 billentyűt a parancsfájl helyi hibakereséséhez.

> [!NOTE]
   > A következő eljárás csak a Visual Studio 2015 esetében működik. A régebbi kiadásokban lehetséges, hogy kézzel kell megadnia a pdb-fájlokat.  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a>Helyi kódok hibakeresése egy hivatkozott C#-projektben

1. Hozzon létre egy C#-szerelvényprojektet, és állítsa be úgy, hogy hozza létre a kimeneti dll-fájlt.
2. Regisztrálja a dll-fájlt egy U-SQL-kivonat használatával:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Állítson be töréspontokat a C#-kódban.
4. Nyomja le az F5 billentyűt a C#-DLL-helyileg fájlra hivatkozó parancsfájl hibakereséséhez.

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a>Használata helyi futtatáskor a Data Lake U-SQL-SDK-ból

U-SQL-parancsfájlok futtatása helyben a Visual Studio használatával, valamint az Azure Data Lake U-SQL SDK használhatja U-SQL-parancsfájlok helyi futtatásához a parancssori és alkalmazásprogramozási felülethez. A fenti méretezheti a U-SQL helyi tesztelése.

További információ [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Következő lépések

* Egy összetettebb lekérdezés megtekintéséhez lásd: [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).
* Feladat részleteinek megtekintése: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).
* Ha szeretné használni a vertex végrehajtási nézetet, lásd [a Vertex végrehajtási nézetet használja a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
