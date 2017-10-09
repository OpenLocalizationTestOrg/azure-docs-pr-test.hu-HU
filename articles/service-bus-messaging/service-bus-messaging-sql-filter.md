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
# <a name="sqlfilter-syntax"></a><span data-ttu-id="f5c5e-103">SQLFilter szintaxis</span><span class="sxs-lookup"><span data-stu-id="f5c5e-103">SQLFilter syntax</span></span>

<span data-ttu-id="f5c5e-104">A *SqlFilter* hello példánya [SqlFilter osztály](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), és egy SQL nyelvi alapú kifejezést kiértékelt jelöli egy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="f5c5e-104">A *SqlFilter* is an instance of hello [SqlFilter class](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), and represents a SQL language-based filter expression that is evaluated against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="f5c5e-105">Egy SqlFilter támogatja az SQL-92 hello standard egy részét.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-105">A SqlFilter supports a subset of hello SQL-92 standard.</span></span>  
  
 <span data-ttu-id="f5c5e-106">Ez a témakör SqlFilter nyelvtan részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-106">This topic lists details about SqlFilter grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="f5c5e-107">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="f5c5e-107">Arguments</span></span>  
  
-   <span data-ttu-id="f5c5e-108">`<scope>`egy nem kötelező karakterlánc, amely hello hello hatókörének `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-108">`<scope>` is an optional string indicating hello scope of hello `<property_name>`.</span></span> <span data-ttu-id="f5c5e-109">Érvényes értékek a következők `sys` vagy `user`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-109">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="f5c5e-110">Hello `sys` érték rendszerek ahol `<property_name>` egy nyilvános tulajdonság neve hello [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="f5c5e-110">hello `sys` value indicates system scope where `<property_name>` is a public property name of hello [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="f5c5e-111">`user`jelzi a felhasználó hatókör ahol `<property_name>` hello kulcs [BrokeredMessage osztály](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) szótárban.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-111">`user` indicates user scope where `<property_name>` is a key of hello [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="f5c5e-112">`user`hatókör hello alapértelmezett hatókörben, ha `<scope>` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-112">`user` scope is hello default scope if `<scope>` is not specified.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="f5c5e-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-113">Remarks</span></span>

<span data-ttu-id="f5c5e-114">Egy kísérlet tooaccess egy nem létező rendszer tulajdonság hiba, amíg egy kísérlet tooaccess egy nem létező felhasználó tulajdonság nincs hiba.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-114">An attempt tooaccess a non-existent system property is an error, while an attempt tooaccess a non-existent user property is not an error.</span></span> <span data-ttu-id="f5c5e-115">Ehelyett egy nem létező felhasználói tulajdonságot belsőleg történik ismeretlen érték.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-115">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="f5c5e-116">Ismeretlen érték kifejezetten operátor kiértékelése közben a rendszer kezeli.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-116">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="f5c5e-117">property_name</span><span class="sxs-lookup"><span data-stu-id="f5c5e-117">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="f5c5e-118">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="f5c5e-118">Arguments</span></span>  

 <span data-ttu-id="f5c5e-119">`<regular_identifier>`hello által képviselt karakterlánc reguláris kifejezés a következő:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-119">`<regular_identifier>` is a string represented by hello following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
<span data-ttu-id="f5c5e-120">Ebben a nyelvtanban azt jelenti, hogy bármilyen karakterlánc, amely betűvel kezdődik, és egy vagy több aláhúzás/betűvel vagy számjeggyel követi.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-120">This grammar means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
<span data-ttu-id="f5c5e-121">`[:IsLetter:]`azt jelenti, hogy bármely Unicode karaktert, amely Unicode betűvel van kategóriába sorolni.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-121">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="f5c5e-122">`System.Char.IsLetter(c)`Visszaadja `true` Ha `c` egy Unicode betűjele.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-122">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
<span data-ttu-id="f5c5e-123">`[:IsDigit:]`azt jelenti, hogy bármely Unicode karaktert, mint egy decimális számjegyet van besorolva.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-123">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="f5c5e-124">`System.Char.IsDigit(c)`Visszaadja `true` Ha `c` Unicode számjegy.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-124">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
<span data-ttu-id="f5c5e-125">A `<regular_identifier>` nem lehet fenntartott kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-125">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
<span data-ttu-id="f5c5e-126">`<delimited_identifier>`van bármilyen karakterlánc, amely szimpla balra vagy jobbra szögletes zárójelek ([]).</span><span class="sxs-lookup"><span data-stu-id="f5c5e-126">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="f5c5e-127">Záró szögletes zárójel két jobb oldali kapcsos zárójeleket jelzi.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-127">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="f5c5e-128">hello következő példákban `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-128">hello following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
<span data-ttu-id="f5c5e-129">`<quoted_identifier>`van bármilyen, amelyek dupla idézőjelek közé zárt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-129">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="f5c5e-130">Dupla idézőjel azonosítóban ki kettő darab idézőjelre.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-130">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="f5c5e-131">Nem ajánlott toouse azonosítók idézőjelek között, mert azt egy karakterlánc-konstansra könnyen összetéveszthetők.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-131">It is not recommended toouse quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="f5c5e-132">Ha lehetséges használja a tagolt azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-132">Use a delimited identifier if possible.</span></span> <span data-ttu-id="f5c5e-133">hello az alábbiakban látható egy példa `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-133">hello following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="f5c5e-134">Minta</span><span class="sxs-lookup"><span data-stu-id="f5c5e-134">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="f5c5e-135">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-135">Remarks</span></span>
  
<span data-ttu-id="f5c5e-136">`<pattern>`a string típusúként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-136">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="f5c5e-137">A LIKE operátor hello mintaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-137">It is used as a pattern for hello LIKE operator.</span></span>      <span data-ttu-id="f5c5e-138">Hello következő helyettesítő karaktereket tartalmazhat:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-138">It can contain hello following wildcard characters:</span></span>  
  
-   <span data-ttu-id="f5c5e-139">`%`: Bármilyen karakterlánc nulla vagy több.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-139">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="f5c5e-140">`_`: Bármilyen karakter.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-140">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="f5c5e-141">escape_char</span><span class="sxs-lookup"><span data-stu-id="f5c5e-141">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="f5c5e-142">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-142">Remarks</span></span>  

<span data-ttu-id="f5c5e-143">`<escape_char>`1 hosszúságú karakterláncként kiértékelt kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-143">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="f5c5e-144">A LIKE operátor hello helyettesítő karakterek szolgál.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-144">It is used as an escape character for hello LIKE operator.</span></span>  
  
 <span data-ttu-id="f5c5e-145">Például `property LIKE 'ABC\%' ESCAPE '\'` megfelelő `ABC%` ahelyett, hogy egy karakterláncot kezdetű `ABC`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-145">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="f5c5e-146">állandó</span><span class="sxs-lookup"><span data-stu-id="f5c5e-146">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="f5c5e-147">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="f5c5e-147">Arguments</span></span>  
  
-   <span data-ttu-id="f5c5e-148">`<integer_constant>`egy olyan karakterlánc, amely nem az idézőjelek közé zárt, és nem tartalmaz a tizedesjegyek számát.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-148">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="f5c5e-149">hello értékek tárolt `System.Int64` belső, és hajtsa végre hello azonos tartományon.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-149">hello values are stored as `System.Int64` internally, and follow hello same range.</span></span>  
  
     <span data-ttu-id="f5c5e-150">Hosszú állandókat az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-150">These are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="f5c5e-151">`<decimal_constant>`egy olyan karakterlánc, szám, amely nem az idézőjelek közé zárt, és tartalmaz a tizedesvessző.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-151">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="f5c5e-152">hello értékek tárolt `System.Double` belső, és ugyanazon tartomány/pontosság hello követi.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-152">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span>  
  
     <span data-ttu-id="f5c5e-153">Egy jövőbeli verziójában ez a szám tárolódhat egy másik adatforráshoz típus toosupport pontos szám szemantikáját, így nem igazolható a hello tény hello alapjául szolgáló adattípusa `System.Double` a `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-153">In a future version, this number might be stored in a different data type toosupport exact number semantics, so you should not rely on hello fact hello underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="f5c5e-154">hello decimális állandókat a következők:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-154">hello following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="f5c5e-155">`<approximate_number_constant>`egy szám nyelven írt tudományos jelölés van.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-155">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="f5c5e-156">hello értékek tárolt `System.Double` belső, és ugyanazon tartomány/pontosság hello követi.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-156">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span> <span data-ttu-id="f5c5e-157">Az alábbiakban hello állandókat a hozzávetőleges száma:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-157">hello following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="f5c5e-158">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="f5c5e-158">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="f5c5e-159">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-159">Remarks</span></span>  

<span data-ttu-id="f5c5e-160">Logikai állandók jelölik hello kulcsszavak **igaz** vagy **hamis**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-160">Boolean constants are represented by hello keywords **TRUE** or **FALSE**.</span></span> <span data-ttu-id="f5c5e-161">hello értékek tárolt `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-161">hello values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="f5c5e-162">string_constant</span><span class="sxs-lookup"><span data-stu-id="f5c5e-162">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="f5c5e-163">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-163">Remarks</span></span>  

<span data-ttu-id="f5c5e-164">A karakterlánckonstansokat egyetlen idézőjelek közé vannak, és a érvényes Unicode-karaktereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-164">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="f5c5e-165">Egy olyan karakterlánc-konstansra ágyazott szimpla idézőjel szerepel, mint két darab szimpla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-165">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="f5c5e-166">Függvény</span><span class="sxs-lookup"><span data-stu-id="f5c5e-166">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="f5c5e-167">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-167">Remarks</span></span>
  
<span data-ttu-id="f5c5e-168">Hello `newid()` működéséhez értéket ad vissza egy **System.Guid** hello által generált `System.Guid.NewGuid()` metódust.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-168">hello `newid()` function returns a **System.Guid** generated by hello `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="f5c5e-169">Hello `property(name)` hello érték hello tulajdonság által hivatkozott `name`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-169">hello `property(name)` function returns hello value of hello property referenced by `name`.</span></span> <span data-ttu-id="f5c5e-170">Hello `name` értéke lehet bármely érvényes kifejezés, amely egy karakterláncértéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-170">hello `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="f5c5e-171">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="f5c5e-171">Considerations</span></span>
  
<span data-ttu-id="f5c5e-172">Vegye figyelembe a következőket hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) szemantika:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-172">Consider hello following [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantics:</span></span>  
  
-   <span data-ttu-id="f5c5e-173">A tulajdonságnevek nem különböztetik meg.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-173">Property names are case-insensitive.</span></span>  
  
-   <span data-ttu-id="f5c5e-174">Operátorok hajtsa végre a C# implicit konverzió szemantikáját amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-174">Operators follow C# implicit conversion semantics whenever possible.</span></span>  
  
-   <span data-ttu-id="f5c5e-175">Rendszer olyan nyilvános tulajdonságok felfedett [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) példányok.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-175">System properties are public properties exposed in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instances.</span></span>  
  
    <span data-ttu-id="f5c5e-176">Vegye figyelembe a következőket hello `IS [NOT] NULL` szemantika:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-176">Consider hello following `IS [NOT] NULL` semantics:</span></span>  
  
    -   <span data-ttu-id="f5c5e-177">`property IS NULL`kiértékelése `true` vagy hello tulajdonság nem létezik, vagy a tulajdonság értékét hello `null`.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-177">`property IS NULL` is evaluated as `true` if either hello property doesn't exist or hello property's value is `null`.</span></span>  
  
### <a name="property-evaluation-semantics"></a><span data-ttu-id="f5c5e-178">Tulajdonság értékelési szemantikájának módosítása</span><span class="sxs-lookup"><span data-stu-id="f5c5e-178">Property evaluation semantics</span></span>  
  
-   <span data-ttu-id="f5c5e-179">Egy nem létező rendszer tulajdonság jelez kísérlet tooevaluate egy [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) kivétel.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-179">An attempt tooevaluate a non-existent system property throws a [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) exception.</span></span>  
  
-   <span data-ttu-id="f5c5e-180">Egy nem létező tulajdonságot belsőleg történik **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-180">A property that does not exist is internally evaluated as **unknown**.</span></span>  
  
 <span data-ttu-id="f5c5e-181">Ismeretlen értékelése az aritmetikai operátor:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-181">Unknown evaluation in arithmetic operators:</span></span>  
  
-   <span data-ttu-id="f5c5e-182">A bináris operátor, ha bármelyik hello balra és/vagy operandusok jobb oldalán történik **ismeretlen**, majd hello eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-182">For binary operators, if either hello left and/or right side of operands is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
-   <span data-ttu-id="f5c5e-183">Az egyoperandusú operátorokat, ha egy operandus kiértékelése **ismeretlen**, majd hello eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-183">For unary operators, if an operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="f5c5e-184">A bináris összehasonlító operátorok ismeretlen értékelése:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-184">Unknown evaluation in binary comparison operators:</span></span>  
  
-   <span data-ttu-id="f5c5e-185">Ha bármelyik hello balra és/vagy operandusok jobb oldalán történik **ismeretlen**, majd hello eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-185">If either hello left and/or right side of operands is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="f5c5e-186">Az ismeretlen értékelési `[NOT] LIKE`:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-186">Unknown evaluation in `[NOT] LIKE`:</span></span>  
  
-   <span data-ttu-id="f5c5e-187">Ha bármely operandus kiértékelése **ismeretlen**, majd hello eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-187">If any operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="f5c5e-188">Az ismeretlen értékelési `[NOT] IN`:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-188">Unknown evaluation in `[NOT] IN`:</span></span>  
  
-   <span data-ttu-id="f5c5e-189">Ha a bal oldali operandusához hello kiértékelése **ismeretlen**, majd hello eredmény **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-189">If hello left operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="f5c5e-190">Az ismeretlen értékelési **és** operátor:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-190">Unknown evaluation in **AND** operator:</span></span>  
  
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
  
 <span data-ttu-id="f5c5e-191">Az ismeretlen értékelési **vagy** operátor:</span><span class="sxs-lookup"><span data-stu-id="f5c5e-191">Unknown evaluation in **OR** operator:</span></span>  
  
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
  
### <a name="operator-binding-semantics"></a><span data-ttu-id="f5c5e-192">Operátor kötés szemantikájának módosítása</span><span class="sxs-lookup"><span data-stu-id="f5c5e-192">Operator binding semantics</span></span>
  
-   <span data-ttu-id="f5c5e-193">Összehasonlító operátorok például `>`, `>=`, `<`, `<=`, `!=`, és `=` kövesse hello azonos szemantikákkal hello C# operátor adatok kötelező, írja be az előléptetések és implicit konverzió.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-193">Comparison operators such as `>`, `>=`, `<`, `<=`, `!=`, and `=` follow hello same semantics as hello C# operator binding in data type promotions and implicit conversions.</span></span>  
  
-   <span data-ttu-id="f5c5e-194">Aritmetikai operátor például `+`, `-`, `*`, `/`, és `%` kövesse hello azonos szemantikákkal hello C# operátor adatok kötelező, írja be az előléptetések és implicit konverzió.</span><span class="sxs-lookup"><span data-stu-id="f5c5e-194">Arithmetic operators such as `+`, `-`, `*`, `/`, and `%` follow hello same semantics as hello C# operator binding in data type promotions and implicit conversions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5c5e-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5c5e-195">Next steps</span></span>

- [<span data-ttu-id="f5c5e-196">SQLFilter osztály</span><span class="sxs-lookup"><span data-stu-id="f5c5e-196">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [<span data-ttu-id="f5c5e-197">SQLRuleAction osztály</span><span class="sxs-lookup"><span data-stu-id="f5c5e-197">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)