---
title: "aaaAzure Stream Analytics JavaScript-a felhasználó által definiált függvények |} Microsoft Docs"
description: "Hajtsa végre a felhasználó által definiált függvények JavaScript speciális lekérdezési mechanika"
keywords: "JavaScript, felhasználó által definiált feladatokat az UDF-ben"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 28eeb8f6437c23989e8887687b950361fed4414c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a>Az Azure Stream Analytics JavaScript felhasználó által definiált függvények
Az Azure Stream Analytics a felhasználó által definiált függvények JavaScript nyelven írt támogatja. A hello széles skáláját **karakterlánc**, **RegExp szolgáltatást**, **matematikai**, **tömb**, és **dátum** módszereket, amelyek JavaScript biztosít, a Stream Analytics-feladatok összetett adatátalakítást könnyebb toocreate válik.

## <a name="javascript-user-defined-functions"></a>Felhasználó által definiált függvények JavaScript
Felhasználó által definiált függvények JavaScript támogatja állapot nélküli, csak számítási skaláris függvények, amelyek nem igényelnek külső kapcsolatot. hello visszatérési érték egy függvény csak a skaláris (önálló) érték lehet. Miután egy felhasználó által definiált függvény JavaScript tooa feladat, hello funkció használata bárhol hello a lekérdezésben, például a beépített skaláris függvényt.

Az alábbiakban néhány forgatókönyv, ahol hasznosak lehetnek felhasználó által definiált függvények JavaScript:
* Elemzés és kezelésére, amelyek reguláris kifejezés funkciók, például karakterláncok **Regexp_Replace()** és **Regexp_Extract()**
* Dekódolás és adatok, például bináris hexadecimális átalakítás kódolás
* JavaScript mathematic számításokat végez **matematikai** funkciók
* Például a rendezési, a csatlakozást, a Keresés és a fill tömb műveletek végrehajtása

A következőkben, amely a Stream Analytics egy JavaScript felhasználó által definiált függvény nem hajtható végre:
* Kimenő külső REST-végpontok, például végrehajtása hívás fordított IP keresési vagy referencia történő adatlekérést külső forrásból
* Hajtsa végre az egyéni esemény szerializálás vagy a bemenetek/kimenetek deszerializálási
* Egyéni összesítések létrehozása

Bár a Funkciók, például **Date.GetDate()** vagy **Math.random()** nem tiltanak hello funkciók meghatározása, az használatuk kerülendő. Ezek a funkciók **nem** visszatérési hello azonos eredményeképpen minden alkalommal, amikor keresheti őket, és hello Azure Stream Analytics szolgáltatás nem őrzi meg a napló, a függvény meghívásához, és adott vissza eredményt. Ha egy olyan függvényt ad vissza különböző eredménye a hello azonos események, ismételhetőség nem garantált a feladat újraindításakor, illetve hello Stream Analytics szolgáltatás.

## <a name="add-a-javascript-user-defined-function-in-hello-azure-portal"></a>Adja hozzá a JavaScript felhasználó által definiált függvény hello Azure-portálon
egy egyszerű JavaScript felhasználó által definiált függvény alapján egy meglévő Stream Analytics-feladat toocreate hajtsa végre ezeket a lépéseket:

1.  Hello Azure-portálon keresse meg a Stream Analytics-feladat.
2.  A **feladat TOPOLÓGIA**, válassza ki a függvényt. Megjelenik a funkciók listája üres.
3.  Válasszon egy új felhasználó által definiált függvény toocreate **Hozzáadás**.
4.  A hello **új függvény** panelen a **függvénytípus**, jelölje be **JavaScript**. Alapértelmezett függvény sablon hello szerkesztő jelenik meg.
5.  A hello **UDF alias**, adja meg **hex2Int**, és módosítsa az alábbiak szerint hello függvény végrehajtása:

    ```
    // Convert Hex value toointeger.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  Kattintson a **Mentés** gombra. A függvény funkciók hello listája jelenik meg.
7.  Jelölje be új hello **hex2Int** működik, és ellenőrizze hello függvény definícióját. Minden függvényeknek a **UDF** előtag hozzáadott toohello függvény aliasa. Túl kell*hello előtagot tartalmaz* hello függvény hívható a Stream Analytics-lekérdezés. Ebben az esetben hívható **UDF.hex2Int**.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>A JavaScript felhasználói függvényt hívni a lekérdezésben

1. Hello lekérdezésben-szerkesztőben a **feladat TOPOLÓGIA**, jelölje be **lekérdezés**.
2.  Szerkessze a lekérdezést, és majd hello felhasználói függvényt hívni, ehhez hasonló:

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  tooupload hello mintaadatfájlokat, kattintson a jobb gombbal hello feladat bemenet.
4.  tootest a select lekérdezés **teszt**.


## <a name="supported-javascript-objects"></a>Támogatott JavaScript-objektumok
Az Azure Stream Analytics JavaScript felhasználó által definiált függvények standard szintű, beépített JavaScript-objektumok támogatja. Ezek az objektumok listáját lásd: [globális objektumok](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>A Stream Analytics és a JavaScript adattípus átalakítása

Hello típusokat támogató hello Stream Analytics lekérdezési nyelv és a JavaScript különbségek vannak. Ez a táblázat közötti két hello hello átalakítás leképezéseket:

Stream Analytics | JavaScript
--- | ---
bigint | Szám (JavaScript csak jelenthet egész számok felfelé tooprecisely 2 ^ 53)
Dátum és idő | Dátum (JavaScript csak támogatja ezredmásodperc)
Dupla | Szám
típus: nvarchar(max) | Karakterlánc
rekord | Objektum
Tömb | Tömb
NULL ÉRTÉKŰ | NULL értékű


Az alábbiakban a JavaScript-Stream Analytics-átalakításhoz:


JavaScript | Stream Analytics
--- | ---
Szám | Bigint (ha hello szám kerek és a hosszú között. A MinValue és a hosszú. MaxValue; Ellenkező esetben dupla)
Dátum | Dátum és idő
Karakterlánc | típus: nvarchar(max)
Objektum | rekord
Tömb | Tömb
NULL, nem definiált | NULL ÉRTÉKŰ
Bármely más típusból (például egy függvény vagy hiba) | Nem támogatott (futásidejű hibát eredményez)

## <a name="troubleshooting"></a>Hibaelhárítás
A JavaScript futásidejű hibák végzetes minősülnek, és keresztül hello tevékenységnapló illesztett. tooretrieve hello napló hello Azure-portálon, nyissa meg tooyour feladatot, és válasszon **tevékenységnapló**.


## <a name="other-javascript-user-defined-function-patterns"></a>Más JavaScript felhasználó által definiált függvény minták

### <a name="write-nested-json-toooutput"></a>Beágyazott JSON toooutput írása
Ha a követési feldolgozási lépést, amely a Stream Analytics-feladatot, kimeneti használja bemeneti adatként, és a JSON formátumban van szükség, egy JSON-karakterlánc toooutput lehet írni. hello a következő példában meghívja a hello **JSON.stringify()** toopack hello minden név-érték pár adjon meg, és ezután írja őket a kimeneti egyetlen karakterlánc értéket működik.

**JavaScript felhasználó által definiált függvény definíciója:**

```
function main(x) {
return JSON.stringify(x);
}
```

**Mintalekérdezés:**
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a>Segítségkérés
Segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Az Azure Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)
