---
title: Az Azure Service Bus SQLFilter szintaxis referencia |} Microsoft Docs
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
ms.openlocfilehash: 3aaec8f9b6a3bbcf814f771405c3b589de6f7ae0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sqlfilter-syntax"></a><span data-ttu-id="952a9-103">SQLFilter szintaxis</span><span class="sxs-lookup"><span data-stu-id="952a9-103">SQLFilter syntax</span></span>

<span data-ttu-id="952a9-104">A *SqlFilter* példánya a [SqlFilter osztály](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), és egy SQL nyelvi alapú kifejezést kiértékelt jelöli egy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="952a9-104">A *SqlFilter* is an instance of the [SqlFilter class](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), and represents a SQL language-based filter expression that is evaluated against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="952a9-105">Egy SqlFilter támogatja az SQL-92 szabvány egy részét.</span><span class="sxs-lookup"><span data-stu-id="952a9-105">A SqlFilter supports a subset of the SQL-92 standard.</span></span>  
  
 <span data-ttu-id="952a9-106">Ez a témakör SqlFilter nyelvtan részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="952a9-106">This topic lists details about SqlFilter grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="952a9-107">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="952a9-107">Arguments</span></span>  
  
-   <span data-ttu-id="952a9-108">`<scope>`egy nem kötelező karakterlánc, amely a hatóköre a `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="952a9-108">`<scope>` is an optional string indicating the scope of the `<property_name>`.</span></span> <span data-ttu-id="952a9-109">Érvényes értékek a következők `sys` vagy `user`.</span><span class="sxs-lookup"><span data-stu-id="952a9-109">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="952a9-110">A `sys` érték rendszerek ahol `<property_name>` egy nyilvános tulajdonság neve a [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="952a9-110">The `sys` value indicates system scope where `<property_name>` is a public property name of the [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="952a9-111">`user`jelzi a felhasználó hatókör ahol `<property_name>` kulcs a [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) szótárban.</span><span class="sxs-lookup"><span data-stu-id="952a9-111">`user` indicates user scope where `<property_name>` is a key of the [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="952a9-112">`user`hatókör esetén az alapértelmezett hatókör `<scope>` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="952a9-112">`user` scope is the default scope if `<scope>` is not specified.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="952a9-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="952a9-113">Remarks</span></span>

<span data-ttu-id="952a9-114">Egy nem létező rendszertulajdonság elérésére tett kísérlet hiba, kiszolgáló, míg egy nem létező felhasználói tulajdonságot elérésére tett kísérlet nem hiba.</span><span class="sxs-lookup"><span data-stu-id="952a9-114">An attempt to access a non-existent system property is an error, while an attempt to access a non-existent user property is not an error.</span></span> <span data-ttu-id="952a9-115">Ehelyett egy nem létező felhasználói tulajdonságot belsőleg történik ismeretlen érték.</span><span class="sxs-lookup"><span data-stu-id="952a9-115">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="952a9-116">Ismeretlen érték kifejezetten operátor kiértékelése közben a rendszer kezeli.</span><span class="sxs-lookup"><span data-stu-id="952a9-116">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="952a9-117">property_name</span><span class="sxs-lookup"><span data-stu-id="952a9-117">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="952a9-118">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="952a9-118">Arguments</span></span>  

 <span data-ttu-id="952a9-119">`<regular_identifier>`a karakterlánc a következő reguláris kifejezésnek jelképezi:</span><span class="sxs-lookup"><span data-stu-id="952a9-119">`<regular_identifier>` is a string represented by the following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
<span data-ttu-id="952a9-120">Ebben a nyelvtanban azt jelenti, hogy bármilyen karakterlánc, amely betűvel kezdődik, és egy vagy több aláhúzás/betűvel vagy számjeggyel követi.</span><span class="sxs-lookup"><span data-stu-id="952a9-120">This grammar means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
<span data-ttu-id="952a9-121">`[:IsLetter:]`azt jelenti, hogy bármely Unicode karaktert, amely Unicode betűvel van kategóriába sorolni.</span><span class="sxs-lookup"><span data-stu-id="952a9-121">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="952a9-122">`System.Char.IsLetter(c)`Visszaadja `true` Ha `c` egy Unicode betűjele.</span><span class="sxs-lookup"><span data-stu-id="952a9-122">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
<span data-ttu-id="952a9-123">`[:IsDigit:]`azt jelenti, hogy bármely Unicode karaktert, mint egy decimális számjegyet van besorolva.</span><span class="sxs-lookup"><span data-stu-id="952a9-123">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="952a9-124">`System.Char.IsDigit(c)`Visszaadja `true` Ha `c` Unicode számjegy.</span><span class="sxs-lookup"><span data-stu-id="952a9-124">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
<span data-ttu-id="952a9-125">A `<regular_identifier>` nem lehet fenntartott kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="952a9-125">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
<span data-ttu-id="952a9-126">`<delimited_identifier>`van bármilyen karakterlánc, amely szimpla balra vagy jobbra szögletes zárójelek ([]).</span><span class="sxs-lookup"><span data-stu-id="952a9-126">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="952a9-127">Záró szögletes zárójel két jobb oldali kapcsos zárójeleket jelzi.</span><span class="sxs-lookup"><span data-stu-id="952a9-127">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="952a9-128">A következő példákban `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="952a9-128">The following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
<span data-ttu-id="952a9-129">`<quoted_identifier>`van bármilyen, amelyek dupla idézőjelek közé zárt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="952a9-129">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="952a9-130">Dupla idézőjel azonosítóban ki kettő darab idézőjelre.</span><span class="sxs-lookup"><span data-stu-id="952a9-130">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="952a9-131">Nem ajánlott, hogy használja a határolójeles azonosítók, mert azt egy karakterlánc-konstansra könnyen összetéveszthetők.</span><span class="sxs-lookup"><span data-stu-id="952a9-131">It is not recommended to use quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="952a9-132">Ha lehetséges használja a tagolt azonosítója.</span><span class="sxs-lookup"><span data-stu-id="952a9-132">Use a delimited identifier if possible.</span></span> <span data-ttu-id="952a9-133">Az alábbiakban egy példát `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="952a9-133">The following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="952a9-134">Minta</span><span class="sxs-lookup"><span data-stu-id="952a9-134">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="952a9-135">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="952a9-135">Remarks</span></span>
  
<span data-ttu-id="952a9-136">`<pattern>`a string típusúként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="952a9-136">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="952a9-137">A LIKE operátor mintaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="952a9-137">It is used as a pattern for the LIKE operator.</span></span>      <span data-ttu-id="952a9-138">A következő helyettesítő karaktereket tartalmazhat:</span><span class="sxs-lookup"><span data-stu-id="952a9-138">It can contain the following wildcard characters:</span></span>  
  
-   <span data-ttu-id="952a9-139">`%`: Bármilyen karakterlánc nulla vagy több.</span><span class="sxs-lookup"><span data-stu-id="952a9-139">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="952a9-140">`_`: Bármilyen karakter.</span><span class="sxs-lookup"><span data-stu-id="952a9-140">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="952a9-141">escape_char</span><span class="sxs-lookup"><span data-stu-id="952a9-141">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="952a9-142">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="952a9-142">Remarks</span></span>  

<span data-ttu-id="952a9-143">`<escape_char>`1 hosszúságú karakterláncként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="952a9-143">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="952a9-144">A LIKE operátor helyettesítő karakterek szolgál.</span><span class="sxs-lookup"><span data-stu-id="952a9-144">It is used as an escape character for the LIKE operator.</span></span>  
  
 <span data-ttu-id="952a9-145">Például `property LIKE 'ABC\%' ESCAPE '\'` megfelelő `ABC%` ahelyett, hogy egy karakterláncot kezdetű `ABC`.</span><span class="sxs-lookup"><span data-stu-id="952a9-145">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="952a9-146">állandó</span><span class="sxs-lookup"><span data-stu-id="952a9-146">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="952a9-147">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="952a9-147">Arguments</span></span>  
  
-   <span data-ttu-id="952a9-148">`<integer_constant>`egy olyan karakterlánc, amely nem az idézőjelek közé zárt, és nem tartalmaz a tizedesjegyek számát.</span><span class="sxs-lookup"><span data-stu-id="952a9-148">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="952a9-149">Az értékek tárolt `System.Int64` belső, és hajtsa végre ugyanezt a porttartományt.</span><span class="sxs-lookup"><span data-stu-id="952a9-149">The values are stored as `System.Int64` internally, and follow the same range.</span></span>  
  
     <span data-ttu-id="952a9-150">Hosszú állandókat az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="952a9-150">These are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="952a9-151">`<decimal_constant>`egy olyan karakterlánc, szám, amely nem az idézőjelek közé zárt, és tartalmaz a tizedesvessző.</span><span class="sxs-lookup"><span data-stu-id="952a9-151">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="952a9-152">Az értékek tárolt `System.Double` belső, és kövesse a ugyanazon tartomány pontosság.</span><span class="sxs-lookup"><span data-stu-id="952a9-152">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span>  
  
     <span data-ttu-id="952a9-153">Egy jövőbeli verziójában ez a szám tárolódhat a különböző adattípusú támogatja a pontos szám szemantikáját, így nem támaszkodhat a tényen az alapul szolgáló adattípusa `System.Double` a `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="952a9-153">In a future version, this number might be stored in a different data type to support exact number semantics, so you should not rely on the fact the underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="952a9-154">A következő példák decimális állandók:</span><span class="sxs-lookup"><span data-stu-id="952a9-154">The following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="952a9-155">`<approximate_number_constant>`egy szám nyelven írt tudományos jelölés van.</span><span class="sxs-lookup"><span data-stu-id="952a9-155">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="952a9-156">Az értékek tárolt `System.Double` belső, és kövesse a ugyanazon tartomány pontosság.</span><span class="sxs-lookup"><span data-stu-id="952a9-156">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span> <span data-ttu-id="952a9-157">A következő példák hozzávetőleges száma állandók:</span><span class="sxs-lookup"><span data-stu-id="952a9-157">The following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="952a9-158">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="952a9-158">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="952a9-159">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="952a9-159">Remarks</span></span>  

<span data-ttu-id="952a9-160">A kulcsszavak jelölik a logikai állandók **igaz** vagy **hamis**.</span><span class="sxs-lookup"><span data-stu-id="952a9-160">Boolean constants are represented by the keywords **TRUE** or **FALSE**.</span></span> <span data-ttu-id="952a9-161">Az értékek tárolt `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="952a9-161">The values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="952a9-162">string_constant</span><span class="sxs-lookup"><span data-stu-id="952a9-162">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="952a9-163">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="952a9-163">Remarks</span></span>  

<span data-ttu-id="952a9-164">A karakterlánckonstansokat egyetlen idézőjelek közé vannak, és a érvényes Unicode-karaktereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="952a9-164">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="952a9-165">Egy olyan karakterlánc-konstansra ágyazott szimpla idézőjel szerepel, mint két darab szimpla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="952a9-165">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="952a9-166">Függvény</span><span class="sxs-lookup"><span data-stu-id="952a9-166">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="952a9-167">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="952a9-167">Remarks</span></span>
  
<span data-ttu-id="952a9-168">A `newid()` működéséhez értéket ad vissza egy **System.Guid** állítja elő a `System.Guid.NewGuid()` metódust.</span><span class="sxs-lookup"><span data-stu-id="952a9-168">The `newid()` function returns a **System.Guid** generated by the `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="952a9-169">A `property(name)` függvény által hivatkozott tulajdonságának `name`.</span><span class="sxs-lookup"><span data-stu-id="952a9-169">The `property(name)` function returns the value of the property referenced by `name`.</span></span> <span data-ttu-id="952a9-170">A `name` értéke lehet bármely érvényes kifejezés, amely egy karakterláncértéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="952a9-170">The `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="952a9-171">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="952a9-171">Considerations</span></span>
  
<span data-ttu-id="952a9-172">Vegye figyelembe a következőket [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) szemantika:</span><span class="sxs-lookup"><span data-stu-id="952a9-172">Consider the following [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantics:</span></span>  
  
-   <span data-ttu-id="952a9-173">A tulajdonságnevek nem különböztetik meg.</span><span class="sxs-lookup"><span data-stu-id="952a9-173">Property names are case-insensitive.</span></span>  
  
-   <span data-ttu-id="952a9-174">Operátorok hajtsa végre a C# implicit konverzió szemantikáját amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="952a9-174">Operators follow C# implicit conversion semantics whenever possible.</span></span>  
  
-   <span data-ttu-id="952a9-175">Rendszer olyan nyilvános tulajdonságok felfedett [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) példányok.</span><span class="sxs-lookup"><span data-stu-id="952a9-175">System properties are public properties exposed in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instances.</span></span>  
  
    <span data-ttu-id="952a9-176">Vegye figyelembe a következőket `IS [NOT] NULL` szemantika:</span><span class="sxs-lookup"><span data-stu-id="952a9-176">Consider the following `IS [NOT] NULL` semantics:</span></span>  
  
    -   <span data-ttu-id="952a9-177">`property IS NULL`kiértékelése `true` , ha a tulajdonság nem létezik, vagy a tulajdonság értéke `null`.</span><span class="sxs-lookup"><span data-stu-id="952a9-177">`property IS NULL` is evaluated as `true` if either the property doesn't exist or the property's value is `null`.</span></span>  
  
### <a name="property-evaluation-semantics"></a><span data-ttu-id="952a9-178">Tulajdonság értékelési szemantikájának módosítása</span><span class="sxs-lookup"><span data-stu-id="952a9-178">Property evaluation semantics</span></span>  
  
-   <span data-ttu-id="952a9-179">Kísérlet egy nem létező rendszer tulajdonságának kiértékelése jelez a [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) kivétel.</span><span class="sxs-lookup"><span data-stu-id="952a9-179">An attempt to evaluate a non-existent system property throws a [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) exception.</span></span>  
  
-   <span data-ttu-id="952a9-180">Egy nem létező tulajdonságot belsőleg történik **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="952a9-180">A property that does not exist is internally evaluated as **unknown**.</span></span>  
  
 <span data-ttu-id="952a9-181">Ismeretlen értékelése az aritmetikai operátor:</span><span class="sxs-lookup"><span data-stu-id="952a9-181">Unknown evaluation in arithmetic operators:</span></span>  
  
-   <span data-ttu-id="952a9-182">A bináris operátor, ha a bal vagy jobb oldalán operandusok kiértékelése **ismeretlen**, akkor az eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="952a9-182">For binary operators, if either the left and/or right side of operands is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
-   <span data-ttu-id="952a9-183">Az egyoperandusú operátorokat, ha egy operandus kiértékelése **ismeretlen**, akkor az eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="952a9-183">For unary operators, if an operand is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="952a9-184">A bináris összehasonlító operátorok ismeretlen értékelése:</span><span class="sxs-lookup"><span data-stu-id="952a9-184">Unknown evaluation in binary comparison operators:</span></span>  
  
-   <span data-ttu-id="952a9-185">Ha a bal vagy jobb oldalán operandusok kiértékelése **ismeretlen**, akkor az eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="952a9-185">If either the left and/or right side of operands is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="952a9-186">Az ismeretlen értékelési `[NOT] LIKE`:</span><span class="sxs-lookup"><span data-stu-id="952a9-186">Unknown evaluation in `[NOT] LIKE`:</span></span>  
  
-   <span data-ttu-id="952a9-187">Ha bármely operandus kiértékelése **ismeretlen**, akkor az eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="952a9-187">If any operand is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="952a9-188">Az ismeretlen értékelési `[NOT] IN`:</span><span class="sxs-lookup"><span data-stu-id="952a9-188">Unknown evaluation in `[NOT] IN`:</span></span>  
  
-   <span data-ttu-id="952a9-189">Ha a bal oldali operandusa kiértékelése **ismeretlen**, akkor az eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="952a9-189">If the left operand is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="952a9-190">Az ismeretlen értékelési **és** operátor:</span><span class="sxs-lookup"><span data-stu-id="952a9-190">Unknown evaluation in **AND** operator:</span></span>  
  
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
  
 <span data-ttu-id="952a9-191">Az ismeretlen értékelési **vagy** operátor:</span><span class="sxs-lookup"><span data-stu-id="952a9-191">Unknown evaluation in **OR** operator:</span></span>  
  
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
  
### <a name="operator-binding-semantics"></a><span data-ttu-id="952a9-192">Operátor kötés szemantikájának módosítása</span><span class="sxs-lookup"><span data-stu-id="952a9-192">Operator binding semantics</span></span>
  
-   <span data-ttu-id="952a9-193">Összehasonlító operátorok például `>`, `>=`, `<`, `<=`, `!=`, és `=` hajtsa végre a azonos szemantikákkal, a C# operátor adatok típusa előléptetések és implicit konverzió kötelező.</span><span class="sxs-lookup"><span data-stu-id="952a9-193">Comparison operators such as `>`, `>=`, `<`, `<=`, `!=`, and `=` follow the same semantics as the C# operator binding in data type promotions and implicit conversions.</span></span>  
  
-   <span data-ttu-id="952a9-194">Aritmetikai operátor például `+`, `-`, `*`, `/`, és `%` hajtsa végre a azonos szemantikákkal, a C# operátor adatok típusa előléptetések és implicit konverzió kötelező.</span><span class="sxs-lookup"><span data-stu-id="952a9-194">Arithmetic operators such as `+`, `-`, `*`, `/`, and `%` follow the same semantics as the C# operator binding in data type promotions and implicit conversions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="952a9-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="952a9-195">Next steps</span></span>

- [<span data-ttu-id="952a9-196">SQLFilter osztály</span><span class="sxs-lookup"><span data-stu-id="952a9-196">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [<span data-ttu-id="952a9-197">SQLRuleAction osztály</span><span class="sxs-lookup"><span data-stu-id="952a9-197">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)