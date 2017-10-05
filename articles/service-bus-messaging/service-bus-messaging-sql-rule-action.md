---
title: Az Azure-ban SQLRuleAction szintaxis referencia |} Microsoft Docs
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
ms.openlocfilehash: 7379b7f58563675f28d77928d933c0d9c7992e71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sqlruleaction-syntax"></a><span data-ttu-id="d46cb-103">SQLRuleAction szintaxis</span><span class="sxs-lookup"><span data-stu-id="d46cb-103">SQLRuleAction syntax</span></span>

<span data-ttu-id="d46cb-104">A *SqlRuleAction* példánya a [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) osztály és jelöli azokat az SQL-nyelve műveletek alapján hajtja végre az szintakszist egy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="d46cb-104">A *SqlRuleAction* is an instance of the [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) class, and represents set of actions written in SQL-language based syntax that is performed against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span>   
  
<span data-ttu-id="d46cb-105">Ez a témakör az SQL-szabály művelet szintaxis részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="d46cb-105">This topic lists details about the SQL rule action grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="d46cb-106">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="d46cb-106">Arguments</span></span>  
  
-   <span data-ttu-id="d46cb-107">`<scope>`egy nem kötelező karakterlánc, amely a hatóköre a `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-107">`<scope>` is an optional string indicating the scope of the `<property_name>`.</span></span> <span data-ttu-id="d46cb-108">Érvényes értékek a következők `sys` vagy `user`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-108">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="d46cb-109">A `sys` érték rendszerek ahol `<property_name>` egy nyilvános tulajdonság neve a [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="d46cb-109">The `sys` value indicates system scope where `<property_name>` is a public property name of the [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="d46cb-110">`user`jelzi a felhasználó hatókör ahol `<property_name>` kulcs a [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) szótárban.</span><span class="sxs-lookup"><span data-stu-id="d46cb-110">`user` indicates user scope where `<property_name>` is a key of the [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="d46cb-111">`user`hatókör esetén az alapértelmezett hatókör `<scope>` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="d46cb-111">`user` scope is the default scope if `<scope>` is not specified.</span></span>  
  
### <a name="remarks"></a><span data-ttu-id="d46cb-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d46cb-112">Remarks</span></span>  

<span data-ttu-id="d46cb-113">Egy nem létező rendszertulajdonság elérésére tett kísérlet hiba, kiszolgáló, míg egy nem létező felhasználói tulajdonságot elérésére tett kísérlet nem hiba.</span><span class="sxs-lookup"><span data-stu-id="d46cb-113">An attempt to access a non-existent system property is an error, while an attempt to access a non-existent user property is not an error.</span></span> <span data-ttu-id="d46cb-114">Ehelyett egy nem létező felhasználói tulajdonságot belsőleg történik ismeretlen érték.</span><span class="sxs-lookup"><span data-stu-id="d46cb-114">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="d46cb-115">Ismeretlen érték kifejezetten operátor kiértékelése közben a rendszer kezeli.</span><span class="sxs-lookup"><span data-stu-id="d46cb-115">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="d46cb-116">property_name</span><span class="sxs-lookup"><span data-stu-id="d46cb-116">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="d46cb-117">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="d46cb-117">Arguments</span></span>  
 <span data-ttu-id="d46cb-118">`<regular_identifier>`a karakterlánc a következő reguláris kifejezésnek jelképezi:</span><span class="sxs-lookup"><span data-stu-id="d46cb-118">`<regular_identifier>` is a string represented by the following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 <span data-ttu-id="d46cb-119">Ez azt jelenti, hogy bármilyen karakterlánc, amely betűvel kezdődik, és egy vagy több aláhúzás/betűvel vagy számjeggyel követi.</span><span class="sxs-lookup"><span data-stu-id="d46cb-119">This means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
 <span data-ttu-id="d46cb-120">`[:IsLetter:]`azt jelenti, hogy bármely Unicode karaktert, amely Unicode betűvel van kategóriába sorolni.</span><span class="sxs-lookup"><span data-stu-id="d46cb-120">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="d46cb-121">`System.Char.IsLetter(c)`Visszaadja `true` Ha `c` egy Unicode betűjele.</span><span class="sxs-lookup"><span data-stu-id="d46cb-121">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
 <span data-ttu-id="d46cb-122">`[:IsDigit:]`azt jelenti, hogy bármely Unicode karaktert, mint egy decimális számjegyet van besorolva.</span><span class="sxs-lookup"><span data-stu-id="d46cb-122">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="d46cb-123">`System.Char.IsDigit(c)`Visszaadja `true` Ha `c` Unicode számjegy.</span><span class="sxs-lookup"><span data-stu-id="d46cb-123">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
 <span data-ttu-id="d46cb-124">A `<regular_identifier>` nem lehet fenntartott kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="d46cb-124">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
 <span data-ttu-id="d46cb-125">`<delimited_identifier>`van bármilyen karakterlánc, amely szimpla balra vagy jobbra szögletes zárójelek ([]).</span><span class="sxs-lookup"><span data-stu-id="d46cb-125">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="d46cb-126">Záró szögletes zárójel két jobb oldali kapcsos zárójeleket jelzi.</span><span class="sxs-lookup"><span data-stu-id="d46cb-126">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="d46cb-127">A következő példákban `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="d46cb-127">The following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 <span data-ttu-id="d46cb-128">`<quoted_identifier>`van bármilyen, amelyek dupla idézőjelek közé zárt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d46cb-128">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="d46cb-129">Dupla idézőjel azonosítóban ki kettő darab idézőjelre.</span><span class="sxs-lookup"><span data-stu-id="d46cb-129">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="d46cb-130">Nem ajánlott, hogy használja a határolójeles azonosítók, mert azt egy karakterlánc-konstansra könnyen összetéveszthetők.</span><span class="sxs-lookup"><span data-stu-id="d46cb-130">It is not recommended to use quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="d46cb-131">Ha lehetséges használja a tagolt azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d46cb-131">Use a delimited identifier if possible.</span></span> <span data-ttu-id="d46cb-132">Az alábbiakban egy példát `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="d46cb-132">The following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="d46cb-133">Minta</span><span class="sxs-lookup"><span data-stu-id="d46cb-133">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="d46cb-134">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d46cb-134">Remarks</span></span>
  
 <span data-ttu-id="d46cb-135">`<pattern>`a string típusúként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d46cb-135">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="d46cb-136">A LIKE operátor mintaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="d46cb-136">It is used as a pattern for the LIKE operator.</span></span>      <span data-ttu-id="d46cb-137">A következő helyettesítő karaktereket tartalmazhat:</span><span class="sxs-lookup"><span data-stu-id="d46cb-137">It can contain the following wildcard characters:</span></span>  
  
-   <span data-ttu-id="d46cb-138">`%`: Bármilyen karakterlánc nulla vagy több.</span><span class="sxs-lookup"><span data-stu-id="d46cb-138">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="d46cb-139">`_`: Bármilyen karakter.</span><span class="sxs-lookup"><span data-stu-id="d46cb-139">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="d46cb-140">escape_char</span><span class="sxs-lookup"><span data-stu-id="d46cb-140">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="d46cb-141">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d46cb-141">Remarks</span></span>
  
 <span data-ttu-id="d46cb-142">`<escape_char>`1 hosszúságú karakterláncként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d46cb-142">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="d46cb-143">A LIKE operátor helyettesítő karakterek szolgál.</span><span class="sxs-lookup"><span data-stu-id="d46cb-143">It is used as an escape character for the LIKE operator.</span></span>  
  
 <span data-ttu-id="d46cb-144">Például `property LIKE 'ABC\%' ESCAPE '\'` megfelelő `ABC%` ahelyett, hogy egy karakterláncot kezdetű `ABC`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-144">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="d46cb-145">állandó</span><span class="sxs-lookup"><span data-stu-id="d46cb-145">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="d46cb-146">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="d46cb-146">Arguments</span></span>  
  
-   <span data-ttu-id="d46cb-147">`<integer_constant>`egy olyan karakterlánc, amely nem az idézőjelek közé zárt, és nem tartalmaz a tizedesjegyek számát.</span><span class="sxs-lookup"><span data-stu-id="d46cb-147">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="d46cb-148">Az értékek tárolt `System.Int64` belső, és hajtsa végre ugyanezt a porttartományt.</span><span class="sxs-lookup"><span data-stu-id="d46cb-148">The values are stored as `System.Int64` internally, and follow the same range.</span></span>  
  
     <span data-ttu-id="d46cb-149">A következő példák hosszú állandók:</span><span class="sxs-lookup"><span data-stu-id="d46cb-149">The following are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="d46cb-150">`<decimal_constant>`egy olyan karakterlánc, szám, amely nem az idézőjelek közé zárt, és tartalmaz a tizedesvessző.</span><span class="sxs-lookup"><span data-stu-id="d46cb-150">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="d46cb-151">Az értékek tárolt `System.Double` belső, és kövesse a ugyanazon tartomány pontosság.</span><span class="sxs-lookup"><span data-stu-id="d46cb-151">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span>  
  
     <span data-ttu-id="d46cb-152">Egy jövőbeli verziójában ez a szám tárolódhat a különböző adattípusú támogatja a pontos szám szemantikáját, így nem támaszkodhat a tényen az alapul szolgáló adattípusa `System.Double` a `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-152">In a future version, this number might be stored in a different data type to support exact number semantics, so you should not rely on the fact the underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="d46cb-153">A következő példák decimális állandók:</span><span class="sxs-lookup"><span data-stu-id="d46cb-153">The following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="d46cb-154">`<approximate_number_constant>`egy szám nyelven írt tudományos jelölés van.</span><span class="sxs-lookup"><span data-stu-id="d46cb-154">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="d46cb-155">Az értékek tárolt `System.Double` belső, és kövesse a ugyanazon tartomány pontosság.</span><span class="sxs-lookup"><span data-stu-id="d46cb-155">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span> <span data-ttu-id="d46cb-156">A következő példák hozzávetőleges száma állandók:</span><span class="sxs-lookup"><span data-stu-id="d46cb-156">The following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="d46cb-157">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="d46cb-157">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="d46cb-158">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d46cb-158">Remarks</span></span>
  
<span data-ttu-id="d46cb-159">A kulcsszavak jelölik a logikai állandók `TRUE` vagy `FALSE`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-159">Boolean constants are represented by the keywords `TRUE` or `FALSE`.</span></span> <span data-ttu-id="d46cb-160">Az értékek tárolt `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-160">The values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="d46cb-161">string_constant</span><span class="sxs-lookup"><span data-stu-id="d46cb-161">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="d46cb-162">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d46cb-162">Remarks</span></span>
  
<span data-ttu-id="d46cb-163">A karakterlánckonstansokat egyetlen idézőjelek közé vannak, és a érvényes Unicode-karaktereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d46cb-163">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="d46cb-164">Egy olyan karakterlánc-konstansra ágyazott szimpla idézőjel szerepel, mint két darab szimpla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="d46cb-164">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="d46cb-165">Függvény</span><span class="sxs-lookup"><span data-stu-id="d46cb-165">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="d46cb-166">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d46cb-166">Remarks</span></span>  

<span data-ttu-id="d46cb-167">A `newid()` működéséhez értéket ad vissza egy **System.Guid** állítja elő a `System.Guid.NewGuid()` metódust.</span><span class="sxs-lookup"><span data-stu-id="d46cb-167">The `newid()` function returns a **System.Guid** generated by the `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="d46cb-168">A `property(name)` függvény által hivatkozott tulajdonságának `name`.</span><span class="sxs-lookup"><span data-stu-id="d46cb-168">The `property(name)` function returns the value of the property referenced by `name`.</span></span> <span data-ttu-id="d46cb-169">A `name` értéke lehet bármely érvényes kifejezés, amely egy karakterláncértéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d46cb-169">The `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="d46cb-170">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="d46cb-170">Considerations</span></span>

- <span data-ttu-id="d46cb-171">Hozzon létre egy új tulajdonságot, vagy frissítse az értéket egy meglévő tulajdonság használatos.</span><span class="sxs-lookup"><span data-stu-id="d46cb-171">SET is used to create a new property or update the value of an existing property.</span></span>
- <span data-ttu-id="d46cb-172">REMOVE segítségével távolítsa el azt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="d46cb-172">REMOVE is used to remove a property.</span></span>
- <span data-ttu-id="d46cb-173">SET hajtja implicit konverzió lehetőség szerint a kifejezés típusa és a meglévő tulajdonság típusa nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="d46cb-173">SET performs implicit conversion if possible when the expression type and the existing property type are different.</span></span>
- <span data-ttu-id="d46cb-174">A művelet sikertelen lesz, ha nem létező Rendszertulajdonságok hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="d46cb-174">Action fails if non-existent system properties were referenced.</span></span>
- <span data-ttu-id="d46cb-175">A művelet sikertelen, ha nem létező felhasználói tulajdonságok hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="d46cb-175">Action does not fail if non-existent user properties were referenced.</span></span>
- <span data-ttu-id="d46cb-176">Egy nem létező felhasználó tulajdonság ki lesz értékelve "Ismeretlen" belsőleg, az azonos szemantikákkal, a következő [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) operátorok kiértékelése során.</span><span class="sxs-lookup"><span data-stu-id="d46cb-176">A non-existent user property is evaluated as "Unknown" internally, following the same semantics as [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) when evaluating operators.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d46cb-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d46cb-177">Next steps</span></span>

- [<span data-ttu-id="d46cb-178">SQLRuleAction osztály</span><span class="sxs-lookup"><span data-stu-id="d46cb-178">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [<span data-ttu-id="d46cb-179">SQLFilter osztály</span><span class="sxs-lookup"><span data-stu-id="d46cb-179">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
