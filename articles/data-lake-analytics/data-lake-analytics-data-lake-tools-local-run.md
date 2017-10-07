---
title: "aaaTest és hibakeresési U-SQL feladatok használatával helyi futtatása és hello Azure Data Lake U-SQL SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio és hello Azure Data Lake U-SQL SDK tootest és hibakeresési U-SQL feladatok a helyi munkaállomáson."
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
ms.openlocfilehash: be04558a504acf6a088e207608ee2d4a011d3ffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-hello-azure-data-lake-u-sql-sdk"></a>Tesztelése és hibakeresése a U-SQL feladatok segítségével helyi futtatásához, hello Azure Data Lake U-SQL-SDK

Használhatja Azure Data Lake Tools for Visual Studio és hello Azure Data Lake U-SQL SDK toorun U-SQL feladatok a munkaállomáson, ugyanúgy, mint a hello Azure Data Lake szolgáltatásban. Ez a két, helyi futtatású szolgáltatás időt takarít meg a U-SQL feladatok tesztelése és hibakeresése során.

## <a name="understand-hello-data-root-folder-and-hello-file-path"></a>Hello adatok-gyökérmappa és hello fájl elérési útja

Helyi Futtatás és a U-SQL SDK hello szükséges adatok gyökérmappájában. hello adatok-gyökérmappája "helyi tároló" hello helyi számítási fiókhoz. Egyenértékű toohello Azure Data Lake Store-fiók egy Data Lake Analytics-fiók is. Váltás tooa különböző adatok-gyökérmappája hasonlóan tooa különböző store-fiók vált. Ha azt szeretné, hogy tooaccess hagyományosan megosztott adatokat különböző adatgyökerében mappákkal, a parancsfájlok abszolút elérési utakat kell használnia. Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) hello adatgyökerében mappa toopoint toohello a megosztott adatok.

hello adatgyökerében mappa a következőkre használható:

- Tárolni a metaadatokat, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket.
- Hello bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL definiált kereséséhez. Relatív elérési utak révén könnyebben toodeploy a U-SQL-projektek tooAzure.

U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat. hello relatív elérési út relatív toohello megadott adatok-gyökérmappa elérési útja. Azt javasoljuk, hogy azt használja "/", hello elérési útját elválasztójel toomake hello kiszolgálóoldali kompatibilis a parancsfájlokat. Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat. Ezekben a példákban C:\LocalRunDataRoot hello adatok-gyökérmappájába.

|Relatív elérési útja|Abszolút elérési útja|
|-------------|-------------|
|/ABC/DEF/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Futtassa a Visual Studio helyi használata

A Data Lake Tools for Visual Studio a Visual Studio U-SQL helyi futtatási élményt nyújt. Ez a funkció használatával a következő műveletek végezhetők el:

- Helyileg, egy U-SQL-parancsfájl futtatása C#-szerelvények együtt.
- A hibakeresési egy C# szerelvény helyileg.
- Létrehozása, megtekintése és a Server Explorer U-SQL katalógusok (helyi adatbázisokat, szerelvényeket, sémákat és táblák) törlése. Hello helyi katalógus is is tájékozódhat a Server Explorer.

    ![A Data Lake Tools for Visual Studio helyi futtatási helyi katalógus](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

hello Data Lake Tools installer egy C:\LocalRunRoot mappa toobe használja hello alapértelmezett adatok hoz. hello alapértelmezett helyi futtatási párhuzamossági: 1.

### <a name="tooconfigure-local-run-in-visual-studio"></a>Futtassa a Visual Studio helyi tooconfigure

1. Nyissa meg a Visual Studiót.
2. Nyissa meg **Server Explorer**.
3. Bontsa ki a **Azure** > **a Data Lake Analytics**.
4. Kattintson a hello **Data Lake** menüben, majd kattintson **lehetőségek és beállítások**.
5. A hello bal konzolfáján bontsa ki a **Azure Data Lake**, majd bontsa ki a **általános**.

    ![A Data Lake Tools for Visual Studio helyi futtatási beállítások konfigurálása](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

A Visual Studio U-SQL projekt végrehajtása helyi futtatásához szükség. Ez a kijelző nem azonos a U-SQL-parancsfájlok futtatásakor, az Azure-ból.

### <a name="toorun-a-u-sql-script-locally"></a>helyileg toorun egy U-SQL-parancsfájl
1. A Visual Studio eszközből a U-SQL projekt megnyitása.   
2. Kattintson a jobb gombbal a Solution Explorer U-SQL parancsfájl, és kattintson **parancsfájl elküldése**.
3. Válassza ki **(helyi)** hello Analytics fiók toorun helyileg a parancsfájlt.
Kattintson a hello **(helyi)** parancsfájl ablak tetején hello fiókot, és kattintson a **küldje el a következőt** (vagy hello Ctrl + F5 billentyűkombináció).

    ![Data Lake Tools for Visual Studio helyi futtatási küldés feladatok](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Parancsfájlok és C#-szerelvények helyi hibakeresése

A hibakeresési C#-szerelvények elküldené és regisztrálná őket tooAzure Data Lake Analytics szolgáltatás nélkül. Töréspontokat állíthat mindkét hello fájl mögötti kódban és a hivatkozott C#-projektben.

#### <a name="toodebug-local-code-in-code-behind-file"></a>helyi kódok toodebug fájl mögötti kódban

1. Állítson be töréspontokat a fájl mögötti kódban hello.
2. Nyomja le az F5 toodebug hello parancsfájl helyi.

> [!NOTE]
   > a következő eljárást csak akkor működik a Visual Studio 2015 hello. A régebbi kiadásokban esetleg toomanually hozzáadása hello pdb-fájlokat.  
   >
   >

#### <a name="toodebug-local-code-in-a-referenced-c-project"></a>a hivatkozott C# projekt helyi kód toodebug

1. Hozzon létre egy C#-szerelvényprojektet, és állítsa be úgy toogenerate hello kimeneti dll-fájlt.
2. Regisztrálja a hello dll-fájlt egy U-SQL utasítás használatával:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Állítson be töréspontokat a C#-kódban hello.
4. Nyomja le az F5 toodebug hello parancsprogram helyileg hello C#-dll hivatkozik.

## <a name="use-local-run-from-hello-data-lake-u-sql-sdk"></a>Használjon helyi hello Data Lake U-SQL-SDK-ről futtatva

Továbbá a toorunning U-SQL Visual Studio használatával helyileg parancsfájlok, használja hello Azure Data Lake U-SQL SDK toorun U-SQL-parancsfájlok helyi a parancssori és alkalmazásprogramozási felülethez. A fenti méretezheti a U-SQL helyi tesztelése.

További információ [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Következő lépések

* egy összetettebb lekérdezés toosee lásd [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).
* feladat részletei tooview, lásd: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).
* toouse hello vertex végrehajtási nézetet, lásd: [használata hello Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
