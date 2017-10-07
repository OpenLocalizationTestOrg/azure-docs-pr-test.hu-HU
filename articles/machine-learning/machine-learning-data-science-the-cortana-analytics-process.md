---
title: "aaaWhat Team adatok tudományos folyamat?  | Microsoft Docs"
description: "hello csapat az tudományos folyamata a rendszeres módszer intelligens speciális elemzésekre használó alkalmazások készítéséhez."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a098aa2e-fd79-4543-8e15-9aae9d8b3ee6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: bradsev
ROBOTS: NOINDEX
redirect_url: data-science-process-overview
redirect_document_id: True
ms.openlocfilehash: 57187be9c884389c13c226eab74aff137f5514a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-team-data-science-process-tdsp"></a>Mi az a hello Team adatok tudományos folyamat (TDSP)?
Hello [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md) toobuilding intelligens alkalmazásokat, amelyek lehetővé teszi, hogy adatokat kutatók toocollaborate csoportjai hatékonyan hello egész életciklusát a szükséges tevékenységek keresztül rendszeres megközelítését ismerteti tooturn termékek ezeket az alkalmazásokat. hello TDSP ismerteti lépések biztosító sorozatát **útmutatást** toodefine hello probléma hello eszközök beállítása és szükség esetén környezet hogyan vonatkozó adatok elemzése, a build és értékelje ki a prediktív modelleket, és ezek a modellek telepíteni vállalati alkalmazások. 

Az alábbiakban ismertetett hello **Team adatok tudományos folyamat**:  

![CAP-munkafolyamat](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

hello folyamat **iteratív**: megismerni az új és meglévő hello vagy hello modellben finomításokra fejlődésének és reworking korábban szerzett a hello feladatütemezési lépéseket igényel. Meglévő szervezeti fejlesztési és a folyamatok projekttervezés **könnyen igazítani** toowork a hello TDSP által definiált feladatütemezési lépéseket. 

hello hello folyamat diagrammed és hello kapcsolással [TDSP képzési terv](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) és folyamata az alábbiakban olvasható.  

## <a name="preparation-steps"></a>Előkészítő lépések
## <a name="p1-plan-hello-analytics-project"></a>P1. Hello analytics projekt tervezése
Indítsa el az analytics-projekt által az üzleti célokat és a problémák meghatározásához. Azokat a **üzleti követelmények**. Ebben a lépésben a központi célkitűzése tooidentify hello üzleti változók (értékesítési előrejelzési vagy valószínűségértékének inverzét rendelés alatt álló csalárd, például hello) hello elemzés igényeinek toopredict toosatisfy ezeket a követelményeket. Majd általában fontosságú toodevelop hello megértéséhez további tervezésre **adatforrások** szükséges tooaddress hello hello projekt analitikai szempontból céljainak. Nem ritka, például toofind, hogy a meglévő rendszerek toocollect, és további típusú adatok tooaddress jelentkezzen probléma hello és hello projekt céljának eléréséhez. Útmutatásért lásd: [tervezze meg a környezetet az hello Team adatok tudományos folyamat](machine-learning-data-science-plan-your-environment.md) és [forgatókönyvek az Azure Machine Learning speciális elemzésekre](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Az elemzési környezet kialakítása
Hello Team adatok tudományos folyamat analytics környezetet több összetevőt foglal magában: 

* **adatok munkaterületek** hello adatok elő van készítve az elemzési és modellezési, ahol 
* egy **feldolgozási infrastruktúra** előzetesen feldolgozni, felfedezése és modellezési hello adatok
* egy **futtatókörnyezeti infrastruktúrán** toooperationalize hello elemzési modellek és használó hello modellek hello intelligens ügyfélalkalmazások futtassa.  

hello analytics infrastruktúra toobe telepítő igénylő gyakran olyan környezetet, amely nem core operációs rendszerek részét képezi. Azonban ez a módszer általában több rendszer hello vállalaton belül, valamint adatforrások külső toohello vállalati adatokat. hello analytics infrastruktúra lehet kizárólag felhőalapú, vagy egy helyszíni telepítőt, vagy a hibrid hello két. További beállítások: [adatok tudományos környezetekben használható hello Team adatok tudományos folyamat beállítása](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Elemzés lépéseket:
## <a name="1-ingest-data-into-hello-analytical-environment"></a>1. Adatok betöltési hello analytical környezetbe
első lépés hello toobring hello vonatkozó adatokat különböző forrásokból, vagy a belül vagy kívül hello vállalati egy elemzési környezetekben, ahol hello azokat a rendszer. Hello **formátum** hello az adatokat a forrás eltérhet hello cél által igényelt hello formátumban. Ezért néhány adatok átalakítása is toobe hello adatfeldolgozást tooling végezhető el. További beállítások: [adatok betöltése az elemzés a tárolási környezetekben](machine-learning-data-science-ingest-data.md)

Továbbá toohello kezdeti adatfeldolgozást az adatok, a számos intelligens alkalmazás a szükséges toorefresh hello adatok rendszeresen egy folyamatos tanulás folyamat részeként. Beállítása ehhez a **adatok adatcsatorna** vagy a munkafolyamat. Ez a hello folyamat, amely tartalmazza az újraépítését, és újra értékelése az hello hello intelligens alkalmazás hello megoldás telepítése által használt analitikus modellek iteratív részét hello részét képezi. Lásd például [tárolt adatok mozgatása egy helyi SQL server tooSQL Azure szolgáltatásban az Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).

## <a name="2-explore-and-pre-process-data"></a>2. Adatok megismerése és előzetes feldolgozása
hello következő lépésre tooobtain hello adatok bemutatják a vizsgálatot a **statisztikák** , kapcsolatok, és ilyen technikák segítségével **képi megjelenítés**. Ez történik akkor is ahol a problémák **az adatminőségi** és integritását, például a hiányzó értékeket, az adatok típuseltérést és a tranzakciós szempontból inkonzisztens adatokat kapcsolatokat, kezeli. Előre feldolgozásra átalakítások előtt további elemzés hello nyers adatok használt tooclean és modellezési akkor kerül sor. Ismertetését lásd: [tooprepare adatainak továbbfejlesztett gépi tanulási feladatok](machine-learning-data-science-prepare-data.md).

## <a name="3-develop-features"></a>3. Szolgáltatások fejlesztéséhez
Adatszakértőkön tartomány szakértőivel közösen meg kell határoznia, hogy a rögzítési hello mintájára hello adatkészlet és tulajdonságait, amely legjobban lehetnek használt toopredict hello üzleti változók tervezés során azonosított hello szolgáltatások. Ezek a funkciók származtatható a meglévő adatok, vagy előfordulhat, hogy gyűjtött adatok toobe. Ezt a folyamatot nevezik **jellemzőkiemelés** és egyikét hello fontos lépések az hatékony prediktív elemzési rendszerek fejlesztése során. Ez a lépés szükséges hello adatok feltárása lépésben beszerzett tartomány szakértelmét és hello insights kreatív kombinációja. Útmutatásért lásd: [Jellemzőkiemelés hello csapat az tudományos folyamata a](machine-learning-data-science-create-features.md).

## <a name="4-create-predictive-models"></a>4. A prediktív modellek létrehozása
Adatszakértőkön build elemzési modellek toopredict hello legfontosabb változók hello üzleti követelmények tervezési lépést a tisztítás befejeztéig adatok használatával és featurized hello definiált azonosít. Machine learning rendszerek támogatják a több **algoritmusok modellezési** , amelyek megfelelő tooa számos esetben. Útmutatásért lásd: [hogyan toochoose algoritmusok Team Azure Machine Learning](machine-learning-algorithm-choice.md).

Adatszakértőkön hello legmegfelelőbb modell előrejelzésének feladatuknak kell választania, és nem ritka, hogy több modellek eredményeinek kell kombinált toobe tooobtain hello legjobb eredmények elérése érdekében. hello modellezési a bemeneti adatok általában oszlik véletlenszerűen három részből áll:

* a tanítási adathalmazt, 
* érvényesítési adatkészlet 
* a tesztelési adatkészletet 

hello modellek hello használatával készített **tanítási adathalmazt**. hello (beállított paraméterekkel) modellek optimális kombinációja szerint ki van választva hello modellek fut, és a mérési hello által jelzett hibákat előrejelzés hello **érvényesítési adatkészlet**. Végül hello **adatkészlet tesztelése** nem használt tootrain független adat a kiválasztott hello modell teljesítményétől használt tooevaluate hello vagy hello modell ellenőrzése.  Az eljárások ismertetéséért lásd [hogyan tooevaluate modell-teljesítmény az Azure Machine Learning](machine-learning-evaluate-model-performance.md).

## <a name="5-deploy-and-consume-models"></a>5. Központi telepítése és modellek felhasználása
Ha van olyan készlete, amely hatékony modellek, el **operationalized** az egyéb alkalmazások tooconsume. Attól függően, hogy hello üzleti követelmények, előrejelzéseket válnak, vagy **valós idejű** vagy egy **kötegelt** alapján. toobe operationalized, hello modellek rendelkezik toobe az elérhetővé tett egy **nyissa meg az API felület** , amely a különböző alkalmazások könnyen felhasznált ilyen online webhelyen, táblázatkezelő programok, irányítópultok vagy üzleti és a háttérkiszolgáló alkalmazások. Lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Összegzés és további lépések
Hello [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) több lépésből többször van modellezve, amely **útmutatást nyújtanak** hello feladatok szükséges toouse advanced analytics toobuild egy intelligens alkalmazások. Egyes lépéseket is részleteit hogyan toouse különböző Microsoft technológiák toocomplete hello feladatokat ismerteti. 

Amíg TDSP nem írja elő az adott típusú **dokumentáció** összetevők, a bevált gyakorlat toodocument hello hello adatok feltárása, modellezési és értékelés eredményei, és toosave hello vonatkozó kódot úgy, hogy hello analysis szükség esetén is többször is. Ez is lehetővé teszi, hogy elemzés működik, amikor más alkalmazások hasonló adatok és az előrejelzés feladatok végzése hello használatának.

Végpont forgatókönyvek, amelyek bemutatják, hello lépéseket a hello folyamata teljes **meghatározott forgatókönyvek** is rendelkezésre állnak. Lásd például:

* [hello Team adatok tudományos folyamat működés közben: SQL Server használata](machine-learning-data-science-process-sql-walkthrough.md)
* [hello Team adatok tudományos folyamat működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md).
* [Spark használata Azure HD.mdnsight Adattudomány](machine-learning-data-science-spark-overview.md)
* [Azure Data Lake méretezhető Adattudomány: egy végpont forgatókönyv](machine-learning-data-science-process-data-lake-walkthrough.md)

