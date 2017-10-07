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
# <a name="sqlruleaction-syntax"></a><span data-ttu-id="4d190-103">SQLRuleAction szintaxis</span><span class="sxs-lookup"><span data-stu-id="4d190-103">SQLRuleAction syntax</span></span>

<span data-ttu-id="4d190-104">A *SqlRuleAction* hello példánya [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) osztály és jelöli azokat az SQL-nyelve műveletek alapján hajtja végre az szintakszist egy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="4d190-104">A *SqlRuleAction* is an instance of hello [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) class, and represents set of actions written in SQL-language based syntax that is performed against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span>   
  
<span data-ttu-id="4d190-105">Ez a témakör hello SQL szabály művelet nyelvtan részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="4d190-105">This topic lists details about hello SQL rule action grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="4d190-106">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="4d190-106">Arguments</span></span>  
  
-   <span data-ttu-id="4d190-107">`<scope>`egy nem kötelező karakterlánc, amely hello hello hatókörének `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="4d190-107">`<scope>` is an optional string indicating hello scope of hello `<property_name>`.</span></span> <span data-ttu-id="4d190-108">Érvényes értékek a következők `sys` vagy `user`.</span><span class="sxs-lookup"><span data-stu-id="4d190-108">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="4d190-109">Hello `sys` érték rendszerek ahol `<property_name>` egy nyilvános tulajdonság neve hello [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="4d190-109">hello `sys` value indicates system scope where `<property_name>` is a public property name of hello [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="4d190-110">`user`jelzi a felhasználó hatókör ahol `<property_name>` hello kulcs [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) szótárban.</span><span class="sxs-lookup"><span data-stu-id="4d190-110">`user` indicates user scope where `<property_name>` is a key of hello [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="4d190-111">`user`hatókör hello alapértelmezett hatókörben, ha `<scope>` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="4d190-111">`user` scope is hello default scope if `<scope>` is not specified.</span></span>  
  
### <a name="remarks"></a><span data-ttu-id="4d190-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d190-112">Remarks</span></span>  

<span data-ttu-id="4d190-113">Egy kísérlet tooaccess egy nem létező rendszer tulajdonság hiba, amíg egy kísérlet tooaccess egy nem létező felhasználó tulajdonság nincs hiba.</span><span class="sxs-lookup"><span data-stu-id="4d190-113">An attempt tooaccess a non-existent system property is an error, while an attempt tooaccess a non-existent user property is not an error.</span></span> <span data-ttu-id="4d190-114">Ehelyett egy nem létező felhasználói tulajdonságot belsőleg történik ismeretlen érték.</span><span class="sxs-lookup"><span data-stu-id="4d190-114">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="4d190-115">Ismeretlen érték kifejezetten operátor kiértékelése közben a rendszer kezeli.</span><span class="sxs-lookup"><span data-stu-id="4d190-115">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="4d190-116">property_name</span><span class="sxs-lookup"><span data-stu-id="4d190-116">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="4d190-117">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="4d190-117">Arguments</span></span>  
 <span data-ttu-id="4d190-118">`<regular_identifier>`hello által képviselt karakterlánc reguláris kifejezés a következő:</span><span class="sxs-lookup"><span data-stu-id="4d190-118">`<regular_identifier>` is a string represented by hello following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 <span data-ttu-id="4d190-119">Ez azt jelenti, hogy bármilyen karakterlánc, amely betűvel kezdődik, és egy vagy több aláhúzás/betűvel vagy számjeggyel követi.</span><span class="sxs-lookup"><span data-stu-id="4d190-119">This means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
 <span data-ttu-id="4d190-120">`[:IsLetter:]`azt jelenti, hogy bármely Unicode karaktert, amely Unicode betűvel van kategóriába sorolni.</span><span class="sxs-lookup"><span data-stu-id="4d190-120">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="4d190-121">`System.Char.IsLetter(c)`Visszaadja `true` Ha `c` egy Unicode betűjele.</span><span class="sxs-lookup"><span data-stu-id="4d190-121">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
 <span data-ttu-id="4d190-122">`[:IsDigit:]`azt jelenti, hogy bármely Unicode karaktert, mint egy decimális számjegyet van besorolva.</span><span class="sxs-lookup"><span data-stu-id="4d190-122">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="4d190-123">`System.Char.IsDigit(c)`Visszaadja `true` Ha `c` Unicode számjegy.</span><span class="sxs-lookup"><span data-stu-id="4d190-123">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
 <span data-ttu-id="4d190-124">A `<regular_identifier>` nem lehet fenntartott kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="4d190-124">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
 <span data-ttu-id="4d190-125">`<delimited_identifier>`van bármilyen karakterlánc, amely szimpla balra vagy jobbra szögletes zárójelek ([]).</span><span class="sxs-lookup"><span data-stu-id="4d190-125">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="4d190-126">Záró szögletes zárójel két jobb oldali kapcsos zárójeleket jelzi.</span><span class="sxs-lookup"><span data-stu-id="4d190-126">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="4d190-127">hello következő példákban `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="4d190-127">hello following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 <span data-ttu-id="4d190-128">`<quoted_identifier>`van bármilyen, amelyek dupla idézőjelek közé zárt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4d190-128">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="4d190-129">Dupla idézőjel azonosítóban ki kettő darab idézőjelre.</span><span class="sxs-lookup"><span data-stu-id="4d190-129">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="4d190-130">Nem ajánlott toouse azonosítók idézőjelek között, mert azt egy karakterlánc-konstansra könnyen összetéveszthetők.</span><span class="sxs-lookup"><span data-stu-id="4d190-130">It is not recommended toouse quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="4d190-131">Ha lehetséges használja a tagolt azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4d190-131">Use a delimited identifier if possible.</span></span> <span data-ttu-id="4d190-132">hello az alábbiakban látható egy példa `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="4d190-132">hello following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="4d190-133">Minta</span><span class="sxs-lookup"><span data-stu-id="4d190-133">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="4d190-134">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d190-134">Remarks</span></span>
  
 <span data-ttu-id="4d190-135">`<pattern>`a string típusúként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4d190-135">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="4d190-136">A LIKE operátor hello mintaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="4d190-136">It is used as a pattern for hello LIKE operator.</span></span>      <span data-ttu-id="4d190-137">Hello következő helyettesítő karaktereket tartalmazhat:</span><span class="sxs-lookup"><span data-stu-id="4d190-137">It can contain hello following wildcard characters:</span></span>  
  
-   <span data-ttu-id="4d190-138">`%`: Bármilyen karakterlánc nulla vagy több.</span><span class="sxs-lookup"><span data-stu-id="4d190-138">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="4d190-139">`_`: Bármilyen karakter.</span><span class="sxs-lookup"><span data-stu-id="4d190-139">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="4d190-140">escape_char</span><span class="sxs-lookup"><span data-stu-id="4d190-140">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="4d190-141">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d190-141">Remarks</span></span>
  
 <span data-ttu-id="4d190-142">`<escape_char>`1 hosszúságú karakterláncként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4d190-142">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="4d190-143">A LIKE operátor hello helyettesítő karakterek szolgál.</span><span class="sxs-lookup"><span data-stu-id="4d190-143">It is used as an escape character for hello LIKE operator.</span></span>  
  
 <span data-ttu-id="4d190-144">Például `property LIKE 'ABC\%' ESCAPE '\'` megfelelő `ABC%` ahelyett, hogy egy karakterláncot kezdetű `ABC`.</span><span class="sxs-lookup"><span data-stu-id="4d190-144">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="4d190-145">állandó</span><span class="sxs-lookup"><span data-stu-id="4d190-145">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="4d190-146">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="4d190-146">Arguments</span></span>  
  
-   <span data-ttu-id="4d190-147">`<integer_constant>`egy olyan karakterlánc, amely nem az idézőjelek közé zárt, és nem tartalmaz a tizedesjegyek számát.</span><span class="sxs-lookup"><span data-stu-id="4d190-147">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="4d190-148">hello értékek tárolt `System.Int64` belső, és hajtsa végre hello azonos tartományon.</span><span class="sxs-lookup"><span data-stu-id="4d190-148">hello values are stored as `System.Int64` internally, and follow hello same range.</span></span>  
  
     <span data-ttu-id="4d190-149">hello hosszú állandókat a következők:</span><span class="sxs-lookup"><span data-stu-id="4d190-149">hello following are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="4d190-150">`<decimal_constant>`egy olyan karakterlánc, szám, amely nem az idézőjelek közé zárt, és tartalmaz a tizedesvessző.</span><span class="sxs-lookup"><span data-stu-id="4d190-150">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="4d190-151">hello értékek tárolt `System.Double` belső, és ugyanazon tartomány/pontosság hello követi.</span><span class="sxs-lookup"><span data-stu-id="4d190-151">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span>  
  
     <span data-ttu-id="4d190-152">Egy jövőbeli verziójában ez a szám tárolódhat egy másik adatforráshoz típus toosupport pontos szám szemantikáját, így nem igazolható a hello tény hello alapjául szolgáló adattípusa `System.Double` a `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="4d190-152">In a future version, this number might be stored in a different data type toosupport exact number semantics, so you should not rely on hello fact hello underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="4d190-153">hello decimális állandókat a következők:</span><span class="sxs-lookup"><span data-stu-id="4d190-153">hello following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="4d190-154">`<approximate_number_constant>`egy szám nyelven írt tudományos jelölés van.</span><span class="sxs-lookup"><span data-stu-id="4d190-154">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="4d190-155">hello értékek tárolt `System.Double` belső, és ugyanazon tartomány/pontosság hello követi.</span><span class="sxs-lookup"><span data-stu-id="4d190-155">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span> <span data-ttu-id="4d190-156">Az alábbiakban hello állandókat a hozzávetőleges száma:</span><span class="sxs-lookup"><span data-stu-id="4d190-156">hello following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="4d190-157">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="4d190-157">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="4d190-158">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d190-158">Remarks</span></span>
  
<span data-ttu-id="4d190-159">Logikai állandók jelölik hello kulcsszavak `TRUE` vagy `FALSE`.</span><span class="sxs-lookup"><span data-stu-id="4d190-159">Boolean constants are represented by hello keywords `TRUE` or `FALSE`.</span></span> <span data-ttu-id="4d190-160">hello értékek tárolt `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="4d190-160">hello values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="4d190-161">string_constant</span><span class="sxs-lookup"><span data-stu-id="4d190-161">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="4d190-162">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d190-162">Remarks</span></span>
  
<span data-ttu-id="4d190-163">A karakterlánckonstansokat egyetlen idézőjelek közé vannak, és a érvényes Unicode-karaktereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4d190-163">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="4d190-164">Egy olyan karakterlánc-konstansra ágyazott szimpla idézőjel szerepel, mint két darab szimpla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="4d190-164">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="4d190-165">Függvény</span><span class="sxs-lookup"><span data-stu-id="4d190-165">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="4d190-166">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d190-166">Remarks</span></span>  

<span data-ttu-id="4d190-167">Hello `newid()` működéséhez értéket ad vissza egy **System.Guid** hello által generált `System.Guid.NewGuid()` metódust.</span><span class="sxs-lookup"><span data-stu-id="4d190-167">hello `newid()` function returns a **System.Guid** generated by hello `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="4d190-168">Hello `property(name)` hello érték hello tulajdonság által hivatkozott `name`.</span><span class="sxs-lookup"><span data-stu-id="4d190-168">hello `property(name)` function returns hello value of hello property referenced by `name`.</span></span> <span data-ttu-id="4d190-169">Hello `name` értéke lehet bármely érvényes kifejezés, amely egy karakterláncértéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4d190-169">hello `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="4d190-170">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="4d190-170">Considerations</span></span>

- <span data-ttu-id="4d190-171">Új tulajdonság vagy frissítés hello értéket egy meglévő tulajdonság használt toocreate lesz.</span><span class="sxs-lookup"><span data-stu-id="4d190-171">SET is used toocreate a new property or update hello value of an existing property.</span></span>
- <span data-ttu-id="4d190-172">Távolítsa el a használt tooremove tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4d190-172">REMOVE is used tooremove a property.</span></span>
- <span data-ttu-id="4d190-173">SET hajtja implicit konverzió lehetőleg hello kifejezés és hello meglévő tulajdonság típusa nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="4d190-173">SET performs implicit conversion if possible when hello expression type and hello existing property type are different.</span></span>
- <span data-ttu-id="4d190-174">A művelet sikertelen lesz, ha nem létező Rendszertulajdonságok hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="4d190-174">Action fails if non-existent system properties were referenced.</span></span>
- <span data-ttu-id="4d190-175">A művelet sikertelen, ha nem létező felhasználói tulajdonságok hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="4d190-175">Action does not fail if non-existent user properties were referenced.</span></span>
- <span data-ttu-id="4d190-176">Egy nem létező felhasználó tulajdonság ki lesz értékelve "Ismeretlen" belső, a következő hello azonos szemantikákkal, [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) operátorok kiértékelése során.</span><span class="sxs-lookup"><span data-stu-id="4d190-176">A non-existent user property is evaluated as "Unknown" internally, following hello same semantics as [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) when evaluating operators.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d190-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d190-177">Next steps</span></span>

- [<span data-ttu-id="4d190-178">SQLRuleAction osztály</span><span class="sxs-lookup"><span data-stu-id="4d190-178">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [<span data-ttu-id="4d190-179">SQLFilter osztály</span><span class="sxs-lookup"><span data-stu-id="4d190-179">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
