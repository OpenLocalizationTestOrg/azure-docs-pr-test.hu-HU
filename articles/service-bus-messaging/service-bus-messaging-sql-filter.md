---
title: "Service Bus SQLFilter szintaxis hivatkozás aaaAzure |} Microsoft Docs"
description: SQLFilter nyelvtan adatait.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: ea49d42e343a6b324eb34c7831ff6be2855346e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sqlfilter-syntax"></a>SQLFilter szintaxis

A *SqlFilter* hello példánya [SqlFilter osztály](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), és egy SQL nyelvi alapú kifejezést kiértékelt jelöli egy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Egy SqlFilter támogatja az SQL-92 hello standard egy részét.  
  
 Ez a témakör SqlFilter nyelvtan részleteit sorolja fel.  
  
```  
<predicate ::=  
      { NOT <predicate> }  
      | <predicate> AND <predicate>  
      | <predicate> OR <predicate>  
      | <expression> { = | <> | != | > | >= | < | <= } <expression>  
      | <property> IS [NOT] NULL  
      | <expression> [NOT] IN ( <expression> [, ...n] )  
      | <expression> [NOT] LIKE <pattern> [ESCAPE <escape_char>]  
      | EXISTS ( <property> )  
      | ( <predicate> )  
  
```  
  
```  
<expression> ::=  
      <constant>   
      | <function>  
      | <property>  
      | <expression> { + | - | * | / | % } <expression>  
      | { + | - } <expression>  
      | ( <expression> )  
  
```  
  
```  
<property> :=   
       [<scope> .] <property_name>  
  
```  
  
## <a name="arguments"></a>Argumentumok  
  
-   `<scope>`egy nem kötelező karakterlánc, amely hello hello hatókörének `<property_name>`. Érvényes értékek a következők `sys` vagy `user`. Hello `sys` érték rendszerek ahol `<property_name>` egy nyilvános tulajdonság neve hello [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user`jelzi a felhasználó hatókör ahol `<property_name>` hello kulcs [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) szótárban. `user`hatókör hello alapértelmezett hatókörben, ha `<scope>` nincs megadva.  
  
## <a name="remarks"></a>Megjegyzések

Egy kísérlet tooaccess egy nem létező rendszer tulajdonság hiba, amíg egy kísérlet tooaccess egy nem létező felhasználó tulajdonság nincs hiba. Ehelyett egy nem létező felhasználói tulajdonságot belsőleg történik ismeretlen érték. Ismeretlen érték kifejezetten operátor kiértékelése közben a rendszer kezeli.  
  
## <a name="propertyname"></a>property_name  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Argumentumok  

 `<regular_identifier>`hello által képviselt karakterlánc reguláris kifejezés a következő:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Ebben a nyelvtanban azt jelenti, hogy bármilyen karakterlánc, amely betűvel kezdődik, és egy vagy több aláhúzás/betűvel vagy számjeggyel követi.  
  
`[:IsLetter:]`azt jelenti, hogy bármely Unicode karaktert, amely Unicode betűvel van kategóriába sorolni. `System.Char.IsLetter(c)`Visszaadja `true` Ha `c` egy Unicode betűjele.  
  
`[:IsDigit:]`azt jelenti, hogy bármely Unicode karaktert, mint egy decimális számjegyet van besorolva. `System.Char.IsDigit(c)`Visszaadja `true` Ha `c` Unicode számjegy.  
  
A `<regular_identifier>` nem lehet fenntartott kulcsszó.  
  
`<delimited_identifier>`van bármilyen karakterlánc, amely szimpla balra vagy jobbra szögletes zárójelek ([]). Záró szögletes zárójel két jobb oldali kapcsos zárójeleket jelzi. hello következő példákban `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>`van bármilyen, amelyek dupla idézőjelek közé zárt karakterlánc. Dupla idézőjel azonosítóban ki kettő darab idézőjelre. Nem ajánlott toouse azonosítók idézőjelek között, mert azt egy karakterlánc-konstansra könnyen összetéveszthetők. Ha lehetséges használja a tagolt azonosítója. hello az alábbiakban látható egy példa `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>Minta  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Megjegyzések
  
`<pattern>`a string típusúként kiértékelt kifejezésnek kell lennie. A LIKE operátor hello mintaként szolgál.      Hello következő helyettesítő karaktereket tartalmazhat:  
  
-   `%`: Bármilyen karakterlánc nulla vagy több.  
  
-   `_`: Bármilyen karakter.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Megjegyzések  

`<escape_char>`1 hosszúságú karakterláncként kiértékelt kifejezésnek kell lennie. A LIKE operátor hello helyettesítő karakterek szolgál.  
  
 Például `property LIKE 'ABC\%' ESCAPE '\'` megfelelő `ABC%` ahelyett, hogy egy karakterláncot kezdetű `ABC`.  
  
## <a name="constant"></a>állandó  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Argumentumok  
  
-   `<integer_constant>`egy olyan karakterlánc, amely nem az idézőjelek közé zárt, és nem tartalmaz a tizedesjegyek számát. hello értékek tárolt `System.Int64` belső, és hajtsa végre hello azonos tartományon.  
  
     Hosszú állandókat az alábbiak:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`egy olyan karakterlánc, szám, amely nem az idézőjelek közé zárt, és tartalmaz a tizedesvessző. hello értékek tárolt `System.Double` belső, és ugyanazon tartomány/pontosság hello követi.  
  
     Egy jövőbeli verziójában ez a szám tárolódhat egy másik adatforráshoz típus toosupport pontos szám szemantikáját, így nem igazolható a hello tény hello alapjául szolgáló adattípusa `System.Double` a `<decimal_constant>`.  
  
     hello decimális állandókat a következők:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`egy szám nyelven írt tudományos jelölés van. hello értékek tárolt `System.Double` belső, és ugyanazon tartomány/pontosság hello követi. Az alábbiakban hello állandókat a hozzávetőleges száma:  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a>boolean_constant  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a>Megjegyzések  

Logikai állandók jelölik hello kulcsszavak **igaz** vagy **hamis**. hello értékek tárolt `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Megjegyzések  

A karakterlánckonstansokat egyetlen idézőjelek közé vannak, és a érvényes Unicode-karaktereket tartalmaz. Egy olyan karakterlánc-konstansra ágyazott szimpla idézőjel szerepel, mint két darab szimpla idézőjelek között.  
  
## <a name="function"></a>Függvény  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Megjegyzések
  
Hello `newid()` működéséhez értéket ad vissza egy **System.Guid** hello által generált `System.Guid.NewGuid()` metódust.  
  
Hello `property(name)` hello érték hello tulajdonság által hivatkozott `name`. Hello `name` értéke lehet bármely érvényes kifejezés, amely egy karakterláncértéket ad vissza.  
  
## <a name="considerations"></a>Megfontolandó szempontok
  
Vegye figyelembe a következőket hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) szemantika:  
  
-   A tulajdonságnevek nem különböztetik meg.  
  
-   Operátorok hajtsa végre a C# implicit konverzió szemantikáját amikor csak lehetséges.  
  
-   Rendszer olyan nyilvános tulajdonságok felfedett [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) példányok.  
  
    Vegye figyelembe a következőket hello `IS [NOT] NULL` szemantika:  
  
    -   `property IS NULL`kiértékelése `true` vagy hello tulajdonság nem létezik, vagy a tulajdonság értékét hello `null`.  
  
### <a name="property-evaluation-semantics"></a>Tulajdonság értékelési szemantikájának módosítása  
  
-   Egy nem létező rendszer tulajdonság jelez kísérlet tooevaluate egy [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) kivétel.  
  
-   Egy nem létező tulajdonságot belsőleg történik **ismeretlen**.  
  
 Ismeretlen értékelése az aritmetikai operátor:  
  
-   A bináris operátor, ha bármelyik hello balra és/vagy operandusok jobb oldalán történik **ismeretlen**, majd hello eredmény **ismeretlen**.  
  
-   Az egyoperandusú operátorokat, ha egy operandus kiértékelése **ismeretlen**, majd hello eredmény **ismeretlen**.  
  
 A bináris összehasonlító operátorok ismeretlen értékelése:  
  
-   Ha bármelyik hello balra és/vagy operandusok jobb oldalán történik **ismeretlen**, majd hello eredmény **ismeretlen**.  
  
 Az ismeretlen értékelési `[NOT] LIKE`:  
  
-   Ha bármely operandus kiértékelése **ismeretlen**, majd hello eredmény **ismeretlen**.  
  
 Az ismeretlen értékelési `[NOT] IN`:  
  
-   Ha a bal oldali operandusához hello kiértékelése **ismeretlen**, majd hello eredmény **ismeretlen**.  
  
 Az ismeretlen értékelési **és** operátor:  
  
```  
+---+---+---+---+  
|AND| T | F | U |  
+---+---+---+---+  
| T | T | F | U |  
+---+---+---+---+  
| F | F | F | F |  
+---+---+---+---+  
| U | U | F | U |  
+---+---+---+---+  
```  
  
 Az ismeretlen értékelési **vagy** operátor:  
  
```  
+---+---+---+---+  
|OR | T | F | U |  
+---+---+---+---+  
| T | T | T | T |  
+---+---+---+---+  
| F | T | F | U |  
+---+---+---+---+  
| U | T | U | U |  
+---+---+---+---+  
```  
  
### <a name="operator-binding-semantics"></a>Operátor kötés szemantikájának módosítása
  
-   Összehasonlító operátorok például `>`, `>=`, `<`, `<=`, `!=`, és `=` kövesse hello azonos szemantikákkal hello C# operátor adatok kötelező, írja be az előléptetések és implicit konverzió.  
  
-   Aritmetikai operátor például `+`, `-`, `*`, `/`, és `%` kövesse hello azonos szemantikákkal hello C# operátor adatok kötelező, írja be az előléptetések és implicit konverzió.

## <a name="next-steps"></a>Következő lépések

- [SQLFilter osztály](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLRuleAction osztály](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)