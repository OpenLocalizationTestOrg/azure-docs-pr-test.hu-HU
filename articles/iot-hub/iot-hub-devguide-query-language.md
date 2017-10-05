---
title: "Az Azure IoT Hub lekérdezési nyelv megismerése |} Microsoft Docs"
description: "Fejlesztői útmutató – az SQL-szerű IoT Hub lekérdezési nyelv eszköz twins és feladatok kapcsolatos információkat kérdezi le az IoT hub leírása."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: a7650104eda58923558892f6f0f6666d16dbce28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Referencia - IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás

Az IoT-központ biztosít egy hatékony SQL-szerű nyelv való adatbeolvasás vonatkozó [eszköz twins] [ lnk-twins] és [feladatok][lnk-jobs], és [üzenet útválasztási][lnk-devguide-messaging-routes]. Ez a cikk mutatja be:

* Az IoT-központ lekérdező nyelv, a fő szolgáltatásainak bemutatása és
* A nyelv részletes leírása.

## <a name="get-started-with-device-twin-queries"></a>Az eszköz iker lekérdezések első lépései
[Eszköz twins] [ lnk-twins] tetszőleges JSON-objektumok címkék és a tulajdonságok is tartalmazhat. Az IoT-központ lehetővé teszi lekérdezés eszköz twins JSON-dokumentumként egyetlen tartalmazó összes iker eszközadatokat.
Tegyük fel például, hogy az IoT hub eszköz twins rendelkezik-e az alábbi szerkezettel:

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

Az IoT-központ nevű dokumentum gyűjteményként elérhetővé teszi az eszköz twins **eszközök**.
Ezért az alábbi lekérdezés lekéri az eszköz twins teljes készletét:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IoT SDK-k] [ lnk-hub-sdks] támogatja a nagyméretű eredmények lapozást.

Az IoT-központ lehetővé teszi tetszőleges feltételek szűrésének eszköz twins. Például

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

beolvassa az eszköz twins rendelkező a **location.region** címke értéke **USA**.
Logikai operátorok és aritmetikai összehasonlítások is támogatott, például

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

az Amerikai Egyesült Államokban legalább percenként telemetriai adatokat küldhet a konfigurált található összes eszköz twins kéri le. A könnyebb is lehetőség az állandókat használja a **IN** és **nA** (nem szereplő) operátor. Például

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

lekéri az összes eszköz twins jelentett Wi-Fi, vagy a vezetékes kapcsolati. Legtöbbször az összes eszköz twins, amelyek tartalmaznak egy adott tulajdonságra azonosításához. Az IoT-Központ támogatja a függvény `is_defined()` erre a célra. Például

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

minden eszköz twins meghatározó beolvasni a `connectivity` jelentett tulajdonság. Tekintse meg a [WHERE záradék] [ lnk-query-where] a szűrési képességek a teljes referencia szakasza.

Csoportosítás és az aggregációhoz is támogatottak. Például

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

minden telemetriai állapotot az eszközök számát adja vissza.

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

Az előző példában a helyzet, ahol három eszközöket jelentett sikeres konfigurációhoz, két továbbra is a konfiguráció alkalmazását, és egy hibát jelentett mutatja be.

### <a name="c-example"></a>C# – példa
A lekérdezési funkciókat tesz elérhetővé a [C# szolgáltatás SDK] [ lnk-hub-sdks] a a **RegistryManager** osztály.
Íme egy példa egy egyszerű lekérdezést:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

Megjegyzés: az **lekérdezés** objektum létrejön az oldalméretet (legfeljebb 1000), és majd a több oldalra hívásával kérhető a **GetNextAsTwinAsync** módszerek több alkalommal.
Vegye figyelembe, hogy a lekérdezés vezérlőnek több **következő\***, attól függően, a deszerializálás beállítást igényel, a lekérdezés, például a kettős vagy feladat eszközobjektumok, vagy az egyszerű JSON leképezések használata esetén használható.

### <a name="nodejs-example"></a>NODE.js – példa
A lekérdezési funkciókat tesz elérhetővé a [Azure IoT szolgáltatás SDK for Node.js] [ lnk-hub-sdks] a a **beállításjegyzék** objektum.
Íme egy példa egy egyszerű lekérdezést:

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

Megjegyzés: az **lekérdezés** objektum létrejön az oldalméretet (legfeljebb 1000), és majd a több oldalra hívásával kérhető a **nextAsTwin** módszerek több alkalommal.
Vegye figyelembe, hogy a lekérdezés vezérlőnek több **következő\***, attól függően, a deszerializálás beállítást igényel, a lekérdezés, például a kettős vagy feladat eszközobjektumok, vagy az egyszerű JSON leképezések használata esetén használható.

### <a name="limitations"></a>Korlátozások
> [!IMPORTANT]
> Lekérdezés eredményei eszköz twins lehet néhány perc múlva késedelem vonatkozóan a legutóbbi értékét. Ha a lekérdezése egyes eszköz twins-azonosító szerint, célszerű mindig történő lekérése eszköz iker API, amely mindig a legfrissebb értéket tartalmaz, és magasabb szabályozás korlátok.

Jelenleg összehasonlítások csak között támogatott primitív típusok (nincs objektumok), például `... WHERE properties.desired.config = properties.reported.config` csak támogatott, ha ezek a tulajdonságok egyszerű értékűek.

## <a name="get-started-with-jobs-queries"></a>Ismerkedés a feladatok lekérdezések
[Feladatok] [ lnk-jobs] lehetőséget nyújtanak olyan meg az eszközök-műveletek végrehajtásához. Minden eszköz iker tartalmazza, amely egy nevű gyűjtemény részét képezi a feladatok **feladatok**.
Logikailag,

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Ez a gyűjtemény jelenleg lekérdezhető mint **devices.jobs** az IoT-központ a lekérdezési nyelv.

> [!IMPORTANT]
> A feladatok tulajdonság jelenleg nem vissza, ha lekérdezése eszköz twins (Ez azt jelenti, hogy lekérdezések tartalmaz "eszközök"). Csak használható közvetlenül a lekérdezések használatával `FROM devices.jobs`.
>
>

Például ahhoz, hogy az összes feladat (elmúlt és ütemezett), amely egyetlen eszközt érintik, használhatja a következő lekérdezést:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Vegye figyelembe, hogy ez a lekérdezés biztosítja az eszközspecifikus állapota (és valószínűleg a közvetlen módszer válasz) minden visszaadott feladat.
Akkor is az összes objektum tulajdonság tetszőleges logikai feltételeknek szűrése a **devices.jobs** gyűjtemény.
Például a következő lekérdezést:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

lekéri az összes befejezett eszköz két frissítési feladatok eszköz **myDeviceId** után 2016 szeptemberétől hozott létre.

Akkor is egyetlen feladat eszközönkénti eredményeit beolvasása.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Korlátozások
A jelenleg, lekérdezi **devices.jobs** nem támogatják:

* Leképezések, ezért csak `SELECT *` lehetséges.
* Tekintse meg a feladat tulajdonságait (lásd az előző szakaszban) mellett az eszköz két feltételnek.
* Összesítéseket, például a száma, avg, a csoportosítás alapját végrehajtása.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Ismerkedés az eszközről a felhőbe üzenet útvonalak lekérdezési kifejezések

Használatával [eszközről a felhőbe útvonalak][lnk-devguide-messaging-routes], konfigurálhatja az IoT-központ átirányítani az egyes üzeneteket értékelni kifejezések alapján különböző végpontokhoz üzenetek eszközről a felhőbe.

Az útvonal [feltétel] [ lnk-query-expressions] ugyanazt az IoT-központ a lekérdezés nyelvet használja, mint a feltételek iker és feladat lekérdezésekben. Útvonal feltételek értékelésének üzenetfejlécek és törzse. Az útválasztási lekérdezési kifejezésben járhatnak csak üzenetfejlécek, csak az üzenettörzs vagy üzenet fejlécek és üzenet törzse. Az IoT-központ azt feltételezi, hogy a fejlécek és az üzenet törzse egy adott séma útválasztásához üzeneteket, és a következő szakaszok ismertetik a megfelelő irányításához az IoT-központ szükséges:

### <a name="routing-on-message-headers"></a>Fejlécek az Útválasztás

Az IoT-központ azt feltételezi, hogy a következő JSON-ábrázolását üzenetfejlécek üzenet útválasztás:

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

Üzenet Rendszertulajdonságok fűzve előtagként a `'$'` szimbólum.
Felhasználói tulajdonságok a nevükkel mindig érhetők el. Ha egy felhasználó tulajdonságnév történik-e a rendszer tulajdonság egybe (például `$to`), a felhasználó tulajdonság veszi a a `$to` kifejezés.
A rendszer tulajdonság használatával zárójeleket mindig elérhető `{}`: például a kifejezés használható `{$to}` eléréséhez a rendszer tulajdonság `to`. Zárójeles tulajdonságnevek mindig a megfelelő rendszer tulajdonság beolvasása.

Ne feledje, hogy tulajdonságnevek megkülönböztetik a kis-és nagybetűket.

> [!NOTE]
> Minden üzenet tulajdonságai olyan karakterláncok. Rendszer tulajdonságai, lásd: a [– útmutató fejlesztőknek][lnk-devguide-messaging-format], jelenleg nem használható lekérdezésekben.
>

Ha például egy `messageType` tulajdonság, érdemes egy végpontot, és egy másik végpont az összes riasztás irányíthatja az összes telemetriai adat. Írhat a telemetriai adatok továbbításához a következő kifejezést:

```sql
messageType = 'telemetry'
```

És a figyelmeztető üzenetek a következő kifejezést:

```sql
messageType = 'alert'
```

Logikai kifejezésen, és a funkciók is támogatottak. Ez a funkció lehetővé teszi, hogy alapján megkülönböztetheti a súlyossági szintet, például:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Tekintse meg a [kifejezés és feltételek] [ lnk-query-expressions] támogatott operátorok és funkciók teljes listája szakaszában.

### <a name="routing-on-message-bodies"></a>Az üzenet törzse Útválasztás

Az IoT-központ csak irányíthatja a üzenettörzs alapján tartalmát, ha az üzenet törzse nem megfelelően formázott JSON-kódolású UTF-8, UTF-16 vagy UTF-32. Meg kell adni az üzenet tartalomtípusa `application/json` és az IoT-központ az üzenet törzse tartalma alapján engedélyezi a üzenetfejlécek a támogatott UTF kódolások egyikét tartalmának kódolását. Ha a fejlécek egyikét nincs megadva, az IoT-központ nem kísérli meg bármely lekérdezési kifejezés használata esetén a szervezet az üzeneten való kiértékelése. Ha az üzenet nem egy JSON-üzenetet, vagy ha az üzenet nem adja meg a tartalom típusa és a tartalmának kódolását, Ön továbbra is használhatja üzenet-útválasztás a fejlécek alapján üzenet.

Használhat `$body` az üzenet a lekérdezési kifejezésben. Használhatja egyszerű törzs hivatkozást, törzs tömb referencia vagy több szervezet hivatkozást a lekérdezési kifejezésben. A lekérdezési kifejezésben kombinálhatja üzenet fejlécének hivatkozással törzs hivatkozást is. Például a következők minden érvényes lekérdezési kifejezések:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>Az IoT-központ lekérdezést alapjai
Minden egyes IoT-központ lekérdezés áll egy jelöljön ki és FROM záradék használata nem kötelező HELYÉT és a GROUP BY záradékban. A JSON-dokumentumok, például az eszköz twins gyűjteménye minden egyes lekérdezés futtatható. A FROM záradék azt jelzi, a dokumentum gyűjteményt, amelyben többször is meg kell (**eszközök** vagy **devices.jobs**). Ezt követően a WHERE záradékban a szűrő alkalmazása. Az összesítéseket, ez a lépés szerint vannak csoportosítva sor jön létre a GROUP BY záradékban, és minden egyes csoport megadva, a SELECT záradékban megadottak szerint.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM záradékban
A **< from_specification > a** záradék csak két érték feltételezheti: **ESZKÖZÖKRŐL**, eszköz twins, lekérdezése vagy **devices.jobs a**, feladat eszközönkénti lekérdezése részletes adatokat.

## <a name="where-clause"></a>A WHERE záradék
A **ahol < filter_condition >** záradék használata nem kötelező. Meghatározza, hogy a JSON-dokumentumok FROM gyűjtemény egy vagy több feltételt meg kell felelnie a eredményének része. Bármely JSON-dokumentum ki kell értékelnie, hogy a megadott feltételeknek, az eredmény szerepeltetni a "true".

Az engedélyezett feltételek részében leírt [kifejezések és a kikötések][lnk-query-expressions].

## <a name="select-clause"></a>SELECT záradékban
A SELECT záradékban (**VÁLASSZA < select_list >**) megadása kötelező, és határozza meg, milyen értékeket olvassa be a lekérdezést. Azt adja meg az új JSON-objektumok létrehozásához használt JSON értékeket.
A FROM gyűjtemény szűrt (és nem kötelezően csoportosított) részhalmazát minden egyes elemhez a leképezés fázis állít elő, egy új JSON-objektum, a SELECT záradékban megadott értékek kialakítani.

Az alábbiakban látható a SELECT záradékban nyelvtani:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

Ha **attribute_name** a JSON-dokumentum FROM gyűjtemény egyik tulajdonságnak sem hivatkozik. Néhány példa a SELECT záradékban található a [Ismerkedés az eszköz iker lekérdezések] [ lnk-query-getstarted] szakasz.

Jelenleg kijelölt záradékot eltérő **válasszon \***  csak az eszköz twins összesített lekérdezései támogat.

## <a name="group-by-clause"></a>GROUP BY záradékban
A **GROUP BY < group_specification >** záradék egy opcionális lépés, amely után a szűrő megadott szerepel a WHERE záradékban, és a leképezés kiválasztása megadott előtt hajtható végre. Dokumentumok egy attribútum alapján csoportosítja azt. Ezek a csoportok a SELECT záradékban megadott összesített értékek generálásához használt.

A GROUP BY lekérdezést példa, hogy:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

A formális GROUP BY szintaxisa a következő:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

Ha **attribute_name** a JSON-dokumentum FROM gyűjtemény egyik tulajdonságnak sem hivatkozik.

Jelenleg a GROUP BY záradék csak támogatott eszköz twins lekérdezésekor.

## <a name="expressions-and-conditions"></a>Kifejezések és a feltételek
Magas szinten egy *kifejezés*:

* (Például a logikai érték, számot, karakterlánc, a tömb vagy objektum), egy JSON-típus egy példánya értékelődik ki és
* Határozza meg a JSON-dokumentum eszköz és a beépített operátorok és függvények használata állandók származó adatok kezelésére.

*Feltételek* kifejezések, amelyek kiértékelik logikai értékként. Bármely állandó eltér a logikai **igaz** minősül **hamis** (beleértve a **null**, **nem definiált**, bármely objektum vagy tömb példány bármilyen karakterlánc, és egyértelműen a logikai **hamis**).

A kifejezés szintaxisa a következő:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

Ahol:

| Szimbólum | Meghatározás |
| --- | --- |
| attribute_name | A JSON-dokumentum található bármely tulajdonságát a **FROM** gyűjtemény. |
| binary_operator | A bináris operátor szerepel a [operátorok](#operators) szakasz. |
| function_name| Bármely függvény szerepel a [funkciók](#functions) szakasz. |
| decimal_literal |Egy lebegőpontos decimális jelöléssel kifejezve. |
| hexadecimal_literal |Egy szám, a karakterlánc a "0 x" hexadecimális számjegyeket tartalmazó karakterlánc követ kifejezve. |
| string_literal |A szövegkonstansok olyan Unicode karakterláncok sorozatát nulla vagy több Unicode-karaktereket vagy escape-karaktersorozatokat. A szövegkonstansok vannak szimpla zárójelek között (aposztróf: ") vagy dupla idézőjel (idézőjel:"). Kilépés engedélyezett: `\'`, `\"`, `\\`, `\uXXXX` az Unicode karaktereket 4 hexadecimális számjegy határozzák meg. |

### <a name="operators"></a>Operátorok
Az alábbi műveleteket támogatja:

| Termékcsalád | Operátorok |
| --- | --- |
| Aritmetikai |+,-,*,/,% |
| Logikai |ÉS, VAGY SEM |
| Összehasonlítása |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Functions
Twins és az egyetlen támogatott feladatok lekérdezésekor függvény van:

| Függvény | Leírás |
| -------- | ----------- |
| IS_DEFINED(property) | Jelző, ha a tulajdonság van rendelve egy érték logikai érték beolvasása (beleértve a `null`). |

Útvonalak feltételek mellett a következő matematikai-funkciók támogatottak:

| Függvény | Leírás |
| -------- | ----------- |
| ABS(x) | A megadott numerikus kifejezés (pozitív) abszolút értékét adja vissza. |
| Exp(x) | Az exponenciális a megadott numerikus kifejezés értékét adja vissza (e ^ x). |
| Power(x,y) | A megadott kifejezés értékét adja vissza a megadott hatványra (x ^ y).|
| Square(x) | Kiszámítja a megadott numerikus érték. |
| CEILING(x) | A legkisebb egész értéket ad vissza, nagyobb vagy egyenlő a megadott numerikus kifejezés. |
| FLOOR(x) | A legnagyobb egész számot ad vissza kisebb vagy egyenlő, mint a megadott numerikus kifejezés. |
| SIGN(x) | A pozitív (+ 1), nulla (0) vagy a megadott numerikus kifejezés mínuszjel (-1) adja vissza.|
| Sqrt(x) | Kiszámítja a megadott numerikus érték. |

Útvonalak állapotától függően a következő típus ellenőrzése és adattípusokról funkciók támogatottak:

| Függvény | Leírás |
| -------- | ----------- |
| AS_NUMBER | A bemeneti karakterlánc alakít egy számot. `noop`Ha a bemeneti érték egy szám; `Undefined` Ha karakterlánc nem felel meg egy számot.|
| IS_ARRAY | Azt jelzi, hogy ha a megadott kifejezés típusú tömb egy logikai értéket ad vissza. |
| IS_BOOL | Azt jelzi, hogy ha a megadott kifejezés típusa olyan logikai érték logikai érték beolvasása. |
| IS_DEFINED | Jelzi, ha a tulajdonság van rendelve egy érték logikai érték beolvasása. |
| IS_NULL | Visszaad egy logikai értéket, amely azt jelzi, ha a megadott kifejezés típusa null. |
| IS_NUMBER | Azt jelzi, hogy ha a típus a megadott kifejezés több olyan logikai értéket ad vissza. |
| IS_OBJECT | Azt jelzi, hogy ha a megadott kifejezés típusa egy JSON-objektum egy logikai értéket ad vissza. |
| IS_PRIMITIVE | Azt jelzi, hogy ha a megadott kifejezés típusa egy primitív egy logikai értéket ad vissza (string, Boolean, numerikus vagy `null`). |
| IS_STRING | Azt jelzi, hogy ha a megadott kifejezés típusa karakterlánc egy logikai értéket ad vissza. |

Útvonalak feltételek a következő karakterlánc-funkciók támogatottak:

| Függvény | Leírás |
| -------- | ----------- |
| CONCAT(x,...) | Karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével eredményét adja vissza. |
| LENGTH(x) | A megadott karakterlánc-kifejezés karakterek számát adja vissza.|
| Lower(x) | Egy karakterlánc-kifejezés után nagybetűt adatok kisbetűssé alakításával adja vissza. |
| Upper(x) | Egy karakterlánc-kifejezés után kisbetűt adatok nagybetűssé alakításával adja vissza. |
| SUBSTRING (karakterlánc, start [, hossz]) | A megadott karakter nulla pozíciótól kezdődően karakterlánc-kifejezés részét adja vissza, és továbbra is fennáll, a megadott időtartam, illetve a karakterlánc végén. |
| (Karakterlánc, töredék) INDEX_OF | A második első előfordulásának kezdőpozícióját adja vissza karakterlánc-kifejezés az első megadott karakterlánc-kifejezés vagy -1, ha a karakterlánc nem található.|
| STARTS_WITH (x, y) | Visszaadja egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés kezdődik-e a második. |
| ENDS_WITH (x, y) | Adja vissza egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés a második végződik. |
| CONTAINS(x,y) | Visszaadja egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés tartalmazza a második. |

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogyan hajtsa végre a lekérdezéseket az alkalmazások a [Azure IoT SDK-k][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
