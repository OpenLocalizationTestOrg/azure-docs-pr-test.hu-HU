---
title: "aaaAzure kezelt alkalmazás, hozzon létre felhasználói felület definition funkciók |} Microsoft Docs"
description: "Hello funkciók toouse ismerteti az Azure által felügyelt alkalmazások felhasználói felületi meghatározások létrehozása"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 7a311a25404ccaec8c19c3ed8cd7038f6887c013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition funkciók
Ez a szakasz egy CreateUiDefinition támogatott feladatai hello aláírások.

toouse egy funkciót, a szögletes zárójelek között legyen hello nyilatkozatot. Példa:

```json
"[function()]"
```

Karakterláncok és egyéb funkciók függvény paramétereinek lehet hivatkozni, de karakterláncok kell körül, szimpla idézőjelben. Példa:

```json
"[fn1(fn2(), 'foobar')]"
```

Adott esetben egy függvény hello kimeneti tulajdonságainak hello pont operátor használatával is hivatkozik. Példa:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Hivatkozási funkciók
Ezek a funkciók hello tulajdonságok vagy egy CreateUiDefinition környezetében használt tooreference kimeneteinek lehet.

### <a name="basics"></a>alapvető tudnivalók
Hello kimeneti értékeit hello alapjai lépésben megadott elemet adja vissza.

hello alábbi példa hello-kimenet visszaadása nevű hello elem `foo` hello alapjai lépésben:

```json
"[basics('foo')]"
```

### <a name="steps"></a>lépések
Hello kimeneti értékeit hello adott lépéssel definiált elemet adja vissza. tooget hello kimeneti értékek hello alapjai lépésben elemek használata `basics()` helyette.

hello alábbi példa hello-kimenet visszaadása nevű hello elem `bar` nevű hello lépésben `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Hello alapjai lépés vagy hello kontextusban kiválasztott hello hely adja vissza.

hello alábbi példa visszaadhatja `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Karakterlánc
Ezek a függvények csak JSON karakterláncok használható.

### <a name="concat"></a>Concat
Egy vagy több karakterlánc fűzi össze.

Például, ha hello kimeneti értéke `element1` Ha `"bar"`, akkor ez a példa hello karakterláncot ad vissza, `"foobar!"`:

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a>Substring
A megadott karakterlánc hello hello részét adja vissza. hello substring hello megadott indexnél elindul, és rendelkezik a hello megadott hossza.

hello következő példa eredménye `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>cserélje le
Egy karakterlánc, mely hello összes előfordulását a hello aktuális karakterláncban megadott karakterláncot ad vissza egy másik karakterlánc helyére kerülnek.

hello következő példa eredménye `"Everything is awesome!"`:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>GUID
Karakterláncot hoz létre globálisan egyedi (GUID).

hello alábbi példa visszaadhatja `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
A konvertált karakterláncot toolowercase adja vissza.

hello következő példa eredménye `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
A konvertált karakterláncot toouppercase adja vissza.

hello következő példa eredménye `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Gyűjtemény-funkciók
Ezek a funkciók gyűjtemények, például a JSON-karakterláncok, tömbök és objektumok használható.

### <a name="contains"></a>tartalmazza
Beolvasása `true` Ha egy karakterláncot tartalmaz hello megadott karakterláncrészlet, tömböt tartalmaz hello megadott értéket, vagy az objektum tartalmazza a megadott kulcs hello.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello következő példa eredménye `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>Hossza
Egy karakterlánc, hello száma egy tömbben szereplő értékeket, vagy az objektum kulcsainak száma hello hello számú karaktert adja vissza.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello következő példa eredménye `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>üres
Beolvasása `true` Ha hello karakterlánc, a tömb vagy objektum értéke null vagy üres.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello következő példa eredménye `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>4. példa: null, és nincs definiálva
Tegyük fel `element1` van `null` vagy nincs megadva. hello következő példa eredménye `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>első
A megadott értéket ad vissza hello első karakterének hello karakterlánc; első érték a megadott tömb hello; vagy hello első kulcs és hello megadott objektum értéket.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
hello következő példa eredménye `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>utolsó
Beolvasása hello utolsó karakterének hello megadott karakterlánc, hello utolsó értéke a megadott tömb hello, vagy utolsó kulcs és a megadott objektum hello értékének hello.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello következő példa eredménye `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>hajtsa végre a megfelelő
A megadott számú hello indítás hello karakterlánc összefüggő karaktert, folytonos értékek hello indítás hello tömb a megadott számú vagy összefüggő kulcsok és értékek hello indítás hello objektum a megadott számú adja vissza.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello következő példa eredménye `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Kihagyása
A megadott számú elemet egy gyűjtemény megkerüli, és elemek fennmaradó hello adja vissza.

#### <a name="example-1-string"></a>1. példa: karakterlánc
hello következő példa eredménye `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>2. példa: tömb
Tegyük fel `element1` adja vissza `[1, 2, 3]`. hello következő példa eredménye `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>3. példa: objektum
Tegyük fel `element1` adja vissza:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
hello következő példa eredménye `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Logikai funkciók
Ezek a funkciók conditionals használható. Egyes funkciókat esetleg nem támogatja az összes JSON-adattípus.

### <a name="equals"></a>egyenlő
Beolvasása `true` Ha mindkét paraméter hello azonos adja meg, és értéket. Ez a függvény minden JSON adattípusokat támogat.

hello következő példa eredménye `true`:

```json
"[equals(0, 0)]"
```

hello következő példa eredménye `true`:

```json
"[equals('foo', 'foo')]"
```

hello következő példa eredménye `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>kevesebb
Beolvasása `true` Ha hello első paraméter szigorúan kisebb, mint a második paraméter hello. Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.

hello következő példa eredménye `true`:

```json
"[less(1, 2)]"
```

hello következő példa eredménye `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Beolvasása `true` hello első paraméter értéke kisebb vagy egyenlő, mint ha toohello második paramétere. Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.

hello következő példa eredménye `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>nagyobb
Beolvasása `true` szigorúan nagyobb, mint a második paraméter hello hello első paraméter esetén. Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.

hello következő példa eredménye `false`:

```json
"[greater(1, 2)]"
```

hello következő példa eredménye `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Beolvasása `true` Ha hello első paraméter nagyobb vagy egyenlő toohello második paramétere. Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.

hello következő példa eredménye `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>és
Beolvasása `true` ha összes hello paraméter kiértékelése túl`true`. Ez a függvény csak logikai típusú két vagy több paramétereket támogatja.

hello következő példa eredménye `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

hello következő példa eredménye `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>vagy
Beolvasása `true` Ha hello paraméterek közül legalább egy értéke túl`true`. Ez a függvény csak logikai típusú két vagy több paramétereket támogatja.

hello következő példa eredménye `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

hello következő példa eredménye `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>nem
Beolvasása `true` Ha hello paraméter értéke túl`false`. Ez a függvény csak logikai típusú paramétereket támogatja.

hello következő példa eredménye `true`:

```json
"[not(false)]"
```

hello következő példa eredménye `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>Egyesítés
Beolvasása hello hello első nem üres paraméter értékét. Ez a függvény minden JSON adattípusokat támogat.

Tegyük fel `element1` és `element2` nincs definiálva. hello következő példa eredménye `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Átalakítás funkciók
Ezek a funkciók használt tooconvert értékek JSON-adattípusok és kódolások között lehet.

### <a name="int"></a>int
Hello paraméter tooan egész számra konvertál. Ez a funkció támogatja a paraméterek száma és a karakterlánc.

hello következő példa eredménye `1`:

```json
"[int('1')]"
```

hello következő példa eredménye `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>Lebegőpontos
Hello paraméter tooa lebegőpontos alakítja. Ez a funkció támogatja a paraméterek száma és a karakterlánc.

hello következő példa eredménye `1.0`:

```json
"[float('1.0')]"
```

hello következő példa eredménye `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>Karakterlánc
Számmá alakít hello paraméter tooa karakterláncot. Ez a függvény minden JSON adattípusú paramétereket támogatja.

hello következő példa eredménye `"1"`:

```json
"[string(1)]"
```

hello következő példa eredménye `"2.9"`:

```json
"[string(2.9)]"
```

hello következő példa eredménye `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

hello következő példa eredménye `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>logikai érték
Hello paraméter tooa logikai alakítja. Ez a funkció támogatja a paraméterek száma, a karakterlánc és a logikai. A JavaScript, kivéve bármely érték hasonló tooBooleans `0` vagy `'false'` adja vissza `true`.

hello következő példa eredménye `true`:

```json
"[bool(1)]"
```

hello következő példa eredménye `false`:

```json
"[bool(0)]"
```

hello következő példa eredménye `true`:

```json
"[bool(true)]"
```

hello következő példa eredménye `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>elemzése
Konvertálja hello paraméter tooa natív típusa. Más szóval ez a funkció akkor hello inverzét `string()`. Ez a függvény csak a karakterlánc típusú paramétereket támogatja.

hello következő példa eredménye `1`:

```json
"[parse('1')]"
```

hello következő példa eredménye `true`:

```json
"[parse('true')]"
```

hello következő példa eredménye `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

hello következő példa eredménye `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Hello paraméter tooa base-64 kódolású karakterlánc kódolja. Ez a függvény csak a karakterlánc típusú paramétereket támogatja.

hello következő példa eredménye `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Dekódol hello paraméter egy base-64 kódolású karakterlánc. Ez a függvény csak a karakterlánc típusú paramétereket támogatja.

hello következő példa eredménye `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeUriComponent
Hello paraméter tooa URL-kódolású karakterlánc kódolja. Ez a függvény csak a karakterlánc típusú paramétereket támogatja.

hello következő példa eredménye `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeUriComponent
Dekódol egy URL-kódolású karakterlánc hello paramétert. Ez a függvény csak a karakterlánc típusú paramétereket támogatja.

hello következő példa eredménye `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematikai függvények
### <a name="add"></a>Hozzáadása
Két számot ad, és hello eredményt adja vissza.

hello következő példa eredménye `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Sub
Csökkenti a második szám hello hello első számot, és hello eredményt adja vissza.

hello következő példa eredménye `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>MUL számú
Két szám szorozza meg, és hello eredményt adja vissza.

hello következő példa eredménye `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>DIV
Hello első száma a hello második száma osztja, és hello eredményt adja vissza. hello eredménye mindig egy egész számot.

hello következő példa eredménye `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>MOD
Hello első száma a hello második száma osztja, és hello maradékot adja vissza.

hello következő példa eredménye `0`:

```json
"[mod(6, 3)]"
```

hello következő példa eredménye `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>perc
Beolvasása hello kis hello két szám.

hello következő példa eredménye `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>maximális
Hello két szám nagyobb értéket ad vissza hello.

hello következő példa eredménye `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>tartomány
Állít elő egy egész sorozatát hello számokat a megadott tartomány.

hello következő példa eredménye `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>VÉL
Egy véletlenszerű visszaadja a megadott tartományon belül hello egész szám. Ez a funkció nem hoz létre olyan titkosítással biztonságos véletlenszerű számból.

hello alábbi példa visszaadhatja `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Emelet
Visszaadja a hello legnagyobb egész szám kisebb vagy egyenlő, mint toohello megadott szám.

hello következő példa eredménye `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>ceil
Beolvasása hello legnagyobb egész szám nagyobb, mint vagy egyenlő toohello megadott szám.

hello következő példa eredménye `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Dátum-funkciók
### <a name="utcnow"></a>utcNow
Az aktuális dátum és idő helyi számítógép hello hello ISO 8601 formátumú karakterláncot ad vissza.

hello alábbi példa visszaadhatja `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>masodpercekHozzaadasa
Hozzáadja a megadott másodperc toohello egész számú időbélyegző.

hello következő példa eredménye `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Hozzáadja a megadott perc toohello egész számú időbélyegző.

hello következő példa eredménye `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Hozzáadja a megadott órák toohello egész számú időbélyegző.

hello következő példa eredménye `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető tooAzure erőforrás-kezelő, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).

