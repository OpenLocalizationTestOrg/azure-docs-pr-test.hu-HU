---
title: az Azure-ban aaaSQLRuleAction szintaxis referencia |} Microsoft Docs
description: SQLRuleAction nyelvtan adatait.
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
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 8ef281f942847bcc535b83a5ffb30d03539734f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sqlruleaction-syntax"></a>SQLRuleAction szintaxis

A *SqlRuleAction* hello példánya [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) osztály és jelöli azokat az SQL-nyelve műveletek alapján hajtja végre az szintakszist egy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).   
  
Ez a témakör hello SQL szabály művelet nyelvtan részleteit sorolja fel.  
  
```  
<statements> ::=
    <statement> [, ...n]  
  
```  
  
```  
<statement> ::=
    <action> [;]
    Remarks
    -------
    Semicolon is optional.  
  
```  
  
```  
<action> ::=
    SET <property> = <expression>
    REMOVE <property>  
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
  
### <a name="remarks"></a>Megjegyzések  

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
  
 Ez azt jelenti, hogy bármilyen karakterlánc, amely betűvel kezdődik, és egy vagy több aláhúzás/betűvel vagy számjeggyel követi.  
  
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
  
     hello hosszú állandókat a következők:  
  
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
  
Logikai állandók jelölik hello kulcsszavak `TRUE` vagy `FALSE`. hello értékek tárolt `System.Boolean`.  
  
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

- Új tulajdonság vagy frissítés hello értéket egy meglévő tulajdonság használt toocreate lesz.
- Távolítsa el a használt tooremove tulajdonság.
- SET hajtja implicit konverzió lehetőleg hello kifejezés és hello meglévő tulajdonság típusa nem egyezik.
- A művelet sikertelen lesz, ha nem létező Rendszertulajdonságok hivatkozott.
- A művelet sikertelen, ha nem létező felhasználói tulajdonságok hivatkozott.
- Egy nem létező felhasználó tulajdonság ki lesz értékelve "Ismeretlen" belső, a következő hello azonos szemantikákkal, [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) operátorok kiértékelése során.

## <a name="next-steps"></a>Következő lépések

- [SQLRuleAction osztály](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter osztály](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
