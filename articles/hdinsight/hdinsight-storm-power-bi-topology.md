---
title: "a Power BI - Azure HDInsight alatt futó Apache Storm aaaUse |} Microsoft Docs"
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
ms.openlocfilehash: 194cd8220bd60475af1d64a110e4b23ef92e1bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-toovisualize-data-from-an-apache-storm-topology"></a>A Power BI toovisualize adatait használhatja az Apache Storm-topológia

A Power BI lehetővé teszi toovisually adatok megjelenítése jelentésként. Ez a dokumentum nyújt arról, hogyan toouse alatt futó Apache Storm a Power BI HDInsight toogenerate adatokon.

> [!NOTE]
> hello jelen dokumentumban leírt lépések használja a Visual Studio a Windows fejlesztői környezetre. hello lefordított projekt lehet elküldött tooa Linux-alapú HDInsight-fürtöt. Csak Linux-alapú fürtökön után létrehozott 10/28/2016 támogatási SCP.NET topológiákat.
>
> toouse C#-topológiák egy Linux-alapú fürttel, frissítés hello Microsoft.SCP.Net.SDK NuGet-csomagot a projekt tooversion 0.10.0.6 által használt vagy újabb verzióját. hello csomag hello verziójának is egyeznie kell telepíteni a HDInsight alatt futó Storm hello főverziója. Például, a HDInsight 3.3 és 3.4 verzióján futó Storm a Storm 0.10.x verzióját használja, míg a HDInsight 3.5 a Storm 1.0.x verziót.
>
> C#-topológiák Linux-alapú fürtökön kell használni a .NET 4.5, és monó toorun hello HDInsight-fürt használatára. A legtöbb dolgot működik. Azonban célszerű lenne ellenőrizni hello [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.
>
> Ebben a projektben működik a Linux- vagy Windows-alapú hdinsight eszközzel, a Java-verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="prerequisites"></a>Előfeltételek

* Egy Azure Active Directory-felhasználó a [Power BI](https://powerbi.com) hozzáférést.
* HDInsight-fürtöt. További információkért lásd: [beolvasása használatába a HDInsight alatt futó Storm](hdinsight-apache-storm-tutorial-get-started-linux.md).

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* A Visual Studio (hello a következő verziók egyike)

  * A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)
  * A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)
  * [A Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * A Visual Studio 2017 (minden kiadás)

* a HDInsight Tools for Visual Studio hello: lásd: [Ismerkedés hello HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) információt a telepítési információkat.

## <a name="how-it-works"></a>Működés

Ebben a példában a C# Storm-topológia véletlenszerűen előállított az Internet Information Services (IIS) naplóadatokat tartalmazza. Ezek az adatok majd írása tooa SQL-adatbázis, és ott használt toogenerate jelentéseket a Power bi-ban.

a következő fájlok megvalósítása hello fő funkcióit a jelen példában hello:

* **SqlAzureBolt.cs**: hello Storm-topológia tooSQL adatbázis előállított adatokat ír az adatbázis.
* **IISLogsTable.sql**: hello Transact-SQL utasítás használt toogenerate hello adatbázis, amely hello adatokat tárolja.

> [!WARNING]
> Hello tábla létrehozása az SQL-adatbázis a HDInsight-fürt hello topológia megkezdése előtt.

## <a name="download-hello-example"></a>Töltse le a hello – példa

Töltse le a hello [HDInsight C# Storm a Power BI példa](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi). toodownload, vagy elágazás/Klónozás használatával [git](http://git-scm.com/), vagy használjon hello **letöltése** hivatkozás toodownload egy .zip hello archívum.

## <a name="create-a-database"></a>Adatbázis létrehozása

1. egy adatbázis toocreate lépésekkel hello a hello [SQL Database oktatóanyag](../sql-database/sql-database-get-started.md) dokumentum.

2. Csatlakozás toohello adatbázis által következő hello szükséges lépések hello [tooa SQL-adatbázis csatlakozzon a Visual Studio](../sql-database/sql-database-connect-query.md) dokumentum.

3. Az Object Explorer ablaktáblában kattintson a jobb gombbal a hello adatbázis, és válassza ki **új lekérdezés**. Illessze be a hello hello tartalmát **IISLogsTable.sql** hello fájl letöltése hello lekérdezési ablakba, és a Ctrl + Shift + E tooexecute hello lekérdezés. Egy üzenet, amely a parancs sikeresen végrehajtva hello kell kapnia.

## <a name="configure-hello-sample"></a>Hello minta konfigurálása

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki az SQL-adatbázis. A hello **Essentials** hello SQL adatbázis panelen válassza szakasza **adatbázis-kapcsolati karakterláncok megjelenítése**. Hello listában megjelenő, másolja a hello **ADO.NET (SQL-hitelesítés)** információkat.

2. Nyissa meg a Visual Studio hello minta. A **Megoldáskezelőben**, nyissa meg hello **App.config** fájlt, és keresse meg a következő bejegyzés hello:

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    Cserélje le a hello **TOBEFILLED ##** hello előző lépésben másolt érték hello adatbázis kapcsolati karakterlánccal. Cserélje le **{a\_felhasználónév}** és **{a\_jelszó}** hello felhasználónévvel és jelszóval hello adatbázis.

3. Mentse és zárja be a hello fájlok.

## <a name="deploy-hello-sample"></a>Hello mintaalkalmazás telepítését

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **StormToSQL** projektre, és válassza ki **elküldeni a HDInsight tooStorm**. SELECT hello HDInsight fürt hello **Storm-fürt** legördülő párbeszédpanel.

   > [!NOTE]
   > A hello néhány másodpercet vehet igénybe **Storm-fürt** legördülő toopopulate kiszolgálónevekkel.
   >
   > Ha a rendszer kéri, adja meg hello bejelentkezési hitelesítő adatait az Azure-előfizetéshez. Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.

2. Hello topológia elküldésekor hello __topológia Viewer__ jelenik meg. tooview a Kiszolgálótopológia, a select hello SqlAzureWriterTopology bejegyzést hello listából.

    ![hello topológiák kijelölt hello topológia](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    Használja a nézet toosee információk hello topológia, vagy kattintson duplán egy bejegyzés (például hello SqlAzureBolt) toosee információt adott tooa összetevő hello topológiában.

3. Hello után topológia rendelkezik futott néhány percig, visszatérési toohello SQL lekérdezési ablakba toocreate hello adatbázis használta. Cserélje le a következő lekérdezés hello hello meglévő utasításokat:

        select * from iislogs;

    Használja a Ctrl + Shift + E tooexecute hello lekérdezést, és meg kell kapnia eredmények hasonló toohello a következő adatokat:

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    Ezeket az adatokat a Storm-topológia hello van írva.

## <a name="create-a-report"></a>Jelentés létrehozása

1. Csatlakozás toohello [Azure SQL Database összekötő](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) a Power BI. 

2. Belül **adatbázisok**, jelölje be **beolvasása**.

3. Válassza ki **Azure SQL Database**, majd válassza ki **Connect**.

    > [!NOTE]
    > Előfordulhat, hogy toodownload hello Power BI Desktop toocontinue kéri. Ha igen, használja a következő lépéseket tooconnect hello:
    >
    > 1. Nyissa meg a Power BI Desktopban, és válassza ki __adatok beolvasása__.
    > 2 válassza __Azure__, majd __Azure SQL adatbázis__.

4. Adja meg a hello információk tooconnect tooyour Azure SQL Database. Ezt az információt találja hello felkeresésével [Azure-portálon](https://portal.azure.com) és az SQL-adatbázis kiválasztásával.

   > [!NOTE]
   > Emellett beállíthatja hello frissítési időköz és egyéni szűrők használatával **speciális beállítások engedélyezése** hello a csatlakozás a párbeszédpanelen.

5. Miután csatlakozott, megjelenik egy új adatkészlet hello azonos nevét hello adatbázisként, csatlakozik. Válassza ki a hello dataset toobegin jelentés tervezéséhez.

6. A **mezők**, bontsa ki a hello **IISLOGS** bejegyzés. toocreate egy jelentést, hogy listák hello URI-törzs, válassza ki a hello jelölőnégyzetét **lévő egyedi AZONOSÍTÓHOZ**.

    ![A jelentés létrehozása](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. A következő húzza **METÓDUS** toohello jelentés. hello jelentés frissítések toolist hello törzs, és HTTP-kérelem hello hello megfelelő HTTP-metódus használható.

    ![hello metódus adatok hozzáadása](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. A hello **képi megjelenítések** oszlop, jelölje be hello **mezők** ikonra, és válassza hello lefelé mutató nyíl mellett túl**METÓDUS** a hello **értékek**szakasz. toodisplay hány alkalommal URI számát elérése, válassza ki **száma**.

    ![Módszerek tooa számának módosítása](./media/hdinsight-storm-power-bi-topology/count.png)

9. Ezután válassza ki a hello **halmozott oszlopdiagram** toochange hogyan hello információk jelennek meg.

    ![Változó tooa halmozott diagram](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. toosave hello jelentés, jelölje be **mentése** , és írja be a hello jelentés nevét.

## <a name="stop-hello-topology"></a>Hello topológia leállítása

hello topológia toorun folytatódik, amíg megállíthatja vagy hello Storm on HDInsight-fürt törlése. toostop hello topológia, hajtsa végre az alábbi lépésekkel hello:

1. A Visual Studio toohello topológia viewer vissza, és válassza a hello topológia.

2. Jelölje be hello **Kill** gomb toostop hello topológia.

    ![A Kill hello topológia összefoglaló gomb](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

Ebből a dokumentumból megtanulta, hogyan toosend adatait egy Storm-topológia tooSQL adatbázis, majd adato hello Power BI használatával. Információ és a többi Azure HDInsight a Storm használatának technológia toowork, tekintse meg a következő dokumentum hello:

* [HDInsight alatt futó Storm példatopológiái](hdinsight-storm-example-topology.md)
