---
title: "a Storm és HBase a HDInsight időbeli aaaCorrelate események"
description: "Megtudhatja, hogyan toocorrelate események kiszolgálófarmban lévő különböző időpontokban, a HDInsight alatt futó Storm és HBase használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: f915eef77bbda5b02bfd02ad0b5a4923ff2f4f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a>Az eseményeket, amelyek különböző időpontokban érkeznek összefüggéseket Storm és HBase használatával

Apache Storm egy állandó adattár használatával hozhatók adatok bejegyzéseit, amelyek a kiszolgálófarmban lévő különböző időpontokban. Például létrehozhatja, ha egy felhasználói munkamenet toocalculate bejelentkezési és kijelentkezési eseményeket hogyan hosszú hello munkamenet tartott.

Ebből a dokumentumból megismerheti, hogyan toocreate egy alapszintű C# Storm-topológia, amely nyomon követi a bejelentkezési és kijelentkezési eseményeit a felhasználói munkamenetekben, és hello munkamenet időtartama alatt hello számítja ki. hello topológia állandó adattárként HBase eszközt használja. A HBase segítségével tooperform kötegelt lekérdezések hello előzményadatokat tooproduce további betekintést nyerjen a. Például hány felhasználói munkamenetek lettek elindítva, vagy egy adott időszakra vonatkozóan befejeződött.

## <a name="prerequisites"></a>Előfeltételek

* A Visual Studio és hello HDInsight Visual Studio eszközök. További információkért lásd: [Ismerkedés hello HDInsight tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

* A HDInsight alatt futó Apache Storm a fürt (Windows-alapú).

  > [!WARNING]
  > SCP.NET topológiák támogatottak a Linux-alapú Storm-fürtök létrehozása után 10/28/2016, hello HBase SDK elérhető 10/28/2016 .NET-csomag nem működik megfelelően a Linux-alapú HDInsight-on

* Apache HBase on HDInsight-fürt (Linux vagy a Windows-alapú).

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Java](https://java.com) 1.7, vagy annál nagyobb a fejlesztési környezetet. Java használt toopackage hello topológia, ha az elküldött toohello HDInsight-fürt.

  * Hello **JAVA_HOME** környezeti változó kell pont toohello tartalmazó könyvtár Java.
  * Hello **%JAVA_HOME%/bin** könyvtárnak kell lennie a hello elérési útja

## <a name="architecture"></a>Architektúra

![Hello adatfolyama keresztül hello topológia ábrája](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

Hello eseményforrás megegyező azonosító adatok események igényel. For example, egy felhasználói Azonosítót, munkamenet-azonosító vagy egyéb adat, amely nem a) egyedi és b) szerepelnek az összes küldött adatok tooStorm. Ez a példa egy GUID-érték toorepresent egy munkamenet-azonosítót.

Ebben a példában két HDInsight-fürtök áll:

* HBase: állandó adattár korábbi adatok
* A Storm: használt tooingest bejövő adatok

hello adatok véletlenszerűen generálja a hello Storm-topológia, és a következő elemek hello áll:

* Munkamenet-azonosító: egy GUID, amely egyedileg azonosítja mindegyik munkamenethez
* Esemény: egy kezdeti vagy záró esemény. Ebben a példában START mindig várakozni vége előtt
* : Hello idő hello esemény.

Ezek az adatok feldolgozása és HBase tárolja.

### <a name="storm-topology"></a>Storm-topológia

A munkamenet akkor kezdődik, amikor egy **START** esemény érkezik hello topológia és tooHBase naplózza. Ha egy **END** esemény érkezik, hello topológia lekéri hello **START** esemény, és kiszámítja az hello idő hello két események között. Ez **időtartam** érték aztán a HBase együtt hello **END** események adatait.

> [!IMPORTANT]
> Ez a topológia alapszintű hello minta mutatja be, amíg egy éles megoldás kell tootake tervezési a következő forgatókönyvek hello:
>
> * Sorrendje nem érkező események
> * Ismétlődő esemény
> * Az eldobott események

a következő összetevők hello hello minta topológia áll:

* Session.cs: hozzon létre egy véletlenszerű munkamenet-Azonosítót, kezdési időt és mennyi ideig hello munkamenet tart a felhasználói munkamenet szimulálja.

* Spout.cs: 100 munkamenetek hoz létre, megfelelően kibocsát egy indítási esemény, hello véletlenszerű időtúllépés megvárja, mindegyik munkamenethez, és majd bocsát ki a záró eseményt. Újrahasznosítja azt majd munkamenetek toogenerate újakat véget ért.

* HBaseLookupBolt.cs: hello munkamenet azonosítója toolook munkamenet adatait a HBase használja. Záró események feldolgozása után hello megfelelő KEZDŐ esemény megkeresése, és hello munkamenet időtartama alatt hello számítja ki.

* HBaseBolt.cs: HBase adatait tárolja.

* TypeHelper.cs: Amikor olvasása / írása tooHBase típuskonverziós nyújt segítséget.

### <a name="hbase-schema"></a>A HBase séma

A HBase hello adatok tárolt egy táblázatot a következő séma/beállításai hello:

* Sorkulcsa: hello munkamenet-Azonosítót használja a tábla sorainak hello kulcsaként.

* Oszlop család: hello családi név megadása "cf". A családban tárolt oszlopok a következők:

  * esemény: KEZDŐ vagy záró.

  * idő: hello idő ezredmásodpercben, amelyet hello esemény történt.

  * időtartam: hello közötti KEZDÉSI és BEFEJEZÉSI esemény.

* VERZIÓK: hello "cf" termékcsalád van beállítva, hogy minden egyes sorára tooretain 5 verzióit.

  > [!NOTE]
  > Azok az egy adott sortól kulcs tárolt előző értékek naplózása verziók. Alapértelmezés szerint a HBase csak egy sor legújabb verziója hello hello értékét adja vissza. Ebben az esetben hello ugyanabban a sorban szolgál az összes esemény (KEZDŐ, záró.) egy sor minden verziója hello timestamp érték azonosít. Verziók, adjon meg egy egyedi azonosítót. a naplózott események ügyfélállapotainak

## <a name="download-hello-project"></a>Hello projekt letöltése

hello minta projekt letölthető [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).

Ez a letöltés a következő C# projektek hello tartalmazza:

* CorrelationTopology: C# Storm-topológia, hogy véletlenszerűen bocsát ki a kezdő és záró események a felhasználói munkamenetekben. Minden munkamenet érvényes, 1 és 5 perc között.

* SessionInfo: C# konzolalkalmazást, amely hello HBase táblát hoz létre, és tájékoztatást mintalekérdezéseket tooreturn tárolt munkamenet-adatokat.

## <a name="create-hello-table"></a>Hello tábla létrehozása

1. Nyissa meg hello **SessionInfo** projektre a Visual Studióban.

2. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **SessionInfo** projektre, és válassza ki **tulajdonságok**.

    ![A kijelölt tulajdonság menü képernyőképe](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. Válassza ki **beállítások**, majd a készlet hello a következő értékeket:

   * HBaseClusterURL: hello URL-cím tooyour HBase-fürtöt. Például https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: hello admin/HTTP felhasználói fiók a fürt

   * HBaseClusterPassword: hello hello admin/HTTP felhasználói fiók jelszavát

   * HBaseTableName: Ebben a példában a hello tábla toouse hello neve

   * HBaseTableColumnFamily: hello oszlop családnév

     ![Beállítások párbeszédpanel képe](./media/hdinsight-storm-correlation-topology/settings.png)

4. Futtassa a hello megoldás. Amikor a rendszer kéri, válassza ki a hello "c" kulcs toocreate hello tábla a HBase-fürtöt.

## <a name="build-and-deploy-hello-storm-topology"></a>Hozza létre és hello Storm-topológia központi telepítéséhez

1. Nyissa meg hello **CorrelationTopology** a Visual Studio megoldás.

2. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **CorrelationTopology** projektre, és válassza a tulajdonságok.

3. Hello tulajdonságai ablakban válassza ki **beállítások** és adja meg a hello konfigurációs ebben a projektben. hello első 5 hello értékeitől használt hello **SessionInfo** projekt:

   * HBaseClusterURL: hello URL-cím tooyour HBase-fürtöt. Például https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: hello admin/HTTP felhasználói fiók a fürt számára.

   * HBaseClusterPassword: hello hello admin/HTTP felhasználói fiók jelszavát.

   * HBaseTableName: Ebben a példában a hello tábla toouse hello neve. Ez az érték van hello azonos módon hello SessionInfo projektben használt tábla neve.

   * HBaseTableColumnFamily: hello oszlop családnév. Ez az érték van hello azonos oszlop családnév szerint hello SessionInfo projektben használt.

   > [!IMPORTANT]
   > Nem módosítható hello HBaseTableColumnNames, mert a hello azok által használt hello nevének **SessionInfo** tooretrieve adatokat.

4. Hello tulajdonságok mentéséhez, majd hello projekt felépítéséhez.

5. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**. Ha a rendszer kéri, adja meg hello hitelesítő adatait az Azure-előfizetéshez.

   ![Hello képe nyújt toostorm menüpont](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. A hello **nyújt topológia** párbeszédablak, ez a topológia a toodeploy kívánt válassza hello Storm-fürt.

   > [!NOTE]
   > hello első alkalommal, amikor elküld, hogy a topológia eltarthat néhány másodpercig tooretrieve hello nevét a HDInsight-fürtök.

7. Hello topológia feltöltött és elküldött toohello fürt követően hello **Storm-topológia nézet** megnyitja és topológia futtató hello megjeleníti. toorefresh hello adatokat, válassza hello **CorrelationTopology** és hello frissítés gomb használatának hello hello oldal jobb felső.

   ![Hello topológia e nézetében képe](./media/hdinsight-storm-correlation-topology/topologyview.png)

   Elindulásakor hello topológia adatok hello hello érték **Emitted** oszlop növekmények esetén.

   > [!NOTE]
   > Ha hello **Storm-topológia nézet** automatikusan nyílik meg, ne használja hello tooopen lépéseket követve:
   >
   > 1. A **Megoldáskezelőben**, bontsa ki a **Azure**, majd bontsa ki a **HDInsight**.
   > 2. Kattintson a jobb gombbal hello Storm-fürt, amely topológia hello fut, és válassza **nézet Storm-topológiák**

## <a name="query-hello-data"></a>Hello adatait kérdezi le.

Amennyiben az adatok kibocsátott használja a következő lépéseket tooquery hello adatok hello.

1. Térjen vissza a toohello **SessionInfo** projekt. Ha nem fut, indítsa el egy új példányát.

2. Amikor a rendszer kéri, válassza ki a **s** toosearch START esemény. Rákérdezéses tooenter egy kezdő és záró idő toodefine egy időtartományt - visszaadott csak események ezen két időpontok között.

    Használjon hello alábbi formázása hello start megadásakor, és befejezési időpontja: óó: PP és "vagyok" vagy "pm". Ha például 23:20 óra.

    tooreturn naplózott események, használja a kezdő időpont előtt hello Storm-topológia telepítve lett, és most a befejezési ideje. hello visszatérési adatainak bejegyzéseket a következő szöveg hasonló toohello tartalmazza:

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

Keresése záró események works hello azonos KEZDŐ eseményként is rögzíti. Azt jelzi, hogy 1 és 5 perc után hello START esemény között véletlenszerűen jönnek-azonban záró eseményeket. Előfordulhat, hogy tootry néhány alkalommal tartományok toofind hello záró eseményeket. ZÁRÓ eseményeket is tartalmazza hello munkamenet időtartama alatt hello - hello esemény kezdete és vége esemény hello különbsége. Itt látható egy példa az END eseményekre vonatkozó adatokat:

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> Míg hello idő értékeknek meg helyi idő, hello hello lekérdezés által visszaadott ideje UTC formátumban.

## <a name="stop-hello-topology"></a>Hello topológia leállítása

Amikor készen áll a toostop hello topológia, térjen vissza a toohello **CorrelationTopology** projektre a Visual Studióban. A hello **Storm-topológia nézet**, válassza ki hello topológiát, majd hello **Kill** hello tetején hello topológia megtekintése gombra.

## <a name="delete-your-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

További Storm, tekintse meg a [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).
