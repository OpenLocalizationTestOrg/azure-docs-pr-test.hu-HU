---
title: 'Azure Cosmos DB: DocumentDB API | Microsoft Docs'
description: "Ismerje meg, hogyan használható az Azure Cosmos DB toostore és lekérdezés nagy mennyiségű JSON-dokumentumokat, kisebb késést, az SQL és a JavaScript használatával."
keywords: "json-adatbázis, dokumentum-adatbázis"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/22/2017
ms.author: mimig
ms.openlocfilehash: c96dfa5e2685782a99d2bcaf992f88dd2bef920b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-documentdb-api"></a>Bevezetés tooAzure Cosmos DB: DocumentDB API

Az [Azure Cosmos DB](introduction.md) a Microsoft globálisan elosztott, többmodelles adatbázis-szolgáltatása az alapvető fontosságú alkalmazásokhoz. Az Azure Cosmos DB biztosít [kulcsrakész globális terjesztési](distribute-data-globally.md), [átviteli sebesség és tárterület a rugalmas méretezést](partition-data.md) világszerte, egyjegyű ezredmásodperces késések: hello 99th PERCENTILIS, [öt jól meghatározott konzisztenciaszintek](consistency-levels.md), és magas rendelkezésre állás érdekében minden biztonsági mentés által garantált [iparágvezető SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Az Azure Cosmos DB [automatikusan elvégzi az adatok](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) anélkül, hogy Ön séma- és index felügyeleti toodeal. Többmodelles szolgáltatás, amely támogatja a dokumentumokat, a kulcs-értékeket, a diagramokat és az oszlopos adatmodelleket. 

![Azure DocumentDB API](./media/documentdb-introduction/cosmosdb-documentdb.png) 

A DocumentDB API hello, Azure Cosmos DB biztosít gazdag és ismerős [SQL lekérdezési képességek](documentdb-sql-query.md) a konzisztens alacsony késleltetésű keresztül séma nélküli JSON-adatokat. Ebben a cikkben hello Azure Cosmos DB DocumentDB API, és hogyan toostore nagy mennyiségű JSON-adatok rájuk, kérdezhet le róluk az ezredmásodperc késés sorrendjének belül, és könnyen fejlődnek hello séma áttekintést nyújtunk. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Milyen lehetőségeket és fő összetevőket kínál a Azure Cosmos DB?
Azure Cosmos-adatbázis, keresztül hello DocumentDB API hello alábbi főbb képességeket és előnyöket kínálja:

* **Rugalmasan méretezhető átviteli sebesség és tárterület:** könnyedén növelheti vagy csökkentheti a JSON-adatbázis toomeet az alkalmazás van szüksége. Az adatok tárolása tartós állapotú meghajtón (SSD) történik az alacsony, előre jelezhető késés érdekében. JSON-adatok tárolására is méretezhető toovirtually gyűjtemények nevű támogatja tárolók Azure Cosmos DB korlátlan tárterület mérete és a létesített átviteli sebesség. Ahogy az alkalmazás növekszik, kiszámítható teljesítmény mellett, rugalmasan és zökkenőmentesen méretezheti az Azure Cosmos DB-t. 


* **Több területi replikációs:** Azure Cosmos DB transzparens módon replikálja a tooall adatterületek tartozó a Azure Cosmos DB-fiókkal, amely lehetővé teszi, ugyanakkor biztosítható a globális hozzáférési toodata igénylő toodevelop alkalmazások mellékhatásokkal konzisztencia, rendelkezésre állásáról és teljesítményéről, mindezt megfelelő garanciák között. Az Azure Cosmos DB többhelyű API-kat, és a hello képességét tooelastically méretezési átviteli sebesség és a tárterület átlátható regionális feladatátvétel lehetővé teszi az hello földgolyó méretét. További információ: [Globális adatterjesztés az Azure Cosmos DB-vel](distribute-data-globally.md).

* **Eseti lekérdezések a megszokott SQL-szintaxissal:** Heterogén JSON-dokumentumokat tárolhat, és a már ismerős SQL-szintaxis használatával kérdezheti le ezeket. Azure Cosmos DB szabad magas egyidejű zárolást használja, naplószerkezetű indexelési technológiát tooautomatically index dokumentumok teljes tartalmának. Ez lehetővé teszi a részletes valós idejű lekérdezéseket hello kell toospecify sémamutatók, másodlagos indexek vagy nézetek nélkül. További információk: [Az Azure Cosmos DB lekérdezése](documentdb-sql-query.md). 
* **JavaScript végrehajtása hello adatbázison belül:** Express úgy az alkalmazáslogikát tárolt eljárások, eseményindítók és felhasználó által megadott funkciókat (UDF) standard JavaScript használatával. Ez lehetővé teszi az alkalmazás logika toooperate adatok anélkül, hogy hello alkalmazás és az adatbázis-séma hello hello eltérő. hello DocumentDB API a JavaScript-alkalmazáslogika hello adatbázismotor belül teljes tranzakciós végrehajtását biztosítja. hello JavaScript szoros integrációja lehetővé teszi, hogy a hello végrehajtásának INSERT, REPLACE, DELETE és JavaScript-programból SELECT műveletek elkülönített tranzakcióként. További információk: [DocumentDB kiszolgálóoldali programozása](programming.md).

* **Aprólékosan beállítható konzisztenciaszintek:** válassza ki a öt jól meghatározott konzisztencia szintek tooachieve közötti optimális kompromisszum konzisztencia és a teljesítmény. A lekérdezések és olvasási műveletek esetében az Azure Cosmos DB öt különböző konzisztenciaszintet kínál: erős, kötött elavulás, munkamenet, konzisztens előtag és végleges. A részletes, jól meghatározott konzisztenciaszintek lehetővé teszik toomake ésszerű kompromisszumot konzisztencia, a rendelkezésre állás és a késleltetés között. További információ: [toomaximize rendelkezésre állásának és teljesítményének konzisztencia használatával szintek](consistency-levels.md).

* **Teljes körűen felügyelt:** hello kell toomanage adatbázis és a gép erőforrásainak megszüntetéséhez. Teljes körűen felügyelt Microsoft Azure szolgáltatás akkor nem kell toomanage virtuális gépek tegye, telepítése és konfiguráljon szoftverhasználat-, méretezés kezelésére vagy összetett adatrétegek frissítésére foglalkozik. Minden adatbázisról automatikus biztonsági mentés készül, és védelmet élveznek a regionális meghibásodásokkal szemben. Egyszerűen Azure Cosmos DB fiók hozzáadásához, és esetleg szükség lenne rá, hogy lehetővé teszi a toofocus üzemeltetése és kezelése az adatbázis helyett az alkalmazásra mértékben kapacitást. 

* **Nyílt kialakítás:** Hamar munkához láthat a meglévő ismeretei és eszközei használatával. Programozási hello DocumentDB API egyszerű, könnyű, és nem kell tooadopt új eszközök vagy toocustom bővítmények tooJSON vagy JavaScript igazodik ellen. Az összes hello adatbázis funkciókat, beleértve a CRUD, a lekérdezés és a JavaScript-feldolgozás egy egyszerű RESTful HTTP-felületen keresztül érheti el. hello DocumentDB API támogatja a meglévő formátumokat, nyelveket és standardokat miközben értékes adatbázis-képességeket.

* **Az automatikus indexeléshez:** alapértelmezés szerint Azure Cosmos DB automatikusan indexeket hello adatbázis található összes hello dokumentum és nem várt vagy igényel semmilyen sémát, illetve másodlagos indexek létrehozását. Nem szeretnék tooindex mindent? Ne aggódjon, [a JSON-fájlok elérési útjainak indexelését](indexing-policies.md) is ki lehet kapcsolni.

* **Változás adatcsatorna-támogatás:** módosítás adatcsatorna biztosít egy dokumentumot egy Azure Cosmos DB gyűjteményt, amelyben a módosítás hello sorrendben rendezett listát. Ez az adatcsatorna a módosítások toodata rendelés tooreplicate adatok használt toolisten, indul el, az API-hívásokban vagy adatfolyam feldolgozása végre frissítések. Változás adatcsatorna automatikusan engedélyezve van, és könnyen toouse: [adatcsatorna módosítása bővebben a további](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>A DocumentDB API hello adatok kezelése
a DocumentDB API hello kezelhetőek JSON-adatok jól meghatározott adatbázis forrásanyagok segítségével. A magas rendelkezésre állás érdekében a rendszer replikálja ezeket az erőforrásokat, amelyek a logikai URI-juk alapján egyedi módon címezhetők. a DocumentDB API hello nyújt egy egyszerű HTTP-alapú RESTful programozási modellt az összes erőforrás. 


hello Azure Cosmos DB adatbázisfiók egy egyedi névtér, amely lehetővé teszi az tooAzure Cosmos-adatbázis eléréséhez. Az adatbázisfiók létrehozása előtt rendelkeznie kell Azure-előfizetéssel, amely lehetővé teszi az tooa különböző Azure-szolgáltatások eléréséhez. 

Az Azure Cosmos DB összes erőforrásának modellezése és tárolása JSON-dokumentumként történik. Az erőforrások elemekként és csatornákként vannak kezelve. Az elemek metaadatokat tartalmazó JSON-dokumentumok, a csatornák pedig elemek gyűjteményei. Az elemkészletek a megfelelő csatornákban találhatók.

hello az alábbi képen látható hello hello Azure Cosmos DB erőforrásainak kapcsolatai:

![az Azure Cosmos adatbázis erőforrásainak hierarchikus kapcsolatai hello][1] 

Egy adatbázis-fiók adatbázis-készletekből áll, amelyek mindegyike több gyűjteményt tartalmaz. A gyűjtemények tárolt eljárásokat, eseményindítókat, felhasználó által megadott függvényeket, dokumentumokat és kapcsolódó mellékleteket tartalmazhatnak. Egy adatbázis is vannak társítva felhasználó engedélyek tooaccess számú különféle egyéb gyűjtemények, tárolt eljárások, eseményindítók, felhasználó által megadott függvények, dokumentumok, vagy a mellékletekben. Amíg az adatbázisok, felhasználók és gyűjtemények rendszer által meghatározott, jól ismert sémákkal rendelkező erőforrások, addig a dokumentumok, tárolt eljárások, eseményindítók, felhasználó által megadott függvények és mellékletek tetszőleges, felhasználó által megadott JSON-tartalmak lehetnek.  

> [!NOTE]
> Mivel hello DocumentDB API hello Azure DocumentDB szolgáltatás korábban elérhető volt, tooprovision, figyelheti és kezelheti a hello Azure Resource felügyeleti REST API használatával létrehozott fiókot, vagy az eszközök segítségével hello Azure DocumentDB is vagy Azure Cosmos DB erőforrás neve. Használjuk hello neve azonos értelemben toohello Azure DocumentDB API-k hivatkozni. 

## <a name="develop"></a>Hogyan fejleszthet hello DocumentDB API-alkalmazások?

Az Azure Cosmos DB keresztül tesz elérhetővé erőforrásokat hello REST API-k, amely képes a HTTP/HTTPS-kérést bármely olyan nyelvvel hívható. Ezenkívül fel a DocumentDB API hello számos népszerű nyelvhez programozási könyvtárakat. hello ügyfél kódtárak sok szempontból leegyszerűsítik mivel például címek gyorsítótárazása, kivételek kezelése, automatikus újrapróbálkozások és így tovább alfolyamatot kezelnek hello API használata. Szalagtárak jelenleg érhetők hello a következő nyelvekhez és platformokhoz:  

| Letöltés | Dokumentáció |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET-kódtár](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js-kódtár](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java-kódtár](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[JavaScript-kódtár](http://azure.github.io/azure-documentdb-js/) |
| n/a |[Kiszolgálóoldali JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Python-kódtár](http://azure.github.io/azure-documentdb-python/) |
| n/a | [API a MongoDB-hez](mongodb-introduction.md)


Hello segítségével [Azure Cosmos DB emulátor](local-emulator.md), létrehozhatja és az alkalmazás helyileg a DocumentDB API hello tesztelése egy Azure-előfizetés létrehozása, és ezzel járó költségeket nélkül. Ha elégedett az alkalmazás alakulását hello emulátorban, toousing hello felhőben Azure Cosmos DB fiók válthat.

Basic túl létrehozása, olvasása, frissítése és törlési műveletek, hello DocumentDB API felületet biztosít a gazdag SQL lekérdezés beolvasása JSON-dokumentumokat és a kiszolgáló oldalán támogatja a JavaScript-alkalmazáslogika tranzakciós végrehajtásához. hello lekérdezés és parancsfájl végrehajtására szolgáló felületek elérhető összes platform könyvtárán, valamint REST API-k hello. 

### <a name="sql-query"></a>SQL-lekérdezés
hello DocumentDB API támogatja a dokumentumok lekérdezését SQL-nyelv, amely a hello feltörték használatával JavaScript írja be a rendszer, és a relációs, hierarchikus és térbeli lekérdezéseket támogató kifejezések. hello DocumentDB lekérdezési nyelv egy egyszerű, mégis erőteljes felülete tooquery JSON-dokumentumokat. hello nyelv támogatja az ANSI SQL-szintaxis egy részét, és hozzáadja a JavaScript objektum, tömbök, objektumkonstrukciók és függvényhívások szoros integrációja. hello DocumentDB API hello fejlesztőtől származó explicit séma vagy indexelési mutatók nélkül a lekérdezési modelljét biztosítja.

Felhasználó által megadott funkciókat (UDF) hello DocumentDB API regisztrálva, és az SQL-lekérdezés részeként hivatkozhatók, ezáltal a hello nyelvtan toosupport egyéni alkalmazáslogika kiterjesztése. A felhasználó által megadott függvények JavaScript programokként írása, és hello adatbázis belül végrehajtani. 

A .NET-fejlesztők számára, a DocumentDB API hello [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) is biztosít, a LINQ lekérdezés szolgáltató. 

### <a name="transactions-and-javascript-execution"></a>Tranzakciók és a JavaScript futtatása
hello DocumentDB API lehetővé teszi a toowrite alkalmazáslogika teljes egészében a JavaScript írt, elnevezett programokként. Ezeket a programokat egy gyűjtemény regisztrálva van, és adhat ki az adatbázis-műveletek hello dokumentumokat adott gyűjteményen belül. A JavaScript regisztrálható eseményindítóként, tárolt eljárásként vagy felhasználó által megadott függvényként való futtatásra. Eseményindítók és tárolt eljárások is létrehozása, olvasása, frissítése és törölhetnek dokumentumokat, míg a felhasználó által definiált függvények futtatása nélkül írási toohello gyűjtemény a hello lekérdezés végrehajtási logikájának részeként.

JavaScript végrehajtása belül hello Cosmos DB relációs adatbázis-rendszerek, ahol a JavaScript a Transact-SQL modern helyettesítője által támogatott hello alapelvek után van modellezve. Minden JavaScript-logika futtatása egy környezeti ACID-tranzakción belül történik pillanatkép-elkülönítéssel. A végrehajtása során hello Ha hello JavaScript kivételt jelez, majd hello teljes tranzakció megszakad.

## <a name="are-there-any-online-courses-on-azure-cosmos-db"></a>Találhatók online tanfolyamok az Azure Cosmos DB-ben?

Igen, az Azure DocumentDB-ben található egy [Microsoft Virtual Academy](https://mva.microsoft.com/en-US/training-courses/azure-documentdb-planetscale-nosql-16847) tanfolyam. 

>[!VIDEO https://mva.microsoft.com/en-US/training-courses-embed/azure-documentdb-planetscale-nosql-16847]
>
>

## <a name="next-steps"></a>Következő lépések
Már van Azure-fiókja? Akkor megkezdheti az Azure Cosmos DB használatát. Ehhez kövesse a [gyors üzembe helyezési útmutatót](../cosmos-db/create-documentdb-dotnet.md), amely végigvezeti a fióklétrehozás folyamatán, és bevezeti a Cosmos DB használatába.

[1]: ./media/documentdb-introduction/json-database-resources1.png

