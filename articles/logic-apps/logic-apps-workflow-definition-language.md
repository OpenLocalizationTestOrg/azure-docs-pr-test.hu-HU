---
title: "aaaWorkflow Definition Language séma - Azure Logic Apps |} Microsoft Docs"
description: "Az Azure Logic Apps hello munkafolyamat-definíció séma alapján munkafolyamatokat meghatározása"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: f12440e9ca269a9236132deeb888dbde7fb734d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-definition-language-schema-for-azure-logic-apps"></a>Az Azure Logic Apps munkafolyamat Definition Language séma

Egy munkafolyamat-definíciót, amely végrehajtja a Logic Apps alkalmazást részeként hello-tényleges logikáját tartalmazza. Ez a definíció tartalmaz egy vagy több eseményindítókat, amelyek start hello logikai alkalmazás, illetve egy vagy több műveleteket az hello logic app tootake.  
  
## <a name="basic-workflow-definition-structure"></a>A munkafolyamat alapvető szerkezete

Íme egy munkafolyamat-definíció felépítése hello:  
  
```json
{
    "$schema": "<schema-of the-definition>",
    "contentVersion": "<version-number-of-definition>",
    "parameters": { <parameter-definitions-of-definition> },
    "triggers": [ { <definition-of-flow-triggers> } ],
    "actions": [ { <definition-of-flow-actions> } ],
    "outputs": { <output-of-definition> }
}
```
  
> [!NOTE]
> Hello [munkafolyamat felügyeleti REST API](https://docs.microsoft.com/rest/api/logic/workflows) dokumentációjában olvashat rendelkezik toocreate és logic app munkafolyamatok kezelése.
  
|Elem neve|Szükséges|Leírás|  
|------------------|--------------|-----------------|  
|$schema|Nem|Hello JSON sémafájl hello definition language hello verzióját leíró hello helyét adja meg. Ezen a helyen, ha a definíció külsőleg hivatkozik. A dokumentum hello helye: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#`|  
|contentVersion|Nem|Hello definition verzió határozza meg. Amikor telepít egy munkafolyamat hello definíciót használ, ez arra, hogy használja-e megfelelő definition hello érték toomake is használhatja.|  
|paraméterek|Nem|Megadja a paraméterek használt tooinput adatokat hello definition. Legfeljebb 50 paraméterekkel definiálható.|  
|Eseményindítók|Nem|Hello eseményindítók hello munkafolyamat kezdeményező adatait adja meg. Legfeljebb 10 eseményindítók meghatározása.|  
|műveletek|Nem|Itt adhatja meg, hello folyamat végrehajtja elvégzett műveletekről. Legfeljebb 250 műveletek definiálhatók.|  
|kimenetek|Nem|Adja meg a központilag telepített hello erőforrásokra vonatkozó információkért. Legfeljebb 10 kimenetek definiálhatók.|  
  
## <a name="parameters"></a>Paraméterek

Ebben a szakaszban használt hello paraméterek hello munkafolyamat-definíciót a központi telepítéskor határozza meg. Minden paraméter deklarálni kell az ebben a szakaszban további fejezeteiben hello definition használat előtt.  
  
hello alábbi példa bemutatja a paraméterek definícióját hello szerkezete:  

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": <default-value-of-parameter>,
        "allowedValues": [ <array-of-allowed-values> ],
        "metadata" : { "key": { "name": "value"} }
    }
}
```

|Elem neve|Szükséges|Leírás|  
|------------------|--------------|-----------------|  
|type|Igen|**Típus**: karakterlánc <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "string"}` <p> **Specifikáció**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Típus**: securestring <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "securestring"}}` <p> **Specifikáció**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Típus**: int <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "int"}}` <p> **Specifikáció**:`"parameters": {"parameter1": {"value" : 5}}` <p> **Típus**: logikai <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "bool"}}` <p> **Specifikáció**:`"parameters": {"parameter1": { "value": true }}` <p> **Típus**: tömb <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "array"}}` <p> **Specifikáció**:`"parameters": {"parameter1": { "value": [ array-of-values ]}}` <p> **Típus**: objektum <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specifikáció**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Típus**: secureobject <p> **Deklaráció**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specifikáció**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Megjegyzés:** hello `securestring` és `secureobject` típusok nem lehet megjeleníteni a `GET` műveletek. Jelszavak, kulcsok és titkos kulcsok ehhez a típushoz kell használnia.|  
|DefaultValue érték|Nem|Megadja a hello hello paraméter alapértelmezett értékét, ha nincs érték megadva hello időpontban hello erőforrás jön létre.|  
|Storageaccount_accounttype|Nem|Megengedett értékek hello paraméter tömbjét adja meg.|  
|Metaadatok|Nem|Megadja az hello paraméter, például a olvasható leírás vagy a Visual Studio vagy egyéb eszközök által használt tervezési idejű adatok további információkat.|  
  
Ez a példa bemutatja, hogyan használja a paraméter hello törzs szakaszban egy művelet.  
  
```json
"body" :
{
  "property1": "@parameters('parameter1')"
}
```

 Paraméterek kimenetek is használható.  
  
## <a name="triggers-and-actions"></a>Triggerek és műveletek  

Eseményindítók és műveletek adja meg a részvételre alkalmas hello hívások munkafolyamat végrehajtása. Ez a szakasz kapcsolatos részletekért lásd: [munkafolyamat-műveleteket és eseményindítók](logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>kimenetek  

Kimenetek információt visszaadható egy munkafolyamat futtatásához adja meg. Például ha egy adott állapot vagy -érték, amelyet az tootrack minden egyes futtatásához, telepíthet is, hogy az adatok hello kimenetek futtathatnak. hello adatok hello felügyeleti REST API-t a rendszert futtató, és a hello felügyeleti felhasználói Felületét a rendszert futtató, a hello Azure-portálon jelenik meg. A kimenetek tooother külső rendszerekből például a powerbi-jal is áramolhasson az irányítópultok létrehozásához. Kimenetek vannak *nem* toorespond tooincoming kérelmek hello szolgáltatás REST API-t használják. toorespond tooan bejövő kérelmet hello `response` művelet típusa, például:
  
```json
"outputs": {  
  "key1": {  
    "value": "value1",  
    "type" : "<type-of-value>"  
  }  
} 
```

|Elem neve|Szükséges|Leírás|  
|------------------|--------------|-----------------|  
|key1|Igen|Megadja a hello kulcsazonosító hello kimenet. Cserélje le **key1** , amelyet az toouse tooidentify hello kimeneti néven.|  
|érték|Igen|Megadja a hello kimeneti hello értékét.|  
|type|Igen|Hello érték megadott hello típusát határozza meg. Lehetséges típusú értékek a következők: <ul><li>`string`</li><li>`securestring`</li><li>`int`</li><li>`bool`</li><li>`array`</li><li>`object`</li></ul>|
  
## <a name="expressions"></a>Kifejezések  

Lehet, hogy a JSON-értékek hello definícióban szövegkonstans, vagy hello definition használatakor kiértékelt kifejezés el. Példa:  
  
```json
"name": "value"
```

 vagy  
  
```json
"name": "@parameters('password') "
```

> [!NOTE]
> Néhány kifejezés, amely nem létezik a hello végrehajtási hello elején futásidejű műveletek értékeik beolvasása sikertelen. Használhat **funkciók** toohelp az alábbi értékek némelyikét beolvasása.  
  
Kifejezések bárhol megjelenhet egy JSON karakterláncértéket, és egy másik JSON-érték mindig eredményez. Ha egy JSON-érték már meghatározott toobe kifejezés, hello kifejezés hello törzsét hello-kukac eltávolításával kivonjuk (@). Egy szöveges karakterlánc van szüksége a kezdetű @, hogy karakterlánc használatával kell megjelölni@. hello következő példák azt szemléltetik, hogyan kifejezések kiértékelése.  
  
|JSON-érték|eredménye|  
|----------------|------------|  
|"paraméterek"|Visszaadja a "parameters" Hello karaktereket.|  
|"[1] Paraméterek"|Visszaadja a "parameters [1]" Hello karaktereket.|  
|"@@"|Egy 1 tartalmazó karakterlánc ' @' adja vissza.|  
|" @"|Egy 2 tartalmazó karakterlánc ' @' adja vissza.|  
  
A *köztes karakterlánc*, akkor is megjelenhet, kifejezések adott kifejezések csomagolni vannak karakterláncok belül `@{ ... }`. Példa: <p>`"name" : "First Name: @{parameters('firstName')} Last Name: @{parameters('lastName')}"`

hello eredménye mindig egy karakterláncot, amely lehetővé teszi a funkció hasonló toohello `concat` függvény. Tegyük fel, hogy az Ön által definiált `myNumber` , `42` és `myString` , `sampleString`:  
  
|JSON-érték|eredménye|  
|----------------|------------|  
|"@parameters("sajatString")"|Beolvasása `sampleString` karakterláncként.|  
|"@{parameters('myString')}"|Beolvasása `sampleString` karakterláncként.|  
|"@parameters("SajatSzam")"|Beolvasása `42` , egy *szám*.|  
|"@{parameters('myNumber')}"|Beolvasása `42` , egy *karakterlánc*.|  
|"Válasz: @{parameters('myNumber')}"|Vissza a karakterlánc hello `Answer is: 42`.|  
|"@concat(" Válasz: ", string(parameters('myNumber')))"|Hello karakterláncot ad vissza`Answer is: 42`|  
|"Válasz: @@ {parameters('myNumber')}"|Vissza a karakterlánc hello `Answer is: @{parameters('myNumber')}`.|  
  
## <a name="operators"></a>Operátorok  

Operátorok hello karakterek kifejezések vagy funkciók belül is használhatja. 
  
|Operátor|Leírás|  
|--------------|-----------------|  
|.|hello pont operátor lehetővé teszi egy objektum tooreference tulajdonságai|  
|?|hello kérdőjel operátor null hivatkozhat egy objektum futásidejű hiba nélkül teszi lehetővé. Például használhatja a kifejezés toohandle null kimenetek indítható el: <p>`@coalesce(trigger().outputs?.body?.property1, 'my default value')`|  
|'|hello szimpla idézőjel hello csak úgy toowrap szövegkonstansok. Kifejezések belül idézőjelek nem használható, mert ez az absztrakt hello JSON ajánlat hello teljes kifejezés nagyságúra ütközik.|  
|[]|hello szögletes zárójelbe használt tooget adott index a tömb közötti érték lehet. Például, ha egy műveletet, amelyet továbbküld `range(0,10)`toohello a `forEach` funkció, a függvény tooget cikkekkel tömbök használhatja:  <p>`myArray[item()]`|  
  
## <a name="functions"></a>Functions  

Ön is meghívhatja kifejezések függvényeket. hello következő táblázatban hello funkciók használható kifejezésben.  
  
|kifejezés|Értékelés|  
|----------------|----------------|  
|"@function("Hello")"|Hívások hello hello első paraméterként hello hello karakterlánckonstanssal Hello definíció függvény tagja.|  
|"@function("Ritkán használt adatok!")"|Meghívja a hello függvény tag hello definíció hello literál karakterlánc a "ritkán!" hello első paraméterként|  
|"@function.prop1 ()"|Beolvasása hello hello hello tul1 tulajdonságának értéke `myfunction` hello definition tagja.|  
|"@function("Hello") .prop1"|Hívások hello-definíció hello literál karakterlánc "Hello" függvény tagja hello tulajdonságként hello első paramétert és értéket ad vissza hello tul1 hello objektum.|  
|"@function(parameters('Hello'))"|Hello Hello paraméter kiértékeli, és továbbadja hello érték toofunction|  
  
### <a name="referencing-functions"></a>Hivatkozási funkciók  

Ezen funkciók tooreference kimeneteinek hello logikai alkalmazás vagy hello logikai alkalmazás létrehozása az átadott értékek lévő egyéb műveletek is használhatja. Például hivatkozhat hello adatok egy lépésben toouse a azt egy másik.  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|paraméterek|Egy meghatározott hello definition paraméter értékét adja vissza. <p>`parameters('password')` <p> **Számú paraméter**: 1 <p> **Név**: paraméter <p> **Leírás**: kötelező. hello paraméter értékekhez hello neve.|  
|A művelet|Lehetővé teszi, hogy egy kifejezés tooderive más JSON-név és érték párok vagy hello kimeneti hello aktuális futásidejű művelet az értékét. a következő példa hello a propertyPath által képviselt hello tulajdonság megadása nem kötelező. Ha nincs megadva a propertyPath, hello hivatkozás az toohello teljes műveleti objektum. Ez a funkció csak akkor használható belül tegye-csak egy művelet feltételeket. <p>`action().outputs.body.propertyPath`|  
|műveletek|Lehetővé teszi, hogy egy kifejezés tooderive más JSON-név és érték párok vagy hello kimeneti hello futásidejű művelet az értékét. Ezek a kifejezések explicit módon deklarálja, hogy egy-egy művelettel függ-e egy másik művelet. a következő példa hello a propertyPath által képviselt hello tulajdonság megadása nem kötelező. Ha nincs megadva a propertyPath, hello hivatkozás az toohello teljes műveleti objektum. Is használhatja, vagy ez az elem vagy hello feltételek elem toospecify függőségek, de nem kell mindkét toouse hello az azonos függő erőforrást. <p>`actions('myAction').outputs.body.propertyPath` <p> **Számú paraméter**: 1 <p> **Név**: a művelet neve <p> **Leírás**: kötelező. hello művelet értékekhez hello neve. <p> A műveleti objektum hello elérhető tulajdonságok a következők: <ul><li>`name`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Lásd: hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850646) meg azokat a tulajdonságokat.|
|Eseményindító|Lehetővé teszi, hogy egy kifejezés tooderive más JSON-név és érték párok vagy hello kimeneti hello futásidejű eseményindító az értékét. a következő példa hello a propertyPath által képviselt hello tulajdonság megadása nem kötelező. Ha nincs megadva a propertyPath, hello hivatkozás az toohello teljes eseményindító objektumhoz. <p>`trigger().outputs.body.propertyPath` <p>A trigger ráfordítások használni, amikor a hello függvény hello előző végrehajtás kimenetének hello adja vissza. Azonban egy eseményindító-feltétel belül használatakor hello `trigger` függvény hello aktuális végrehajtás kimenetének hello adja vissza. <p> Hello eseményindító objektumhoz rendelkezésre álló tulajdonságok a következők: <ul><li>`name`</li><li>`scheduledTime`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Lásd: hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850644) meg azokat a tulajdonságokat.|
|actionOutputs|Ez a funkció csak a rövid szintaxist`actions('actionName').outputs` <p> **Számú paraméter**: 1 <p> **Név**: a művelet neve <p> **Leírás**: kötelező. hello művelet értékekhez hello neve.|  
|actionBody és törzsét.|Ezek a funkciók a rövid szintaxist`actions('actionName').outputs.body` <p> **Számú paraméter**: 1 <p> **Név**: a művelet neve <p> **Leírás**: kötelező. hello művelet értékekhez hello neve.|  
|triggerOutputs|Ez a funkció csak a rövid szintaxist`trigger().outputs`|  
|triggerBody|Ez a funkció csak a rövid szintaxist`trigger().outputs.body`|  
|Elem|Ismétlődő művelet használni, amikor ezt a funkciót, hogy megtalálható-e közelítés hello művelet hello tömb hello elemet adja vissza. Például ha egy futtató művelet egyes elemekhez tartozó üzenetek tömbje, használhatja ezt a szintaxist: <p>`"input1" : "@item().subject"`| 
  
### <a name="collection-functions"></a>Gyűjtemény-funkciók  

Ezeket a funkciókat hatnak a gyűjteményeket, és általában a tooArrays, karakterláncok és egyes esetekben szótárak érvényesek.  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|tartalmazza|Igaz értéket ad eredményül, ha a szótár tartalmaz-e, kulcslista értéket tartalmaz, vagy karakterlánc karakterláncrészletet tartalmazza. Például, a függvény `true`: <p>`contains('abacaba','aca')` <p> **Számú paraméter**: 1 <p> **Név**: gyűjteményen belül <p> **Leírás**: kötelező. hello gyűjtemény toosearch belül. <p> **Számú paraméter**: 2. régiója <p> **Név**: objektum keresése <p> **Leírás**: kötelező. objektum toofind belül hello hello **gyűjteményben**.|  
|Hossza|Beolvasása hello tömb vagy karakterlánc lévő elemek számánál. Például, a függvény `3`:  <p>`length('abc')` <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény <p> **Leírás**: kötelező. hello gyűjteményt tooget hello karakter hosszú.|  
|üres|Igaz értéket ad eredményül, ha az objektum, a tömb vagy karakterlánc üres. Például, a függvény `true`: <p>`empty('')` <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény <p> **Leírás**: kötelező. hello gyűjtemény toocheck, ha üres.|  
|metszetének|Visszaadja egy egyetlen tömb vagy objektum, amely a tömb, vagy az átadott objektumok közötti közös elemmel rendelkezik. Például, a függvény `[1, 2]`: <p>`intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])` <p>hello függvény paramétereinek hello lehet egy objektum vagy egy lemezkészlet tömbök (nem keverékével is). Ha hello két objektumnak azonos nevet, hello végső objektum hello utolsó objektum ezen a néven jelenik meg. <p> **Számú paraméter**: 1...*n* <p> **Név**: gyűjtemény*n* <p> **Leírás**: kötelező. hello gyűjtemények tooevaluate. Az objektum tooappear hello eredményben az átadott összes gyűjteményt kell lennie.|  
|a UNION|Egyetlen tömb vagy objektum tömb vagy objektum átadott toothis függvény összes hello elemekkel ad vissza. Például, a függvény `[1, 2, 3, 10, 101]`: <p>`union([1, 2, 3], [101, 2, 1, 10])` <p>hello függvény paramétereinek hello lehet egy objektum vagy egy lemezkészlet tömbök (nem ezek keveréke). Ha hello végső kimenetet a hello neve megegyezik a két objektum, hello végső objektum utolsó objektum hello ezen a néven jelenik meg. <p> **Számú paraméter**: 1...*n* <p> **Név**: gyűjtemény*n* <p> **Leírás**: kötelező. hello gyűjtemények tooevaluate. Hello gyűjtemények bármelyikében objektum is hello eredmény jelenik meg.|  
|első|Beolvasása hello hello tömb első eleme, vagy az átadott karakterlánc. Például, a függvény `0`: <p>`first([0,2,3])` <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény <p> **Leírás**: kötelező. hello gyűjtemény tootake hello első objektumot.|  
|utolsó|Beolvasása hello hello tömb utolsó eleme, vagy az átadott karakterlánc. Például, a függvény `3`: <p>`last('0123')` <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény <p> **Leírás**: kötelező. hello gyűjtemény tootake hello utolsó objektumot.|  
|hajtsa végre a megfelelő|Először hello értéket ad vissza **száma** átadott hello tömb vagy karakterlánc elemeit. Például, a függvény `[1, 2]`:  <p>`take([1, 2, 3, 4], 2)` <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény <p> **Leírás**: kötelező. Ha tootake először hello hello gyűjteményben **száma** objektumok. <p> **Számú paraméter**: 2. régiója <p> **Név**: száma <p> **Leírás**: kötelező. a hello objektumok tootake száma hello **gyűjtemény**. Pozitív egész számnak kell lennie.|  
|Kihagyása|Beolvasása hello indextől kezdődő hello tömb elemeinek **száma**. Például, a függvény `[3, 4]`: <p>`skip([1, 2 ,3 ,4], 2)` <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény <p> **Leírás**: kötelező. először hello gyűjtemény tooskip hello **száma** a következő helyről objektumokat. <p> **Számú paraméter**: 2. régiója <p> **Név**: száma <p> **Leírás**: kötelező. hello előre hozása az objektumok tooremove száma hello **gyűjtemény**. Pozitív egész számnak kell lennie.|  
|csatlakozás|Az egyes elemek tömb tartományhoz elválasztó, például ez visszaadja a karakterláncot ad vissza, `"1,2,3,4"`:<br /><br /> `join([1, 2, 3, 4], ',')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: gyűjtemény<br /><br /> **Leírás**: kötelező. hello gyűjtemény toojoin szereplő elemeket.<br /><br /> **Számú paraméter**: 2. régiója<br /><br /> **Név**: elválasztó<br /><br /> **Leírás**: kötelező. hello karakterlánc toodelimit elemet.|  
  
### <a name="string-functions"></a>Karakterlánc

a következő funkciók csak hello toostrings alkalmazni. Néhány gyűjtemény funkciókat is használhatja, ha a karakterláncok.  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|Concat|Tetszőleges számú karakterláncok együtt egyesíti. Például, ha a paraméter 1 `p1`, a függvény `somevalue-p1-somevalue`: <p>`concat('somevalue-',parameters('parameter1'),'-somevalue')` <p> **Számú paraméter**: 1...*n* <p> **Név**: karakterlánc*n* <p> **Leírás**: kötelező. hello karakterláncok toocombine egyetlen karakterlánccá egyesít.|  
|Substring|Egy karakterlánc karakterből álló alkészletet ad vissza. Például, a függvény `abc`: <p>`substring('somevalue-abc-somevalue',10,3)` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, mely hello a substring használatban van. <p> **Számú paraméter**: 2. régiója <p> **Név**: kezdőindex <p> **Leírás**: kötelező. ahol a hello substring az 1. paraméter kezdődik hello indexe. <p> **Számú paraméter**: 3 <p> **Név**: hossza <p> **Leírás**: kötelező. hello substring hello hossza.|  
|cserélje le|Lecseréli a karakterlánc egy adott karakterláncot. Például, a függvény `hello new string`: <p>`replace('hello old string', 'old', 'new')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, amely a 2. paraméter keresendő és frissül az paraméterrel 3, 2. paraméter 1 paraméter található. <p> **Számú paraméter**: 2. régiója <p> **Név**: régi karakterlánc <p> **Leírás**: kötelező. hello karakterlánc tooreplace 3, ha van egyezés paraméter 1 paraméterrel <p> **Számú paraméter**: 3 <p> **Név**: új karakterlánc <p> **Leírás**: kötelező. Hello, amely használt tooreplace hello karakterlánc paraméter 2 karakterlánc, amikor a program egyezést talál, az 1. paraméter.|  
|GUID|Ez a függvény karakterláncot hoz létre globálisan egyedi (GUID). Ez a funkció például ennek a GUID Azonosítónak hozhat létre:`c2ecc88d-88c8-4096-912c-d6f2e2b138ce` <p>`guid()` <p> **Számú paraméter**: 1 <p> **Név**: formátum <p> **Leírás**: nem kötelező. Egyetlen formátummegadó jelző [hogyan tooformat hello érték a GUID](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). hello formátum paraméter "N", "D", "B", "P" vagy "X" lehet. Ha formátum nem áll rendelkezésre, "D" szolgál.|  
|toLower|Egy karakterlánc toolowercase alakítja. Például, a függvény `two by two is four`: <p>`toLower('Two by Two is Four')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc tooconvert toolower kis-és nagybetűhasználatot. Ha egy karakter hello karakterláncban nincs nagybetűket egyenértékűként, hello karakter megtalálható változatlan hello karakterláncot adott vissza.|  
|toUpper|Egy karakterlánc toouppercase alakítja. Például, a függvény `TWO BY TWO IS FOUR`: <p>`toUpper('Two by Two is Four')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc tooconvert tooupper kis-és nagybetűhasználatot. Ha egy karakter hello karakterláncban nem rendelkezik nagybetűs egyenértékű, hello karakter megtalálható változatlan hello karakterláncot adott vissza.|  
|indexof|Egy karakterlánc esetben belüli értéket hello indexe insensitively található. Például, a függvény `7`: <p>`indexof('hello, world.', 'world')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, amely tartalmazhat hello érték. <p> **Számú paraméter**: 2. régiója <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello érték toosearch hello indexe.|  
|lastindexof|Egy karakterlánc esetben belüli értéket utolsó indexe hello insensitively található. Például, a függvény `3`: <p>`lastindexof('foofoo', 'foo')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, amely tartalmazhat hello érték. <p> **Számú paraméter**: 2. régiója <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello érték toosearch hello indexe.|  
|startswith elemnek|Ellenőrzi, hogy hello karakterlánc kezdődik-e érték eset insensitively. Például, a függvény `true`: <p>`startswith('hello, world', 'hello')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, amely tartalmazhat hello érték. <p> **Számú paraméter**: 2. régiója <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello hello értékkarakterlánc kezdődik.|  
|megadott módon végződő|Ellenőrzi, hogy ha hello karakterlánc végződik érték eset insensitively. Például, a függvény `true`: <p>`endswith('hello, world', 'world')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, amely tartalmazhat hello érték. <p> **Számú paraméter**: 2. régiója <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello érték hello karakterlánc végén is.|  
|felosztás|Felosztja egy elválasztó hello karakterláncot. Például, a függvény `["a", "b", "c"]`: <p>`split('a;b;c',';')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc, amely van szétosztva. <p> **Számú paraméter**: 2. régiója <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello elválasztó.|  
  
### <a name="logical-functions"></a>Logikai funkciók  

Ezek a funkciók feltételek belül lehetnek hasznosak, és használt tooevaluate logika bármilyen típusú.  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|egyenlő|Igaz értéket ad eredményül, ha két érték egyenlő. Például ha parameter1 someValue, a függvény `true`: <p>`equals(parameters('parameter1'), 'someValue')` <p> **Számú paraméter**: 1 <p> **Név**: 1 objektum <p> **Leírás**: kötelező. objektum toocompare túl hello**objektum 2**. <p> **Számú paraméter**: 2. régiója <p> **Név**: 2 objektum <p> **Leírás**: kötelező. objektum toocompare túl hello**objektum 1**.|  
|kevesebb|Igaz értéket ad eredményül, ha hello első argumentum nem kisebb, mint hello második. Vegye figyelembe, hogy értékek csak lehetnek típusa egész szám, lebegőpontos vagy karakterlánc. Például, a függvény `true`: <p>`less(10,100)` <p> **Számú paraméter**: 1 <p> **Név**: 1 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha kevesebb mint **objektum 2**. <p> **Számú paraméter**: 2. régiója <p> **Név**: 2 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha nagyobb, mint **objektum 1**.|  
|lessOrEquals|Igaz értéket ad eredményül, ha hello első argumentum nem kisebb, mint vagy egyenlő második toohello. Vegye figyelembe, hogy értékek csak lehetnek típusa egész szám, lebegőpontos vagy karakterlánc. Például, a függvény `true`: <p>`lessOrEquals(10,10)` <p> **Számú paraméter**: 1 <p> **Név**: 1 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha kisebb vagy egyenlő túl**objektum 2**. <p> **Számú paraméter**: 2. régiója <p> **Név**: 2 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha nagyobb vagy egyenlő túl**objektum 1**.|  
|nagyobb|Igaz értéket ad eredményül, ha hello első argumentum értéke nagyobb, mint hello második. Vegye figyelembe, hogy értékek csak lehetnek típusa egész szám, lebegőpontos vagy karakterlánc. Például, a függvény `false`:  <p>`greater(10,10)` <p> **Számú paraméter**: 1 <p> **Név**: 1 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha nagyobb, mint **objektum 2**. <p> **Számú paraméter**: 2. régiója <p> **Név**: 2 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha kevesebb mint **objektum 1**.|  
|greaterOrEquals|Igaz értéket ad vissza, ha hello első argumentum értéke nagyobb vagy egyenlő toohello második. Vegye figyelembe, hogy értékek csak lehetnek típusa egész szám, lebegőpontos vagy karakterlánc. Például, a függvény `false`: <p>`greaterOrEquals(10,100)` <p> **Számú paraméter**: 1 <p> **Név**: 1 objektum <p> **Leírás**: kötelező. hello objektum toocheck, ha nagyobb vagy egyenlő túl**objektum 2**. <p> **Számú paraméter**: 2. régiója <p> **Név**: 2 objektum <p> **Leírás**: kötelező. objektum toocheck hello kisebb vagy egyenlő, mint ha túl**objektum 1**.|  
|és|Igaz értéket ad eredményül, ha mindkét paraméter értéke igaz. Mindkét argumentumot kell toobe logikai értékek. Például, a függvény `false`: <p>`and(greater(1,10),equals(0,0))` <p> **Számú paraméter**: 1 <p> **Név**: logikai 1 <p> **Leírás**: kötelező. első argumentum kell lennie a hello `true`. <p> **Számú paraméter**: 2. régiója <p> **Név**: logikai 2 <p> **Leírás**: kötelező. hello második argumentumot kell `true`.|  
|vagy|Igaz értéket ad eredményül, ha bármelyik paraméter értéke true. Mindkét argumentumot kell toobe logikai értékek. Például, a függvény `true`: <p>`or(greater(1,10),equals(0,0))` <p> **Számú paraméter**: 1 <p> **Név**: logikai 1 <p> **Leírás**: kötelező. első argumentum, amelyek esetleg hello `true`. <p> **Számú paraméter**: 2. régiója <p> **Név**: logikai 2 <p> **Leírás**: kötelező. hello második argumentum lehet `true`.|  
|nem|Igaz értéket ad eredményül, ha hello paraméterek `false`. Mindkét argumentumot kell toobe logikai értékek. Például, a függvény `true`: <p>`not(contains('200 Success','Fail'))` <p> **Számú paraméter**: 1 <p> **Név**: logikai <p> **Leírás**: IGAZ értéket ad vissza, ha hello paraméterek `false`. Mindkét argumentumot kell toobe logikai értékek. Ez a függvény visszaadja `true`:`not(contains('200 Success','Fail'))`|  
|Ha|E eredményezett hello kifejezés alapján egy megadott értéket adja vissza `true` vagy `false`.  Például, a függvény `"yes"`: <p>`if(equals(1, 1), 'yes', 'no')` <p> **Számú paraméter**: 1 <p> **Név**: kifejezés <p> **Leírás**: kötelező. Határozza meg, hogy mely értékkifejezésnek hello logikai értéket kell visszaadnia. <p> **Számú paraméter**: 2. régiója <p> **Név**: igaz <p> **Leírás**: kötelező. hello érték tooreturn, ha hello kifejezés `true`. <p> **Számú paraméter**: 3 <p> **Név**: hamis <p> **Leírás**: kötelező. hello érték tooreturn, ha hello kifejezés `false`.|  
  
### <a name="conversion-functions"></a>Átalakítás funkciók  

Ezek a funkciók használt tooconvert egyes hello nyelvi hello natív típusa között:  
  
- Karakterlánc  
  
- egész szám  
  
- Lebegőpontos  
  
- Logikai érték  
  
- Tömbök  
  
- szótárak  

-   Űrlap  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|int|Az átalakítás hello paraméter tooan egész szám. Ez a funkció például 100 karakterlánc helyett egy számot adja vissza: <p>`int('100')` <p> **Számú paraméter**: 1 <p> **Név**: érték <p> **Leírás**: kötelező. hello érték, amely a konvertált tooan egész szám lehet.|  
|Karakterlánc|Az átalakítás hello paraméter tooa karakterlánc. Például, a függvény `'10'`: <p>`string(10)` <p>Egy objektum tooa karakterlánc is alakíthatja. Például, ha hello `myPar` paraméter egy tulajdonság az objektum `abc : xyz`, akkor ez a függvény visszaadja `{"abc" : "xyz"}`: <p>`string(parameters('myPar'))` <p> **Számú paraméter**: 1 <p> **Név**: érték <p> **Leírás**: kötelező. hello érték, amely konvertált tooa karakterlánc.|  
|JSON-ban|Átalakítás hello tooa JSON típusú paraméterérték, és az ellentétes hello `string()`. Például, a függvény `[1,2,3]` tömb, nem pedig egy karakterlánc: <p>`json('[1,2,3]')` <p>Hasonlóképpen konvertálhatja tooan karakterlánc-objektum. Például, a függvény `{ "abc" : "xyz" }`: <p>`json('{"abc" : "xyz"}')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterláncértéket, amely konvertált tooa natív típusa. <p>Hello `json()` függvény túl a bemeneti XML használatát támogatja. Például hello paraméter értéke: <p>`<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>` <p>az átalakított toothis JSON: <p>`{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|Lebegőpontos|Az átalakítás hello paraméter argumentum tooa lebegőpontos szám. Például, a függvény `10.333`: <p>`float('10.333')` <p> **Számú paraméter**: 1 <p> **Név**: érték <p> **Leírás**: kötelező. hello érték, amely konvertált tooa lebegőpontos szám.|  
|logikai érték|Az átalakítás hello paraméter tooa logikai érték. Például, a függvény `false`: <p>`bool(0)` <p> **Számú paraméter**: 1 <p> **Név**: érték <p> **Leírás**: kötelező. hello értéket, amely konvertált tooa logikai.|  
|Egyesítés|Hello argumentumként átadott hello első nem null objektumot ad vissza. **Megjegyzés:**: üres karakterlánc értéke nem null. Például, ha nincsenek megadva paraméterek 1 és 2, a függvény `fallback`:  <p>`coalesce(parameters('parameter1'), parameters('parameter2') ,'fallback')` <p> **Számú paraméter**: 1...*n* <p> **Név**: objektum*n* <p> **Leírás**: kötelező. hello objektumok toocheck a NULL értékű.|  
|a Base64|Értéket ad vissza a bemeneti karakterlánc hello base64 ábrázolását hello. Például, a függvény `c29tZSBzdHJpbmc=`: <p>`base64('some string')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc-1 <p> **Leírás**: kötelező. hello karakterlánc tooencode base64 helyére.|  
|base64ToBinary|A base64 kódolású karakterlánc bináris alakot adja vissza. Például, a függvény bináris megjelenítése hello `some string`: <p>`base64ToBinary('c29tZSBzdHJpbmc=')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello base64 kódolású karakterlánc.|  
|base64ToString|A based64 kódolású karakterlánc karakterlánc alakot adja vissza. Például, a függvény `some string`: <p>`base64ToString('c29tZSBzdHJpbmc=')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello base64 kódolású karakterlánc.|  
|Bináris|Egy bináris értéket képviselő alakot adja vissza.  Például, a függvény a bináris megjelenítése `some string`: <p>`binary('some string')` <p> **Számú paraméter**: 1 <p> **Név**: érték <p> **Leírás**: kötelező. hello értéket, amely konvertált toobinary.|  
|dataUriToBinary|Egy adat-URI azonosító egy bináris alakot adja vissza. Például, a függvény bináris megjelenítése hello `some string`: <p>`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello adatok URI tooconvert toobinary ábrázolását.|  
|dataUriToString|Egy adat-URI azonosító egy karakterlánc alakot adja vissza. Például, a függvény `some string`: <p>`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc<p> **Leírás**: kötelező. hello adatok URI tooconvert tooString ábrázolását.|  
|dataUri|Egy adat-URI azonosító a értékét adja vissza. Például, a függvény az adat-URI azonosító `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=`: <p>`dataUri('some string')` <p> **Számú paraméter**: 1<p> **Név**: érték<p> **Leírás**: kötelező. hello érték tooconvert toodata URI.|  
|decodeBase64|Egy bemeneti based64 karakterlánc karakterlánc alakot adja vissza. Például, a függvény `some string`: <p>`decodeBase64('c29tZSBzdHJpbmc=')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: egy bemeneti based64 karakterlánc karakterlánc alakot adja vissza.|  
|encodeUriComponent|URL-Kilépés hello karakterlánc, amely kerül átadásra. Például, a függvény `You+Are%3ACool%2FAwesome`: <p>`encodeUriComponent('You Are:Cool/Awesome')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc URL nem biztonságos tooescape karaktereit.|  
|decodeUriComponent|Az átadott UN-URL-Kilépés hello karakterlánc. Például, a függvény `You Are:Cool/Awesome`: <p>`encodeUriComponent('You+Are%3ACool%2FAwesome')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc toodecode URL nem biztonságos hello karaktereit.|  
|decodeDataUri|Egy bemeneti adatokat URI karakterlánc bináris alakot adja vissza. Például, a függvény bináris megjelenítése hello `some string`: <p>`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello dataURI toodecode azokat a bináris megjelenítése.|  
|uriComponent|Egy URI-t adja vissza egy értékének kódolva. Például, a függvény `You+Are%3ACool%2FAwesome`: <p>`uriComponent('You Are:Cool/Awesome')` <p> **Számú paraméter**: 1<p> **Név**: karakterlánc <p> **Leírás**: kötelező. hello karakterlánc toobe URI-kódolt.|  
|uriComponentToBinary|Visszaadja a bináris megjelenítése egy URI-kódolású karakterlánc. Például, a függvény a bináris megjelenítése `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Számú paraméter**: 1 <p> **Név**: karakterlánc<p> **Leírás**: kötelező. hello URI-kódolású karakterlánc.|  
|uriComponentToString|Visszaadja a URI karakterlánc-ábrázolása kódolású karakterlánc. Például, a függvény `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Számú paraméter**: 1<p> **Név**: karakterlánc<p> **Leírás**: kötelező. hello URI-kódolású karakterlánc.|  
|xml|XML-ábrázolása hello érték visszaadása. Például ezt a funkciót XML-kódot eredményezzen tartalom által képviselt `'\<name>Alan\</name>'`: <p>`xml('\<name>Alan\</name>')` <p>Hello `xml()` függvény túl a bemeneti JSON-objektum használatát támogatja. Például hello paraméter `{ "abc": "xyz" }` a konvertált tooXML tartalom:`\<abc>xyz\</abc>` <p> **Számú paraméter**: 1<p> **Név**: érték<p> **Leírás**: kötelező. hello érték tooconvert tooXML.|  
|XPath|Egy érték, amely hello xpath kifejezés értéke hello xpath kifejezésnek megfelelő XML-csomópontnak tömbjét adja vissza. <p> **1. példa** <p>Hello paraméter értékének feltételezik `p1` az XML-kód egy karakterlánc-ábrázolása: <p>`<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>` <p>Ezt a kódot:`xpath(xml(parameters('p1'), '/lab/robot/name')` <p>adja vissza <p>`[ <name>R1</name>, <name>R2</name> ]` <p>Amikor ezt a kódot: <p>`xpath(xml(parameters('p1'), ' sum(/lab/robot/parts)')` <p>adja vissza <p>`13` <p> <p> **2. példa** <p>A következő XML-tartalmat adott hello: <p>`<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>` <p>Ezt a kódot:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')` <p>vagy ezt a kódot: <p>`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')` <p>adja vissza <p>`<Location xmlns="http://abc.com">xyz</Location>` <p>És ezzel a kóddal:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')` <p>adja vissza <p>``xyz`` <p> **Számú paraméter**: 1 <p> **Név**: Xml <p> **Leírás**: kötelező. hello XML mely tooevaluate a hello XPath kifejezés. <p> **Számú paraméter**: 2. régiója <p> **Név**: XPath <p> **Leírás**: kötelező. hello XPath kifejezés tooevaluate.|  
|A tömb|Az átalakítás hello tooan paramétertömb. Például, a függvény `["abc"]`: <p>`array('abc')` <p> **Számú paraméter**: 1 <p> **Név**: érték <p> **Leírás**: kötelező. hello érték, amely a konvertált tooan tömb.|
|createArray|Létrehoz egy tömb hello paraméterek. Például, a függvény `["a", "c"]`: <p>`createArray('a', 'c')` <p> **Számú paraméter**: 1...*n* <p> **Név**: bármely*n* <p> **Leírás**: kötelező. hello értékek toocombine egy tömbbe.|
|triggerFormDataValue|Űrlap-adatok, vagy a képernyő-kódolású eseményindító kimenetét hello kulcsnév megfelelő egyetlen értéket ad vissza.  Ha egyszerre több megfelelő azt fogja hiba.  Például hello következő visszaadható `bar`:`triggerFormDataValue('foo')`<br /><br />**Számú paraméter**: 1<br /><br />**Név**: kulcs neve<br /><br />**Leírás**: kötelező. hello hello űrlap adatok érték tooreturn kulcs neve.|
|triggerFormDataMultiValues|Űrlap-adatok, vagy a képernyő-kódolású eseményindító kimenetét hello kulcsnév egyező tömbjét adja vissza.  Például hello következő visszaadható `["bar"]`:`triggerFormDataValue('foo')`<br /><br />**Számú paraméter**: 1<br /><br />**Név**: kulcs neve<br /><br />**Leírás**: kötelező. hello hello űrlap adatok értékek tooreturn kulcs neve.|
|triggerMultipartBody|Értéket ad vissza egy része egy több részből álló kimenet hello eseményindító törzse hello.<br /><br />**Számú paraméter**: 1<br /><br />**Név**: Index<br /><br />**Leírás**: kötelező. hello rész tooretrieve hello indexe.|
|formDataValue|Az űrlap-adatok vagy a képernyő-kódolású műveleti kimenet hello kulcsnév megfelelő egyetlen értéket ad vissza.  Ha egyszerre több megfelelő azt fogja hiba.  Például hello következő visszaadható `bar`:`formDataValue('someAction', 'foo')`<br /><br />**Számú paraméter**: 1<br /><br />**Név**: a művelet neve<br /><br />**Leírás**: kötelező. űrlap-adatok vagy a képernyő-kódolású válasz hello művelet hello neve.<br /><br />**Számú paraméter**: 2. régiója<br /><br />**Név**: kulcs neve<br /><br />**Leírás**: kötelező. hello hello űrlap adatok érték tooreturn kulcs neve.|
|formDataMultiValues|Az űrlap-adatok vagy a képernyő-kódolású műveleti kimenet hello kulcsnév egyező tömbjét adja vissza.  Például hello következő visszaadható `["bar"]`:`formDataMultiValues('someAction', 'foo')`<br /><br />**Számú paraméter**: 1<br /><br />**Név**: a művelet neve<br /><br />**Leírás**: kötelező. űrlap-adatok vagy a képernyő-kódolású válasz hello művelet hello neve.<br /><br />**Számú paraméter**: 2. régiója<br /><br />**Név**: kulcs neve<br /><br />**Leírás**: kötelező. hello hello űrlap adatok értékek tooreturn kulcs neve.|
|multipartBody|Értéket ad vissza egy része egy több részből álló kimeneti művelet törzse hello.<br /><br />**Számú paraméter**: 1<br /><br />**Név**: a művelet neve<br /><br />**Leírás**: kötelező. egy több részből álló válasz hello művelet hello neve.<br /><br />**Számú paraméter**: 2. régiója<br /><br />**Név**: Index<br /><br />**Leírás**: kötelező. hello rész tooretrieve hello indexe.|

### <a name="math-functions"></a>Matematikai függvények  

Ezeket a funkciókat használhatja bármelyik típusú számot: **egész számok** és **képest az előtérben**.  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|Hozzáadása|Hello eredményt adjanak hozzá hello két számot adja vissza. Például, a függvény `20.333`: <p>`add(10,10.333)` <p> **Számú paraméter**: 1 <p> **Név**: Summand 1 <p> **Leírás**: kötelező. szám tooadd túl hello**Summand 2**. <p> **Számú paraméter**: 2. régiója <p> **Név**: Summand 2 <p> **Leírás**: kötelező. szám tooadd túl hello**Summand 1**.|  
|Sub|A két szám különbsége hello eredményt adja vissza. Például, a függvény `-0.333`: <p>`sub(10,10.333)` <p> **Számú paraméter**: 1 <p> **Név**: Minuend <p> **Leírás**: kötelező. hello szám, amely **Subtrahend** törlődik. <p> **Számú paraméter**: 2. régiója <p> **Név**: Subtrahend <p> **Leírás**: kötelező. a hello számú tooremove hello **Minuend**.|  
|MUL számú|A két szám hello szorzásával hello eredményt adja vissza. Például, a függvény `103.33`: <p>`mul(10,10.333)` <p> **Számú paraméter**: 1 <p> **Név**: Multiplicand 1 <p> **Leírás**: kötelező. hello számú toomultiply **Multiplicand 2** együtt. <p> **Számú paraméter**: 2. régiója <p> **Név**: Multiplicand 2 <p> **Leírás**: kötelező. hello számú toomultiply **Multiplicand 1** együtt.|  
|DIV|Két szám hello osztásával kapott hello eredményt adja vissza. Például, a függvény `1.0333`: <p>`div(10.333,10)` <p> **Számú paraméter**: 1 <p> **Név**: osztandó <p> **Leírás**: kötelező. szám toodivide hello által hello **osztó**. <p> **Számú paraméter**: 2. régiója <p> **Név**: osztó <p> **Leírás**: kötelező. hello számú toodivide hello **osztandó** által.|  
|MOD|Hello két szám (modulo) felosztásával után hello maradékot adja vissza. Például, a függvény `2`: <p>`mod(10,4)` <p> **Számú paraméter**: 1 <p> **Név**: osztandó <p> **Leírás**: kötelező. szám toodivide hello által hello **osztó**. <p> **Számú paraméter**: 2. régiója <p> **Név**: osztó <p> **Leírás**: kötelező. hello számú toodivide hello **osztandó** által. Hello felosztás után ezeket a hello fennmaradó használatban van.|  
|perc|Ez a függvény hívásakor két különböző mintát van. <p>Itt `min` tömb lehet, és hello függvény `0`: <p>`min([0,1,2])` <p>Azt is megteheti, ez a funkció is igénybe vehet, értékeket, és visszaadja a vesszővel tagolt listáját `0`: <p>`min(0,1,2)` <p> **Megjegyzés:**: összes érték számokból kell állniuk, így ha hello paraméter egy tömb, hello tömb tooonly rendelkezik-e a számokat. <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény vagy érték <p> **Leírás**: kötelező. Vagy tömbje értékek toofind hello minimális értéket, vagy olyan hello első értékét. <p> **Számú paraméter**: 2...*n* <p> **Név**: érték*n* <p> **Leírás**: nem kötelező. Ha hello első paramétert egy érték, akkor további értékeket is továbbítja, és hello átadott értékek minimális adja vissza.|  
|maximális|Ez a függvény hívásakor két különböző mintát van. <p>Itt `max` tömb lehet, és hello függvény `2`: <p>`max([0,1,2])` <p>Azt is megteheti, ez a funkció is igénybe vehet, értékeket, és visszaadja a vesszővel tagolt listáját `2`: <p>`max(0,1,2)` <p> **Megjegyzés:**: összes érték számokból kell állniuk, így ha hello paraméter egy tömb, hello tömb tooonly rendelkezik-e a számokat. <p> **Számú paraméter**: 1 <p> **Név**: gyűjtemény vagy érték <p> **Leírás**: kötelező. Vagy tömbje értékek toofind hello maximális értéket, vagy olyan hello első értékét. <p> **Számú paraméter**: 2...*n* <p> **Név**: érték*n* <p> **Leírás**: nem kötelező. Ha hello első paramétert egy érték, akkor további értékeket is továbbítja, és hello legfeljebb minden átadott értéket ad vissza.|  
|tartomány|Egy adott értéket egész számok tömbje állít elő. Megadhatja a hello hosszát hello tömböt adott vissza. <p>Például, a függvény `[3,4,5,6]`: <p>`range(3,4)` <p> **Számú paraméter**: 1 <p> **Név**: kezdőindex <p> **Leírás**: kötelező. hello első egész hello tömbben. <p> **Számú paraméter**: 2. régiója <p> **Név**: száma <p> **Leírás**: kötelező. Ez az érték hello szám számokból álló hello tömbben.|  
|VÉL|Létrehoz egy véletlenszerű egész hello belül a megadott tartomány (csak az első vége között lehet). Például ezt a funkciót adhat vissza vagy `0` vagy "1": <p>`rand(0,2)` <p> **Számú paraméter**: 1 <p> **Név**: minimális <p> **Leírás**: kötelező. hello legalacsonyabb egész számban visszaadható. <p> **Számú paraméter**: 2. régiója <p> **Név**: maximális <p> **Leírás**: kötelező. Ez az érték hello következő egész számra hello legnagyobb egész számot visszatérő utánra esik.|  
  
### <a name="date-functions"></a>Dátum-funkciók  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|utcnow|Beolvasása hello aktuális időbélyeg karakterláncként, például: `2017-03-15T13:27:36Z`: <p>`utcnow()` <p> **Számú paraméter**: 1 <p> **Név**: formátum <p> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|masodpercekHozzaadasa|Hozzáadja a másodperc tooa karakterlánc Timestamp átadott egész szám. azon másodpercek számát hello lehet pozitív vagy negatív. Alapértelmezés szerint hello eredménye karakterláncként ISO 8601 formátumban ("no"), kivéve, ha a formátummegadóval való ábrázoláshoz valósul meg. Például: `2015-03-15T13:27:00Z`: <p>`addseconds('2015-03-15T13:27:36Z', -36)` <p> **Számú paraméter**: 1 <p> **Név**: időbélyeg <p> **Leírás**: kötelező. Hello idő tartalmazó karakterlánc. <p> **Számú paraméter**: 2. régiója <p> **Név**: másodperc <p> **Leírás**: kötelező. másodperc tooadd hello száma. Negatív toosubtract másodperc lehet. <p> **Számú paraméter**: 3 <p> **Név**: formátum <p> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|addminutes|Hozzáadja a perc tooa karakterlánc Timestamp átadott egész szám. hello hány perc lehet pozitív vagy negatív. Alapértelmezés szerint hello eredménye karakterláncként ISO 8601 formátumban ("no"), kivéve, ha a formátummegadóval való ábrázoláshoz valósul meg. Például: `2015-03-15T14:00:36Z`: <p>`addminutes('2015-03-15T13:27:36Z', 33)` <p> **Számú paraméter**: 1 <p> **Név**: időbélyeg <p> **Leírás**: kötelező. Hello idő tartalmazó karakterlánc. <p> **Számú paraméter**: 2. régiója <p> **Név**: perc <p> **Leírás**: kötelező. perc tooadd hello száma. Negatív toosubtract perc is lehet. <p> **Számú paraméter**: 3 <p> **Név**: formátum <p> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|addhours|Hozzáadja a óra tooa karakterlánc Timestamp átadott egész szám. hello ennyi ideig lehet pozitív vagy negatív. Alapértelmezés szerint hello eredménye karakterláncként ISO 8601 formátumban ("no"), kivéve, ha a formátummegadóval való ábrázoláshoz valósul meg. Például: `2015-03-16T01:27:36Z`: <p>`addhours('2015-03-15T13:27:36Z', 12)` <p> **Számú paraméter**: 1 <p> **Név**: időbélyeg <p> **Leírás**: kötelező. Hello idő tartalmazó karakterlánc. <p> **Számú paraméter**: 2. régiója <p> **Név**: üzemideje (óra) <p> **Leírás**: kötelező. óra tooadd hello száma. Negatív toosubtract óra is lehet. <p> **Számú paraméter**: 3 <p> **Név**: formátum <p> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|napokHozzaadasa|Hozzáadja a nap tooa karakterlánc Timestamp átadott egész szám. hello hány napig lehet pozitív vagy negatív. Alapértelmezés szerint hello eredménye karakterláncként ISO 8601 formátumban ("no"), kivéve, ha a formátummegadóval való ábrázoláshoz valósul meg. Például: `2015-02-23T13:27:36Z`: <p>`addseconds('2015-03-15T13:27:36Z', -20)` <p> **Számú paraméter**: 1 <p> **Név**: időbélyeg <p> **Leírás**: kötelező. Hello idő tartalmazó karakterlánc. <p> **Számú paraméter**: 2. régiója <p> **Név**: napok <p> **Leírás**: kötelező. nap tooadd hello száma. Negatív toosubtract nap lehet. <p> **Számú paraméter**: 3 <p> **Név**: formátum <p> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|FormatDateTime|Dátum formátumú karakterláncot ad vissza. Alapértelmezés szerint hello eredménye karakterláncként ISO 8601 formátumban ("no"), kivéve, ha a formátummegadóval való ábrázoláshoz valósul meg. Például: `2015-02-23T13:27:36Z`: <p>`formatDateTime('2015-03-15T13:27:36Z', 'o')` <p> **Számú paraméter**: 1 <p> **Név**: dátuma <p> **Leírás**: kötelező. Hello dátumot tartalmazó karakterlánc. <p> **Számú paraméter**: 2. régiója <p> **Név**: formátum <p> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|startOfHour|Beolvasása hello hello óra tooa karakterlánc időbélyeg átadott elejére. Például `2017-03-15T13:00:00Z`:<br /><br /> `startOfHour('2017-03-15T13:27:36Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.<br /><br />**Számú paraméter**: 2. régiója<br /><br /> **Név**: formátum<br /><br /> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.|  
|startOfDay|Beolvasása hello hello nap tooa karakterlánc időbélyeg átadott elejére. Például `2017-03-15T00:00:00Z`:<br /><br /> `startOfDay('2017-03-15T13:27:36Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.<br /><br />**Számú paraméter**: 2. régiója<br /><br /> **Név**: formátum<br /><br /> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.| 
|startOfMonth|Beolvasása hello hello hónap tooa karakterlánc időbélyeg átadott elejére. Például `2017-03-01T00:00:00Z`:<br /><br /> `startOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.<br /><br />**Számú paraméter**: 2. régiója<br /><br /> **Név**: formátum<br /><br /> **Leírás**: nem kötelező. Vagy egy [egyetlen formátum megadása](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) vagy egy [egyéni formátum mintát](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) azt jelzi, hogy hogyan tooformat hello a Timestamp értéket. Ha formátum nem áll rendelkezésre, hello ISO 8601 formátumban ("no") használatos.| 
|DayOfWeek|Vissza a nap, hét összetevőjének karakterlánc időbélyeg hello.  Vasárnap 0, a hétfői 1, és így tovább. Például `3`:<br /><br /> `dayOfWeek('2017-03-15T13:27:36Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.| 
|DayOfMonth|Értéket ad vissza a hónap összetevőjét karakterlánc időbélyeg napja hello. Például `15`:<br /><br /> `dayOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.| 
|DayOfYear|Értéket ad vissza az év összetevőjét karakterlánc időbélyeg napja hello. Például `74`:<br /><br /> `dayOfYear('2017-03-15T13:27:36Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.| 
|Ticks|Beolvasása hello ölések karakterlánc időbélyeg tulajdonsága. Például `1489603019`:<br /><br /> `ticks('2017-03-15T18:36:59Z')`<br /><br /> **Számú paraméter**: 1<br /><br /> **Név**: időbélyeg<br /><br /> **Leírás**: kötelező. Ez az hello idő tartalmazó karakterlánc.| 
  
### <a name="workflow-functions"></a>Munkafolyamat-funkciók  

Ezek a funkciók segítséget nyújt a futási időben hello munkafolyamat maga adatainak beolvasása.  
  
|Függvény neve|Leírás|  
|-------------------|-----------------|  
|listCallbackUrl|Egy karakterlánc toocall tooinvoke hello eseményindító vagy művelet adja vissza. <p> **Megjegyzés:**: Ez a függvény csak a használható egy **httpWebhook** és **apiConnectionWebhook**, nem a egy **manuális**, **ismétlődési**, **http**, vagy **apiConnection**. <p>Például hello `listCallbackUrl()` működéshez adja vissza: <p>`https://prod-01.westus.logic.azure.com:443/workflows/1235...ABCD/triggers/manual/run?api-version=2015-08-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxx...xxx` |  
|munkafolyamat|Ez a funkció akkor összes hello részletes adatokat biztosít a hello munkafolyamat maga hello futásidőben. <p> Hello munkafolyamat objektum rendelkezésre álló tulajdonságok a következők: <ul><li>`name`</li><li>`type`</li><li>`id`</li><li>`location`</li><li>`run`</li></ul> <p> hello értékének hello `run` tulajdonsága egy objektum a következő tulajdonságokkal: <ul><li>`name`</li><li>`type`</li><li>`id`</li></ul> <p>Lásd: hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=525617) meg azokat a tulajdonságokat.<p> Például tooget hello neve aktuális hello futtatni, használható hello `"@workflow().run.name"` kifejezés. |

## <a name="next-steps"></a>Következő lépések

[Munkafolyamat-műveletek és eseményindítók](logic-apps-workflow-actions-triggers.md)
