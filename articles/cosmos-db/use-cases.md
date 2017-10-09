---
title: "aaaCommon esetek és forgatókönyvek használata Azure Cosmos DB |} Microsoft Docs"
description: "További tudnivalók: hello felső öt használati esetekben az Azure Cosmos DB: felhasználók létrehozott tartalom, eseménynaplózás, a katalógus adatainak, felhasználói beállítások adatok és az eszközök internetes hálózatát (IoT)."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: eca68a58-1a8c-4851-8cf8-6e4d2b889905
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: mimig
ms.openlocfilehash: 115a418e7b18d5033c4d7e64591bf302aea0190b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="common-azure-cosmos-db-use-cases"></a>Azure Cosmos DB gyakori alkalmazási esetei
Ez a cikk számos gyakori alkalmazási esetei áttekintést nyújt az Azure Cosmos DB rendszerhez.  Ez a cikk javaslatait hello szolgál, kiindulási pontként a Cosmos DB alkalmazást fejleszt.   

A cikk elolvasása után be fog tudni tooanswer hello a következő kérdéseket: 

* Mik azok a gyakori alkalmazási esetei hello Azure Cosmos DB?
* Mik azok a hello előnyei Azure Cosmos DB kereskedelmi alkalmazásokhoz?
* Mik azok a hello előnyei a Azure Cosmos DB adattárként az eszközök internetes hálózatát (IoT) rendszerek?
* Mik a webes és mobilalkalmazásokhoz Azure Cosmos DB használatának előnyei egy hello?

## <a name="introduction"></a>Bevezetés
[Az Azure Cosmos DB](../cosmos-db/introduction.md) globálisan elosztott adatbázis a Microsoft-szolgáltatás. hello szolgáltatást tervezett tooallow ügyfelek tooelastically (és egymástól függetlenül) átviteli sebesség és tárolási méretek tetszőleges számú földrajzi régiók között. Azure Cosmos DB hello első globálisan elosztott adatbázis szolgáltatás hello piaci ma toooffer átfogó [szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/cosmos-db/) átviteli, a késés, a rendelkezésre állási és a konzisztencia magába. 

hello Azure Cosmos DB projekt elindítása 2011 "Projekt Firenzében" tooaddress, fejlesztői problémás-pontok, amelyek a Microsoft vállalaton belül nagy méretű Internet alkalmazások szemben. Tartják be, hogy ezek a problémák nem egyedi tooMicrosoft alkalmazások, 2015 hello formájában döntöttünk toomake Azure Cosmos DB általánosan elérhető tooexternal fejlesztők [Azure DocumentDB](https://azure.microsoft.com/blog/documentdb-moving-to-general-availability/). hello szolgáltatást a belső használatú ubiquitously a Microsofton belül, és külsőleg Azure fejlesztők által használt hello leggyorsabban szolgáltatások egyike. 

Azure Cosmos-adatbázis egy olyan globális elosztott, több modellre adatbázis, az alkalmazások és a használati esetek széles használt. Akkor érdemes használni az összes [kiszolgáló nélküli](http://azure.com/serverless) alkalmazás, amely kis sorrendben az ezredmásodperces válaszidők kell, és gyorsan és globálisan tooscale kell. Támogatja a több adatmodellekben (kulcs-érték, a dokumentumok, a diagramok és oszlopos) és az adatok sok API-k eléréséhez, beleértve [MongoDB](mongodb-introduction.md), [a DocumentDB SQL](documentdb-introduction.md), [Gremlin](graph-introduction.md), és [ Azure-táblákban](table-introduction.md) natív módon, és a bővíthető módon. 

Az alábbiakban hello Azure Cosmos-adatbázis bizonyos attribútumok, amelyek nagy teljesítményű alkalmazások, a globális léptéke jól alkalmazható.

* Azure Cosmos-adatbázis natív módon felosztja a magas rendelkezésre állás és méretezhetőség adatait. Azure Cosmos DB 99,99 % garanciák rendelkezésre állási, átviteli sebességet, alacsony késéssel és konzisztencia nyújtja.
* Azure Cosmos-adatbázis biztonsági SSD-tároló, a kis késleltetésű sorrendben az ezredmásodperces válaszidők rendelkezik.
* Például végleges, egységes előtag, munkamenet és kötött elavulás konzisztenciaszintek Azure Cosmos DB tartozó támogatása lehetővé teszi, hogy teljesen rugalmasan és alacsony költség-teljesítmény aránya. Nincs adatbázis-szolgáltatás, Azure Cosmos DB mértékű rugalmasságot nyújt, szintek egységességét. 
* Azure Cosmos DB rugalmas adatok mobilbarát árképzési modellt, amely mérőszámok tárolási és átviteli egymástól függetlenül rendelkezik.
* Az Azure Cosmos DB fenntartott átviteli sebességet modell lehetővé teszi a toothink olvasása/írása az alapul szolgáló hardver hello Processzor/memória/IOPs helyett számát.
* Azure Cosmos-adatbázis tervező segítségével toomassive kérelem kötetek méretezni hello naponta kérelmek tízbillió sorrendje.

Ezek az attribútumok előnyösek webes, mobil-, játék, és az IoT-alkalmazásokhoz, amelyek alacsony válaszidők kell és az olvasási és írási műveletek nagy mennyiségű toohandle.

## <a name="iot-and-telematics"></a>IoT és telematika
Az IoT alkalmazási helyzetek általában néhány minták hogyan azok betöltési, folyamat, megoszthatja, és adatok tárolásához.  Ezek a rendszerek először tooingest felszakadásáig adatok az eszköz érzékelők a különböző területi beállításokhoz kell. Ezután ezek a rendszerek feldolgozni, és elemezze a streamelési adatok tooderive valós idejű elemzése. hello adata majd toocold tárhely kötegelt elemzésekben az archivált. A Microsoft Azure kínál, amelyek használhatók az IoT gazdag szolgáltatások használati esetekben Azure Cosmos DB, az Azure Event Hubs, Azure Stream Analytics, Azure Notification Hub, Azure Machine Learning, Azure HDInsight és Power bi. 

![Az Azure Cosmos DB IoT-referenciaarchitektúra](./media/use-cases/iot.png)

Az adatok felszakadásáig meghatározták Azure Event Hubs által, a magas teljesítmény adatfeldolgozást, kisebb késést biztosít. Lehet, hogy az adatokat okozhatnak, amelyet a valós idejű betekintés a feldolgozott toobe funneled tooAzure Stream Analytics valós idejű elemzések. Adatok lekérdezése ad hoc Azure Cosmos DB betölthető. Hello adatok Azure Cosmos DB tölti be, miután hello adata készen áll a toobe kérdezhetők le. Emellett új és módosítások tooexisting adatainak olvasható a módosítás adatcsatorna. Változás adatcsatorna egy állandó, a csak naplózási tároló változik tooCosmos DB gyűjtemények sorrendben hozzáfűzése. hello minden adathoz és az Azure Cosmos Adatbázisba csak módosítások toodata használható referenciaadatok valós idejű elemzési részeként. Ezenkívül adatok tovább kell is és dolgozza fel a Pig, a Hive vagy a térkép vagy csökkentse a feladatokhoz kapcsolódó Azure Cosmos DB adatok tooHDInsight.  Kifinomultabb adatok majd be van töltve a hátsó tooAzure Cosmos DB jelentéskészítéshez.   

A minta az IoT-megoldások Azure Cosmos DB, EventHubs és Storm használatával, lásd: hello [GitHub tárházából hdinsight-storm-példák](https://github.com/hdinsight/hdinsight-storm-examples/).

Az Azure IoT ajánlatok további információkért lásd: [létrehozás hello az eszközök internetes a hálózatát](http://www.microsoft.com/server-cloud/internet-of-things.aspx). 

## <a name="retail-and-marketing"></a>Kereskedelmi és marketing
Azure Cosmos-adatbázis a Microsoft saját elektronikus kereskedelmi platformok, hello Windows áruház és az XBox Live futtató nagymértékben használt. Is használatban van hello kereskedelmi iparági eseménykatalógus-adatok tárolásához és folyamatok feldolgozási sorrendben forrás esemény.

Katalógus adatok használati forgatókönyvek tartalmaz, amely tárolja, és lekérdezi-attribútumok entitások, például a személyek, helyek és termékek. Eseménykatalógus-adatok egyes példák felhasználói fiókok, termék katalógusok, IoT eszköz nyilvántartó és számlázási anyagok rendszerek. Ezekre az adatokra vonatkozóan attribútumok változhat, és toofit alkalmazás időigény keresztül módosíthatja.

Vegye figyelembe a termékkatalógus például egy való részek szállító. Minden része lehet a saját attribútumok hozzáadása toohello általános attribútumokkal rendelkeznek, amelyek bármely részét. Ezenkívül egy adott részére attribútumok követő év, amikor megjelenik egy új modell hello módosításához. Azure Cosmos DB támogatja a rugalmas sémák és a hierarchikus adatokhoz, és ezért kiválóan alkalmas termék katalógus adatainak tárolásához.

![Az Azure Cosmos DB kereskedelmi katalógus referencia-architektúrában](./media/use-cases/product-catalog.png)

Azure Cosmos DB használatával toopower eseményvezérelt architektúrák forrás esemény gyakran használják a [adatcsatorna módosítása](change-feed.md) funkciót. hello módosítás adatcsatorna alárendelt mikroszolgáltatások hello képességét tooreliably és fokozatosan olvasási beszúrása és frissítéseket (például rendelés események) tooan Azure Cosmos DB biztosít. Ez a funkció egy állandó esemény tárolásához állapot módosítása események és a meghajtó rendelés feldolgozása munkafolyamat között számos mikroszolgáltatások üzenet közvetítőként kihasználhatók tooprovide lehet (amely is implementálható [kiszolgáló nélküli Azure Functions](http://azure.com/serverless)).

![Azure Cosmos-adatbázis rendezési csővezeték referencia-architektúrában](./media/use-cases/event-sourcing.png)

Emellett Azure Cosmos DB tárolt adatok integrálható a HDInsight a big data elemzésre szolgáló Apache Spark feladatok keresztül. További részletek a Spark on Azure Cosmos DB-összekötő hello: [Cosmos-adatbázis és a HDInsight Spark feladat futtatása](spark-connector.md).

## <a name="gaming"></a>Játékok
adatbázis-rétegből hello játékalkalmazások kritikus fontosságú része. Modern játékok grafikus feldolgozási mobile/konzol ügyfeleken, de a testre szabott hello felhő toodeliver alapulnak, és személyre szabott tartalmakhoz, például a játékbeli statisztikák, közösségi integrációs vagy nagy-pontszám ranglisták. Játékok gyakran single-ezredmásodperces késések kérése olvasási és írási tooprovide kommunikáció zajlik a játékbeli élményt. Game adatbázis gyors toobe kell, és képes toohandle nagy teljesítményt, a kérelem díjszabás kell új játék elindítja és a szolgáltatás-frissítéseket során.

Például a játékok által használt Azure Cosmos-adatbázis [hello elhalt érdekében: nem Man föld](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) által [következő játékok](http://www.nextgames.com/), és [Halo 5: felügyeletet](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Azure Cosmos DB biztosít hello következő előnyökkel jár a toogame fejlesztőknek:

* Az Azure Cosmos DB lehetővé teszi a méretezhető teljesítmény toobe felfelé vagy lefelé, rugalmasan. Ez lehetővé teszi, hogy a profil és több tucat a statisztikák frissítése játékok toohandle toomillions az egyidejű gamers azáltal, hogy egyetlen API-hívással.
* Azure Cosmos DB támogatja ezredmásodperces beolvassa és toohelp bármely késedelmes jelentések játék során ne használjon.
* Azure Cosmos-adatbázis automatikus indexeléshez lehetővé teszi, hogy több különböző tulajdonságai valós idejű szűrése, keresse meg például a belső Windows Media Player azonosítók lejátszókról, vagy a GameCenter, a Facebook, a Google azonosítóját vagy a lekérdezés alapján egy guild player tagjának kell lennie. Ez nem lehetséges összetett indexelő vagy horizontális infrastruktúra.
* Közösségi szolgáltatásai, beleértve a játékbeli csevegési üzeneteket, a player guild tagságát, a kihívás befejeződött, a nagy-pontszám ranglisták és a közösségi diagramjait rugalmas sémával könnyebb tooimplement.
* Egy felügyelt platformon,--szolgáltatás (PaaS), Azure Cosmos-adatbázis szükséges lehető legkevesebb beállítás és kezelés munkahelyi tooallow gyors iterációs, és csökkentheti a idő toomarket.

![Az Azure Cosmos DB játék referencia-architektúrában](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Webes és mobilalkalmazások
Azure Cosmos DB webes és mobilalkalmazásokhoz belül általában arra használják, és kiválóan alkalmas közösségi kapcsolati modellezési integrálása a harmadik féltől származó szolgáltatással, és a gazdag személyre szabott élmény felépítése. hello Cosmos DB SDK-k lehet használt build gazdag iOS és Android-alkalmazások használatával népszerű hello [Xamarin keretrendszer](mobile-apps-with-xamarin.md).  

### <a name="social-applications"></a>Közösségi alkalmazások
Az Azure Cosmos DB gyakori használati eset webes, mobil, és a közösségi média alkalmazások toostore és lekérdezés felhasználók által létrehozott tartalmakat (UGC). Néhány példa a UGC munkameneteket, Twitter-üzeneteket, blogbejegyzések, értékelések és hozzászólások. A közösségi média alkalmazások UGC hello gyakran, szabad formátumú szöveg, a tulajdonságok, a címkék és a kapcsolatokat, amelyek nem korlátozódnak képzési struktúra keverékéből. Tartalom például csevegés, megjegyzések és a POST tárolhatja Cosmos DB átalakítások vagy a komplex objektum toorelational leképezési rétegek nélkül.  Adatok tulajdonságok lehet hozzáadni, vagy egyszerűen toomatch követelmények módosítható, mert a fejlesztők ismétlés hello alkalmazáskód, így a gyors fejlesztés támogatása.  

Alkalmazások, amelyek integrálják a külső közösségi hálózatokkal toochanging sémák kell válaszolnia a ezeket a hálózatokat. Adatok automatikusan indexelésre alapértelmezés szerint a Cosmos DB, az adatok bármikor lekérdezett készen toobe. Ezért ezek az alkalmazások rendelkezik hello rugalmasságot tooretrieve leképezések saját igényeiknek megfelelően.

Számos hello közösségi alkalmazás globális méretekben futtatható, és előre nem látható használati minták is lehetnek. A rugalmasság a skálázás hello adattár fontos hello alkalmazásréteg méretezi toomatch használati igény szerint.  Ki lehet terjeszteni további adatok partíciók egy Cosmos DB fiókkal hozzáadásával.  Különféle régiókban ezenkívül további Cosmos DB fiókot is létrehozhat. A Cosmos DB szolgáltatás régiónkénti elérhetőség, lásd: [Azure-régiókat](https://azure.microsoft.com/regions/#services).

![Az Azure Cosmos DB webes alkalmazás referencia-architektúrában](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Személyre szabás
Napjainkban a modern alkalmazások összetett nézetek és elemek rendelkeznek. Ezek a általában dinamikus, toouser beállítások vagy hangulatának megfelelően Élelmezési és branding igényeinek. Emiatt alkalmazásokat kell toobe képes tooretrieve személyre szabott beállítások hatékonyan a toorender felhasználói felületi elemek és lép gyorsan. 

JSON formátuma nem támogatott Cosmos DB megegyezik egy hatékony toorepresent felhasználói felület elrendezése adatok formázása nem csak egyszerűsített, de is könnyen értelmezhető JavaScript által. Cosmos DB konzisztenciaszinteket, amelyek lehetővé teszik a gyors olvasást a kis késleltetésű írások kínál. Emiatt adattárolás felhasználói felület elrendezése személyre szabott beállításokat is beleértve, mert a JSON-dokumentumokat az Adatbázisba az Cosmos egy hatékony eszközöket tooget ezeket az adatokat hello keresztülhaladnak a hálózaton keresztül.

![Az Azure Cosmos DB webes alkalmazás referencia-architektúrában](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Következő lépések
hajtsa végre az Azure Cosmos DB, használatába tooget a [gyors üzembe helyezések](create-documentdb-dotnet.md), amely végigvezetik Önt egy fiókot és az első lépések Cosmos DB létrehozása. 

Vagy, ha szeretné, hogy az ügyfelek Cosmos DB használatával kapcsolatos további tooread, hello következő vevő szövegek érhetők el:

* [Jet.com](https://jet.com). Elektronikus kereskedelmi kihívó szem hello felső helyszínen, hello Microsoft felhő fut, globális léptékű Cosmos DB használja.
* [Asos.com](http://www.asos.com/). Asos.com brit online módon és szépséget áruházbeli. Célja a fiatal felnőttek, Asos értékesít több mint 850 márkákat, valamint a saját számos ruházati és kellékek.
* [Toyota](https://www.toyota.com/). Toyota Motor Corporation a japán való gyártó. Toyota Cosmos DB globális IoT alkalmazások elkészítéséhez használja.
* [Citrix](https://customers.microsoft.com/story/citrix). Citrix házon belül fejlesztett alkalmazásokra single-sign-on megoldás Azure Service Fabric és Azure Cosmos DB használatával
* [TEXA](https://customers.microsoft.com/story/texaspa) TEXA tartozó Forradalmi IoT-megoldás a vehicle tulajdonosai Mentés időpontja, a pénz, a gáz segítségével –, és esetleg.
* [Domino tartozó pizzaszósz](https://www.dominos.com). Domino tartozó pizzaszósz Inc. egy amerikai pizzaszósz éttermi lánc.
* [Meghatározza, Johnson](http://www.johnsoncontrols.com). Johnson vezérlők egy globális változatos technológia és a kiszolgáló egy ügyfelek széles köre által 150-nél több országban több ipari vezető.
* [Microsoft Windows, univerzális tár, Azure IoT Hub, Xbox Live és egyéb Internet-skálázási szolgáltatások](https://azure.microsoft.com/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/). A Microsoft hogyan nagymértékben méretezhető szolgáltatások, Azure Cosmos DB használatával hoz létre.
* [A Microsoft Data és az elemzések team](https://customers.microsoft.com/story/microsoftdataandanalytics). A Microsoft adatok és analitikák team bolygónk méretű big data gyűjtemény Azure Cosmos DB éri el.
* [Sulekha.com](https://customers.microsoft.com/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha használ India Azure Cosmos DB tooconnect ügyfelek és a vállalatok számára.
* [NewOrbit](https://customers.microsoft.com/story/neworbit-takes-flight-with-azure-documentdb). NewOrbit a felhőszolgáltató közötti átviteléhez az Azure Cosmos DB vesz igénybe.
* [Affinio](https://customers.microsoft.com/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio AWS tooAzure Cosmos DB tooharness közösségi adatok léptékű vált.
* [Ezután játékokhoz](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). hello Walking kézbesítetlen: nem Man Népi föld játék soars túl #1 által támogatott Azure Cosmos DB.
* [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Hogyan Halo 5 megvalósított közösségi játék Azure Cosmos DB használatával.
* [A Cortana Analytics gyűjtemény](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Gallery - épülő Azure Cosmos DB méretezhető közösségi hely.
* [Gyerekjáték](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Nemzetközi vállalkozások globális Insight integráló vezető adja meg a rugalmas felhőalapú technológiái által nyújtott perc.
* [Hírek Köztársaság](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Az eszközintelligencia toohello hírek tooprovide információk céllal minden polgárok hozzáadása. 
* [SGS nemzetközi](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Hello földgömb között konzisztens szín bankkártyát tooSGS kapcsolja be. És SGS bekapcsolja a tooAzure.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Vezető Telenor hello felhő toomove használ, egy indítás hello sebessége. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). gyors keresési és hello egyszerűen az adatok áramlását hello jövőbeli felhőkörnyezetben hello tárolójában.
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). Azure-alapú szoftver platform vállalatok és az ügyfelek közötti akadályok megszakad.
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Weka intelligens kombinált hűtőszekrények javítja a vakcina felügyeleti, így több ember betegségek elleni védelme
* [Narancssárga Tribes](https://customers.microsoft.com/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Nincs több toothat étele app mint hello szem vagy hello szájához megfelel-e.
* [Valós Madridi](https://customers.microsoft.com/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Valós Madridi számos lehetőséget kínál hello stadium szorosabb too450 millió ventilátorok hello földgolyó méretét, a Microsoft Cloud hello körül.
* [Tuku](https://customers.microsoft.com/story/tuku-makes-car-buying-fun-with-help-from-azure-services). TUKU lehetővé teszi az Azure-szolgáltatások súgójában Szórakozás vásárlás autó
