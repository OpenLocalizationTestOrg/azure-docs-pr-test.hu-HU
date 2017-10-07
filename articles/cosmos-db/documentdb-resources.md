---
title: "aaaAzure Cosmos DB erőforrás-modellje és fogalmai |} Microsoft Docs"
description: "További információk a adatbázisok, gyűjtemények, felhasználó által definiált függvény (UDF), dokumentumok, engedélyek toomanage erőforrások és további Azure Cosmos DB hierarchikus modelljét."
keywords: Hierarchikus modell cosmosdb, az azure, a Microsoft azure
services: cosmos-db
documentationcenter: 
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: ef9d5c0c-0867-4317-bb1b-98e219799fd5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: anhoh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc3642232b86cc27901ebd97456c386829324632
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Az Azure Cosmos DB hierarchikus erőforrás-modellje és alapfogalmai
hello Azure Cosmos DB kezelő adatbázis entitások legyenek hivatkozott tooas **erőforrások**. Az egyes erőforrások egyedileg azonosít egy logikai URI-t. Hello erőforrások szabványos HTTP-műveletek, a kérelem/válasz fejlécek és a állapotkódokat alállapotkódok használatával kommunikálhat. 

A cikk elolvasása lesz képes tooanswer hello a következő kérdéseket:

* Mi az erőforrás-modellje Cosmos DB?
* Mik azok a rendszer meghatározott erőforrások definiált megakadályozását toouser erőforrásként?
* Hogyan kezelje a egy erőforrást?
* Hogyan működik a gyűjteményekkel?
* Hogyan működik a tárolt eljárások, eseményindítók és felhasználó által megadott funkciókat (UDF)?

## <a name="hierarchical-resource-model"></a>Hierarchikus erőforrás-modellje
Az hello alábbi ábrán látható módon, hello Cosmos DB hierarchikus **erőforrás-modellje** áll egy adatbázis-fiók, logikai és állandó URI segítségével minden egyes megcímezhető alatt lévő erőforrások csoportja. Erőforráscsoport lesz hivatkozott tooas egy **hírcsatorna** ebben a cikkben. 

> [!NOTE]
> Azure Cosmos DB kínál a rendkívül hatékony TCP protokoll, amely egyben a RESTful a kommunikációt a modellben hello keresztül elérhető [DocumentDB .NET ügyfél API-ja](documentdb-sdk-dotnet.md).
> 
> 

![Az Azure Cosmos DB hierarchikus erőforrás-modellje][1]  
**Hierarchikus erőforrás-modellje**   

az erőforrásokkal működik toostart kell [adatbázisfiók létrehozása](create-documentdb-dotnet.md) használata az Azure-előfizetéshez. Az adatbázisfiók állhat egy **adatbázisok**, több tartalmazó **gyűjtemények**, minden egyes, viszont tartalmazó **tárolt eljárások, eseményindítók, felhasználó által megadott függvények, dokumentumok**és kapcsolódó **mellékletek**. Egy adatbázisban is van társítva **felhasználók**, minden egyes számú **engedélyek** tooaccess gyűjtemények, tárolt eljárások, eseményindítók, felhasználó által megadott függvények, dokumentumok vagy a mellékletekben. Míg adatbázisok, felhasználókat és gyűjtemények rendszer által meghatározott erőforrások jól ismert sémákkal rendelkező, dokumentumok és mellékletek tartalmaznak tetszőleges, felhasználó által megadott JSON-tartalmak.  

| Erőforrás | Leírás |
| --- | --- |
| Adatbázis-fiók |Az adatbázisfiók adatbázisok és a rögzített méretű mellékletek blob-tároló állítja be hozzá rendelve. Létrehozhat egy vagy több adatbázis fiókot az Azure-előfizetését használja. További tudnivalókért keresse fel a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Adatbázis |Egy adatbázis a dokumentumtároló gyűjtemények között particionált logikai tárolója. Akkor is a felhasználók tárolójába kerülnek. |
| Felhasználó |hello logikai névterét hatókörének engedélyeket. |
| Engedély |Egy engedélyezési jogkivonatot egy felhasználó hozzáférési tooa adott erőforrás társított. |
| Gyűjtemény |A gyűjtemény egy JSON-dokumentumokat tároló, hello kapcsolódó JavaScript-alkalmazáslogika. A gyűjtemény egy számlázható entitás, akkor, ha hello [költség](performance-levels.md) hello hello gyűjteményhez társított teljesítményszint határozza meg. Gyűjtemények egy vagy több partícióra/kiszolgálóra is kiterjedhetnek, és méretezhető toohandle gyakorlatilag korlátlan mennyiségű tárterület vagy átviteli sebesség. |
| Tárolt eljárás |Egy gyűjtemény regisztrált és tranzakciós úton futtatásuk hello adatbázismotor JavaScript nyelven írt alkalmazás logikáját. |
| Eseményindító |Az alkalmazáslogikát végrehajtása előtt vagy után vagy egy INSERT utasítás, JavaScript nyelven írt cseréje vagy törlési művelet. |
| AZ UDF |JavaScript nyelven írt alkalmazás logikáját. Felhasználó által megadott függvények engedélyezi egy egyéni lekérdezés operátor toomodel, és ezáltal a hello core DocumentDB API lekérdezési nyelv kiterjesztése. |
| A dokumentum |Felhasználó által definiált (tetszőleges) JSON-tartalmak. Alapértelmezés szerint nincs a sémában definiált toobe kell, és másodlagos indexek igényelnek toobe megadott összes hello dokumentumok tooa gyűjteményhez adni. |
| Melléklet |Egy mellékletet tartalmazó hivatkozásokat és a külső blob/médiához kapcsolódó metaadatok különleges dokumentumot. hello fejlesztői választhat, toohave hello blob Cosmos DB felügyeli, vagy tárolja el azt egy külső blob-szolgáltatónál, például a onedrive-on, Dropbox, stb. |

## <a name="system-vs-user-defined-resources"></a>A rendszer és a felhasználó által definiált erőforrások
Erőforrások, például adatbázis-fiókokat, adatbázisok, gyűjtemények, felhasználók, engedélyek, tárolt eljárások, eseményindítók és felhasználó által megadott függvények - rögzített sémájába rendelkeznek, és rendszererőforrásokat nevezzük. Ezzel szemben erőforrások, például a dokumentumok és mellékletek egyáltalán nem korlátozza a hello séma, és a felhasználó által definiált erőforrások példák. A Cosmos DB, rendszer és a felhasználó definiált erőforrások képviselt és standard-kompatibilis JSON-ként kezeli. Minden erőforrás, a rendszer vagy felhasználó által definiált, hello alábbi közös tulajdonságokkal rendelkezik.

> [!NOTE]
> Vegye figyelembe, hogy az összes rendszer az erőforrás tulajdonságai létre fűzve előtagként a saját JSON-megjelenítés aláhúzással (_).
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Tulajdonság</strong></p></td>
            <td valign="top"><p><strong>Állítható be a felhasználó vagy rendszer által létrehozott?</strong></p></td>
            <td valign="top"><p><strong>Cél</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Rendszer által létrehozott, egyedi és a hierarchikus hello erőforrás-azonosítója</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>az egyidejű hozzáférések optimista vezérlését szükséges hello erőforrás ETag</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Hello erőforrás utolsó frissített időbélyegzője</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Egyedi megcímezhető URI hello erőforrás</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Felhasználó által definiált hello erőforrás egyedi nevét (a hello ugyanaz a partícióazonosító kulcsérték). Hello felhasználói azonosító nem határoz meg, ha egy azonosítót kell-e a rendszer által létrehozott</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Átviteli megjelenítésűre erőforrások
Cosmos DB nem határozza meg, a saját bővítmények toohello JSON standard vagy különleges kódolások; standard megfelelő JSON-dokumentumok együttműködik.  

### <a name="addressing-a-resource"></a>Egy erőforrás címzés
Minden erőforrás URI-címmel rendelkező. hello értékének hello **_self** tulajdonság egy erőforrás jelöli hello hello erőforrás relatív URI. hello hello URI formátuma áll hello /\<hírcsatorna\>/ {_rid} szegmenst:  

| Hello _self értéke | Leírás |
| --- | --- |
| /dbs |Adatcsatorna adatbázisok egy adatbázis-fiókkal |
| /dbs/ {%{dbname/} |Megfelelő hello érték {%{dbname/} azonosítójú adatbázis |
| {%{dbname/} /dbs/ /colls/ |A gyűjtemények az adatbázis adatcsatorna |
| {%{dbname/} /dbs/ /colls/ {collName} |Egy megfelelő hello érték {collName} azonosítójú gyűjtemény |
| {%{dbname/} /dbs/ /colls/ {collName} / docs |A gyűjtemény dokumentumok adatcsatorna |
| {%{dbname/} /dbs/ /colls/ {collName} /docs/ {dokumentumazonosító} |Hello érték {doc} egyező azonosítójú dokumentálása |
| {%{dbname/} /dbs/ /felhasználók/ |A felhasználók egy adatbázist a hírcsatorna |
| {%{dbname/} /dbs/ /felhasználók/ {userId} |Megfelelő hello érték {felhasználói} azonosítóval rendelkező felhasználó |
| {%{dbname/} /dbs/ /felhasználók/ {userId} / engedélyek |Adatcsatorna egy felhasználói engedélyek |
| {%{dbname/} /dbs/ /felhasználók/ {userId} /permissions/ {permissionId} |Egy megfelelő hello érték {engedély} azonosítójú engedély |

Az egyes erőforrások hello azonosítóját megadó tulajdonságot keresztül elérhetővé tett egyedi felhasználó által megadott névvel rendelkezik. Megjegyzés: a dokumentumok, hello felhasználó nem adja meg az azonosító, ha a támogatott SDK-k automatikusan létrehoz egy egyedi azonosítóra hello dokumentum. hello azonosítója nem egy felhasználó által megadott karakterláncot too256 karakter, amely egy adott szülő erőforrás hello környezeten belül egyedi másolatot. 

Az egyes erőforrások is rendelkezik a rendszer által létrehozott hierarchikus erőforrás-azonosítója (is hivatkozott tooas egy relatív azonosító), elérhető hello _rid tulajdonsága révén. hello RID kódolja a teljes hierarchia hello egy adott erőforrás és a megfelelő belső megjelenítése egy elosztott módon tooenforce hivatkozási integritási használt. a relatív AZONOSÍTÓK hello egyedi belül egy adatbázis-fiók, és belső használható Cosmos DB anélkül, hogy több partíció keresések hatékony útválasztást. hello hello _self és hello _rid tulajdonságainak értékei erőforrás mind a másodlagos, és a kanonikus ábrázolásai. 

hello REST API-k támogatása az erőforrások címzést és útválasztást kérelmek hello azonosítója és hello _rid tulajdonságok is.

## <a name="database-accounts"></a>Adatbázis-fiókok
Megadhat egy vagy több Cosmos DB adatbázis fiókot az Azure-előfizetését használja.

Hozhat létre és kezelheti a Cosmos DB adatbázis fiókokat hello Azure portálon keresztül, [http://portal.azure.com/](https://portal.azure.com/). Létrehozását és kezelését egy adatbázis-fiók rendszergazdai hozzáférésre van szüksége, és csak az Azure-előfizetéshez tartozó hajtható végre. 

### <a name="database-account-properties"></a>Adatbázis-fiók tulajdonságai
Kiépítése és kezelése az adatbázisfiók részeként konfigurálja, és olvassa el a következő tulajdonságai hello:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Tulajdonság neve</strong></p></td>
            <td valign="top"><p><strong>Leírás</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Konzisztencia-házirend</p></td>
            <td valign="top"><p>A tulajdonság tooconfigure hello alapértelmezett konzisztencia a szintjének beállítása az adatbázis-fiókjában összes hello gyűjteményt. Ha szeretné felülbírálni az hello konzisztenciaszint használatával [x-ms-konzisztencia-szint] hello fejléc / kérés alapon. <p><p>Vegye figyelembe, hogy ez a tulajdonság csak érvényes toohello <i>felhasználó által definiált erőforrások</i>. Minden rendszer által definiált erőforrás olvasási konfigurált toosupport lekérdezések erős konzisztencia.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Engedélyezési kulcsok</p></td>
            <td valign="top"><p>Ezek a hello elsődleges, és a másodlagos fő- és a csak olvasható kulcsok biztosító felügyeleti hozzáférési tooall hello erőforrások hello adatbázisfiók alapján.</p></td>
        </tr>
    </tbody>
</table>

Vegye figyelembe, hogy a hozzáadása tooprovisioning, konfigurálása és kezelése az adatbázisfiók hello Azure portál, az akkor is szoftveresen is fiókok létrehozásakor és kezelésekor Cosmos DB adatbázis hello segítségével [Azure Cosmos DB REST API-k](/rest/api/documentdb/) , jól [ügyfél SDK-k](documentdb-sdk-dotnet.md).  

## <a name="databases"></a>Adatbázisok
Egy Cosmos DB adatbázisban egy olyan logikai tároló egy vagy több gyűjteményt és a felhasználók, ahogy az ábra a következő hello. Tetszőleges számú adatbázishoz alatt egy Cosmos DB adatbázis fiók tulajdonosának toooffer korlátok hozhat létre.  

![Fiók és a gyűjtemények hierarchikus adatbázismodell][2]  
**Egy adatbázis egy olyan logikai tároló, a felhasználók és gyűjtemények**

Egy adatbázis gyakorlatilag korlátlan dokumentumtároló gyűjtemények belül particionált tartalmazhat.

### <a name="elastic-scale-of-a-cosmos-db-database"></a>Rugalmasan méretezhető Cosmos DB-adatbázis
A Cosmos DB adatbázisa rugalmas alapértelmezés – kezdve a néhány GB toopetabytes dokumentumtároló biztonsági SSD és a létesített átviteli sebesség. 

Ellentétben a hagyományos RDBMS egy adatbázist egy Cosmos DB adatbázisa nem hatókörön belüli tooa egyetlen számítógépen. A Cosmos DB az alkalmazás skálázási igények toogrow, létrehozhat több gyűjteményt, adatbázisok vagy mindkettőt. Valóban különböző első felek alkalmazásainak a Microsofton belül használt Cosmos DB egy fogyasztói léptékű által a dokumentumtároló terabájt az egyes gyűjtemények tartalmazó ezer rendkívül nagy Cosmos DB adatbázisok létrehozásához. Növekedhet, és egy adatbázis zsugorítása hozzáadásával vagy eltávolításával gyűjtemények toomeet az alkalmazás méretkövetelményekhez. 

Tetszőleges számú belül egy adatbázis-tulajdonos toohello ajánlat gyűjteményeket hozhat létre. Minden gyűjtemény rendelkezik, a biztonsági SSD-tárolóba, attól függően, hogy a kijelölt teljesítményszinttel hello meg kiosztott átviteli sebesség.

A Cosmos DB adatbázis is egy olyan tároló, a felhasználók. Egy felhasználó, szolgálna, az engedélyek egy készletét, minden részletre kiterjedő engedélyezési és hozzáférés toocollections, dokumentumok és mellékletek védelmét biztosító logikai névterét.  

Hello Cosmos DB erőforrás-modellje más erőforrásokat, adatbázisok hozható létre, cserélni, törlése, olvasása vagy számba könnyen használatával vagy hello [REST API-k](/rest/api/documentdb/) vagy bármelyik hello [ügyfél SDK-k](documentdb-sdk-dotnet.md). Cosmos DB vagy hello metaadatok adatbázis erőforrás lekérdezése az erős konzisztencia biztosítja. Egy adatbázis törlése automatikusan biztosítja, hogy nem tud hozzáférni az hello gyűjtemények vagy az abban szereplő felhasználók.   

## <a name="collections"></a>Gyűjtemények
A Cosmos DB gyűjtemény egy olyan tároló, a JSON-dokumentumok. 

### <a name="elastic-ssd-backed-document-storage"></a>Rugalmas biztonsági SSD-dokumentumtároló
Egy gyűjtemény belsőleg rugalmas – automatikusan növekszik, és vegye fel vagy távolítsa el a dokumentumok mértékben változik. Gyűjtemények logikai erőforrások, egy vagy több fizikai partíciók, sem kiszolgálók is kiterjedhetnek. a gyűjteményen belül partíciók száma hello Cosmos DB hello tároló méretét és a gyűjtemény hello kiosztott átviteli sebesség alapján határozza meg. Minden partíció Cosmos DB SSD-biztonsági tárolási társítva a rögzített méretű rendelkezik, és a magas rendelkezésre állású replikálódik. Partíció felügyeleti teljes mértékben felügyelt Azure Cosmos DB, és nem rendelkezik toowrite komplex kódot, vagy a partíciók kezeléséhez. A cosmos DB gyűjtemények **gyakorlatilag korlátlan** tárolási és átviteli tekintetében. 

### <a name="automatic-indexing-of-collections"></a>Az automatikus indexeléshez gyűjtemények
Cosmos DB egy valódi sémamentes adatbázisrendszer. Az feltételez vagy igényel semmilyen sémát hello JSON-dokumentumok. Dokumentumok tooa gyűjteményhez való hozzáadása, Cosmos DB automatikusan elvégzi a őket, és hogy tooquery elérhetők. Automatikus indexelés dokumentumok anélkül, hogy a séma vagy a másodlagos indexek Cosmos-adatbázis kulcs képesség és írási optimalizált, a zárolás szabad és a naplószerkezetű karbantartási eljárások szerint engedélyezve van. Cosmos DB konzisztens lekérdezések szolgálatban nagyon gyorsan írások tartós kötet támogatja. A dokumentum és a index tárolása minden gyűjtemény által felhasznált toocalculate hello tárterületet használja. Hello tárterületi és teljesítménybeli kompromisszumot társított indexelő hello indexelési házirendet egy gyűjtemény konfigurálásával szabályozhatja. 

### <a name="configuring-hello-indexing-policy-of-a-collection"></a>A gyűjtemény-hello indexelési házirend konfigurálása
házirend egyes gyűjtemények indexelő hello lehetővé teszi toomake teljesítmény- és tárolási kompromisszumot indexelő társított. hello következő lehetőségek állnak rendelkezésre tooyou indexelési konfiguráció részeként:  

* Válassza ki, hogy hello gyűjtemény automatikusan elvégzi a hello dokumentumokat vagy sem. Alapértelmezés szerint automatikusan indexeli ugyan összes dokumentumot. Válassza ki az automatikus indexeléshez tooturn, és csak bizonyos dokumentumokhoz toohello index szelektív hozzáadása. Ezzel szemben dönthet úgy, szelektív módon tooexclude csak bizonyos dokumentumokhoz. Ez érhet el, IGAZ vagy hamis beállítása hello automatikus tulajdonság toobe hello indexingPolicy gyűjtemény, hello [x-ms-indexingdirective] fejléc segítségével beszúrni, cseréje vagy dokumentum törlése közben.  
* Válassza ki, hogy tooinclude vagy kizárási egyedi elérési utak vagy a dokumentumokat a minták hello index. Érhető el ez beállítás includedPaths és a gyűjtemény hello indexingPolicy excludedPaths kulcsattribútumokkal. Is hello tárterületi és teljesítménybeli kompromisszumot tartomány konfigurálása és a megadott elérési út mintára lekérdezések kivonat. 
* Itt választhat szinkron (konzisztens) és aszinkron (lazy) index frissítéseket. Alapértelmezés szerint hello index frissítése szinkron módon minden insert, replace vagy dokumentum toohello gyűjtemény törlése. Ez lehetővé teszi, hogy a hello lekérdezések toohonor hello konzisztencia szinttel azonos hello dokumentum olvasások. Cosmos DB írási optimalizálva, és támogatja a tartós kötetek dokumentum írások szinkron index karbantartási és egységes lekérdezések szolgáltató együtt, konfigurálhat bizonyos gyűjtemények tooupdate indexét lazily. A lusta indexelési növekedhet további hello írási teljesítmény és tömeges adatfeldolgozást forgatókönyvek elsősorban az olvasási műveleteket gyűjtemények ideális.

házirend indexelő hello feldolgozás alatt álló PUT hello gyűjtemény módosíthatja. Ez lehet hello keresztül elért [ügyfél SDK](documentdb-sdk-dotnet.md), hello [Azure Portal](https://portal.azure.com) vagy hello [REST API-k](/rest/api/documentdb/).

### <a name="querying-a-collection"></a>A gyűjtemény lekérdezése
a gyűjteményen belül hello dokumentumok lehet tetszőleges sémák és kérdezheti le egy gyűjteményen belül dokumentumok anélkül, hogy semmilyen sémát, illetve másodlagos indexek előzetes megfizetése esetén. Hello gyűjtemény hello segítségével lekérheti [Azure Cosmos DB DocumentDB API: SQL-szintaxis hivatkozás](https://msdn.microsoft.com/library/azure/dn782250.aspx), amely biztosítja a gazdag hierarchikus, relációs és térbeli operátorokat és bővíthetőséget JavaScript-alapú felhasználó által megadott függvények. A JSON-szintaxis lehetővé teszi a JSON-dokumentumok fákként hello fa csomópontként címkékkel modellezési. Ez lehet kihasználni a egyaránt DocumentDB API automatikus indexelési technikái, valamint a DocumentDB API SQL dialektusa szerint. hello DocumetDB API lekérdezési nyelv három fő szempontjait foglalja magában:   

1. A lekérdezési műveletek, amelyek kapcsolódnak természetes toohello faszerkezetben, beleértve a hierarchikus lekérdezések és leképezések egy kis készletét. 
2. Egy relációs műveleteket, köztük a összeállításban, szűrő, leképezések, összesítések és automatikus illesztések részét. 
3. Tiszta JavaScript-alapú, amelyek használhatók a felhasználó által megadott függvények (1) és (2).  

hello Cosmos DB lekérdezési modelljét kísérel meg toostrike funkciót, a hatékonyság és az egyszerűség egyensúlyára. hello Cosmos-adatbázis adatbázis-kezelő alapértelmezés szerint lefordítja a és hello SQL lekérdezési utasítás végrehajtása. Egy gyűjtemény hello segítségével lekérheti [REST API-k](/rest/api/documentdb/) vagy bármelyik hello [ügyfél SDK-k](documentdb-sdk-dotnet.md). a LINQ szolgáltatónál hello .NET SDK-val rendelkezik.

> [!TIP]
> Próbálja ki hello DocumentDB API, és SQL-lekérdezések futtatása az adatkészletet a hello [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Többdokumentumos tranzakció
Adatbázis-tranzakció biztonságos és megbízható programozási modellt biztosít a egyidejű változtatások toohello adatok kezelésével. RDBMS, a hello hagyományos módon toowrite üzleti logikát a toowrite **tárolt eljárások** és/vagy **eseményindítók** , és küldje el az adatbázis-kiszolgáló toohello tranzakciós végrehajtásához. RDBMS hello alkalmazás-programozó szükség toodeal két különböző programozási nyelvek: 

* (nem tranzakciós) hello alkalmazás programozási nyelv (pl. JavaScript, Python, C#, Java, stb.)
* T-SQL, hello tranzakciós programozási nyelv hello adatbázis natív módon futtatja

Részletes kötelezettségvállalás tooJavaScript és JSON közvetlenül hello adatbázismotor belül, a Cosmos DB egy intuitív programozási modellt biztosít, végrehajtás alatt álló JavaScript-alapú úgy az alkalmazáslogikát közvetlenül a tárolt eljárások szempontjából hello gyűjtemények és eseményindítók. Ez lehetővé teszi, hogy mindkét hello következő:

* Párhuzamossági hatékony végrehajtásának szabályozásához helyreállítási automatikus hello JSON objektumgrafikonok közvetlenül a hello adatbázismotor indexelése
* Természetesen kifejező folyamatábrán, a változó hatókörének, a hozzárendelés és a kivételkezelő primitívek az adatbázis tranzakcióihoz közvetlenül tekintetében hello JavaScript programozási nyelv integrációja

hello JavaScript-logika regisztrálva, a gyűjtemény szintjén majd kiadhatnak hello gyűjteményben megadott Helyadatbázis-műveletek hello dokumentumokat. Cosmos DB implicit módon JavaScript-alapú tárolt eljárások és eseményindítók belül pillanatkép-elkülönítéssel között egy környezeti ACID-tranzakciókat becsomagolja hello dokumentumok egy gyűjteményen belül. A végrehajtása során hello Ha hello JavaScript kivételt jelez, majd hello teljes tranzakció megszakad. hello eredményül kapott programozási modell egy nagyon egyszerű még hatékony. JavaScript fejlesztők "tartós" programozási modellt kap a szalagtár primitívek, valamint a megszokott nyelvi szerkezetek továbbra is használatakor.   

hello képességét tooexecute JavaScript közvetlenül belül hello adatbázis-motort hello azonos címterének, hello pufferkészlet lehetővé teszi, hogy performant és adatbázis-művelet hello dokumentumokon végzett gyűjtemény tranzakciós végrehajtását. Ezenkívül Cosmos-adatbázis adatbázis-kezelő lehetővé teszi a mély kötelezettségvállalás toohello JSON, és a JavaScript kiküszöböli bármely alkalmazás hello típus rendszerek és hello adatbázis impedancia eltérést.   

Gyűjtemény létrehozása után regisztrálhatja tárolt eljárások, eseményindítók és felhasználó által megadott függvények használatával hello gyűjtemény [REST API-k](/rest/api/documentdb/) vagy bármelyik hello [ügyfél SDK-k](documentdb-sdk-dotnet.md). A regisztrációt követően hivatkoznak, és azokat hajtható végre. Vegye figyelembe a hello következő tárolt eljárás írt teljes egészében a JavaScript-kódot hello két argumentummal (könyv nevét és Szerző neve) és új dokumentum létrehozása, lekérdezi egy dokumentumot, és frissíti azt – összes egy implicit ACID-tranzakción belül. Hello végrehajtása során bármikor Ha a JavaScript kivételt vált ki, hello teljes tranzakció megszakítása.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);

                        context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

hello ügyfél "elküldhet a" hello fent JavaScript logika toohello adatbázis via HTTP POST tranzakciós végrehajtásához. HTTP-metódus használatával kapcsolatos további információkért lásd: [Azure Cosmos DB erőforrások RESTful interakció](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Figyelje meg, hogy hello adatbázis natív módon együttműködik a JSON és a JavaScript, mert nincs nincs rendszer Típuseltérés, nincs "OR leképezési" vagy a kód generálása magic szükséges.   

Tárolt eljárások és eseményindítók kommunikálni egy adatgyűjtési és -hello dokumentumok egy jól meghatározott hálózatiobjektum-modellje, amely felfedi a hello kontextusban gyűjtemény segítségével egy gyűjteményen belül.  

A DocumentDB API is létrehozható, törlése, hello gyűjtemények olvasására vagy könnyen használatával vagy hello számba [REST API-k](/rest/api/documentdb/) vagy bármelyik hello [ügyfél SDK-k](documentdb-sdk-dotnet.md). hello DocumentDB API mindig vagy hello metaadat-gyűjtemény lekérdezése az erős konzisztencia biztosítja. Egy gyűjtemény automatikus törlése biztosítja, hogy nem fér hozzá egyik hello dokumentumok, a mellékleteket, a tárolt eljárások, eseményindítók, és az abban szereplő felhasználó által megadott függvények.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Tárolt eljárások, eseményindítók és felhasználó definiált függvény (UDF)
Hello előző szakaszban leírtak írhat alkalmazás logika toorun közvetlenül belül hello adatbázismotor tranzakción belül. hello alkalmazáslogika teljes egészében a JavaScript írhatók, és a tárolt eljárás, eseményindító vagy egy UDF modellezhető. hello tárolt eljárás vagy eseményindító JavaScript-kód beszúrása, cserélje le, törlése, olvasása vagy a lekérdezés dokumentumok egy gyűjteményen belül. A hello ugyanakkor, hello JavaScript egy UDF belül nem lehet beszúrni, cseréje vagy törölhetnek dokumentumokat. Felhasználó által megadott függvények hello dokumentumok, a lekérdezés eredményhalmazából enumerálása, és előállít egy másik eredményhalmaz. A több-bérlős Cosmos DB egy szigorú foglalás alapú erőforrás irányítás érvénybe lépteti. Minden egyes tárolt eljárást, eseményindító vagy egy UDF lekérdezi az operációs rendszer erőforrások toodo rögzített quantum teendőit. Továbbá a tárolt eljárások, eseményindítók vagy felhasználó által megadott függvények külső JavaScript szalagtárak szemben nem lehet csatolni, és is feketelistára teszi hello erőforrás költségvetések toothem lefoglalt túllépése esetén. Akkor regisztrálása, unregister tárolt eljárások, eseményindítók vagy felhasználó által megadott függvények használatával gyűjtemény hello REST API-k.  Regisztráláskor tárolt eljárás, eseményindító vagy egy UDF, előre összeállított és a rendszer, bájt kód, amely később hajtsa végre. a következő szakasz hello bemutatják, hogyan használja a hello Cosmos DB JavaScript SDK tooregister, végrehajtási és regisztrációjának törlése a tárolt eljárás, eseményindító és egy UDF. hello JavaScript SDK értéke egy egyszerű burkoló feletti hello [REST API-k](/rest/api/documentdb/). 

### <a name="registering-a-stored-procedure"></a>A tárolt eljárás regisztrálása
Regisztrációs tárolt eljárás egy új tárolt eljárás erőforrás egy gyűjtemény HTTP POST használatával hoz létre.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();

            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>A tárolt eljárás végrehajtása
A tárolt eljárás végrehajtása végezhető el egy HTTP POST elleni meglévő tárolt eljárás erőforrás kiállító úgy, hogy a paraméterek toohello eljárás hello kérés törzsében.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>A regisztráció megszüntetését tárolt eljárás
Beállításjegyzékből való törlésekor a tárolt eljárás egy HTTP DELETE elleni meglévő tárolt eljárás erőforrás kiállításával egyszerűen történik.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Regisztrálás előtti eseményindító
Új eseményindító erőforrás létrehozása egy gyűjteményen keresztül HTTP POST regisztrációs eseményindító végezhető el. Ha hello eseményindító előre, vagy a feladás egy vagy több eseményindító és hello típusú műveletet lehet társítva (pl. létrehozása, Replace, Delete vagy az összes) is megadhat.   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Egy előtti eseményindító végrehajtása
Egy eseményindító végrehajtása egy meglévő eseményindító nevét hello hello helyreállításkor hello POST/PUT vagy DELETE kérelmet hello kérelem fejléce dokumentum erőforrás megadásával történik.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in hello Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>A regisztráció megszüntetését előtti eseményindító
Egy eseményindító regisztrációját kiadása egy HTTP DELETE eseményindító erőforrással ellen használatával egyszerűen történik.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Egy UDF regisztrálása
Egy UDF-regisztráció UDF új erőforrás létrehozása egy gyűjteményen keresztül HTTP POST végezhető el.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-hello-query"></a>Egy UDF hello lekérdezés részeként végrehajtása
Egy UDF hello SQL-lekérdezés részeként adható meg, és úgy tooextend hello alapszintű használt [SQL lekérdező nyelve a DocumentDB API hello](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>A regisztráció megszüntetését olyan UDF-ben
Beállításjegyzékből való törlésekor a UDF egy HTTP DELETE elleni meglévő UDF erőforrás kiállításával egyszerűen történik.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Bár a fenti hello kódtöredékek bemutatta hello regisztrációs (POST), a regisztrációjának (PUT), a olvasás/list (GET) és a végrehajtási (POST) keresztül hello [JavaScript SDK](https://github.com/Azure/azure-documentdb-js), használhatja a hello [REST API-k](/rest/api/documentdb/) vagy más [ügyfél SDK-k](documentdb-sdk-dotnet.md). 

## <a name="documents"></a>Dokumentumok
Insert, cserélje le, törlése, olvashatja, számbavétele és lekérdezni egy gyűjtemény tetszőleges JSON-dokumentumokat. Cosmos DB határozza meg, hogy semmilyen sémát, és nem igényel másodlagos indexek sorrendje toosupport egy gyűjtemény dokumentumok lekérdezését. hello maximális dokumentum mérete 2 MB.   

Folyamatban egy valóban megnyitott adatbázis-szolgáltatás, Cosmos DB nem készlet semmilyen speciális adattípusok (pl. dátum idő) vagy adott kódolások JSON-dokumentumok. Vegye figyelembe, hogy Cosmos DB nincs szükség semmilyen különleges JSON egyezmények toocodify hello közötti kapcsolatok különböző dokumentumok; hello Cosmos-adatbázis SQL-szintaxis biztosít nagyon hatékony hierarchikus és relációs lekérdezési operátorok tooquery és a projekt dokumentumok különleges jegyzeteket vagy dokumentumok megkülönböztető tulajdonságai között szükség toocodify kapcsolatok nélkül.  

Mint minden más erőforrásnál dokumentumok hozhatók létre, helyett törölni, olvassa el, számba és lekérdezett egyszerűen a REST API-k vagy bármelyik hello segítségével [ügyfél SDK-k](documentdb-sdk-dotnet.md). Dokumentum törlése azonnal területet szabadít fel hello kvóta megfelelő tooall beágyazott hello mellékletek. hello olvassa el a dokumentumok konzisztenciaszint hello adatbázisfiók hello konzisztencia házirend következik. Ez a házirend attól függően, hogy az adatok konzisztenciájának követelményeinek, az alkalmazás kérelem alapon felülbírálható. Dokumentumok lekérdezésekor hello olvasási módban a hello gyűjteményen indexelő konzisztencia követi hello. A "konzisztens" Ez a következő hello fiók konzisztencia házirend. 

## <a name="attachments-and-media"></a>Mellékletek és az adathordozó
Cosmos DB toostore bináris blobok/media Cosmos DB (legfeljebb 2 GB fiókonként) vagy saját távoli médiatárbeli tooyour lehetővé teszi. Azt is lehetővé teszi egy media nevű melléklet különleges dokumentum tekintetében toorepresent hello metaadatait. A Cosmos DB mellékletet hivatkozások hello media/blob máshol tárolt különleges (JSON) dokumentum. Csatolmány egyszerűen egy különleges dokumentumot, amely hello metaadatok (például hely, Szerző stb.) egy olyan távoli media storage-ban tárolt adathordozó rögzíti. 

Fontolja meg egy közösségi olvasási alkalmazást, amely Cosmos DB toostore szabadkézi jegyzetek használ, és megjegyzéseket, beleértve a metaadatok kiemeli, könyvjelzők, minősítések, stb. tartozó, egy adott felhasználó e-könyv kedveli/dislikes.   

* hello hello könyv maga tartalmát tárolja hello media tároló vagy egy távoli médiatárbeli vagy Cosmos DB adatbázisfiók részeként elérhető. 
* Az alkalmazás minden felhasználó tárolhatjuk egy különálló dokumentumként – pl. /colls/joe/docs/book1 által hivatkozott dokumentum Füzet1 Joe metaadatait tárolja. 
* Egy felhasználó egy adott könyv toohello tartalom lapjain mutató mellékletek hello megfelelő dokumentum pl. /colls/joe/docs/book1/chapter1 tárolt /colls/joe/docs/book1/chapter2 stb. 

Vegye figyelembe, hogy a fent felsorolt hello példák rövid azonosítók tooconvey hello erőforrás hierarchia használatát. Erőforrások hello REST API-kon keresztül egyedi erőforrás-azonosítók keresztül érhetők el. 

Hello adathordozó Cosmos DB által felügyelt hello _media tulajdonság hello mellékletek hello media az URI hivatkozik. Cosmos DB toogarbage gyűjtése hello media biztosítja, ha az összes fennmaradó hivatkozást hello eldobott. Cosmos DB automatikusan hozza létre hello melléklet hello új adathordozó feltöltésekor és hello _media toopoint toohello újonnan hozzáadott media tölti fel. Ha a távoli blob-tárolóban (például a OneDrive, az Azure Storage DropBox stb.) által felügyelt toostore hello media, mellékletek tooreference hello media továbbra is használhatja. Ebben az esetben létrehoz hello melléklet saját magának, és feltölti a _media tulajdonsága.   

Csakúgy, mint minden más erőforrásnál mellékletek hozhatók létre, cserélni, törlése, olvassa el vagy számba egyszerűen a REST API-k vagy bármelyik hello ügyfél SDK-k használatával. A dokumentumok, hello olvasás konzisztenciaszint mellékletek következik hello konzisztencia házirend hello adatbázis-fiók. Ez a házirend attól függően, hogy az adatok konzisztenciájának követelményeinek, az alkalmazás kérelem alapon felülbírálható. Mellékletek lekérdezésekor hello olvasási módban a hello gyűjteményen indexelő konzisztencia követi hello. A "konzisztens" Ez a következő hello fiók konzisztencia házirend. 
 

## <a name="users"></a>Felhasználók
A Cosmos DB felhasználói engedélyek csoportosítása logikai névterének jelöli. Egy Cosmos DB felhasználói tooa felhasználó is megfelel az identity management rendszer vagy egy előre megadott alkalmazás-szerepkör. A Cosmos DB a felhasználó egyszerűen egy absztrakciós toogroup engedélyekkel az adatbázis jelöli.   

Több-bérlős megvalósítása az alkalmazásban, a felhasználók az Cosmos Adatbázisba, amely megfelel a tooyour tényleges felhasználók vagy az alkalmazás hello bérlők is létrehozhat. Ezután létrehozhat egy adott felhasználó engedélyeit, amelyek megfelelnek a hozzáférés-vezérlés toohello keresztül különböző gyűjtemények, dokumentumok, a mellékletek, stb.   

Az alkalmazások a felhasználó növekedési rendelkező tooscale van, az adatok különböző módokon tooshard is elfogadja. A felhasználók az alábbiak szerint is modell:   

* Minden felhasználó maps tooa adatbázis.
* Minden felhasználó maps tooa gyűjtemény. 
* Megfelelő toomultiple felhasználók dokumentumok nyissa meg dedikált tooa gyűjtemény. 
* Megfelelő toomultiple felhasználók dokumentumok lépjen a gyűjtemények tooa készletét.   

Hello megadott horizontális skálázási stratégia mellett dönt, függetlenül a tényleges felhasználók modell Cosmos DB adatbázisban felhasználóként, és a finom nyomtatott engedélyek tooeach felhasználót hozzárendelheti.  

![Felhasználói gyűjtemények][3]  
**Horizontális azokat a stratégiákat és modellezési felhasználók**

Más erőforrások, például felhasználói Cosmos DB hozhatók létre, helyett, törlése, olvasása vagy számba egyszerűen a REST API-k vagy bármelyik hello ügyfél SDK-k használatával. Cosmos DB mindig vagy egy felhasználói erőforrás hello metaadatok lekérdezése az erős konzisztencia biztosítja. Válassza, hogy nem férhet hozzá az abban szereplő hello engedélyek biztosítja, hogy automatikusan törli a felhasználó érdemes. Annak ellenére, hogy hello Cosmos DB úgy szabadít fel a hello kvóta hello engedélyek hello törölt felhasználói hello háttérben részeként, törölt hello engedélyek érhető el azonnal újra az Ön toouse.  

## <a name="permissions"></a>Engedélyek
Access control szempontból, erőforrások, például adatbázis-fiókokat, adatbázisok, felhasználók és engedéllyel minősülnek *felügyeleti* erőforrásokat, mivel ezek a rendszergazdai engedélyek szükségesek. A hello ugyanakkor, beleértve hello gyűjtemények, dokumentumok, mellékletek, tárolt eljárások, eseményindítók, erőforrások és felhasználó által megadott függvények alapján egy adott adatbázisnak hatókörű, és figyelembe veendő *alkalmazás-erőforrásokat*. Megfelelő toohello kétféle típusú erőforrások és a hozzáférésüket (azaz hello rendszergazdai és felhasználói) hello szerepkörtől, hello engedélyezési modell meghatározása kétféle *hívóbetűk*: *főkulcs* és *erőforráskulcs*. hello főkulcs hello adatbázisfiók része, és biztosított toohello developer (vagy a rendszergazda) ki van kiépítés hello adatbázis-fiók. A főkulcs szemantikájú rendszergazda, abban, hogy azok használt tooauthorize hozzáférés tooboth felügyeleti és alkalmazás-erőforrásokat. Ezzel szemben egy erőforrás kulcsa a részletes elérési kulcsot, amely lehetővé teszi a hozzáférést tooa *adott* alkalmazás erőforrás. Emiatt hello kapcsolatát hello felhasználói adatbázis rögzíti, és hello engedélyek hello felhasználó rendelkezik-e egy adott erőforráshoz (pl. gyűjtemény, a dokumentum, melléklet, tárolt eljárás, eseményindító vagy UDF-ben).   

hello csak úgy tooobtain egy erőforrás kulcsa által egy adott felhasználói engedélyt erőforrás létrehozása. Vegye figyelembe, hogy a rendelés toocreate engedély beolvasása, vagy a főkulcs biztosítani kell a hello authorization fejlécet. Engedély erőforrás ties hello erőforrás, hozzáférési és hello felhasználó. Engedély erőforrás létrehozása után hello felhasználónak csak toopresent hello társított erőforráskulcs rendelés toogain hozzáférés toohello megfelelő erőforrás a van szüksége. Emiatt egy erőforráskulcs tekinthető hello engedély erőforrás a logikai és kompakt megjelenítése.  

Csakúgy, mint minden más erőforrásnál Cosmos DB engedélyek hozhatók létre, cserélni, törlése, olvassa el vagy számba egyszerűen a REST API-k vagy bármelyik hello ügyfél SDK-k használatával. Cosmos DB mindig vagy az engedély hello metaadatok lekérdezése az erős konzisztencia biztosítja. 

## <a name="next-steps"></a>Következő lépések
További tudnivalókat a HTTP-parancsokat kell használnia, erőforrásokkal [Cosmos DB erőforrások RESTful interakció](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

