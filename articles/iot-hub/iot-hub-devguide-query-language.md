---
title: "aaaUnderstand hello Azure IoT Hub lekérdezési nyelv |} Microsoft Docs"
description: "Fejlesztői útmutató - hello SQL-szerű IoT Hub lekérdezési nyelv leírása használt eszköz twins és az IoT hub-feladatok tooretrieve információt."
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
ms.openlocfilehash: 01a7c8ffdf44c6c27b834739d02c8fef1dd3d3fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Referencia - IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás

Az IoT-központ tájékoztatást ad a hatékony SQL-szerű nyelv tooretrieve vonatkozó [eszköz twins] [ lnk-twins] és [feladatok][lnk-jobs], és [üzenet útválasztási][lnk-devguide-messaging-routes]. Ez a cikk mutatja be:

* Egy bevezető toohello főbb hello IoT-központ lekérdező nyelv, szolgáltatásainak és
* hello részletes hello nyelvi leírása.

## <a name="get-started-with-device-twin-queries"></a>Az eszköz iker lekérdezések első lépései
[Eszköz twins] [ lnk-twins] tetszőleges JSON-objektumok címkék és a tulajdonságok is tartalmazhat. Az IoT-központ JSON-dokumentumként egyetlen összes iker Eszközadatokat tartalmazó lehetővé teszi tooquery eszköz twins.
Tegyük fel például, hogy az IoT hub eszköz twins rendelkezik-e a struktúra a következő hello:

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

Az IoT-központ hello eszköz twins mutatja egy nevű dokumentumgyűjteményt **eszközök**.
Ezért a következő lekérdezés hello lekéri hello teljes készletének eszközt twins:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IoT SDK-k] [ lnk-hub-sdks] támogatja a nagyméretű eredmények lapozást.

Az IoT-központ lehetővé teszi tetszőleges feltételek szűrésének tooretrieve eszköz twins. Például

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

lekéri a hello az eszköz twins hello **location.region** címke beállítása túl**USA**.
Logikai operátorok és aritmetikai összehasonlítások is támogatott, például

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

hello VELÜNK konfigurált toosend telemetriai kisebb gyakran percenként található összes eszköz twins kéri le. A könnyebb egyben a hello lehetséges toouse állandókat **IN** és **nA** (nem szereplő) operátor. Például

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

lekéri az összes eszköz twins jelentett Wi-Fi, vagy a vezetékes kapcsolati. Ez gyakran szükséges tooidentify minden eszköz twins, amelyek tartalmaznak egy adott tulajdonságra van. Az IoT-Központ támogatja hello függvény `is_defined()` erre a célra. Például

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

minden eszköz twins hello meghatározó beolvasott `connectivity` jelentett tulajdonság. Tekintse meg a toohello [WHERE záradék] [ lnk-query-where] szűrési lehetőségek hello szakasza hello teljes referenciaként.

Csoportosítás és az aggregációhoz is támogatottak. Például

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

minden telemetriai konfiguráció állapota hello eszközök hello számát adja vissza.

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

hello előző példa azt mutatja be olyan helyzet, ahol három eszközöket jelentett sikeres konfigurációhoz, továbbra is alkalmazzák, két hello konfigurációs és egy hibát jelzett.

### <a name="c-example"></a>C# – példa
hello lekérdezési funkciókat tesz elérhetővé hello [C# szolgáltatás SDK] [ lnk-hub-sdks] a hello **RegistryManager** osztály.
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

Megjegyzés: hogyan hello **lekérdezés** (felfelé too1000) lap méretű objektum létrejön, és majd lekérhetők több lapokat hívó hello **GetNextAsTwinAsync** módszerek többször.
Vegye figyelembe, hogy hello lekérdezés vezérlőnek több **következő\***, attól függően, hogy hello deszerializálása beállítás szükséges eszköz iker vagy feladat objektumok, például a hello lekérdezés vagy egyszerű JSON toobe leképezések használni.

### <a name="nodejs-example"></a>NODE.js – példa
hello lekérdezési funkciókat tesz elérhetővé hello [Azure IoT szolgáltatás SDK for Node.js] [ lnk-hub-sdks] a hello **beállításjegyzék** objektum.
Íme egy példa egy egyszerű lekérdezést:

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed toofetch hello results: ' + err.message);
    } else {
        // Do something with hello results
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

Megjegyzés: hogyan hello **lekérdezés** (felfelé too1000) lap méretű objektum létrejön, és majd lekérhetők több lapokat hívó hello **nextAsTwin** módszerek többször.
Vegye figyelembe, hogy hello lekérdezés vezérlőnek több **következő\***, attól függően, hogy hello deszerializálása beállítás szükséges eszköz iker vagy feladat objektumok, például a hello lekérdezés vagy egyszerű JSON toobe leképezések használni.

### <a name="limitations"></a>Korlátozások
> [!IMPORTANT]
> Lekérdezés eredményei eszköz twins tekintetben toohello legújabb értékekkel késedelem néhány perc is lehet. Ha lekérdezése egyes eszköz twins-azonosító szerint, a rendszer mindig előnyösebb toouse hello eszköz iker API, amely mindig hello legújabb értékeket tartalmaz, és magasabb szabályozás korlátok beolvasása.

Jelenleg összehasonlítások csak között támogatott primitív típusok (nincs objektumok), például `... WHERE properties.desired.config = properties.reported.config` csak támogatott, ha ezek a tulajdonságok egyszerű értékűek.

## <a name="get-started-with-jobs-queries"></a>Ismerkedés a feladatok lekérdezések
[Feladatok] [ lnk-jobs] egy módon tooexecute meg az eszközök műveleteket biztosítsanak. Minden eszköz iker hello feladatok, amelyek nevű gyűjtemény részét képezi hello információkat tartalmaz **feladatok**.
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

Ez a gyűjtemény jelenleg lekérdezhető mint **devices.jobs** az IoT-központ lekérdezési nyelv hello.

> [!IMPORTANT]
> Hello feladatok tulajdonság jelenleg, soha nem vissza, ha az eszköz twins (Ez azt jelenti, hogy lekérdezések "ESZKÖZÖKRŐL" tartalmazó) lekérdezését. Csak használható közvetlenül a lekérdezések használatával `FROM devices.jobs`.
>
>

Például tooget összes feladatot (elmúlt és ütemezett), amely egyetlen eszközt érintik, a következő lekérdezés hello használhatja:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Vegye figyelembe, hogy ez a lekérdezés biztosítja a hello eszközspecifikus állapota (és valószínűleg hello közvetlen metódusra adott válasz) minden visszaadott feladat.
Egyúttal a tetszőleges logikai feltétellel hello szereplő összes objektum tulajdonság lehetséges toofilter **devices.jobs** gyűjtemény.
Például hello a következő lekérdezést:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

lekéri az összes befejezett eszköz két frissítési feladatok eszköz **myDeviceId** után 2016 szeptemberétől hozott létre.

Akkor is lehetséges tooretrieve hello eszközönkénti eredményeit egyetlen feladat.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Korlátozások
A jelenleg, lekérdezi **devices.jobs** nem támogatják:

* Leképezések, ezért csak `SELECT *` lehetséges.
* Az feltételeket, amelyek toohello eszköz iker hozzáadása toojob tulajdonságai (lásd a szakasz fenti hello) hivatkozik.
* Összesítéseket, például a száma, avg, a csoportosítás alapját végrehajtása.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Ismerkedés az eszközről a felhőbe üzenet útvonalak lekérdezési kifejezések

Használatával [eszközről a felhőbe útvonalak][lnk-devguide-messaging-routes], IoT-központ toodispatch eszközről-a-felhőbe üzenetek az egyes üzeneteket értékelni kifejezések alapján toodifferent végpontok is beállíthat.

hello útvonal [feltétel] [ lnk-query-expressions] használja a két-és feladat feltételek megegyező IoT-központ lekérdezés nyelvű hello. Útvonal feltételek értékelésének hello üzenetfejlécek és törzse. Az útválasztási lekérdezési kifejezésben csak üzenetfejlécek, csak egy hello üzenettörzs járhatnak vagy üzenet fejlécek és üzenet törzse. Az IoT-központ azt feltételezi, hogy egy adott séma hello fejlécek és az üzenet törzse rendelés tooroute üzenetek, és hello a következő szakaszok ismertetik az IoT-központ tooproperly útvonal szükséges:

### <a name="routing-on-message-headers"></a>Fejlécek az Útválasztás

Az IoT-központ azt feltételezi, hogy a következő üzenet irányításához üzenetfejlécek JSON-ábrázolását hello:

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

Üzenet Rendszertulajdonságok fűzve előtagként hello `'$'` szimbólum.
Felhasználói tulajdonságok a nevükkel mindig érhetők el. Ha egy felhasználó tulajdonságnév történik-e a rendszer tulajdonsággal toocoincide (például `$to`), a hello veszi hello felhasználói tulajdonság `$to` kifejezés.
Mindig elérhető hello rendszer tulajdonságon keresztül zárójeleket `{}`: például hello kifejezés használható `{$to}` tooaccess hello rendszer tulajdonság `to`. Zárójeles tulajdonságnevek mindig hello megfelelő rendszer tulajdonság beolvasása.

Ne feledje, hogy tulajdonságnevek megkülönböztetik a kis-és nagybetűket.

> [!NOTE]
> Minden üzenet tulajdonságai olyan karakterláncok. Rendszer tulajdonságai, a hello [– útmutató fejlesztőknek][lnk-devguide-messaging-format], jelenleg nem érhető el toouse lekérdezésekben.
>

Ha például egy `messageType` tulajdonság, érdemes tooroute összes telemetriai tooone végpont, és minden riasztások tooanother végpont. A következő kifejezés tooroute hello telemetriai hello írhat be:

```sql
messageType = 'telemetry'
```

És a következő kifejezés tooroute hello figyelmeztető üzenetek hello:

```sql
messageType = 'alert'
```

Logikai kifejezésen, és a funkciók is támogatottak. Ez a funkció lehetővé teszi, hogy toodistinguish közötti súlyossági szintet, például:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Tekintse meg a toohello [kifejezés és feltételek] [ lnk-query-expressions] hello teljes lista szakasza támogatott operátort és függvényt.

### <a name="routing-on-message-bodies"></a>Az üzenet törzse Útválasztás

Az IoT-központ csak irányíthatja a üzenettörzs alapján tartalmát, ha hello az üzenet törzse nem megfelelően formázott JSON-kódolású UTF-8, UTF-16 vagy UTF-32. Be kell állítani az üdvözlő üzenet tartalomtípusa hello túl`application/json` és a hello kódolási tooone tartalom hello támogatott UTF kódolások hello üzenet fejlécek tooallow IoT-központ tooroute üdvözlőüzenetére hello törzs tartalma alapján. Ha hello fejlécek egyikét nincs megadva, az IoT-központ nem kísérli meg tooevaluate bármely hello törzs elleni üdvözlőüzenetére érintő lekérdezési kifejezésben. Ha az üzenet nem egy JSON-üzenetet, vagy üdvözlőüzenetére nem adja meg a hello tartalomtípus és tartalmának kódolását, Ön továbbra is használhatja üzenettovábbítás tooroute üdvözlőüzenetére hello fejlécek alapján.

Használhat `$body` hello lekérdezési kifejezés tooroute hello üzenetben. Használhatja egyszerű törzs hivatkozást, törzs tömb referencia vagy több szervezet hivatkozást hello a lekérdezési kifejezésben. A lekérdezési kifejezésben kombinálhatja üzenet fejlécének hivatkozással törzs hivatkozást is. Például hello az alábbiakban az összes érvényes lekérdezési kifejezések:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>Az IoT-központ lekérdezést alapjai
Minden egyes IoT-központ lekérdezés áll egy jelöljön ki és FROM záradék használata nem kötelező HELYÉT és a GROUP BY záradékban. A JSON-dokumentumok, például az eszköz twins gyűjteménye minden egyes lekérdezés futtatható. hello FROM záradék azt jelzi, hello dokumentum gyűjtemény toobe többször is meg (**eszközök** vagy **devices.jobs**). Ezt követően hello szűrő a hello amikor záradék van érvényben. Az összesítéseket, a megadott hello GROUP BY záradékban, a ebben a lépésben hello eredményeit vannak csoportosítva, és az egyes csoportok sor jön létre a SELECT záradékban hello megadottak szerint.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM záradékban
Hello **a < from_specification >** záradék csak két értéket veheti fel: **ESZKÖZÖKRŐL**, tooquery eszköz twins, vagy **devices.jobs a**, tooquery feladat eszközönként részletes adatokat.

## <a name="where-clause"></a>A WHERE záradék
Hello **ahol < filter_condition >** záradék használata nem kötelező. Meghatározza, hogy hello JSON-dokumentumok hello FROM gyűjtemény hello eredmény részét képező toobe meg kell felelnie egy vagy több feltételt. Ki kell értékelnie minden bármely JSON-dokumentum hello megadott feltételek túl "true" hello eredményben toobe.

hello engedélyezett feltételek részében leírt [kifejezések és a kikötések][lnk-query-expressions].

## <a name="select-clause"></a>SELECT záradékban
hello SELECT záradékban (**VÁLASSZA < select_list >**) megadása kötelező, és határozza meg, milyen értékeket lekért hello lekérdezés. Azt adja meg, hogy hello JSON értékek toobe használt toogenerate új JSON-objektumok.
Az egyes elemeinek hello hello FROM gyűjtemény szűrt (és nem kötelezően csoportosított) részhalmazát, hello leképezése fázis hoz létre egy új JSON-objektum, a hello SELECT záradékban megadott hello értékek kialakítani.

Az alábbiakban látható a SELECT záradékban hello hello nyelvtan:

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

Ha **attribute_name** hello JSON-dokumentum hello FROM gyűjtemény tooany tulajdonsága hivatkozik. Néhány példa a SELECT záradékban található hello [Ismerkedés az eszköz iker lekérdezések] [ lnk-query-getstarted] szakasz.

Jelenleg kijelölt záradékot eltérő **válasszon \***  csak az eszköz twins összesített lekérdezései támogat.

## <a name="group-by-clause"></a>GROUP BY záradékban
Hello **GROUP BY < group_specification >** záradék egy opcionális lépés után hello szűrő hello WHERE záradékban megadott hajtható végre, és hello megadott hello leképezése előtt válassza ki. Az attribútum értékének hello alapján dokumentumok felsorolását tartalmazza. Ezek a csoportok értékeket összesítve használt toogenerate hello SELECT záradékban megadott.

A GROUP BY lekérdezést példa, hogy:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

hello formális GROUP BY szintaxisa a következő:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

Ha **attribute_name** hello JSON-dokumentum hello FROM gyűjtemény tooany tulajdonsága hivatkozik.

Jelenleg hello GROUP BY záradék használata csak támogatott eszköz twins lekérdezésekor.

## <a name="expressions-and-conditions"></a>Kifejezések és a feltételek
Magas szinten egy *kifejezés*:

* Kiértékeli tooan típusú példány JSON (például a logikai érték, számot, karakterlánc, a tömb vagy objektum), és
* Határozza meg hello eszköz JSON-dokumentum és a beépített operátorok és függvények használata állandók származó adatok kezelésére.

*Feltételek* kifejezések, amelyek kiértékelik tooa logikai érték. Bármely állandó eltér a logikai **igaz** minősül **hamis** (beleértve a **null**, **nem definiált**, bármely objektum vagy tömb példány bármilyen karakterlánc, és egyértelműen hello logikai **hamis**).

hello kifejezések szintaxisa a következő:

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
| attribute_name | Bármely tulajdonságát a hello hello JSON-dokumentum **FROM** gyűjtemény. |
| binary_operator | A bináris operátor szerepel a hello [operátorok](#operators) szakasz. |
| function_name| Bármely függvény szerepel hello [funkciók](#functions) szakasz. |
| decimal_literal |Egy lebegőpontos decimális jelöléssel kifejezve. |
| hexadecimal_literal |Egy szám kifejezni hello karakterlánc "0 x" hexadecimális számjegyeket tartalmazó karakterlánc követ. |
| string_literal |A szövegkonstansok olyan Unicode karakterláncok sorozatát nulla vagy több Unicode-karaktereket vagy escape-karaktersorozatokat. A szövegkonstansok vannak szimpla zárójelek között (aposztróf: ") vagy dupla idézőjel (idézőjel:"). Kilépés engedélyezett: `\'`, `\"`, `\\`, `\uXXXX` az Unicode karaktereket 4 hexadecimális számjegy határozzák meg. |

### <a name="operators"></a>Operátorok
a következő operátor hello támogatottak:

| Termékcsalád | Operátorok |
| --- | --- |
| Aritmetikai |+,-,*,/,% |
| Logikai |ÉS, VAGY SEM |
| Összehasonlítása |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Functions
Csak a támogatott twins és feladatok hello lekérdezésekor függvény van:

| Függvény | Leírás |
| -------- | ----------- |
| IS_DEFINED(property) | Jelző, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása (beleértve a `null`). |

Útvonalak állapotától függően a következő matematikai függvények hello támogat:

| Függvény | Leírás |
| -------- | ----------- |
| ABS(x) | Értéket ad vissza hello abszolút (pozitív) hello a megadott numerikus kifejezés. |
| Exp(x) | Értéket ad vissza hello exponenciális hello a megadott numerikus kifejezés (e ^ x). |
| Power(x,y) | Értéket ad vissza a megadott hello értékének hello kifejezés toohello megadott power (x ^ y).|
| Square(x) | Beolvasása hello négyzetes hello a megadott numerikus érték. |
| CEILING(x) | Beolvasása hello legkisebb egész szám nagyobb értékre, vagy egyenlő, hello megadott numerikus kifejezés. |
| FLOOR(x) | Visszaadja hello legnagyobb egész szám kisebb vagy egyenlő, mint a toohello megadott numerikus kifejezés. |
| SIGN(x) | Beolvasása hello pozitív (+ 1), nulla (0), vagy a hello mínuszjel (-1) megadott numerikus kifejezés.|
| Sqrt(x) | Beolvasása hello négyzetes hello a megadott numerikus érték. |

Az útvonalak feltételek a következő típus ellenőrzése és a funkciók leadó hello támogatottak:

| Függvény | Leírás |
| -------- | ----------- |
| AS_NUMBER | Hello bemeneti karakterlánc tooa számot konvertálja. `noop`Ha a bemeneti érték egy szám; `Undefined` Ha karakterlánc nem felel meg egy számot.|
| IS_ARRAY | Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy tömb. |
| IS_BOOL | Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés olyan logikai érték. |
| IS_DEFINED | Jelzi, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása. |
| IS_NULL | Visszaadja egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés értéke null. |
| IS_NUMBER | Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy szám. |
| IS_OBJECT | Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés egy JSON-objektum. |
| IS_PRIMITIVE | Ha hello hello megadva kifejezés egy egyszerű jelző logikai érték beolvasása (string, Boolean, numerikus vagy `null`). |
| IS_STRING | Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték: karakterlánc. |

Útvonalak feltételek, a karakterlánc a következő hello támogatottak:

| Függvény | Leírás |
| -------- | ----------- |
| CONCAT(x,...) | Egy karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével hello eredményét adja vissza. |
| LENGTH(x) | A megadott karakterlánc-kifejezés hello karakterét hello számát adja vissza.|
| Lower(x) | Egy karakterlánc-kifejezés nagybetűt adatok toolowercase átalakítása után adja vissza. |
| Upper(x) | Egy karakterlánc-kifejezés kisbetűt adatok toouppercase átalakítása után adja vissza. |
| SUBSTRING (karakterlánc, start [, hossz]) | Hello kezdődő karakterlánc-kifejezés részét adja vissza a megadott karakter nulláról indulva számolt helyzetét, és folytatja a toohello megadott hosszúság vagy hello karakterlánc toohello végét. |
| (Karakterlánc, töredék) INDEX_OF | Hello hello második karakterlánc-kifejezés hello első megadott karakterlánc-kifejezés vagy-1 érték első előfordulásának pozícióját a indítása, ha hello karakterlánc nem található hello adja vissza.|
| STARTS_WITH (x, y) | Hogy hello első karakterlánc-kifejezés indításakor hello második jelző logikai érték beolvasása. |
| ENDS_WITH (x, y) | E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása. |
| CONTAINS(x,y) | Hogy hello első karakterlánc-kifejezés második tartalmaz-e hello jelző logikai érték beolvasása. |

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan tooexecute lekérdezi az alkalmazások a [Azure IoT SDK-k][lnk-hub-sdks].

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
