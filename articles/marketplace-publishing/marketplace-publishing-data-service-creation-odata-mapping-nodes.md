---
title: "egy szolgáltatás a piactér hello aaaGuide toocreating |} Microsoft Docs"
description: "Hogyan toocreate, hitelesíthet és az adatok szolgáltatás telepítése részletes utasításokat vásárolja meg a hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: e3d88412389d43d104662dc4434363b6ad9475f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-nodes-schema-for-mapping-an-existing-web-service-tooodata-through-csdl"></a>Egy meglévő webes szolgáltatás tooOData keresztül CSDL megfeleltetéséhez hello csomópontok séma ismertetése
> [!IMPORTANT]
> **Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.** Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners). Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Ez a dokumentum segít tisztázni hello csomópont struktúra egy OData protokoll tooCSDL megfeleltetéséhez. Fontos, hogy jól-e hello csomópont struktúra toonote XML formátumú. Ezért legfelső szintű szülő és gyermek séma az OData-leképezés tervezése során is alkalmazható.

## <a name="ignored-elements"></a>Figyelmen kívül hagyott elemek
Az alábbiakban hello hello magas szintű CSDL elemek (XML-csomópontnak), amelyek nem kerülnek toobe hello Azure piactér háttér hello webes szolgáltatás metaadatai hello importálása során használja. Akkor használható, de figyelmen kívül hagyja.

| Elem | Hatókör |
| --- | --- |
| Elem használatával |hello csomópont, a sub csomópontok és az összes attribútum |
| Documentation elem |hello csomópont, a sub csomópontok és az összes attribútum |
| ComplexType |hello csomópont, a sub csomópontok és az összes attribútum |
| Társítás elem |hello csomópont, a sub csomópontok és az összes attribútum |
| A bővített tulajdonság |hello csomópont, a sub csomópontok és az összes attribútum |
| EntityContainer |Csak hello következő vannak hagyva: *bővíti* és *AssociationSet* |
| Séma |Csak hello következő vannak hagyva: *Namespace* |
| FunctionImport |Csak hello következő vannak hagyva: *mód* (alapértelmezett értéke ln feltételezve) |
| EntityType |Csak hello következő sub csomópontok figyelmen kívül lesznek hagyva: *kulcs* és *PropertyRef* |

hello következő szakasz ismerteti a hello módosításokat (fel, és figyelmen kívül hagyja az elemek) toohello részletesen különböző CSDL XML-csomópontnak.

## <a name="functionimport-node"></a>FunctionImport csomópont
Egy FunctionImport csomópont egy URL-címe (belépési pont), amely elérhetővé teszi a szolgáltatás toohello végfelhasználói jelöli. hello csomópont lehetővé teszi, hogy hogyan hello URL-címe felé, mely paraméterek elérhető toohello végfelhasználói, és ezek a paraméterek biztosított hogyan leíró.

Ez a csomópont adatait találhatók a(z) [itt][MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

hello az alábbiakban további attribútumok hello (vagy kiegészítéseket tooattributes), amelyek ki vannak téve az hello FunctionImport csomópont:

**d:BaseUri** -hello URI-sablonja, amely elérhetővé tooMarketplace hello REST-erőforrás. A piactér hello sablon tooconstruct hello REST-alapú webszolgáltatás lekérdezéseket. hello URI sablon tartalma helyőrzőit hello paraméterek használatát a tartománynevekben hello {parameterName}, ahol parameterName hello hello paraméter neve. Példa apiVersion = {apiVersion}.
Paraméterek engedélyezettek tooappear URI-paraméterek vagy hello URI elérési út egy része. Hello esetben hello megjelenésének hello elérési úton mindig kötelező (nem jelölhető meg a nullázható). *Példa:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Név** -hello hello neve importált függvény.  Nem lehet ugyanaz, mint a más definiált nevek a hello CSDL hello.  Példa Name = "GetModelUsageFile"

**EntitySet** *(nem kötelező)* – hello függvény visszatérési értéke, egy entitástípusok gyűjteménye, hello hello értékének **EntitySet** hello entitást set toowhich hello gyűjtemény tartozik kell lennie. Ellenkező esetben hello **EntitySet** attribútum nem használható. *Példa:*`EntitySet="GetUsageStatisticsEntitySet"`

**A returnType típust** *(nem kötelező)* -hello URI által visszaadott elemek hello típust ad meg.  Ne használja ezt az attribútumot, ha hello függvény nem ad vissza értéket. Az alábbiakban hello hello támogatott típusok:

* **Gyűjtemény (<Entity type name>)**: Adja meg a definiált entitástípusok gyűjteménye. hello neve megtalálható-e hello Name attribútum hello EntityType csomópont. Példa: a gyűjtemény (WXC. HourlyResult).
* **Nyers (<mime type>)**: nyers dokumentum/blob visszaadott toohello felhasználói határozza meg. Példa van Raw(image/jpeg) más példák:

  * ReturnType="Raw(text/plain)"
  * A returnType típust = "gyűjtemény (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** -határozza meg, hogyan kezeli a lapozófájl hello REST-erőforrás. hello értékeket fogja használni a kapcsos ágazatok belül paraméter, pl. lap = {$page} & az elemek laponkénti száma = {$size} hello elérhető lehetőségek a következők:

* **Nincs:** lapozás nem érhető el
* **Kihagyás:** lapozást egy logikai "skip" és "hajtsa végre a megfelelő" (felső) van megadva. Kihagyás egyszer M elemeket, és hajtsa végre a megfelelő értéket ad vissza a következő N elemek hello majd. Paraméter értéke: $skip
* **Hajtsa végre a megfelelő:** hajtsa végre a megfelelő hello következő N elemeket adja vissza. Paraméter értéke: $take
* **A pageSize értékének:** lapozás van kifejezve a logikai mérete és (oldalankénti elemek) keresztül. Lap hello aktuális lap visszaadott jelöli. Paraméter értéke: $page
* **Méret:** méretét az összes lapon visszaküldött elemek hello számát jelenti. Paraméter értéke: $size

**d:AllowedHttpMethods** *(nem kötelező)* -határozza meg, melyik utasítás kezeli hello REST-erőforrás. Emellett korlátozza az elfogadott művelet toohello megadott érték.  Alapértelmezett = POST.  *Példa:* `d:AllowedHttpMethods="GET"` hello elérhető lehetőségek a következők:

* **GET:** általában használt tooreturn adatok
* **POST:** tooinsert új adatokat általában használt
* **PUT:** általában használt tooupdate adatok
* **TÖRLÉS:** használt toodelete adatok

További gyermekcsomópontok (nem fedi le hello CSDL dokumentációja) hello FunctionImport csomóponton belül van:

**d:RequestBody** *(nem kötelező)* -hello kérelem törzse használt tooindicate, amely a kérelem hello egy szervezet toobe küldött vár. Paraméterek belül hello kérelemtörzset adható meg. Vannak megadva kapcsos zárójelek, pl. {parameterName}. Ezek a paraméterek vannak leképezve a hello hello szervezet, amely a felhasználói bevitel toohello tartalomszolgáltató szolgáltatás továbbítja. hello requestBody elemnek neve httpMethod attribútumot. hello attribútum lehetővé teszi, hogy a két érték:

* **POST:** használja, ha hello kérelem HTTP-KÖZZÉTÉTELLEL
* **GET:** használja, ha hello kérelem olyan HTTP GET

    Példa:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** és **d:Namespace** – Ez a csomópont hello névterek hello hello függvényimport (URI-végpont) által visszaadott XML definiált ismerteti. hello hello háttér-szolgáltatás által visszaadott XML tartalmazhat tetszőleges számú névterek toodifferentiate hello tartalmat adja vissza. **A névterek, ha d:Map vagy d:Match XPath-lekérdezést használja összes kell, hogy a felsorolt toobe.** hello d:Namespaces csomópont d:Namespace csomópontok egy set/listáját tartalmazza. Azok a hello háttér szolgáltatás válasza szerepel egy névtér sorolja fel. Az alábbiakban hello hello attribútum hello d:Namespace csomópont:

* **d:prefix:** hello névtér előtagját hello hello XML által adott eredmények hello szolgáltatást, pl. f:FirstName, f:LastName, ahol f az hello előtag látható módon.
* **d:URI:** hello hello névtér hello eredmény dokumentumban használt a teljes URI-címe. Hello értékét jelöli az adott hello előtag oldva tooat futásidejű.

**d:ErrorHandling** *(nem kötelező)* – Ez a csomópont tartalmazza a hibakezelés feltételeit. Hello feltételek mindegyikének hello tartalomszolgáltató szolgáltatás által visszaadott hello eredményeknél van hitelesítve. Ha egy feltétel egyezik a HTTP-hibakódot javasolt hello egy hibaüzenet toohello végfelhasználói ad vissza.

**d:ErrorHandling** *(nem kötelező)* és **d:Condition** *(nem kötelező)* -feltétel csomópontja rendelkezik egy olyan feltétel, amely be van jelölve, a hello által visszaadott eredmény hello tartalomszolgáltató szolgáltatás. hello az alábbiakban hello **szükséges** attribútumok:

* **d:Match:** az XPath-kifejezés, amely ellenőrzi, hogy egy adott csomópont-érték megtalálható-e a tartalomszolgáltató hello kimenetneve XML. hello XPath hello kimeneti vonatkozóan fut le, és IGAZ, ha hello feltétel a match vagy hamis más módon kell visszaadnia.
* **d:HttpStatusCode:** kell visszatérési piactér hello eset hello feltétel egyezik a HTTP-állapotkód: hello. Piactér signalizes hibák toohello felhasználói HTTP-állapotkódok keresztül. A HTTP-állapotkódok listáját http://en.wikipedia.org/wiki/HTTP_status_code webhelyen érhetők el
* **d:errorMessage:** van –, hello HTTP-állapotkód – visszaadott toohello végfelhasználói hello hibaüzenet. A felhasználóbarát hibaüzenet, amely nem tartalmazza a titkos kulcsok legyen.

**d:Title** *(nem kötelező)* -lehetővé teszi, hogy a leíró hello cím hello függvény. hello érték hello cím származik

* hello választható térkép attribútum (xpath) adja meg, ahol toofind hello cím hello válaszként visszaadott hello szolgáltatás kérelemből.
* – Vagy – hello cím hello csomópont megadva.

**d:Rights** *(nem kötelező)* -hello (pl. szerzői jogi) hello funkcióval kapcsolódó jogosultságok. hello jogok hello értéke származik:

* hello választható térkép attribútum (xpath) adja meg, ahol toofind hello jogok hello válaszként visszaadott hello szolgáltatás kérelemből.
* – Vagy – hello jogok hello csomópont megadva.

**d:Description** *(nem kötelező)* - egy rövid leírást hello függvény. hello hello leírása a következő származik

* hello választható térkép attribútum (xpath) adja meg, ahol toofind hello leírás hello válaszként visszaadott hello szolgáltatás kérelemből.
* – Vagy – hello csomópont értékeként megadott hello leírása.

**d:EmitSelfLink** - *lásd a fenti "A visszaadott adatok"lapozást"FunctionImport" – példa*

**d:EncodeParameterValue** -választható bővítmény tooOData

**d:QueryResourceCost** -választható bővítmény tooOData

**d:Map** -választható bővítmény tooOData

**d:Headers** -választható bővítmény tooOData

**d:Headers** -választható bővítmény tooOData

**d:value** -választható bővítmény tooOData

**d:HttpStatusCode** -választható bővítmény tooOData

**d:errorMessage** -választható bővítmény tooOData

## <a name="parameter-node"></a>A paraméter csomópont
Ebben a csomópontban jelöl egy paraméter, amely fel van fedve hello URI sablon részeként / a kérelem törzsében megadott hello FunctionImport csomópontban.

Egy nagyon hasznos részleteket dokumentumoldalra hello "Paraméter elem" csomópont a következő címen található [Itt](http://msdn.microsoft.com/library/ee473431.aspx) (használata hello **egyéb verzió** legördülő tooselect egy másik verziót, ha a szükséges tooview hello dokumentációjában). *Példa:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Paraméter-attribútumhoz | Szükséges | Érték |
| --- | --- | --- |
| Név |Igen |hello hello paraméter neve. Kis-és nagybetűket!  Hello BaseUri nagybetű. **Példa:**`<Property Name="IsDormant" Type="Byte" />` |
| Típus |Igen |hello paraméter típusa. hello értéknek kell lennie egy **EDMSimpleType** vagy összetett típus, amely hello modell hello hatókörén belül van. További információkért tekintse meg a "6 támogatott paramétere/paraméter tulajdonsága típusok".  (Kis-és nagybetűket! Első karakter nagybetűssé, rest kisbetű.)  Is láthatja, [fogalmi modell típusa (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Példa:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Mód |Nem |**A**, lejárt, vagy be/ki attól függően, hogy hello paraméter egy bemeneti, kimeneti vagy bemeneti/kimeneti paraméter. (Csak "az IN" érhető el az Azure piactéren.) **Példa:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength |Nem |hello paraméter hossza hello maximálisan megengedett. **Példa:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Pontosság |Nem |hello pontosság hello paraméter. **Példa:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Méretezés |Nem |hello méretezési hello paraméter. **Példa:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

Az alábbiakban hello toohello CSDL-specifikáció hozzáadott hello attribútumok:

| Paraméter-attribútumhoz | Leírás |
| --- | --- |
| **d:Regex** *(nem kötelező)* |A regex utasításban használt hello paraméter toovalidate hello bemeneti érték. Ha hello bemeneti érték nem egyezik meg a hello utasítás hello érték elutasítva. Ez lehetővé teszi a toospecify is lehetséges értékek, például ^ [0-9] +? $ tooonly számok engedélyezése. **Példa:** "< paraméternév ="name"mód ="A"típus"Karakterlánc"-d =: nullázható ="false"d:Regex =" ^ [a-zA-Z] * $"d:Description ="A név nem tartalmazhat szóközt vagy nem alfanumerikus nem angol karakterek"d:SampleValues ="George |
| **d:enum** *(nem kötelező)* |A cső elválasztott értékekből hello paraméter érvénytelen. hello értékek hello típusa kell toomatch definiált hello hello paraméter típusa. Példa: ' angol nyelven |
| **d: nullázható** *(nem kötelező)* |Megadhatja, hogy a paraméter null értékű lehet. hello alapértelmezett érték: igaz. Azonban hello elérési útvonalában hello URI sablonban feltárt nem lehetnek null értékűek. Ha hello attribútum értéke toofalse e paraméterek – hello felhasználói bevitel felülbírálja. **Példa:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(nem kötelező)* |Egy minta értéke, a felhasználói felület hello Megjegyzés toohello ügyfél toodisplay.  Lehetséges több egy csövet használatával határolt tooadd listájában, azaz "egy |

## <a name="entitytype-node"></a>EntityType csomópont
Ez a csomópont jelenti. a piactér toohello végfelhasználói visszaadott hello típusok egyikét. Hello kimeneti hello tartalomszolgáltató szolgáltatás toohello értékek visszaadott toohello végfelhasználói által megtalált hello-leképezés is tartalmaz.

Ez a csomópont adatait találhatók [Itt](http://msdn.microsoft.com/library/bb399206.aspx) (használata hello **egyéb verzió** legördülő tooselect egy másik verziót, ha a szükséges tooview hello dokumentációját.)

| Attribútum neve | Szükséges | Érték |
| --- | --- | --- |
| Név |Igen |hello entitástípus hello neve. **Példa:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType |Nem |hello neve egy másik entitás típusa, amely meghatározza a hello entitástípus hello alaptípusa. **Példa:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Az alábbiakban hello toohello CSDL-specifikáció hozzáadott hello attribútumok:

**d:Map** -elleni hello szolgáltatást kimeneti hajtotta végre az XPath kifejezés. hello itt feltételezése, hogy hello szolgáltatást kimeneti tartalmaz elemei, ismétlődő, például egy ATOM hírcsatornát, ha nincs meg ismétlődő bejegyzést csomópontok. Ezen ismétlődő csomópontok egy bejegyzést tartalmaz. hello XPath nem egyedi ismétlődő csomópontjának hello hello tartalomszolgáltató szolgáltatás eredmény megadott toopoint, amely értékei hello az adott bejegyzéshez. Példa hello szolgáltatás eredményét:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

hello XPath kifejezés /foo/bar lenne, mert a hello sáv csomópont egy hello ismétlődő hello csomópontja kimeneti és hello toohello végfelhasználói visszaadott tényleges tartalmat is tartalmaz.

**Kulcs** -Ez az attribútum piactér figyelmen kívül hagyja. REST-alapú webszolgáltatások, általában nem teszi közzé a elsődleges kulcs.

## <a name="property-node"></a>Tulajdonság-csomópont
Ez a csomópont hello rekord egy tulajdonságot tartalmaz.

Ez a csomópont adatait találhatók [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (használata hello **egyéb verzió** legördülő tooselect egy másik verziót, ha a szükséges tooview hello dokumentációját.) *Példa:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| Attribútumnév | Szükséges | Érték |
| --- | --- | --- |
| Név |Igen |hello tulajdonság hello neve. |
| Típus |Igen |hello hello tulajdonság értékének típusa. hello tulajdonságérték-típus kell lennie egy **EDMSimpleType** vagy a komplex típus (egy teljesen minősített nevet jelölve), amely hello modell hatókörén belül van. További információkért tekintse meg a fogalmi modell típusa (CSDL). |
| Nullázható |Nem |**Igaz** (alapértelmezett érték hello) vagy **hamis** attól függően, hogy hello tulajdonság is null értékű. Megjegyzés: A hello CSDL verzióját jelzi hello [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) névtér, rendelkeznie kell egy összetett típus tulajdonság Nullable = "False". |
| DefaultValue érték |Nem |hello hello tulajdonság alapértelmezett értéke. |
| MaxLength |Nem |hello hello tulajdonság értékének maximális hossza. |
| FixedLength |Nem |**Igaz** vagy **hamis** attól függően, hogy e hello tulajdonság értéke fogja tárolni egy fiexed hosszúságú karakterlánc. |
| Pontosság |Nem |Toohello számjegyek tooretain hello numerikus értéket a maximális számát jelenti. |
| Méretezés |Nem |Hello számértéket a tizedesjegyek tooretain maximális száma. |
| Unicode |Nem |**Igaz** vagy **hamis** attól függően, hogy kell-e hello tulajdonság értéke egy Unicode karakterlánc tárolja. |
| Rendezés |Nem |Hello szétválogatása feladatütemezési toobe hello adatforrás használt karakterlánc. |
| ConcurrencyMode |Nem |**Nincs** (alapértelmezett érték hello) vagy **rögzített**. Ha hello érték túl**rögzített**, hello tulajdonság értéket fogja használni az optimista konkurencia ellenőrzése. |

Az alábbiakban hello hello további attribútumok toohello CSDL-specifikáció lettek hozzáadva:

**d:Map** -hello szolgáltatás végre XPath kifejezés kimeneti, és kinyeri hello kimenet egy tulajdonságot. az XPath megadott hello relatív toohello ismétlődő hello EntityType csomópont XPath a kijelölt csomópont. Az is lehetséges toospecify egy abszolút XPath tooallow statikus erőforrás beleértve az egyes hello kimeneti csomópontok, mint például a szerzői jogi utasítást, amely csak megtalálható-e után a hello eredeti szolgáltatás kimeneti, de az egyes sorok hello hello OData szerepelnie kell kimeneti. Példa hello szolgáltatásból:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Itt hello XPath kifejezés lenne ./bar/baz0 tooget hello baz0 csomópont hello tartalomszolgáltató szolgáltatásból.

**d:CharMaxLength** -karakterlánc típusú, a maximális hossz hello adhat meg. DataService CSDL-példa

**d:IsPrimaryKey** -azt jelzi, hogy hello oszlop elsődleges kulcs hello hello tábla/nézet. Tekintse meg a DataService CSDL példát.

**d:isExposed** -határozza meg, ha hello táblaséma tesz elérhetővé (általában igaz). DataService CSDL-példa

**d:IsView** *(nem kötelező)* – igaz, ha ez a táblázat, hanem egy nézet alapul.  DataService CSDL-példa

**d:Tableschema** -DataService CSDL-példa

**d:ColumnName** -hello hello tábla/nézet hello oszlop neve.  DataService CSDL-példa

**d:IsReturned** -hello logikai érték, amely meghatározza, hogy ha hello szolgáltatás elérhetővé teszi az érték toohello ügyfél.  DataService CSDL-példa

**d:IsQueryable** -hello logikai érték, amely meghatározza, hogy ha egy adatbázis-lekérdezés hello oszlop használható.   DataService CSDL-példa

**d:OrdinalPosition** -hello oszlop numerikus helyzete megjelenését, x-hello tábla vagy nézet hello, ahol x származik-e 1 toohello hello tábla oszlopainak száma.  DataService CSDL-példa

**d:DatabaseDataType** -hello adattípusú hello oszlop hello adatbázisban, azaz SQL adattípus. DataService CSDL-példa

## <a name="supported-parametersproperty-types"></a>Támogatott paraméterek/tulajdonság típusa
Az alábbiakban hello hello támogatott típusok a paraméterek és a tulajdonságok. (Kis-és nagybetűket)

| Az egyszerű típusok | Leírás |
| --- | --- |
| NULL értékű |Hello hiányában a érték jelenti. |
| Logikai érték |Bináris értékű logika matematikai fogalma hello jelenti. |
| Bájt |Aláíratlan 8 bites egész szám |
| Dátum és idő |Dátum és idő jelöli a 12:00:00 éjféltől. január 1, 1753 A.D. közötti értékeket 11:59:59 óráig, December 9999 A.D. keresztül |
| Decimális |A rögzített pontosság és a skála numerikus értéket jelöli. Ez a típus leírhatja numerikus érték negatív 10 a ^ 255 + 1 toopositive 10 ^ 255 -1 |
| Dupla |A lebegőpontos szám értékek ± 2.23e-308 osztályig + 1.79e hozzávetőleges tartománnyal jelölő 15 jegy pontossággal jelöli +308. **Használja a decimális tooExel exportálási probléma miatt** |
| Egyetlen |A lebegőpontos szám értékek ± 1.18e-38 osztályig + 3.40e hozzávetőleges tartománnyal jelölő 7 számjegyek pontossággal jelöli + 38 |
| GUID |Egy 16 bájtos (128 bites) egyedi azonosító értékét jelöli |
| Int16 |Egy aláírt 16 bites egész értéket jelöli. |
| Int32 |Egy aláírt 32 bites egész értéket jelöli. |
| Int64 |Egy aláírt 64 bites egész értéket jelöli. |
| Karakterlánc |Rögzített - vagy változó hosszúságú karakteres adatot jelöli. |

## <a name="see-also"></a>Lásd még:
* A cikkből megtudhatja, hogyan készítheti ismertetése a hello általános OData-megfeleltetési folyamat és a cél, [szolgáltatás OData megfeleltetése](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definíciókat, és a utasításokat.
* Ha érdekli példák megtekintésével, olvassa el ebben a cikkben [adatok szolgáltatás OData leképezési példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee példakód és kód szintaxis és a környezetben.
* Ebben a cikkben egy adatszolgáltatás toohello Azure piactér-közzététel elérési előírt tooreturn toohello [adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md).
