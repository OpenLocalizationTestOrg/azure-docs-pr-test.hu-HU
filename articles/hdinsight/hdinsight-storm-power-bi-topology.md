---
title: "A Power BI - az Azure HDInsight alatt futó Apache Storm használható |} Microsoft Docs"
description: "A C#-topológiák egy hdinsight alatt futó Apache Storm-fürt futó használ az adatok Power BI-jelentés létrehozása."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 36fe3b9c-5232-4464-8d75-95403b6da7a1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/31/2017
ms.author: larryfr
ms.openlocfilehash: 36487c0c34e5a4bb955bbc15c8c96b9e838aeb44
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-power-bi-to-visualize-data-from-an-apache-storm-topology"></a>Az Apache Storm-topológia adatainak megjelenítése Power BI használatával

A Power BI lehetővé teszi jelentések adatok vizuálisan megjeleníteni. Ez a dokumentum egy HDInsight alatt futó Apache Storm segítségével hozhat létre az adatokat a Power BI példát.

> [!NOTE]
> A jelen dokumentumban leírt lépések a Visual Studio Windows fejlesztői környezetre támaszkodnak. A lefordított projekt küldheti el a Linux-alapú HDInsight-fürthöz. Csak Linux-alapú fürtökön után létrehozott 10/28/2016 támogatási SCP.NET topológiákat.
>
> C#-topológiák használata a Linux-alapú fürtöt, frissítse a Microsoft.SCP.Net.SDK NuGet-csomagot a projekt által használt 0.10.0.6 verzió vagy újabb. A csomag verziójának a HDInsightban telepített Storm főverziójával is egyeznie kell. Például, a HDInsight 3.3 és 3.4 verzióján futó Storm a Storm 0.10.x verzióját használja, míg a HDInsight 3.5 a Storm 1.0.x verziót.
>
> A Linux-alapú fürtök C#-topológiáinak a .NET 4.5-öt kell használnia, és a Mono segítségével futhatnak a HDInsight-fürtön. A legtöbb dolgot működik. Azonban ellenőrizni kell a [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.
>
> Ebben a projektben működik a Linux- vagy Windows-alapú hdinsight eszközzel, a Java-verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="prerequisites"></a>Előfeltételek

* Egy Azure Active Directory-felhasználó a [Power BI](https://powerbi.com) hozzáférést.
* HDInsight-fürtöt. További információkért lásd: [beolvasása használatába a HDInsight alatt futó Storm](hdinsight-apache-storm-tutorial-get-started-linux.md).

  > [!IMPORTANT]
  > A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* A Visual Studio (a következő verziók egyike)

  * A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)
  * A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)
  * [A Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * A Visual Studio 2017 (minden kiadás)

* A HDInsight Tools for Visual Studio: lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) információt a telepítési információkat.

## <a name="how-it-works"></a>Működés

Ebben a példában a C# Storm-topológia véletlenszerűen előállított az Internet Information Services (IIS) naplóadatokat tartalmazza. Ezek az adatok majd írni egy SQL-adatbázis, és az ott szolgál a Power BI-jelentések készítéséhez.

A következő fájlok valósítja meg a jelen példában a fő funkciókat:

* **SqlAzureBolt.cs**: az SQL Database Storm-topológia előállított adatokat ír az adatbázis.
* **IISLogsTable.sql**: az adatbázis tárolt adatok létrehozásához használja a Transact-SQL utasítások.

> [!WARNING]
> A tábla létrehozása az SQL-adatbázis a topológia a HDInsight-fürt megkezdése előtt.

## <a name="download-the-example"></a>A példa letöltése

Töltse le a [HDInsight C# Storm a Power BI példa](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi). Letöltheti, vagy elágazás/Klónozás használatával [git](http://git-scm.com/), vagy használja a **letöltése** egy .zip az archívum letöltésére mutató hivatkozás.

## <a name="create-a-database"></a>Adatbázis létrehozása

1. Hozzon létre egy adatbázist, használja a lépéseket a [SQL Database oktatóanyag](../sql-database/sql-database-get-started.md) dokumentum.

2. Az adatbázis lépéseit követve csatlakozhat a [Csatlakozás SQL adatbázishoz a Visual Studio](../sql-database/sql-database-connect-query.md) dokumentum.

3. Az Object Explorer ablaktáblában kattintson a jobb gombbal az adatbázist, és válassza ki **új lekérdezés**. Tartalmának beillesztése a **IISLogsTable.sql** fájlt a letöltött projektből szerepel a lekérdezési ablakba, és majd használja a Ctrl + Shift + E végrehajtsák a lekérdezést. A parancsok sikeresen befejeződött a következő üzenetet kell látnia.

## <a name="configure-the-sample"></a>A minta konfigurálása

1. Az a [Azure-portálon](https://portal.azure.com), válassza ki az SQL-adatbázis. Az a **Essentials** SQL adatbázis paneljén válassza szakasza **adatbázis-kapcsolati karakterláncok megjelenítése**. A megjelenő listában másolja a **ADO.NET (SQL-hitelesítés)** információkat.

2. Nyissa meg a minta a Visual Studióban. A **Megoldáskezelőben**, nyissa meg a **App.config** fájlt, és keresse meg a következő bejegyzést:

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    Cserélje le a **TOBEFILLED ##** az előző lépésben másolt érték és az adatbázis-kapcsolati karakterlánc. Cserélje le **{a\_felhasználónév}** és **{a\_jelszó}** a felhasználónevet és jelszót az adatbázishoz.

3. Mentse és zárja be a fájlokat.

## <a name="deploy-the-sample"></a>A minta telepítése

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a **StormToSQL** projektre, és válassza ki **Submit a HDInsight alatt futó Storm**. Válassza ki a HDInsight-fürt a **Storm-fürt** legördülő párbeszédpanel.

   > [!NOTE]
   > Néhány másodpercet vehet igénybe a **Storm-fürt** legördülő kiszolgálónevekkel feltöltéséhez.
   >
   > Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatok az Azure-előfizetéshez. Ha egynél több előfizetéssel rendelkezik, jelentkezzen be, amely tartalmazza a Storm on HDInsight-fürt.

2. A topológia elküldésekor a __topológia Viewer__ jelenik meg. Ez a topológia megtekintéséhez jelölje ki a SqlAzureWriterTopology bejegyzést a listából.

    ![A topológia esetén a kiválasztott topológia](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    A topológia információk megjelenítéséhez használja ezt a nézetet, vagy a topológia egy összetevő vonatkozó információk megjelenítéséhez kattintson duplán a (például a SqlAzureBolt) bejegyzése.

3. Után a topológia rendelkezik néhány percig, lépjen vissza az adatbázis létrehozásához használt SQL-lekérdezés ablak futott. Cserélje le a meglévő utasítások a következő lekérdezést:

        select * from iislogs;

    Használja a Ctrl + Shift + E végrehajtani a lekérdezést, és eredmények eléréséhez a következő adatokat kell kapnia:

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    Ezeket az adatokat a Storm-topológia a van írva.

## <a name="create-a-report"></a>Jelentés létrehozása

1. Csatlakozás a [Azure SQL Database összekötő](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) a Power BI. 

2. Belül **adatbázisok**, jelölje be **beolvasása**.

3. Válassza ki **Azure SQL Database**, majd válassza ki **Connect**.

    > [!NOTE]
    > A folytatáshoz a Power BI Desktop letöltése kérheti. Ha igen, tegye a következőket való csatlakozáshoz:
    >
    > 1. Nyissa meg a Power BI Desktopban, és válassza ki __adatok beolvasása__.
    > 2 válassza __Azure__, majd __Azure SQL adatbázis__.

4. Adja meg az adatokat az Azure SQL Database adatbázishoz való kapcsolódáshoz. Ezt az információt találja látogasson el a [Azure-portálon](https://portal.azure.com) és az SQL-adatbázis kiválasztásával.

   > [!NOTE]
   > Is beállíthatja a frissítési időköz és egyéni szűrők használatával **speciális beállítások engedélyezése** a connect párbeszédpanelről.

5. Miután csatlakozott, látni fogja az azonos nevű új adatkészlet csatlakozva-adatbázisként. Válassza ki a jelentés elkezd adatkészlet.

6. A **mezők**, bontsa ki a **IISLOGS** bejegyzés. Az URI szárakat felsoroló jelentés létrehozásához jelölje be a **lévő egyedi AZONOSÍTÓHOZ**.

    ![A jelentés létrehozása](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. A következő húzza **METÓDUS** a jelentéshez. A jelentés frissítése a szárakat és a megfelelő használja a HTTP-kérelem HTTP-metódus.

    ![a metódus adatok hozzáadása](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. Az a **képi megjelenítések** oszlopból válassza ki a **mezők** ikonra, majd jelölje be a lefelé mutató nyílra, és **METÓDUS** a a **értékek** szakasz. Megjeleníti az URI elérése hány alkalommal számát, jelölje be **száma**.

    ![A metódusok számát módosítása](./media/hdinsight-storm-power-bi-topology/count.png)

9. Ezután válassza ki a **halmozott oszlopdiagram** az információk megjelenítésének módosítása.

    ![Váltás halmozott diagram](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. Mentse a jelentést, jelölje be **mentése** , és adja meg a jelentés nevét.

## <a name="stop-the-topology"></a>A topológia leállítása

A topológia továbbra is futni megállíthatja vagy a Storm on HDInsight-fürt törlése. A topológia leállítása, hajtsa végre az alábbi lépéseket:

1. A Visual Studióban lépjen vissza a topológia megjelenítő, és válassza ki a topológiát.

2. Válassza ki a **Kill** a topológia leállítása gombra.

    ![A topológia összefoglaló gombjára leállítása](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

Ebben a dokumentumban megtanulhatta a Storm-topológia adatokat küldeni az SQL-adatbázis, majd a Power BI használatával adatainak megjelenítése. Az egyéb Azure hdinsight alatt futó Storm használatával technológiák használata információkért lásd: a következő dokumentumban:

* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)
