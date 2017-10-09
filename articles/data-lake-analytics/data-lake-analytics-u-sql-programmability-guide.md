---
title: "Azure Data Lake aaaU-SQL programozástámogatási útmutatója |} Microsoft Docs"
description: "További információk a hello beállítása az Azure Data Lake szolgáltatások, amelyek lehetővé teszik toocreate egy felhőalapú big Data típusú adatok platform."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: saveenr
ms.openlocfilehash: cc8f126234c6106a0dc633ce85a1d9ab1e634e30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-programmability-guide"></a><span data-ttu-id="dd70e-103">U-SQL programozástámogatási útmutató</span><span class="sxs-lookup"><span data-stu-id="dd70e-103">U-SQL programmability guide</span></span>

<span data-ttu-id="dd70e-104">U-SQL lekérdezésnyelvet, amely a big data-típusú munkaterhelések célja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-104">U-SQL is a query language that's designed for big data-type of workloads.</span></span> <span data-ttu-id="dd70e-105">U-SQL egyedi jellemzői hello egyik hello SQL-szerű nyelv deklaratív hello bővíthetőség és a C# által biztosított programozhatóság hello kombinációja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-105">One of hello unique features of U-SQL is hello combination of hello SQL-like declarative language with hello extensibility and programmability that's provided by C#.</span></span> <span data-ttu-id="dd70e-106">Az útmutató azt összpontosíthat hello bővítési és programozhatóság hello U-SQL nyelv, amely szerint a C# engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="dd70e-106">In this guide, we concentrate on hello extensibility and programmability of hello U-SQL language that's enabled by C#.</span></span>

## <a name="requirements"></a><span data-ttu-id="dd70e-107">Követelmények</span><span class="sxs-lookup"><span data-stu-id="dd70e-107">Requirements</span></span>

<span data-ttu-id="dd70e-108">Töltse le és telepítse [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="dd70e-108">Download and install [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="get-started-with-u-sql"></a><span data-ttu-id="dd70e-109">Ismerkedés a U-SQL</span><span class="sxs-lookup"><span data-stu-id="dd70e-109">Get started with U-SQL</span></span>  

<span data-ttu-id="dd70e-110">Nézzük hello U-SQL-parancsfájl a következő:</span><span class="sxs-lookup"><span data-stu-id="dd70e-110">Let’s look at hello following U-SQL script:</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso",   1500.0, "2017-03-39"),
            ("Woodgrove", 2700.0, "2017-04-10")
        ) AS 
              D( customer, amount );
@results =
    SELECT
        customer,
    amount,
    date
    FROM @a;    
```

<span data-ttu-id="dd70e-111">Meghatározza a sorhalmaz nevű @a , és létrehoz egy sorhalmaz nevű @results a @a.</span><span class="sxs-lookup"><span data-stu-id="dd70e-111">It defines a RowSet called @a and creates a RowSet called @results from @a.</span></span>

## <a name="c-types-and-expressions-in-u-sql-script"></a><span data-ttu-id="dd70e-112">C# típusok és kifejezések a U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="dd70e-112">C# types and expressions in U-SQL script</span></span>

<span data-ttu-id="dd70e-113">U-SQL-kifejezést, például a U-SQL logikai műveletekkel együtt egy C# kifejezés `AND`, `OR`, és `NOT`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-113">A U-SQL Expression is a C# expression combined with U-SQL logical operations such `AND`, `OR`, and `NOT`.</span></span> <span data-ttu-id="dd70e-114">Válassza ki, a KINYERÉSI, U-SQL kifejezések használhatók WHERE, GROUP BY és DEKLARÁLHATÓ.</span><span class="sxs-lookup"><span data-stu-id="dd70e-114">U-SQL Expressions can be used with SELECT, EXTRACT, WHERE, HAVING, GROUP BY and DECLARE.</span></span>

<span data-ttu-id="dd70e-115">Például a következő parancsfájl hello elemez egy karakterlánc hello SELECT záradékban lévő DateTime típusú értékké.</span><span class="sxs-lookup"><span data-stu-id="dd70e-115">For example, hello following script parses a string a DateTime value in hello SELECT clause.</span></span>

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

<span data-ttu-id="dd70e-116">hello következő parancsfájl elemzi a karakterlánc egy DateTime értéket egy DECLARE utasítást.</span><span class="sxs-lookup"><span data-stu-id="dd70e-116">hello following script parses a string a DateTime value in a DECLARE statement.</span></span>

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a><span data-ttu-id="dd70e-117">C# kifejezések használhatók adatok típuskonverziók</span><span class="sxs-lookup"><span data-stu-id="dd70e-117">Use C# expressions for data type conversions</span></span>
<span data-ttu-id="dd70e-118">hello a következő példa bemutatja, hogyan teheti a datetime adatok átalakítás C# kifejezések használatával.</span><span class="sxs-lookup"><span data-stu-id="dd70e-118">hello following example demonstrates how you can do a datetime data conversion by using C# expressions.</span></span> <span data-ttu-id="dd70e-119">Ez az adott esetben karakterlánc dátum és idő adata konvertált toostandard datetime éjfél 00:00:00 idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="dd70e-119">In this particular scenario, string datetime data is converted toostandard datetime with midnight 00:00:00 time notation.</span></span>

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 too@output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a><span data-ttu-id="dd70e-120">C# kifejezések használhatók mai dátum</span><span class="sxs-lookup"><span data-stu-id="dd70e-120">Use C# expressions for today’s date</span></span>
<span data-ttu-id="dd70e-121">toopull mai dátum is használhatók hello C# kifejezés a következő:</span><span class="sxs-lookup"><span data-stu-id="dd70e-121">toopull today’s date, we can use hello following C# expression:</span></span>

```
DateTime.Now.ToString("M/d/yyyy")
```

<span data-ttu-id="dd70e-122">Íme egy példa bemutatja, hogyan toouse a kifejezés egy parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="dd70e-122">Here's an example of how toouse this expression in a script:</span></span>

```
@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        user,
        des
    FROM @rs0
    GROUP BY user, des;
```



## <a name="using-net-assemblies"></a><span data-ttu-id="dd70e-123">.NET-szerelvények használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-123">Using .NET assemblies</span></span>
<span data-ttu-id="dd70e-124">U-SQL-kiterjeszthetőségi modell hello képességét tooadd egyéni kód fokozottan támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="dd70e-124">U-SQL’s extensibility model relies heavily on hello ability tooadd custom code.</span></span> <span data-ttu-id="dd70e-125">Jelenleg U-SQL biztosít módon tooadd a saját Microsoft. A NET-kód (különösen a C#).</span><span class="sxs-lookup"><span data-stu-id="dd70e-125">Currently, U-SQL provides you with easy ways tooadd your own Microsoft .NET-based code (in particular, C#).</span></span> <span data-ttu-id="dd70e-126">Azonban más .NET nyelven, például a VB.NET vagy F # írt egyéni kód is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-126">However, you can also add custom code that's written in other .NET languages, such as VB.NET or F#.</span></span> 

### <a name="register-a-net-assembly"></a><span data-ttu-id="dd70e-127">A .NET-szerelvény regisztrálása</span><span class="sxs-lookup"><span data-stu-id="dd70e-127">Register a .NET assembly</span></span>

<span data-ttu-id="dd70e-128">Hello CREATE ASSEMBLY utasítás tooplace egy .NET-szerelvényt a U-SQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="dd70e-128">Use hello CREATE ASSEMBLY statement tooplace a .NET assembly into a U-SQL Database.</span></span> <span data-ttu-id="dd70e-129">Amikor egy szerelvény-adatbázisban, U-SQL-parancsfájlok használhatja ezeket a szerelvények hello referencia szerelvény utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="dd70e-129">Once an assembly is in a database, U-SQL scripts can use those assemblies by using hello REFERENCE ASSEMBLY statement.</span></span> 

<span data-ttu-id="dd70e-130">a következő kód bemutatja hogyan hello tooregister egy szerelvény:</span><span class="sxs-lookup"><span data-stu-id="dd70e-130">hello following code shows how tooregister an assembly:</span></span>

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

<span data-ttu-id="dd70e-131">a következő kód bemutatja hogyan hello tooreference egy szerelvény:</span><span class="sxs-lookup"><span data-stu-id="dd70e-131">hello following code shows how tooreference an assembly:</span></span>

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

<span data-ttu-id="dd70e-132">Tekintse át a hello [szerelvény regisztrációs utasítások](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) , hogy ez a témakör részletesebben foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="dd70e-132">Consult hello [assembly registration instructions](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) that covers this topic in greater detail.</span></span>


### <a name="use-assembly-versioning"></a><span data-ttu-id="dd70e-133">Használja a szerelvény verziószámozása</span><span class="sxs-lookup"><span data-stu-id="dd70e-133">Use assembly versioning</span></span>
<span data-ttu-id="dd70e-134">Jelenleg a U-SQL hello .NET-keretrendszer 4.5-ös verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-134">Currently, U-SQL uses hello .NET Framework version 4.5.</span></span> <span data-ttu-id="dd70e-135">Ezért figyeljen oda arra, hogy a saját szerelvények kompatibilisek, hogy hello futásidejű verzióját.</span><span class="sxs-lookup"><span data-stu-id="dd70e-135">So ensure that your own assemblies are compatible with that version of hello runtime.</span></span>

<span data-ttu-id="dd70e-136">A korábbiak U-SQL kódot futtat egy 64 bites (x 64) formátumban.</span><span class="sxs-lookup"><span data-stu-id="dd70e-136">As mentioned earlier, U-SQL runs code in a 64-bit (x64) format.</span></span> <span data-ttu-id="dd70e-137">Ezért győződjön meg arról, hogy a kód a x64 lefordított toorun.</span><span class="sxs-lookup"><span data-stu-id="dd70e-137">So make sure that your code is compiled toorun on x64.</span></span> <span data-ttu-id="dd70e-138">Ellenkező esetben kap korábban bemutatott hello helytelen formátuma hibás.</span><span class="sxs-lookup"><span data-stu-id="dd70e-138">Otherwise you get hello incorrect format error shown earlier.</span></span>

<span data-ttu-id="dd70e-139">Minden egyes szerelvény dll-fájl feltöltése és erőforrás-fájl, például egy másik futásidejű, natív szerelvény vagy egy konfigurációs fájl legfeljebb 400 MB lehet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-139">Each uploaded assembly DLL and resource file, such as a different runtime, a native assembly, or a config file, can be at most 400 MB.</span></span> <span data-ttu-id="dd70e-140">hello telepített erőforrások telepítése erőforrás keresztül vagy hivatkozások tooassemblies és a további fájlok teljes mérete nem haladhatja meg a 3 GB.</span><span class="sxs-lookup"><span data-stu-id="dd70e-140">hello total size of deployed resources, either via DEPLOY RESOURCE or via references tooassemblies and their additional files, cannot exceed 3 GB.</span></span>

<span data-ttu-id="dd70e-141">Végül vegye figyelembe, hogy minden U-SQL-adatbázist csak tartalmazhat bármely adott valamely verzióját.</span><span class="sxs-lookup"><span data-stu-id="dd70e-141">Finally, note that each U-SQL database can only contain one version of any given assembly.</span></span> <span data-ttu-id="dd70e-142">Például mind a 7-es verzió, és a hello NewtonSoft Json.Net könyvtár 8 verziója van szükség, ha szüksége tooregister két különböző adatbázisokban őket.</span><span class="sxs-lookup"><span data-stu-id="dd70e-142">For example, if you need both version 7 and version 8 of hello NewtonSoft Json.Net library, you need tooregister them in two different databases.</span></span> <span data-ttu-id="dd70e-143">Továbbá minden parancsprogram csak hivatkozhatnak egy adott szerelvény DLL tooone verziója.</span><span class="sxs-lookup"><span data-stu-id="dd70e-143">Furthermore, each script can only refer tooone version of a given assembly DLL.</span></span> <span data-ttu-id="dd70e-144">Ebben a tekintetben a U-SQL következő hello C# szerelvény felügyeleti és versioning szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="dd70e-144">In this respect, U-SQL follows hello C# assembly management and versioning semantics.</span></span>


## <a name="use-user-defined-functions-udf"></a><span data-ttu-id="dd70e-145">Felhasználó által definiált függvények: UDF-ben</span><span class="sxs-lookup"><span data-stu-id="dd70e-145">Use user-defined functions: UDF</span></span>
<span data-ttu-id="dd70e-146">U-SQL-felhasználó által definiált függvények, illetve UDF, vannak programozási rutinok fogadnak el paramétereket, egy műveletet (például egy bonyolult számításhoz), amely hello értékként a művelet eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="dd70e-146">U-SQL user-defined functions, or UDF, are programming routines that accept parameters, perform an action (such as a complex calculation), and return hello result of that action as a value.</span></span> <span data-ttu-id="dd70e-147">hello vissza UDF értéke csak lehet egyetlen skaláris.</span><span class="sxs-lookup"><span data-stu-id="dd70e-147">hello return value of UDF can only be a single scalar.</span></span> <span data-ttu-id="dd70e-148">U-SQL UDF U-SQL alapvető parancsfájlban, mint bármely más C# skaláris függvényt hívható.</span><span class="sxs-lookup"><span data-stu-id="dd70e-148">U-SQL UDF can be called in U-SQL base script like any other C# scalar function.</span></span>

<span data-ttu-id="dd70e-149">Azt javasoljuk, hogy a U-SQL felhasználó által definiált függvények, inicializálni **nyilvános** és **statikus**.</span><span class="sxs-lookup"><span data-stu-id="dd70e-149">We recommend that you initialize U-SQL user-defined functions as **public** and **static**.</span></span>

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

<span data-ttu-id="dd70e-150">Első hello egyszerű példa egy UDF létrehozása vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="dd70e-150">First let’s look at hello simple example of creating a UDF.</span></span>

<span data-ttu-id="dd70e-151">Ilyenkor használati eset toodetermine hello időszakban, beleértve a hello pénzügyi negyedév és hello első bejelentkezés hello adott felhasználó pénzügyi hónapja kell.</span><span class="sxs-lookup"><span data-stu-id="dd70e-151">In this use-case scenario, we need toodetermine hello fiscal period, including hello fiscal quarter and fiscal month of hello first sign-in for hello specific user.</span></span> <span data-ttu-id="dd70e-152">hello esetünkben hello év első pénzügyi hónapja. június.</span><span class="sxs-lookup"><span data-stu-id="dd70e-152">hello first fiscal month of hello year in our scenario is June.</span></span>

<span data-ttu-id="dd70e-153">toocalculate időszakban, a következő C# függvény hello bemutatása után:</span><span class="sxs-lookup"><span data-stu-id="dd70e-153">toocalculate fiscal period, we introduce hello following C# function:</span></span>

```
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

<span data-ttu-id="dd70e-154">Egyszerűen csak a pénzügyi hónap és negyedév számítja ki, és egy karakterláncértéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="dd70e-154">It simply calculates fiscal month and quarter and returns a string value.</span></span> <span data-ttu-id="dd70e-155">A júniusi, hello első hónapja, hello pénzügyi negyedév első, "Q1:P1" használjuk.</span><span class="sxs-lookup"><span data-stu-id="dd70e-155">For June, hello first month of hello first fiscal quarter, we use "Q1:P1".</span></span> <span data-ttu-id="dd70e-156">Júliusi "Q1:P2" felhasználása, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="dd70e-156">For July, we use "Q1:P2", and so on.</span></span>

<span data-ttu-id="dd70e-157">Ez az a rendszeres C# függvény folyamatos toouse dolgozunk a U-SQL projekt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-157">This is a regular C# function that we are going toouse in our U-SQL project.</span></span>

<span data-ttu-id="dd70e-158">Íme hello háttérkód szakasz megjelenését az ebben a forgatókönyvben:</span><span class="sxs-lookup"><span data-stu-id="dd70e-158">Here is how hello code-behind section looks in this scenario:</span></span>

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }

    }

}
```

<span data-ttu-id="dd70e-159">Most fogjuk toocall ezt a funkciót a hello alap U-SQL parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="dd70e-159">Now we are going toocall this function from hello base U-SQL script.</span></span> <span data-ttu-id="dd70e-160">toodo, kell tooprovide hello funkciót, beleértve hello névtér, amely a jelen esetben ez NameSpace.Class.Function(parameter) egy teljesen minősített nevet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-160">toodo this, we have tooprovide a fully qualified name for hello function, including hello namespace, which in this case is NameSpace.Class.Function(parameter).</span></span>

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

<span data-ttu-id="dd70e-161">Az alábbiakban látható hello tényleges U-SQL alapvető parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-161">Following is hello actual U-SQL base script:</span></span>

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

<span data-ttu-id="dd70e-162">Az alábbiakban látható hello kimeneti fájl hello parancsfájl végrehajtásának:</span><span class="sxs-lookup"><span data-stu-id="dd70e-162">Following is hello output file of hello script execution:</span></span>

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

<span data-ttu-id="dd70e-163">Ez a példa azt mutatja be, a egyszerű beágyazott UDF U-SQL-használat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-163">This example demonstrates a simple usage of inline UDF in U-SQL.</span></span>

### <a name="keep-state-between-udf-invocations"></a><span data-ttu-id="dd70e-164">Között UDF állapotban tartása</span><span class="sxs-lookup"><span data-stu-id="dd70e-164">Keep state between UDF invocations</span></span>
<span data-ttu-id="dd70e-165">U-SQL C# programozhatóság objektumok is lehet több kifinomult, interaktív keresztül hello háttérkód globális változókat használ.</span><span class="sxs-lookup"><span data-stu-id="dd70e-165">U-SQL C# programmability objects can be more sophisticated, utilizing interactivity through hello code-behind global variables.</span></span> <span data-ttu-id="dd70e-166">A következő üzleti használati eset forgatókönyv hello vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="dd70e-166">Let’s look at hello following business use-case scenario.</span></span>

<span data-ttu-id="dd70e-167">Nagy méretű szervezeteknek, a felhasználók belső alkalmazások fajtáinak válthat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-167">In large organizations, users can switch between varieties of internal applications.</span></span> <span data-ttu-id="dd70e-168">Ezek lehetnek Microsoft Dynamics CRM-, Power BI, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="dd70e-168">These can include Microsoft Dynamics CRM, PowerBI, and so on.</span></span> <span data-ttu-id="dd70e-169">Az ügyfelek érdemes tooapply a telemetriai adatok elemzését hogyan felhasználók váltogatnak a különböző alkalmazások között, milyen hello használati trendeket vannak, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="dd70e-169">Customers might want tooapply a telemetry analysis of how users switch between different applications, what hello usage trends are, and so on.</span></span> <span data-ttu-id="dd70e-170">hello hello vállalati célja toooptimize alkalmazások használatáról.</span><span class="sxs-lookup"><span data-stu-id="dd70e-170">hello goal for hello business is toooptimize application usage.</span></span> <span data-ttu-id="dd70e-171">Akkor is érdemes toocombine különböző alkalmazások vagy adott bejelentkezés rutinok.</span><span class="sxs-lookup"><span data-stu-id="dd70e-171">They also might want toocombine different applications or specific sign-on routines.</span></span>

<span data-ttu-id="dd70e-172">tooachieve ezen cél esetében tudunk toodetermine munkamenet-azonosítók és időbeli hello történt legutóbbi munkamenet között.</span><span class="sxs-lookup"><span data-stu-id="dd70e-172">tooachieve this goal, we have toodetermine session IDs and lag time between hello last session that occurred.</span></span>

<span data-ttu-id="dd70e-173">Azt egy előző bejelentkezés toofind kell, és hozzárendelheti a létrehozott toohello folyamatban van a bejelentkezési tooall munkamenetek ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="dd70e-173">We need toofind a previous sign-in and then assign this sign-in tooall sessions that are being generated toohello same application.</span></span> <span data-ttu-id="dd70e-174">hello első kérdés, hogy alap U-SQL-parancsfájl nem lehetővé teszik a számunkra tooapply számítások már számított oszlopokat tartalmazó LAG függvény keresztül.</span><span class="sxs-lookup"><span data-stu-id="dd70e-174">hello first challenge is that U-SQL base script doesn't allow us tooapply calculations over already-calculated columns with LAG function.</span></span> <span data-ttu-id="dd70e-175">hello második probléma, hogy rendelkezik-e tookeep hello adott munkamenet hello belül minden munkamenetben időközhöz.</span><span class="sxs-lookup"><span data-stu-id="dd70e-175">hello second challenge is that we have tookeep hello specific session for all sessions within hello same time period.</span></span>

<span data-ttu-id="dd70e-176">toosolve Ez a probléma a háttérkód szakaszon belül egy globális változó használjuk: `static public string globalSession;`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-176">toosolve this problem, we use a global variable inside a code-behind section: `static public string globalSession;`.</span></span>

<span data-ttu-id="dd70e-177">A globális változó alkalmazott toohello teljes sorhalmaz a parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="dd70e-177">This global variable is applied toohello entire rowset during our script execution.</span></span>

<span data-ttu-id="dd70e-178">A U-SQL program hello háttérkód szakasza a következő:</span><span class="sxs-lookup"><span data-stu-id="dd70e-178">Here is hello code-behind section of our U-SQL program:</span></span>

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

<span data-ttu-id="dd70e-179">Ez a példa azt mutatja be a globális változó hello `static public string globalSession;` hello belül használt `getStampUserSession` és az első újrainicializálni minden alkalommal hello munkamenet paraméter módosul.</span><span class="sxs-lookup"><span data-stu-id="dd70e-179">This example shows hello global variable `static public string globalSession;` used inside hello `getStampUserSession` function and getting reinitialized each time hello Session parameter is changed.</span></span>

<span data-ttu-id="dd70e-180">hello alap U-SQL-parancsfájl a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="dd70e-180">hello U-SQL base script is as follows:</span></span>

```
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    too@out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

<span data-ttu-id="dd70e-181">Függvény `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` hello második memória sorhalmaz számítás során itt nevezik.</span><span class="sxs-lookup"><span data-stu-id="dd70e-181">Function `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` is called here during hello second memory rowset calculation.</span></span> <span data-ttu-id="dd70e-182">Hello haladnak `UserSessionTimestamp` oszlop és értéket ad vissza értéket, amíg hello `UserSessionTimestamp` megváltozott.</span><span class="sxs-lookup"><span data-stu-id="dd70e-182">It passes hello `UserSessionTimestamp` column and returns hello value until `UserSessionTimestamp` has changed.</span></span>

<span data-ttu-id="dd70e-183">hello kimeneti fájl a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="dd70e-183">hello output file is as follows:</span></span>

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

<span data-ttu-id="dd70e-184">Ez a példa egy bonyolultabb használati eset forgatókönyv, amelyben azt globális változók használata belül alkalmazott toohello teljes memória sorkészlet háttérkód szakasz mutatja be.</span><span class="sxs-lookup"><span data-stu-id="dd70e-184">This example demonstrates a more complicated use-case scenario in which we use a global variable inside a code-behind section that's applied toohello entire memory rowset.</span></span>

## <a name="use-user-defined-types-udt"></a><span data-ttu-id="dd70e-185">Felhasználó által definiált tárolótípus: UDT</span><span class="sxs-lookup"><span data-stu-id="dd70e-185">Use user-defined types: UDT</span></span>
<span data-ttu-id="dd70e-186">Felhasználó által definiált típusok vagy UDT, egy másik programozástámogatás az U-SQL.</span><span class="sxs-lookup"><span data-stu-id="dd70e-186">User-defined types, or UDT, is another programmability feature of U-SQL.</span></span> <span data-ttu-id="dd70e-187">U-SQL UDT úgy viselkedik, mint egy normál C# felhasználó által definiált típus.</span><span class="sxs-lookup"><span data-stu-id="dd70e-187">U-SQL UDT acts like a regular C# user-defined type.</span></span> <span data-ttu-id="dd70e-188">C# egy szigorú típusmegadású nyelv, amely lehetővé teszi a beépített és egyéni, felhasználó által definiált típusok hello használatát.</span><span class="sxs-lookup"><span data-stu-id="dd70e-188">C# is a strongly typed language that allows hello use of built-in and custom user-defined types.</span></span>

<span data-ttu-id="dd70e-189">U-SQL implicit módon nem szerializálható vagy deszerializálható tetszőleges UDTs, ha a hello UDT csúcsban lévő készletek között.</span><span class="sxs-lookup"><span data-stu-id="dd70e-189">U-SQL cannot implicitly serialize or de-serialize arbitrary UDTs when hello UDT is passed between vertices in rowsets.</span></span> <span data-ttu-id="dd70e-190">Ez azt jelenti, hogy hello felhasználó tooprovide egy explicit formázó hello IFormatter felület használatával.</span><span class="sxs-lookup"><span data-stu-id="dd70e-190">This means that hello user has tooprovide an explicit formatter by using hello IFormatter interface.</span></span> <span data-ttu-id="dd70e-191">Ez biztosít a U-SQL hello szerializáltuk és hello UDT módszert.</span><span class="sxs-lookup"><span data-stu-id="dd70e-191">This provides U-SQL with hello serialize and de-serialize methods for hello UDT.</span></span>

> [!NOTE]
> <span data-ttu-id="dd70e-192">U-SQL beépített vagyis outputters jelenleg szerializálni és nem deszerializálható UDT adatok tooor hello IFormatter beállítása mellett is fájlokból.</span><span class="sxs-lookup"><span data-stu-id="dd70e-192">U-SQL’s built-in extractors and outputters currently cannot serialize or de-serialize UDT data tooor from files even with hello IFormatter set.</span></span> <span data-ttu-id="dd70e-193">Ezért amikor UDT tooa adatfájlban hello kimeneti utasítás ír le, vagy az egy kivonatoló adatellenőrzés toopass rendelkezik egy karakterlánc- vagy bájt tömbként azt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-193">So when you're writing UDT data tooa file with hello OUTPUT statement, or reading it with an extractor, you have toopass it as a string or byte array.</span></span> <span data-ttu-id="dd70e-194">Ezután hello szerializálási hívja, és kifejezetten deszerializálási kódot (Ez azt jelenti, hogy hello UDT ToString() módszer).</span><span class="sxs-lookup"><span data-stu-id="dd70e-194">Then you call hello serialization and deserialization code (that is, hello UDT’s ToString() method) explicitly.</span></span> <span data-ttu-id="dd70e-195">Vagyis felhasználói és outputters, a más hello, olvasási és írási UDTs oldalon is.</span><span class="sxs-lookup"><span data-stu-id="dd70e-195">User-defined extractors and outputters, on hello other hand, can read and write UDTs.</span></span>

<span data-ttu-id="dd70e-196">Ha azt próbálja toouse UDT készülék vagy OUTPUTTER (kívül előző SELECT), az itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="dd70e-196">If we try toouse UDT in EXTRACTOR or OUTPUTTER (out of previous SELECT), as shown here:</span></span>

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

<span data-ttu-id="dd70e-197">Hiba a következő hello érkező:</span><span class="sxs-lookup"><span data-stu-id="dd70e-197">We receive hello following error:</span></span>

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used toooutput column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how tooserialize this type, or call a serialization method on hello type in
hello preceding SELECT. C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

<span data-ttu-id="dd70e-198">a outputter az UDT toowork, vagy tudunk tooserialize azt a toostring ToString() metódus hello, vagy hozzon létre egy egyéni outputter.</span><span class="sxs-lookup"><span data-stu-id="dd70e-198">toowork with UDT in outputter, we either have tooserialize it toostring with hello ToString() method or create a custom outputter.</span></span>

<span data-ttu-id="dd70e-199">UDTs jelenleg nem használható GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="dd70e-199">UDTs currently cannot be used in GROUP BY.</span></span> <span data-ttu-id="dd70e-200">Ha UDT használja a GROUP BY, hello a következő hiba történt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-200">If UDT is used in GROUP BY, hello following error is thrown:</span></span>

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want toouse with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

<span data-ttu-id="dd70e-201">egy UDT toodefine, igazolnia kell:</span><span class="sxs-lookup"><span data-stu-id="dd70e-201">toodefine a UDT, we have to:</span></span>

* <span data-ttu-id="dd70e-202">Adja hozzá a következő névterek hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-202">Add hello following namespaces:</span></span>

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* <span data-ttu-id="dd70e-203">Adja hozzá `Microsoft.Analytics.Interfaces`, ami azonban szükséges az hello UDT felületek.</span><span class="sxs-lookup"><span data-stu-id="dd70e-203">Add `Microsoft.Analytics.Interfaces`, which is required for hello UDT interfaces.</span></span> <span data-ttu-id="dd70e-204">Ezenkívül `System.IO` szükséges toodefine hello IFormatter-felület lehet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-204">In addition, `System.IO` might be needed toodefine hello IFormatter interface.</span></span>

* <span data-ttu-id="dd70e-205">Adja meg a SqlUserDefinedType attribútum által definiált típus.</span><span class="sxs-lookup"><span data-stu-id="dd70e-205">Define a used-defined type with SqlUserDefinedType attribute.</span></span>

<span data-ttu-id="dd70e-206">**SqlUserDefinedType** U-SQL-ben a felhasználó által definiált típus (UDT) szerelvények típusdefinícióban használt toomark van.</span><span class="sxs-lookup"><span data-stu-id="dd70e-206">**SqlUserDefinedType** is used toomark a type definition in an assembly as a user-defined type (UDT) in U-SQL.</span></span> <span data-ttu-id="dd70e-207">hello attribútum hello tulajdonságainak hello UDT fizikai jellemzőivel hello tükrözik.</span><span class="sxs-lookup"><span data-stu-id="dd70e-207">hello properties on hello attribute reflect hello physical characteristics of hello UDT.</span></span> <span data-ttu-id="dd70e-208">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-208">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-209">SqlUserDefinedType UDT definíciójának egyik kötelező attribútuma.</span><span class="sxs-lookup"><span data-stu-id="dd70e-209">SqlUserDefinedType is a required attribute for UDT definition.</span></span>

<span data-ttu-id="dd70e-210">hello konstruktor hello osztály:</span><span class="sxs-lookup"><span data-stu-id="dd70e-210">hello constructor of hello class:</span></span>  

* <span data-ttu-id="dd70e-211">SqlUserDefinedTypeAttribute (típus formázó)</span><span class="sxs-lookup"><span data-stu-id="dd70e-211">SqlUserDefinedTypeAttribute (type formatter)</span></span>

* <span data-ttu-id="dd70e-212">Írja be a formázó: kötelező paraméter toodefine az UDT formázó – hello pontosabban hello típusú `IFormatter` felület itt kell átadni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-212">Type formatter: Required parameter toodefine an UDT formatter--specifically, hello type of hello `IFormatter` interface must be passed here.</span></span>

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* <span data-ttu-id="dd70e-213">Tipikus UDT ezenkívül definition hello IFormatter illesztőfelület, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-213">Typical UDT also requires definition of hello IFormatter interface, as shown in hello following example:</span></span>

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

<span data-ttu-id="dd70e-214">Hello `IFormatter` felület rendezi sorba, és vonja rendezi sorba hello legfelső szintű típusú objektumdiagram \<typeparamref name = "T" >.</span><span class="sxs-lookup"><span data-stu-id="dd70e-214">hello `IFormatter` interface serializes and de-serializes an object graph with hello root type of \<typeparamref name="T">.</span></span>

<span data-ttu-id="dd70e-215">\<típusparaméter neve = "T" > hello object graph tooserialize legfelső szintű típus hello és deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-215">\<typeparam name="T">hello root type for hello object graph tooserialize and de-serialize.</span></span>

* <span data-ttu-id="dd70e-216">**Deszerializálni**: deszerializálni rendezi sorba hello adatokat a megadott hello adatfolyamok és reconstitutes hello graph-objektumok.</span><span class="sxs-lookup"><span data-stu-id="dd70e-216">**Deserialize**: De-serializes hello data on hello provided stream and reconstitutes hello graph of objects.</span></span>

* <span data-ttu-id="dd70e-217">**Szerializálható**: rendezi sorba az objektum, vagy graph-objektumok, a legfelső szintű megadott toohello adatfolyam megadott hello.</span><span class="sxs-lookup"><span data-stu-id="dd70e-217">**Serialize**: Serializes an object, or graph of objects, with hello given root toohello provided stream.</span></span>

<span data-ttu-id="dd70e-218">`MyType`példány: hello típus példánya.</span><span class="sxs-lookup"><span data-stu-id="dd70e-218">`MyType` instance: Instance of hello type.</span></span>  
<span data-ttu-id="dd70e-219">`IColumnWriter`író / `IColumnReader` olvasó: hello az alapul szolgáló oszlop adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="dd70e-219">`IColumnWriter` writer / `IColumnReader` reader: hello underlying column stream.</span></span>  
<span data-ttu-id="dd70e-220">`ISerializationContext`a környezetben: származó hello cél- és környezeti hello adatfolyamhoz megadja a szerializálás során jelzők álló készletet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="dd70e-220">`ISerializationContext` context: Enum that defines a set of flags that specifies hello source or destination context for hello stream during serialization.</span></span>

* <span data-ttu-id="dd70e-221">**Köztes**: Megadja, hogy adott hello cél- és a környezetben nem megőrzött tároló.</span><span class="sxs-lookup"><span data-stu-id="dd70e-221">**Intermediate**: Specifies that hello source or destination context is not a persisted store.</span></span>

* <span data-ttu-id="dd70e-222">**Adatmegőrzési**: Megadja, hogy hello cél- és környezetben egy megőrzött tárolóban.</span><span class="sxs-lookup"><span data-stu-id="dd70e-222">**Persistence**: Specifies that hello source or destination context is a persisted store.</span></span>

<span data-ttu-id="dd70e-223">Rendszeres C# típus, egy U-SQL UDT definition például felülbírálások az operátorok, mint / == /! =.</span><span class="sxs-lookup"><span data-stu-id="dd70e-223">As a regular C# type, a U-SQL UDT definition can include overrides for operators such as +/==/!=.</span></span> <span data-ttu-id="dd70e-224">Statikus metódusok is tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-224">It can also include static methods.</span></span> <span data-ttu-id="dd70e-225">Például fogjuk toouse az UDT mint egy paraméterrel tooa U-SQL MIN összesítő függvény, hogy toodefine < operátor felülbírálás.</span><span class="sxs-lookup"><span data-stu-id="dd70e-225">For example, if we are going toouse this UDT as a parameter tooa U-SQL MIN aggregate function, we have toodefine < operator override.</span></span>

<span data-ttu-id="dd70e-226">Című szakaszában talál pénzügyi időszak azonosításához az adott dátum hello hello formátumban (Q1:P10) Qn:Pn példa azt mutatja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-226">Earlier in this guide, we demonstrated an example for fiscal period identification from hello specific date in hello format Qn:Pn (Q1:P10).</span></span> <span data-ttu-id="dd70e-227">hello a következő példa bemutatja, hogyan toodefine egyéni írja be a pénzügyi küszöbidőszakok értékeit.</span><span class="sxs-lookup"><span data-stu-id="dd70e-227">hello following example shows how toodefine a custom type for fiscal period values.</span></span>

<span data-ttu-id="dd70e-228">Az alábbiakban látható egy példa egy egyéni UDT és IFormatter felülettel háttérkód szakaszt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-228">Following is an example of a code-behind section with custom UDT and IFormatter interface:</span></span>

```
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

<span data-ttu-id="dd70e-229">hello definiált típust tartalmaz két szám: negyedév, hónap és.</span><span class="sxs-lookup"><span data-stu-id="dd70e-229">hello defined type includes two numbers: quarter and month.</span></span> <span data-ttu-id="dd70e-230">Operátorok == /! = / > / < és statikus metódus ToString() itt meghatározása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-230">Operators ==/!=/>/< and static method ToString() are defined here.</span></span>

<span data-ttu-id="dd70e-231">Amint azt korábban említettük, az UDT kiválasztási kifejezések használhatók, de nem használható OUTPUTTER/KIVONATOLÓ egyedi szerializálás nélkül.</span><span class="sxs-lookup"><span data-stu-id="dd70e-231">As mentioned earlier, UDT can be used in SELECT expressions, but cannot be used in OUTPUTTER/EXTRACTOR without custom serialization.</span></span> <span data-ttu-id="dd70e-232">Vagy rendelkezik toobe szerializálható kézjegyként ToString() karakterláncnak vagy egy egyéni OUTPUTTER KIVONATOLÓ használni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-232">It either has toobe serialized as a string with ToString() or used with a custom OUTPUTTER/EXTRACTOR.</span></span>

<span data-ttu-id="dd70e-233">Most tegyük a UDT használatát tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-233">Now let’s discuss usage of UDT.</span></span> <span data-ttu-id="dd70e-234">A háttérkód szakasz változtattuk a GetFiscalPeriod függvény toohello következő:</span><span class="sxs-lookup"><span data-stu-id="dd70e-234">In a code-behind section, we changed our GetFiscalPeriod function toohello following:</span></span>

```
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

<span data-ttu-id="dd70e-235">Ahogy látja, a FiscalPeriod típus hello értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="dd70e-235">As you can see, it returns hello value of our FiscalPeriod type.</span></span>

<span data-ttu-id="dd70e-236">Itt nyújtunk hogyan toouse azt a további alap U-SQL-parancsfájl egy példát.</span><span class="sxs-lookup"><span data-stu-id="dd70e-236">Here we provide an example of how toouse it further in U-SQL base script.</span></span> <span data-ttu-id="dd70e-237">Ebben a példában a U-SQL parancsfájl UDT foglalja különböző űrlapok mutatja be.</span><span class="sxs-lookup"><span data-stu-id="dd70e-237">This example demonstrates different forms of UDT invocation from U-SQL script.</span></span>

```
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
           start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in hello prior SELECT.  Passing hello UDT toothis subsequent SELECT would have failed if hello UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    too@output_file 
    USING Outputters.Text();
```

<span data-ttu-id="dd70e-238">Íme egy példa a teljes háttérkód szakasz:</span><span class="sxs-lookup"><span data-stu-id="dd70e-238">Here's an example of a full code-behind section:</span></span>

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a><span data-ttu-id="dd70e-239">Felhasználó által definiált összesítések használata: UDAGG</span><span class="sxs-lookup"><span data-stu-id="dd70e-239">Use user-defined aggregates: UDAGG</span></span>
<span data-ttu-id="dd70e-240">Felhasználó által definiált összesítések bármely összesítési funkciókat, amelyek nem képezte a a-kész U-SQL.</span><span class="sxs-lookup"><span data-stu-id="dd70e-240">User-defined aggregates are any aggregation-related functions that are not shipped out-of-the-box with U-SQL.</span></span> <span data-ttu-id="dd70e-241">hello példa is lehet egy összesített tooperform egyéni matematikai számítások, karakterláncok összefűzése feldolgozás karakterláncok, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="dd70e-241">hello example can be an aggregate tooperform custom math calculations, string concatenations, manipulations with strings, and so on.</span></span>

<span data-ttu-id="dd70e-242">hello felhasználó által definiált összesítő alaposztály definíciója a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="dd70e-242">hello user-defined aggregate base class definition is as follows:</span></span>

```c#
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

<span data-ttu-id="dd70e-243">**SqlUserDefinedAggregate** azt jelzi, hogy hello típusa, a felhasználó által definiált összesítő kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-243">**SqlUserDefinedAggregate** indicates that hello type should be registered as a user-defined aggregate.</span></span> <span data-ttu-id="dd70e-244">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-244">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-245">Az attribútum SqlUserDefinedType **választható** UDAGG definíciójának.</span><span class="sxs-lookup"><span data-stu-id="dd70e-245">SqlUserDefinedType attribute is **optional** for UDAGG definition.</span></span>


<span data-ttu-id="dd70e-246">hello alaposztály lehetővé teszi a három toopass absztrakt paraméterek: két bemeneti paraméterek, a másik hello eredményként.</span><span class="sxs-lookup"><span data-stu-id="dd70e-246">hello base class allows you toopass three abstract parameters: two as input parameters and one as hello result.</span></span> <span data-ttu-id="dd70e-247">hello adattípusok változóak, és meg kell határozni az osztályöröklődést során.</span><span class="sxs-lookup"><span data-stu-id="dd70e-247">hello data types are variable and should be defined during class inheritance.</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* <span data-ttu-id="dd70e-248">**Init** hív meg, egyszer az egyes csoportok számítás során.</span><span class="sxs-lookup"><span data-stu-id="dd70e-248">**Init** invokes once for each group during computation.</span></span> <span data-ttu-id="dd70e-249">Egy inicializálási rutin biztosít az egyes összesítést.</span><span class="sxs-lookup"><span data-stu-id="dd70e-249">It provides an initialization routine for each aggregation group.</span></span>  
* <span data-ttu-id="dd70e-250">**Felhalmozhat** minden egyes értékre egyszer végrehajtja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-250">**Accumulate** is executed once for each value.</span></span> <span data-ttu-id="dd70e-251">Hello összesítési algoritmushoz hello fő funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="dd70e-251">It provides hello main functionality for hello aggregation algorithm.</span></span> <span data-ttu-id="dd70e-252">Lehet, hogy a használt tooaggregate értékek osztályöröklődést során meghatározott különböző adattípusokkal.</span><span class="sxs-lookup"><span data-stu-id="dd70e-252">It can be used tooaggregate values with various data types that are defined during class inheritance.</span></span> <span data-ttu-id="dd70e-253">Változó adattípust két paramétert fogadja a.</span><span class="sxs-lookup"><span data-stu-id="dd70e-253">It can accept two parameters of variable data types.</span></span>
* <span data-ttu-id="dd70e-254">**Állítsa le** hello végén toooutput hello eredmény az egyes csoportok feldolgozásának összesítő csoportonként egyszer végrehajtja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-254">**Terminate** is executed once per aggregation group at hello end of processing toooutput hello result for each group.</span></span>

<span data-ttu-id="dd70e-255">toodeclare megfelelő bemeneti és kimeneti adattípus hello osztálykiterjesztések definíciója a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="dd70e-255">toodeclare correct input and output data types, use hello class definition as follows:</span></span>

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* <span data-ttu-id="dd70e-256">A T1: Első paraméter tooaccumulate</span><span class="sxs-lookup"><span data-stu-id="dd70e-256">T1: First parameter tooaccumulate</span></span>
* <span data-ttu-id="dd70e-257">T2: Első tooaccumulate a paraméter</span><span class="sxs-lookup"><span data-stu-id="dd70e-257">T2: First parameter tooaccumulate</span></span>
* <span data-ttu-id="dd70e-258">TResult: Visszatérési típusa megszakítás miatt</span><span class="sxs-lookup"><span data-stu-id="dd70e-258">TResult: Return type of terminate</span></span>

<span data-ttu-id="dd70e-259">Példa:</span><span class="sxs-lookup"><span data-stu-id="dd70e-259">For example:</span></span>

```
public class GuidAggregate : IAggregate<string, int, int>
```

<span data-ttu-id="dd70e-260">vagy</span><span class="sxs-lookup"><span data-stu-id="dd70e-260">or</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a><span data-ttu-id="dd70e-261">A U-SQL UDAGG használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-261">Use UDAGG in U-SQL</span></span>
<span data-ttu-id="dd70e-262">toouse UDAGG, először meg háttérkód vagy hello létező programozhatóság DLL-ből hivatkozni rá, a fentiekben taglaltak.</span><span class="sxs-lookup"><span data-stu-id="dd70e-262">toouse UDAGG, first define it in code-behind or reference it from hello existent programmability DLL as discussed earlier.</span></span>

<span data-ttu-id="dd70e-263">Kövesse a következő szintaxist hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-263">Then use hello following syntax:</span></span>

```
AGG<UDAGG_functionname>(param1,param2)
```

<span data-ttu-id="dd70e-264">Íme egy példa UDAGG:</span><span class="sxs-lookup"><span data-stu-id="dd70e-264">Here is an example of UDAGG:</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

<span data-ttu-id="dd70e-265">És a következő alap U-SQL-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-265">And base U-SQL script:</span></span>

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

<span data-ttu-id="dd70e-266">Használati eset ebben a forgatókönyvben azt összefűzésére osztály GUID hello bizonyos felhasználók részére.</span><span class="sxs-lookup"><span data-stu-id="dd70e-266">In this use-case scenario, we concatenate class GUIDs for hello specific users.</span></span>

## <a name="use-user-defined-objects-udo"></a><span data-ttu-id="dd70e-267">Felhasználó által definiált-objektumokat használ: UDO</span><span class="sxs-lookup"><span data-stu-id="dd70e-267">Use user-defined objects: UDO</span></span>
<span data-ttu-id="dd70e-268">U-SQL lehetővé teszi toodefine egyéni programozhatóság objektumokat, amely a felhasználói objektumok vagy UDO nevezzük.</span><span class="sxs-lookup"><span data-stu-id="dd70e-268">U-SQL enables you toodefine custom programmability objects, which are called user-defined objects or UDO.</span></span>

<span data-ttu-id="dd70e-269">hello az alábbiakban látható a U-SQL UDO listáját:</span><span class="sxs-lookup"><span data-stu-id="dd70e-269">hello following is a list of UDO in U-SQL:</span></span>

* <span data-ttu-id="dd70e-270">Vagyis felhasználói</span><span class="sxs-lookup"><span data-stu-id="dd70e-270">User-defined extractors</span></span>
    * <span data-ttu-id="dd70e-271">Soronként extract</span><span class="sxs-lookup"><span data-stu-id="dd70e-271">Extract row by row</span></span>
    * <span data-ttu-id="dd70e-272">Tooimplement adatok kinyerése a egyéni strukturált fájlokban használt</span><span class="sxs-lookup"><span data-stu-id="dd70e-272">Used tooimplement data extraction from custom structured files</span></span>

* <span data-ttu-id="dd70e-273">Felhasználó által definiált outputters</span><span class="sxs-lookup"><span data-stu-id="dd70e-273">User-defined outputters</span></span>
    * <span data-ttu-id="dd70e-274">Kimeneti soronként</span><span class="sxs-lookup"><span data-stu-id="dd70e-274">Output row by row</span></span>
    * <span data-ttu-id="dd70e-275">Egyéni adattípusok toooutput vagy egyéni fájlformátumok</span><span class="sxs-lookup"><span data-stu-id="dd70e-275">Used toooutput custom data types or custom file formats</span></span>

* <span data-ttu-id="dd70e-276">Felhasználó által definiált processzor</span><span class="sxs-lookup"><span data-stu-id="dd70e-276">User-defined processors</span></span>
    * <span data-ttu-id="dd70e-277">Egy olyan sor, majd előállítanak egy sor</span><span class="sxs-lookup"><span data-stu-id="dd70e-277">Take one row and produce one row</span></span>
    * <span data-ttu-id="dd70e-278">Használt tooreduce hello számát vagy a termék új oszlopainak egy meglévő oszlop készletből származó értékekkel</span><span class="sxs-lookup"><span data-stu-id="dd70e-278">Used tooreduce hello number of columns or produce new columns with values that are derived from an existing column set</span></span>

* <span data-ttu-id="dd70e-279">Felhasználó által definiált appliers</span><span class="sxs-lookup"><span data-stu-id="dd70e-279">User-defined appliers</span></span>
    * <span data-ttu-id="dd70e-280">Egy olyan sor, majd előállítanak 0 toon sor</span><span class="sxs-lookup"><span data-stu-id="dd70e-280">Take one row and produce 0 toon rows</span></span>
    * <span data-ttu-id="dd70e-281">KÜLSŐ/határokon ALKALMAZ együtt</span><span class="sxs-lookup"><span data-stu-id="dd70e-281">Used with OUTER/CROSS APPLY</span></span>

* <span data-ttu-id="dd70e-282">Felhasználó által definiált combiners</span><span class="sxs-lookup"><span data-stu-id="dd70e-282">User-defined combiners</span></span>
    * <span data-ttu-id="dd70e-283">Egyesíti a sorkészletek – felhasználó által definiált illesztéseket</span><span class="sxs-lookup"><span data-stu-id="dd70e-283">Combines rowsets--user-defined JOINs</span></span>

* <span data-ttu-id="dd70e-284">Felhasználó által definiált szűkítő</span><span class="sxs-lookup"><span data-stu-id="dd70e-284">User-defined reducers</span></span>
    * <span data-ttu-id="dd70e-285">N sort, majd előállítanak egy sor</span><span class="sxs-lookup"><span data-stu-id="dd70e-285">Take n rows and produce one row</span></span>
    * <span data-ttu-id="dd70e-286">Használt tooreduce hello sorok száma</span><span class="sxs-lookup"><span data-stu-id="dd70e-286">Used tooreduce hello number of rows</span></span>

<span data-ttu-id="dd70e-287">UDO neve általában explicit módon a U-SQL parancsfájl U-SQL-utasítások a következő hello részeként:</span><span class="sxs-lookup"><span data-stu-id="dd70e-287">UDO is typically called explicitly in U-SQL script as part of hello following U-SQL statements:</span></span>

* <span data-ttu-id="dd70e-288">EXTRACT</span><span class="sxs-lookup"><span data-stu-id="dd70e-288">EXTRACT</span></span>
* <span data-ttu-id="dd70e-289">KIMENETI</span><span class="sxs-lookup"><span data-stu-id="dd70e-289">OUTPUT</span></span>
* <span data-ttu-id="dd70e-290">FOLYAMAT</span><span class="sxs-lookup"><span data-stu-id="dd70e-290">PROCESS</span></span>
* <span data-ttu-id="dd70e-291">CSOPORTOSÍTÁS</span><span class="sxs-lookup"><span data-stu-id="dd70e-291">COMBINE</span></span>
* <span data-ttu-id="dd70e-292">CSÖKKENTÉSE</span><span class="sxs-lookup"><span data-stu-id="dd70e-292">REDUCE</span></span>

> [!NOTE]  
> <span data-ttu-id="dd70e-293">UDO által korlátozott tooconsume 0,5 Gb memória.</span><span class="sxs-lookup"><span data-stu-id="dd70e-293">UDO’s are limited tooconsume 0.5Gb memory.</span></span>  <span data-ttu-id="dd70e-294">Ez a memória-korlátozás nem érvényes toolocal végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="dd70e-294">This memory limitation does not apply toolocal executions.</span></span>

## <a name="use-user-defined-extractors"></a><span data-ttu-id="dd70e-295">Felhasználó által definiált vagyis használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-295">Use user-defined extractors</span></span>
<span data-ttu-id="dd70e-296">U-SQL lehetővé teszi tooimport külső adatok kivonat utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="dd70e-296">U-SQL allows you tooimport external data by using an EXTRACT statement.</span></span> <span data-ttu-id="dd70e-297">Egy KIVONATOT utasítás beépített UDO vagyis használhatja:</span><span class="sxs-lookup"><span data-stu-id="dd70e-297">An EXTRACT statement can use built-in UDO extractors:</span></span>  

* <span data-ttu-id="dd70e-298">*Extractors.Text()*: különböző kódolások kivonása a tagolt szövegfájlok biztosít.</span><span class="sxs-lookup"><span data-stu-id="dd70e-298">*Extractors.Text()*: Provides extraction from delimited text files of different encodings.</span></span>

* <span data-ttu-id="dd70e-299">*Extractors.Csv()*: kibontási vesszővel tagolt (CSV) fájljai különböző kódolások biztosít.</span><span class="sxs-lookup"><span data-stu-id="dd70e-299">*Extractors.Csv()*: Provides extraction from comma-separated value (CSV) files of different encodings.</span></span>

* <span data-ttu-id="dd70e-300">*Extractors.Tsv()*: biztosít lapon tagolt kibontási különböző kódolások (TSV) fájljait.</span><span class="sxs-lookup"><span data-stu-id="dd70e-300">*Extractors.Tsv()*: Provides extraction from tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="dd70e-301">Hasznos toodevelop egyéni kivonatoló lehet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-301">It can be useful toodevelop a custom extractor.</span></span> <span data-ttu-id="dd70e-302">Ez hasznos lehet adatok importálása során szeretnénk toodo bármelyik hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="dd70e-302">This can be helpful during data import if we want toodo any of hello following tasks:</span></span>

* <span data-ttu-id="dd70e-303">Módosítsa a bemeneti adatok felosztása oszlopok, és egyedi értékek módosítása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-303">Modify input data by splitting columns and modifying individual values.</span></span> <span data-ttu-id="dd70e-304">hello PROCESSZOR funkcióinak jobb az oszlopok kombinálásával.</span><span class="sxs-lookup"><span data-stu-id="dd70e-304">hello PROCESSOR functionality is better for combining columns.</span></span>
* <span data-ttu-id="dd70e-305">Strukturálatlan adatok, például a weblapok és e-maileket vagy félig strukturált adatok, például az XML/JSON elemezni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-305">Parse unstructured data such as Web pages and emails, or semi-unstructured data such as XML/JSON.</span></span>
* <span data-ttu-id="dd70e-306">A kódolás nem támogatott adatok elemzése.</span><span class="sxs-lookup"><span data-stu-id="dd70e-306">Parse data in unsupported encoding.</span></span>

<span data-ttu-id="dd70e-307">a felhasználó által definiált kivonatoló toodefine szükséges, igazolnia kell a toocreate vagy egy `IExtractor` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-307">toodefine a user-defined extractor, or UDE, we need toocreate an `IExtractor` interface.</span></span> <span data-ttu-id="dd70e-308">Összes bemeneti paraméterek toohello kivonatoló, például a oszlop vagy sor elválasztókat és kódolást kell toobe hello konstruktor hello osztály definiálva.</span><span class="sxs-lookup"><span data-stu-id="dd70e-308">All input parameters toohello extractor, such as column/row delimiters, and encoding, need toobe defined in hello constructor of hello class.</span></span> <span data-ttu-id="dd70e-309">Hello `IExtractor` felületet is tartalmaznia kell a következő definícióját: hello `IEnumerable<IRow>` bírálja felül az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="dd70e-309">hello `IExtractor`  interface should also contain a definition for hello `IEnumerable<IRow>` override as follows:</span></span>

```
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

<span data-ttu-id="dd70e-310">Hello **SqlUserDefinedExtractor** attribútum azt jelzi, hogy hello típusa, a felhasználó által definiált kivonatoló kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-310">hello **SqlUserDefinedExtractor** attribute indicates that hello type should be registered as a user-defined extractor.</span></span> <span data-ttu-id="dd70e-311">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-311">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-312">SqlUserDefinedExtractor szükséges definíciója nem kötelező attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="dd70e-312">SqlUserDefinedExtractor is an optional attribute for UDE definition.</span></span> <span data-ttu-id="dd70e-313">Hello szükséges objektum toodefine AtomicFileProcessing tulajdonság használja azt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-313">It used toodefine AtomicFileProcessing property for hello UDE object.</span></span>

* <span data-ttu-id="dd70e-314">logikai AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="dd70e-314">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="dd70e-315">**Igaz** = annak jelzése, hogy a kivonatoló igényel atomi bemeneti fájlok (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="dd70e-315">**true** = Indicates that this extractor requires atomic input files (JSON, XML, ...)</span></span>
* <span data-ttu-id="dd70e-316">**hamis** = annak jelzése, hogy a kivonatoló tudnak kezelni felosztása / elosztott fájlok (CSV, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="dd70e-316">**false** = Indicates that this extractor can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="dd70e-317">hello fő szükséges programozhatóság objektumok **bemeneti** és **kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="dd70e-317">hello main UDE programmability objects are **input** and **output**.</span></span> <span data-ttu-id="dd70e-318">hello bemeneti objektum bemeneti adatok használt tooenumerate `IUnstructuredReader`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-318">hello input object is used tooenumerate input data as `IUnstructuredReader`.</span></span> <span data-ttu-id="dd70e-319">hello kimeneti objektum használt tooset kimeneti adatok hello kivonatoló tevékenységek miatt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-319">hello output object is used tooset output data as a result of hello extractor activity.</span></span>

<span data-ttu-id="dd70e-320">hello bemeneti adatokhoz keresztül `System.IO.Stream` és `System.IO.StreamReader`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-320">hello input data is accessed through `System.IO.Stream` and `System.IO.StreamReader`.</span></span>

<span data-ttu-id="dd70e-321">Bemeneti oszlopok számbavétel azt először ossza fel hello bemeneti adatfolyam egy sor elválasztó használatával.</span><span class="sxs-lookup"><span data-stu-id="dd70e-321">For input columns enumeration, we first split hello input stream by using a row delimiter.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

<span data-ttu-id="dd70e-322">Ezt követően további felosztása bemeneti sor oszlop részre.</span><span class="sxs-lookup"><span data-stu-id="dd70e-322">Then, further split input row into column parts.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

<span data-ttu-id="dd70e-323">a kimeneti adatok tooset, hello használjuk `output.Set` metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-323">tooset output data, we use hello `output.Set` method.</span></span>

<span data-ttu-id="dd70e-324">Fontos, hogy egyéni kivonatoló hello toounderstand csak kimeneti oszlopok és értékek meghatározott hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-324">It's important toounderstand that hello custom extractor only outputs columns and values that are defined with hello output.</span></span> <span data-ttu-id="dd70e-325">Állítsa be a metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-325">Set method call.</span></span>

```
output.Set<string>(count, part);
```

<span data-ttu-id="dd70e-326">hello tényleges kivonatoló kimeneti kiváltásáról meghívásával `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-326">hello actual extractor output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="dd70e-327">Az alábbiakban hello kivonatoló példa látható:</span><span class="sxs-lookup"><span data-stu-id="dd70e-327">Following is hello extractor example:</span></span>

```
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read hello input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split hello input by hello column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert tooUPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep hello rest of hello columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

<span data-ttu-id="dd70e-328">Ilyenkor használati eset hello kivonatoló újragenerálja a "guid" oszlophoz tartozó GUID hello és hello értékeket a "user" oszlop tooupper eset konvertálja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-328">In this use-case scenario, hello extractor regenerates hello GUID for “guid” column and converts hello values of “user” column tooupper case.</span></span> <span data-ttu-id="dd70e-329">Egyéni vagyis is eredményt bonyolultabb bemeneti adatok elemzése és módosítja azokat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-329">Custom extractors can produce more complicated results by parsing input data and manipulating it.</span></span>

<span data-ttu-id="dd70e-330">Az alábbiakban olvashat egy egyéni kivonatoló használó alap U-SQL-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-330">Following is base U-SQL script that uses a custom extractor:</span></span>

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 too@output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a><span data-ttu-id="dd70e-331">Felhasználó által definiált outputters használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-331">Use user-defined outputters</span></span>
<span data-ttu-id="dd70e-332">Felhasználó által definiált outputter egy másik U-SQL UDO, amely lehetővé teszi tooextend beépített U-SQL funkciókat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-332">User-defined outputter is another U-SQL UDO that allows you tooextend built-in U-SQL functionality.</span></span> <span data-ttu-id="dd70e-333">Hasonló toohello kivonatoló számos beépített outputters van.</span><span class="sxs-lookup"><span data-stu-id="dd70e-333">Similar toohello extractor, there are several built-in outputters.</span></span>

* <span data-ttu-id="dd70e-334">*Outputters.Text()*: a különböző kódolások szövegfájlok írja az adatokat toodelimited.</span><span class="sxs-lookup"><span data-stu-id="dd70e-334">*Outputters.Text()*: Writes data toodelimited text files of different encodings.</span></span>
* <span data-ttu-id="dd70e-335">*Outputters.Csv()*: adatok toocomma tagolt (CSV) fájljai különböző kódolások írja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-335">*Outputters.Csv()*: Writes data toocomma-separated value (CSV) files of different encodings.</span></span>
* <span data-ttu-id="dd70e-336">*Outputters.Tsv()*: írja az adatokat tootab tagolt különböző kódolások (TSV) fájljait.</span><span class="sxs-lookup"><span data-stu-id="dd70e-336">*Outputters.Tsv()*: Writes data tootab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="dd70e-337">Egyéni outputter lehetővé teszi toowrite adatok meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="dd70e-337">Custom outputter allows you toowrite data in a custom defined format.</span></span> <span data-ttu-id="dd70e-338">Ez lehet hasznos, ha a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-338">This can be useful for hello following tasks:</span></span>

* <span data-ttu-id="dd70e-339">Adatok toosemi strukturált vagy strukturálatlan fájlok írása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-339">Writing data toosemi-structured or unstructured files.</span></span>
* <span data-ttu-id="dd70e-340">Írás a nem támogatott kódolások.</span><span class="sxs-lookup"><span data-stu-id="dd70e-340">Writing data not supported encodings.</span></span>
* <span data-ttu-id="dd70e-341">Kimeneti adatok módosítása vagy hozzáadása az egyéni attribútumok.</span><span class="sxs-lookup"><span data-stu-id="dd70e-341">Modifying output data or adding custom attributes.</span></span>

<span data-ttu-id="dd70e-342">felhasználó által definiált toodefine outputter kell toocreate hello `IOutputter` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-342">toodefine user-defined outputter, we need toocreate hello `IOutputter` interface.</span></span>

<span data-ttu-id="dd70e-343">Az alábbiakban az alapszintű hello `IOutputter` osztály végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="dd70e-343">Following is hello base `IOutputter` class implementation:</span></span>

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

<span data-ttu-id="dd70e-344">Az összes bemeneti paraméterek toohello outputter, például oszlop vagy sor elválasztó karaktert, kódolási és így tovább kell toobe hello konstruktor hello osztály definiálva.</span><span class="sxs-lookup"><span data-stu-id="dd70e-344">All input parameters toohello outputter, such as column/row delimiters, encoding, and so on, need toobe defined in hello constructor of hello class.</span></span> <span data-ttu-id="dd70e-345">Hello `IOutputter` felületet is tartalmaznia kell a következő definícióját: `void Output` bírálja felül.</span><span class="sxs-lookup"><span data-stu-id="dd70e-345">hello `IOutputter` interface should also contain a definition for `void Output` override.</span></span> <span data-ttu-id="dd70e-346">hello attribútum `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` atomi üzenetfájl-feldolgozása nem kötelezően állítható be.</span><span class="sxs-lookup"><span data-stu-id="dd70e-346">hello attribute `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` can optionally be set for atomic file processing.</span></span> <span data-ttu-id="dd70e-347">További információkért tekintse meg a következő adatok hello.</span><span class="sxs-lookup"><span data-stu-id="dd70e-347">For more information, see hello following details.</span></span>

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* <span data-ttu-id="dd70e-348">`Output`minden egyes bemeneti sorára neve.</span><span class="sxs-lookup"><span data-stu-id="dd70e-348">`Output` is called for each input row.</span></span> <span data-ttu-id="dd70e-349">Hello vissza `IUnstructuredWriter output` sorkészlete.</span><span class="sxs-lookup"><span data-stu-id="dd70e-349">It returns hello `IUnstructuredWriter output` rowset.</span></span>
* <span data-ttu-id="dd70e-350">hello konstruktor osztály használt toopass paraméterek toohello felhasználói outputter.</span><span class="sxs-lookup"><span data-stu-id="dd70e-350">hello Constructor class is used toopass parameters toohello user-defined outputter.</span></span>
* <span data-ttu-id="dd70e-351">`Close`használt toooptionally toorelease, olcsóbbá állapot felülbírálása, vagy felfedheti, ha hello utolsó sorának készült.</span><span class="sxs-lookup"><span data-stu-id="dd70e-351">`Close` is used toooptionally override toorelease expensive state or determine when hello last row was written.</span></span>

<span data-ttu-id="dd70e-352">**SqlUserDefinedOutputter** attribútum azt jelzi, hogy hello típusa, a felhasználó által definiált outputter kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-352">**SqlUserDefinedOutputter** attribute indicates that hello type should be registered as a user-defined outputter.</span></span> <span data-ttu-id="dd70e-353">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-353">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-354">SqlUserDefinedOutputter egy nem kötelező attribútum felhasználó által definiált outputter meghatározása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-354">SqlUserDefinedOutputter is an optional attribute for a user-defined outputter definition.</span></span> <span data-ttu-id="dd70e-355">Ez a toodefine hello AtomicFileProcessing tulajdonság használatban van.</span><span class="sxs-lookup"><span data-stu-id="dd70e-355">It's used toodefine hello AtomicFileProcessing property.</span></span>

* <span data-ttu-id="dd70e-356">logikai AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="dd70e-356">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="dd70e-357">**Igaz** = annak jelzése, hogy a outputter igényel atomi kimeneti fájlok (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="dd70e-357">**true** = Indicates that this outputter requires atomic output files (JSON, XML, ...)</span></span>
* <span data-ttu-id="dd70e-358">**hamis** = annak jelzése, hogy a outputter tudnak kezelni felosztása / elosztott fájlok (CSV, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="dd70e-358">**false** = Indicates that this outputter can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="dd70e-359">hello fő programozhatóság objektumok **sor** és **kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="dd70e-359">hello main programmability objects are **row** and **output**.</span></span> <span data-ttu-id="dd70e-360">Hello **sor** objektum használt tooenumerate kimeneti adatok `IRow` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-360">hello **row** object is used tooenumerate output data as `IRow` interface.</span></span> <span data-ttu-id="dd70e-361">**Kimeneti** használt tooset kimeneti adatok toohello cél fájl.</span><span class="sxs-lookup"><span data-stu-id="dd70e-361">**Output** is used tooset output data toohello target file.</span></span>

<span data-ttu-id="dd70e-362">hello kimeneti adatok rendszeren keresztül elérhető hello `IRow` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-362">hello output data is accessed through hello `IRow` interface.</span></span> <span data-ttu-id="dd70e-363">Kimeneti adatok átadása egy sor egyszerre.</span><span class="sxs-lookup"><span data-stu-id="dd70e-363">Output data is passed a row at a time.</span></span>

<span data-ttu-id="dd70e-364">hello egyedi értékeket az hello IRow illesztőfelületet az hello Get metódus meghívásával enumerálása:</span><span class="sxs-lookup"><span data-stu-id="dd70e-364">hello individual values are enumerated by calling hello Get method of hello IRow interface:</span></span>

```
row.Get<string>("column_name")
```

<span data-ttu-id="dd70e-365">Egyedi oszlopnevek meghívásával lehet meghatározni `row.Schema`:</span><span class="sxs-lookup"><span data-stu-id="dd70e-365">Individual column names can be determined by calling `row.Schema`:</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="dd70e-366">Ez a megközelítés lehetővé teszi egy rugalmas outputter minden metaadat-séma toobuild.</span><span class="sxs-lookup"><span data-stu-id="dd70e-366">This approach enables you toobuild a flexible outputter for any metadata schema.</span></span>

<span data-ttu-id="dd70e-367">hello kimeneti adatot ír toofile használatával `System.IO.StreamWriter`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-367">hello output data is written toofile by using `System.IO.StreamWriter`.</span></span> <span data-ttu-id="dd70e-368">hello adatfolyam paraméter értéke túl`output.BaseStrea` részeként `IUnstructuredWriter output`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-368">hello stream parameter is set too`output.BaseStrea` as part of `IUnstructuredWriter output`.</span></span>

<span data-ttu-id="dd70e-369">Ne feledje, hogy fontos tooflush hello puffer toohello adatfájl mindegyik sor iteráció után.</span><span class="sxs-lookup"><span data-stu-id="dd70e-369">Note that it's important tooflush hello data buffer toohello file after each row iteration.</span></span> <span data-ttu-id="dd70e-370">Ezenkívül hello `StreamWriter` objektum kell használni, hello rendelkezésre álló attribútum engedélyezve (alapértelmezett), valamint hello **használatával** kulcsszó:</span><span class="sxs-lookup"><span data-stu-id="dd70e-370">In addition, hello `StreamWriter` object must be used with hello Disposable attribute enabled (default) and with hello **using** keyword:</span></span>

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

<span data-ttu-id="dd70e-371">Ellenkező esetben Flush() metódus hívható explicit módon mindegyik iteráció után.</span><span class="sxs-lookup"><span data-stu-id="dd70e-371">Otherwise, call Flush() method explicitly after each iteration.</span></span> <span data-ttu-id="dd70e-372">Megmutatjuk, ez a példa a következő hello.</span><span class="sxs-lookup"><span data-stu-id="dd70e-372">We show this in hello following example.</span></span>

### <a name="set-headers-and-footers-for-user-defined-outputter"></a><span data-ttu-id="dd70e-373">A felhasználó által definiált outputter fejlécek és láblécek beállítása</span><span class="sxs-lookup"><span data-stu-id="dd70e-373">Set headers and footers for user-defined outputter</span></span>
<span data-ttu-id="dd70e-374">tooset fejlécet, egyetlen iterációs végrehajtási folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-374">tooset a header, use single iteration execution flow.</span></span>

```
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

<span data-ttu-id="dd70e-375">először hello hello kód `if (isHeaderRow)` blokk csak egyszer végrehajtja.</span><span class="sxs-lookup"><span data-stu-id="dd70e-375">hello code in hello first `if (isHeaderRow)` block is executed only once.</span></span>

<span data-ttu-id="dd70e-376">Hello lábléchez hello hivatkozás toohello-példány használata `System.IO.Stream` objektum (`output.BaseStream`).</span><span class="sxs-lookup"><span data-stu-id="dd70e-376">For hello footer, use hello reference toohello instance of `System.IO.Stream` object (`output.BaseStream`).</span></span> <span data-ttu-id="dd70e-377">A hello hello Close() metódusában hello élőláb írása `IOutputter` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-377">Write hello footer in hello Close() method of hello `IOutputter` interface.</span></span>  <span data-ttu-id="dd70e-378">(További információkért lásd a következő példa hello.)</span><span class="sxs-lookup"><span data-stu-id="dd70e-378">(For more information, see hello following example.)</span></span>

<span data-ttu-id="dd70e-379">Az alábbiakban látható egy példa egy felhasználó által definiált outputter:</span><span class="sxs-lookup"><span data-stu-id="dd70e-379">Following is an example of a user-defined outputter:</span></span>

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // hello Close method is used toowrite hello footer toohello file. It's executed only once, after all rows
    public override void Close().
    {
    //Reference tooIO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization tooenumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required toomatch hello distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference toohello instance of hello IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define hello factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

<span data-ttu-id="dd70e-380">És alap U-SQL-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-380">And U-SQL base script:</span></span>

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    too@output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="dd70e-381">Ez az egy HTML outputter, amely hoz létre egy HTML-fájlba tábla adatai.</span><span class="sxs-lookup"><span data-stu-id="dd70e-381">This is an HTML outputter, which creates an HTML file with table data.</span></span>

### <a name="call-outputter-from-u-sql-base-script"></a><span data-ttu-id="dd70e-382">A U-SQL alapvető parancsprogramból hívás outputter</span><span class="sxs-lookup"><span data-stu-id="dd70e-382">Call outputter from U-SQL base script</span></span>
<span data-ttu-id="dd70e-383">egy egyéni outputter hello alap U-SQL parancsfájl a toocall, hello új példány hello outputter objektum rendelkezik létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="dd70e-383">toocall a custom outputter from hello base U-SQL script, hello new instance of hello outputter object has toobe created.</span></span>

```sql
OUTPUT @rs0 too@output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="dd70e-384">Alap parancsfájlban objektum hello példányának tooavoid vannak, létrehozható egy függvény burkoló a korábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="dd70e-384">tooavoid creating an instance of hello object in base script, we can create a function wrapper, as shown in our earlier example:</span></span>

```c#
        // Define hello factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

<span data-ttu-id="dd70e-385">Ebben az esetben hello eredeti hívás következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-385">In this case, hello original call looks like hello following:</span></span>

```
OUTPUT @rs0 
too@output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a><span data-ttu-id="dd70e-386">Felhasználó által definiált processzor</span><span class="sxs-lookup"><span data-stu-id="dd70e-386">Use user-defined processors</span></span>
<span data-ttu-id="dd70e-387">Processzor felhasználói vagy UDP, egyfajta U-SQL UDO, amely lehetővé teszi programozhatóság szolgáltatások alkalmazásával tooprocess hello bejövő sorokat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-387">User-defined processor, or UDP, is a type of U-SQL UDO that enables you tooprocess hello incoming rows by applying programmability features.</span></span> <span data-ttu-id="dd70e-388">UDP lehetővé teszi a toocombine oszlopok, értékek módosítását és szükség esetén új oszlopok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-388">UDP enables you toocombine columns, modify values, and add new columns if necessary.</span></span> <span data-ttu-id="dd70e-389">Alapvetően egy sorhalmaz szükséges tooproduce adatelemek tooprocess segít.</span><span class="sxs-lookup"><span data-stu-id="dd70e-389">Basically, it helps tooprocess a rowset tooproduce required data elements.</span></span>

<span data-ttu-id="dd70e-390">toodefine egy UDP, igazolnia kell a toocreate egy `IProcessor` hello illesztőfelület `SqlUserDefinedProcessor` attribútumot, amely nem kötelező UDP-hez.</span><span class="sxs-lookup"><span data-stu-id="dd70e-390">toodefine a UDP, we need toocreate an `IProcessor` interface with hello `SqlUserDefinedProcessor` attribute, which is optional for UDP.</span></span>

<span data-ttu-id="dd70e-391">Ez az interfész tartalmaznia kell a hello hello definíciója `IRow` felület sorhalmaz bírálja felül, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-391">This interface should contain hello definition for hello `IRow` interface rowset override, as shown in hello following example:</span></span>

```
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

<span data-ttu-id="dd70e-392">**SqlUserDefinedProcessor** azt jelzi, hogy hello típusa, felhasználó által definiált processzort kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-392">**SqlUserDefinedProcessor** indicates that hello type should be registered as a user-defined processor.</span></span> <span data-ttu-id="dd70e-393">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-393">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-394">hello SqlUserDefinedProcessor attribútum **választható** UDP definíciójához.</span><span class="sxs-lookup"><span data-stu-id="dd70e-394">hello SqlUserDefinedProcessor attribute is **optional** for UDP definition.</span></span>

<span data-ttu-id="dd70e-395">hello fő programozhatóság objektumok **bemeneti** és **kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="dd70e-395">hello main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="dd70e-396">hello bemeneti objektum használt tooenumerate bemeneti oszlopok és kimeneti, de tooset kimeneti adatok hello processzortevékenység miatt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-396">hello input object is used tooenumerate input columns and output, and tooset output data as a result of hello processor activity.</span></span>

<span data-ttu-id="dd70e-397">Bemeneti oszlopok számbavétel hello használjuk `input.Get` metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-397">For input columns enumeration, we use hello `input.Get` method.</span></span>

```
string column_name = input.Get<string>("column_name");
```

<span data-ttu-id="dd70e-398">hello paramétere `input.Get` metódus egy oszlopot, amely hello megnevezése `PRODUCE` hello záradékában `PROCESS` alapszintű hello U-SQL parancsfájl utasítást.</span><span class="sxs-lookup"><span data-stu-id="dd70e-398">hello parameter for `input.Get` method is a column that's passed as part of hello `PRODUCE` clause of hello `PROCESS` statement of hello U-SQL base script.</span></span> <span data-ttu-id="dd70e-399">Igazolnia kell, hogy toouse hello helyes adatokat írja be ide.</span><span class="sxs-lookup"><span data-stu-id="dd70e-399">We need toouse hello correct data type here.</span></span>

<span data-ttu-id="dd70e-400">A kimenet, használja a hello `output.Set` metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-400">For output, use hello `output.Set` method.</span></span>

<span data-ttu-id="dd70e-401">Fontos toonote egyéni termelő csak kimeneti oszlopok és értékek rendelkező hello `output.Set` metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-401">It's important toonote that custom producer only outputs columns and values that are defined with hello `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", mycolumn);
```

<span data-ttu-id="dd70e-402">hello tényleges processzor kimeneti kiváltásáról meghívásával `return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-402">hello actual processor output is triggered by calling `return output.AsReadOnly();`.</span></span>

<span data-ttu-id="dd70e-403">Az alábbiakban egy processzor példa látható:</span><span class="sxs-lookup"><span data-stu-id="dd70e-403">Following is a processor example:</span></span>

```
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

<span data-ttu-id="dd70e-404">Ilyenkor használati eset hello processzor hello meglévő oszlopok – ebben az esetben a "user" nagybetű, és a "des" kombinálásával "full_description" nevű új oszlopot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dd70e-404">In this use-case scenario, hello processor is generating a new column called “full_description” by combining hello existing columns--in this case, “user” in upper case, and “des”.</span></span> <span data-ttu-id="dd70e-405">Újragenerálja egy GUID Azonosítót és a hello eredeti és új GUID értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="dd70e-405">It also regenerates a GUID and returns hello original and new GUID values.</span></span>

<span data-ttu-id="dd70e-406">Ahogy látja, hello előző példa, hívása során módszerek C# `output.Set` metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-406">As you can see from hello previous example, you can call C# methods during `output.Set` method call.</span></span>

<span data-ttu-id="dd70e-407">Az alábbiakban látható használó egyéni alap U-SQL-parancsfájl egy példát:</span><span class="sxs-lookup"><span data-stu-id="dd70e-407">Following is an example of base U-SQL script that uses a custom processor:</span></span>

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a><span data-ttu-id="dd70e-408">Felhasználó által definiált appliers használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-408">Use user-defined appliers</span></span>
<span data-ttu-id="dd70e-409">A U-SQL-felhasználó által definiált applier lehetővé teszi minden egyes sorára hello külső tábla kifejezés egy lekérdezés által visszaadott tooinvoke egyéni C#-függvény.</span><span class="sxs-lookup"><span data-stu-id="dd70e-409">A U-SQL user-defined applier enables you tooinvoke a custom C# function for each row that's returned by hello outer table expression of a query.</span></span> <span data-ttu-id="dd70e-410">jobb oldali bemeneti hello hello bal oldali bemeneti minden egyes sorára kiértékeli, és hello azon sorait, amelyek előállítják a rendszer kombinálja hello kimenetként.</span><span class="sxs-lookup"><span data-stu-id="dd70e-410">hello right input is evaluated for each row from hello left input, and hello rows that are produced are combined for hello final output.</span></span> <span data-ttu-id="dd70e-411">hello APPLY operátor szükséges hozzák létre oszlopok hello listája olyan hello kombinációja hello hello bal és jobb oldali bemeneti hello oszlopainak halmaza.</span><span class="sxs-lookup"><span data-stu-id="dd70e-411">hello list of columns that are produced by hello APPLY operator are hello combination of hello set of columns in hello left and hello right input.</span></span>

<span data-ttu-id="dd70e-412">Felhasználó által definiált applier van meghívott hello USQL válasszon kifejezés részeként.</span><span class="sxs-lookup"><span data-stu-id="dd70e-412">User-defined applier is being invoked as part of hello USQL SELECT expression.</span></span>

<span data-ttu-id="dd70e-413">tipikus hívás toohello felhasználói applier láthatóhoz hasonló hello hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-413">hello typical call toohello user-defined applier looks like hello following:</span></span>

```
SELECT …
FROM …
CROSS APPLYis used toopass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

<span data-ttu-id="dd70e-414">Válassza ki a kifejezésben appliers használatával kapcsolatos további információkért lásd: [U-SQL KERESZT-alkalmazás és külső alkalmazás kiválasztása kijelölésével](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd70e-414">For more information about using appliers in a SELECT expression, see [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span></span>

<span data-ttu-id="dd70e-415">hello applier alaposztály felhasználói definíciója a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="dd70e-415">hello user-defined applier base class definition is as follows:</span></span>

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

<span data-ttu-id="dd70e-416">a felhasználó által definiált applier toodefine, igazolnia kell a toocreate hello `IApplier` hello illesztőfelület [`SqlUserDefinedApplier`] attribútumot, amely nem kötelező megadni egy felhasználói applier definíciója.</span><span class="sxs-lookup"><span data-stu-id="dd70e-416">toodefine a user-defined applier, we need toocreate hello `IApplier` interface with hello [`SqlUserDefinedApplier`] attribute, which is optional for a user-defined applier definition.</span></span>

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* <span data-ttu-id="dd70e-417">Alkalmazása hello külső tábla minden sorára nevezik.</span><span class="sxs-lookup"><span data-stu-id="dd70e-417">Apply is called for each row of hello outer table.</span></span> <span data-ttu-id="dd70e-418">Hello vissza `IUpdatableRow` sorhalmaz kimeneti.</span><span class="sxs-lookup"><span data-stu-id="dd70e-418">It returns hello `IUpdatableRow` output rowset.</span></span>
* <span data-ttu-id="dd70e-419">hello konstruktor osztály használt toopass paraméterek toohello felhasználói applier.</span><span class="sxs-lookup"><span data-stu-id="dd70e-419">hello Constructor class is used toopass parameters toohello user-defined applier.</span></span>

<span data-ttu-id="dd70e-420">**SqlUserDefinedApplier** azt jelzi, hogy hello típusa, a felhasználó által definiált applier kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-420">**SqlUserDefinedApplier** indicates that hello type should be registered as a user-defined applier.</span></span> <span data-ttu-id="dd70e-421">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-421">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-422">**SqlUserDefinedApplier** van **választható** felhasználói applier meghatározása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-422">**SqlUserDefinedApplier** is **optional** for a user-defined applier definition.</span></span>


<span data-ttu-id="dd70e-423">hello fő programozhatóság objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="dd70e-423">hello main programmability objects are as follows:</span></span>

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

<span data-ttu-id="dd70e-424">Bemeneti sorkészletek átadása pedig `IRow` bemeneti.</span><span class="sxs-lookup"><span data-stu-id="dd70e-424">Input rowsets are passed as `IRow` input.</span></span> <span data-ttu-id="dd70e-425">hello kimeneti sorok akkor jönnek létre, mint a `IUpdatableRow` kimeneti felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-425">hello output rows are generated as `IUpdatableRow` output interface.</span></span>

<span data-ttu-id="dd70e-426">Egyedi oszlopnevek lehet megállapítani a hívó hello `IRow` séma metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-426">Individual column names can be determined by calling hello `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="dd70e-427">tooget hello tényleges adatai alapján bejövő hello `IRow`, hello Get() módszert használjuk `IRow` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-427">tooget hello actual data values from hello incoming `IRow`, we use hello Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="dd70e-428">Vagy hello séma oszlopnév használjuk:</span><span class="sxs-lookup"><span data-stu-id="dd70e-428">Or we use hello schema column name:</span></span>

```
row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="dd70e-429">hello kimeneti értéket be kell állítani a `IUpdatableRow` kimenete:</span><span class="sxs-lookup"><span data-stu-id="dd70e-429">hello output values must be set with `IUpdatableRow` output:</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="dd70e-430">Fontos, hogy egyéni appliers csak kimeneti oszlopok és értékek rendelkező toounderstand `output.Set` metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-430">It is important toounderstand that custom appliers only output columns and values that are defined with `output.Set` method call.</span></span>

<span data-ttu-id="dd70e-431">hello tényleges kimeneti kiváltásáról meghívásával `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-431">hello actual output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="dd70e-432">hello felhasználói applier paraméterek toohello konstruktor függvénynek adható át.</span><span class="sxs-lookup"><span data-stu-id="dd70e-432">hello user-defined applier parameters can be passed toohello constructor.</span></span> <span data-ttu-id="dd70e-433">Applier adhat vissza oszlopokat hello applier hívni az alap U-SQL parancsfájl során meghatározott toobe igénylő változó számú.</span><span class="sxs-lookup"><span data-stu-id="dd70e-433">Applier can return a variable number of columns that need toobe defined during hello applier call in base U-SQL Script.</span></span>

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

<span data-ttu-id="dd70e-434">Íme hello felhasználói applier példa:</span><span class="sxs-lookup"><span data-stu-id="dd70e-434">Here is hello user-defined applier example:</span></span>

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

<span data-ttu-id="dd70e-435">Az alábbiakban látható a felhasználó által definiált applier hello alap U-SQL parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-435">Following is hello base U-SQL script for this user-defined applier:</span></span>

```
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

<span data-ttu-id="dd70e-436">A felhasználási forgatókönyve, a felhasználó által definiált applier viselkedik egy vesszővel elválasztott értéket program segítségével hello autó flotta tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="dd70e-436">In this use case scenario, user-defined applier acts as a comma-delimited value parser for hello car fleet properties.</span></span> <span data-ttu-id="dd70e-437">hello bemeneti fájl sorok hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="dd70e-437">hello input file rows look like hello following:</span></span>

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

<span data-ttu-id="dd70e-438">Tipikus tabulátorral tagolt TSV fájl car tulajdonságait, például gyártmányának és modelljének tulajdonságai oszlopa.</span><span class="sxs-lookup"><span data-stu-id="dd70e-438">It is a typical tab-delimited TSV file with a properties column that contains car properties such as make and model.</span></span> <span data-ttu-id="dd70e-439">Ezek a Tulajdonságok elemzett toohello a táblázat oszlopaihoz kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dd70e-439">Those properties must be parsed toohello table columns.</span></span> <span data-ttu-id="dd70e-440">hello applier biztosított is lehetővé teszi egy dinamikus számú tulajdonság szerepel hello eredménye sorhalmaz, átadott hello paraméter alapján toogenerate.</span><span class="sxs-lookup"><span data-stu-id="dd70e-440">hello applier that's provided also enables you toogenerate a dynamic number of properties in hello result rowset, based on hello parameter that's passed.</span></span> <span data-ttu-id="dd70e-441">Az összes tulajdonság vagy a tulajdonságok csak egy meghatározott is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="dd70e-441">You can generate either all properties or a specific set of properties only.</span></span>

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

<span data-ttu-id="dd70e-442">hello felhasználói applier applier objektum egy új példányt hívható meg:</span><span class="sxs-lookup"><span data-stu-id="dd70e-442">hello user-defined applier can be called as a new instance of applier object:</span></span>

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

<span data-ttu-id="dd70e-443">Vagy a burkoló gyári metódus hívásához hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-443">Or with hello invocation of a wrapper factory method:</span></span>

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a><span data-ttu-id="dd70e-444">Felhasználó által definiált combiners használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-444">Use user-defined combiners</span></span>
<span data-ttu-id="dd70e-445">Felhasználó által definiált egyesítő vagy UDC, lehetővé teszi, a bal és jobb sorkészletek, egyéni logikán alapuló toocombine sorát.</span><span class="sxs-lookup"><span data-stu-id="dd70e-445">User-defined combiner, or UDC, enables you toocombine rows from left and right rowsets, based on custom logic.</span></span> <span data-ttu-id="dd70e-446">Felhasználó által definiált egyesítő EGYESÍTÉSE kifejezéssel szolgál.</span><span class="sxs-lookup"><span data-stu-id="dd70e-446">User-defined combiner is used with COMBINE expression.</span></span>

<span data-ttu-id="dd70e-447">Egy egyesítő van meghívott hello EGYESÍTÉSE kifejezéssel, amely mindkét hello bemeneti sorkészletek hello szükséges információkat biztosít, csoportosítási oszlopok, hello hello várt eredményt, valamint a további információt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-447">A combiner is being invoked with hello COMBINE expression that provides hello necessary information about both hello input rowsets, hello grouping columns, hello expected result schema, and additional information.</span></span>

<span data-ttu-id="dd70e-448">egy alapszintű U-SQL parancsfájl egyesítő toocall, használjuk hello a következő szintaxist:</span><span class="sxs-lookup"><span data-stu-id="dd70e-448">toocall a combiner in a base U-SQL script, we use hello following syntax:</span></span>

```
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

<span data-ttu-id="dd70e-449">További információkért lásd: [EGYESÍTÉSE kifejezés (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd70e-449">For more information, see [COMBINE Expression (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span></span>

<span data-ttu-id="dd70e-450">a felhasználó által definiált egyesítő toodefine, igazolnia kell a toocreate hello `ICombiner` hello illesztőfelület [`SqlUserDefinedCombiner`] attribútumot, amely nem kötelező megadni egy felhasználói egyesítő definíciója.</span><span class="sxs-lookup"><span data-stu-id="dd70e-450">toodefine a user-defined combiner, we need toocreate hello `ICombiner` interface with hello [`SqlUserDefinedCombiner`] attribute, which is optional for a user-defined Combiner definition.</span></span>

<span data-ttu-id="dd70e-451">Alap `ICombiner` osztály definíciója:</span><span class="sxs-lookup"><span data-stu-id="dd70e-451">Base `ICombiner` class definition:</span></span>

```
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

<span data-ttu-id="dd70e-452">egyéni végrehajtásának hello egy `ICombiner` felület hello definíciójának tartalmaznia kell egy `IEnumerable<IRow>` kombinálhatja a felülbírálás.</span><span class="sxs-lookup"><span data-stu-id="dd70e-452">hello custom implementation of an `ICombiner` interface should contain hello definition for an `IEnumerable<IRow>` Combine override.</span></span>

```
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

<span data-ttu-id="dd70e-453">Hello **SqlUserDefinedCombiner** attribútum azt jelzi, hogy hello típusa, a felhasználó által definiált egyesítő kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-453">hello **SqlUserDefinedCombiner** attribute indicates that hello type should be registered as a user-defined combiner.</span></span> <span data-ttu-id="dd70e-454">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-454">This class cannot be inherited.</span></span>

<span data-ttu-id="dd70e-455">**SqlUserDefinedCombiner** használt toodefine hello egyesítő mód tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="dd70e-455">**SqlUserDefinedCombiner** is used toodefine hello Combiner mode property.</span></span> <span data-ttu-id="dd70e-456">A felhasználó által definiált egyesítő definíciója nem kötelező attribútum is.</span><span class="sxs-lookup"><span data-stu-id="dd70e-456">It is an optional attribute for a user-defined combiner definition.</span></span>

<span data-ttu-id="dd70e-457">CombinerMode mód</span><span class="sxs-lookup"><span data-stu-id="dd70e-457">CombinerMode     Mode</span></span>

<span data-ttu-id="dd70e-458">CombinerMode enum is igénybe vehet a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-458">CombinerMode enum can take hello following values:</span></span>

* <span data-ttu-id="dd70e-459">Teljes (0) minden kimenő sor potenciálisan függ minden hello bemeneti sor a bal oldali, és közvetlenül a hello ugyanaz a kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="dd70e-459">Full  (0) Every output row potentially depends on all hello input rows from left and right       with hello same key value.</span></span>

* <span data-ttu-id="dd70e-460">Minden kimeneti sor függ hello balról egyetlen bemeneti sor balra (1) (és potenciálisan összes olyan sort a jogosultsággal rendelkező hello hello azonos értéket).</span><span class="sxs-lookup"><span data-stu-id="dd70e-460">Left  (1) Every output row depends on a single input row from hello left (and potentially all rows       from hello right with hello same key value).</span></span>

* <span data-ttu-id="dd70e-461">Minden kimeneti sor attól függ, a jobb oldali hello egyetlen bemeneti sor jobb (2) (és potenciálisan összes olyan sort a hello hello balról azonos értéket).</span><span class="sxs-lookup"><span data-stu-id="dd70e-461">Right (2)     Every output row depends on a single input row from hello right (and potentially all rows       from hello left with hello same key value).</span></span>

* <span data-ttu-id="dd70e-462">Belső (3) minden kimenő sor attól függ, egyetlen bemeneti sor: a bal és jobb hello az azonos érték.</span><span class="sxs-lookup"><span data-stu-id="dd70e-462">Inner (3) Every output row depends on a single input row from left and right with hello same value.</span></span>

<span data-ttu-id="dd70e-463">Példa: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span><span class="sxs-lookup"><span data-stu-id="dd70e-463">Example:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span></span>


<span data-ttu-id="dd70e-464">hello fő programozhatóság objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="dd70e-464">hello main programmability objects are:</span></span>

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

<span data-ttu-id="dd70e-465">Bemeneti sorkészletek átadása pedig **bal oldali** és **jobb** `IRowset` interfész típusa.</span><span class="sxs-lookup"><span data-stu-id="dd70e-465">Input rowsets are passed as **left** and **right** `IRowset` type of interface.</span></span> <span data-ttu-id="dd70e-466">Mindkét sorkészletek kell számba feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="dd70e-466">Both rowsets must be enumerated for processing.</span></span> <span data-ttu-id="dd70e-467">Csak enumerálása csatolóhoz egyszer, így azt tooenumerate rendelkezik, és gyorsítótárazása azt, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd70e-467">You can only enumerate each interface once, so we have tooenumerate and cache it if necessary.</span></span>

<span data-ttu-id="dd70e-468">Gyorsítótárazás céljából, létrehozhatjuk listáját\<T\> memória struktúra típusú következtében LINQ lekérdezés végrehajtása, kifejezetten lista <`IRow`>.</span><span class="sxs-lookup"><span data-stu-id="dd70e-468">For caching purposes, we can create a List\<T\> type of memory structure as a result of a LINQ query execution, specifically List<`IRow`>.</span></span> <span data-ttu-id="dd70e-469">hello névtelen adattípus használható, valamint a számbavételi művelet során.</span><span class="sxs-lookup"><span data-stu-id="dd70e-469">hello anonymous data type can be used during enumeration as well.</span></span>

<span data-ttu-id="dd70e-470">Lásd: [bemutatása tooLINQ lekérdezéseket (C#)](https://msdn.microsoft.com/library/bb397906.aspx) LINQ-lekérdezések kapcsolatos további információk és [IEnumerable\<T\> felület](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) IEnumerablekapcsolatostovábbiinformációk\<T\> felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-470">See [Introduction tooLINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) for more information about LINQ queries, and [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) for more information about IEnumerable\<T\> interface.</span></span>

<span data-ttu-id="dd70e-471">tooget hello tényleges adatai alapján bejövő hello `IRowset`, hello Get() módszert használjuk `IRow` felületet.</span><span class="sxs-lookup"><span data-stu-id="dd70e-471">tooget hello actual data values from hello incoming `IRowset`, we use hello Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="dd70e-472">Egyedi oszlopnevek lehet megállapítani a hívó hello `IRow` séma metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-472">Individual column names can be determined by calling hello `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="dd70e-473">Vagy hello séma oszlopnév használatával:</span><span class="sxs-lookup"><span data-stu-id="dd70e-473">Or by using hello schema column name:</span></span>

```
c# row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="dd70e-474">általános enumerálása hello LINQ a következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-474">hello general enumeration with LINQ looks like hello following:</span></span>

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

<span data-ttu-id="dd70e-475">Mindkét sorkészletek enumerálása, után fogjuk tooloop keresztül az összes sort.</span><span class="sxs-lookup"><span data-stu-id="dd70e-475">After enumerating both rowsets, we are going tooloop through all rows.</span></span> <span data-ttu-id="dd70e-476">Minden egyes sorára hello bal sorhalmazban fogjuk toofind összes sorát, amelyek megfelelnek a egyesítő hello feltétele.</span><span class="sxs-lookup"><span data-stu-id="dd70e-476">For each row in hello left rowset, we are going toofind all rows that satisfy hello condition of our combiner.</span></span>

<span data-ttu-id="dd70e-477">hello kimeneti értéket be kell állítani a `IUpdatableRow` kimeneti.</span><span class="sxs-lookup"><span data-stu-id="dd70e-477">hello output values must be set with `IUpdatableRow` output.</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="dd70e-478">hello tényleges kimeneti váltja ki hívása túl`yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-478">hello actual output is triggered by calling too`yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="dd70e-479">Az alábbiakban egy egyesítő példa látható:</span><span class="sxs-lookup"><span data-stu-id="dd70e-479">Following is a combiner example:</span></span>

```
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

<span data-ttu-id="dd70e-480">Használati eset ebben a forgatókönyvben azt által létrehozott egy hello közvetítő analytics jelentést.</span><span class="sxs-lookup"><span data-stu-id="dd70e-480">In this use-case scenario, we are building an analytics report for hello retailer.</span></span> <span data-ttu-id="dd70e-481">hello célja toofind összes olyan terméket, amelyre több mint 20 000 $ és, hogy eladásra gyorsabb, mint a hello rendszeres közvetítő időszakon belül keresztül hello webhelyen keresztül.</span><span class="sxs-lookup"><span data-stu-id="dd70e-481">hello goal is toofind all products that cost more than $20,000 and that sell through hello website faster than through hello regular retailer within a certain time frame.</span></span>

<span data-ttu-id="dd70e-482">Itt található hello alap U-SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-482">Here is hello base U-SQL script.</span></span> <span data-ttu-id="dd70e-483">Összehasonlíthatja hello logika egy reguláris ILLESZTÉSI és egy egyesítő között:</span><span class="sxs-lookup"><span data-stu-id="dd70e-483">You can compare hello logic between a regular JOIN and a combiner:</span></span>

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 too@output_file1 USING Outputters.Tsv();
OUTPUT @rs2 too@output_file2 USING Outputters.Tsv();
```

<span data-ttu-id="dd70e-484">A felhasználó által definiált egyesítő hello applier objektum egy új példányt hívható meg:</span><span class="sxs-lookup"><span data-stu-id="dd70e-484">A user-defined combiner can be called as a new instance of hello applier object:</span></span>

```
USING new MyNameSpace.MyCombiner();
```


<span data-ttu-id="dd70e-485">Vagy a burkoló gyári metódus hívásához hello:</span><span class="sxs-lookup"><span data-stu-id="dd70e-485">Or with hello invocation of a wrapper factory method:</span></span>

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a><span data-ttu-id="dd70e-486">Felhasználó által definiált szűkítő használata</span><span class="sxs-lookup"><span data-stu-id="dd70e-486">Use user-defined reducers</span></span>

<span data-ttu-id="dd70e-487">U-SQL hello operátor felhasználó által definiált bővítési keretrendszert használja, és az IReducer illesztőfelületet megvalósító lehetővé teszi a C# toowrite egyéni sorhalmaz szűkítő.</span><span class="sxs-lookup"><span data-stu-id="dd70e-487">U-SQL enables you toowrite custom rowset reducers in C# by using hello user-defined operator extensibility framework and implementing an IReducer interface.</span></span>

<span data-ttu-id="dd70e-488">Felhasználó által definiált nyomáscsökkentő vagy UDR, adatok kinyerése (importálás) során használt tooeliminate szükségtelen sorok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="dd70e-488">User-defined reducer, or UDR, can be used tooeliminate unnecessary rows during data extraction (import).</span></span> <span data-ttu-id="dd70e-489">Azt is lehetnek használt toomanipulate és kiértékelheti a sorok és oszlopok.</span><span class="sxs-lookup"><span data-stu-id="dd70e-489">It also can be used toomanipulate and evaluate rows and columns.</span></span> <span data-ttu-id="dd70e-490">Programozható logikán alapuló, azt is meghatározhat melyik sort kell kibontott toobe.</span><span class="sxs-lookup"><span data-stu-id="dd70e-490">Based on programmability logic, it can also define which rows need toobe extracted.</span></span>

<span data-ttu-id="dd70e-491">toodefine UDR osztály, igazolnia kell a toocreate egy `IReducer` felületet és egy opcionális `SqlUserDefinedReducer` attribútum.</span><span class="sxs-lookup"><span data-stu-id="dd70e-491">toodefine a UDR class, we need toocreate an `IReducer` interface with an optional `SqlUserDefinedReducer` attribute.</span></span>

<span data-ttu-id="dd70e-492">Ez az osztály az interfész tartalmaznia kell a következő definícióját: hello `IEnumerable` felület sorhalmaz bírálja felül.</span><span class="sxs-lookup"><span data-stu-id="dd70e-492">This class interface should contain a definition for hello `IEnumerable` interface rowset override.</span></span>

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

<span data-ttu-id="dd70e-493">Hello **SqlUserDefinedReducer** attribútum azt jelzi, hogy hello típusa, a felhasználó által definiált nyomáscsökkentő kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="dd70e-493">hello **SqlUserDefinedReducer** attribute indicates that hello type should be registered as a user-defined reducer.</span></span> <span data-ttu-id="dd70e-494">Ez az osztály nem örökölhető.</span><span class="sxs-lookup"><span data-stu-id="dd70e-494">This class cannot be inherited.</span></span>
<span data-ttu-id="dd70e-495">**SqlUserDefinedReducer** egy nem kötelező attribútum felhasználó által definiált nyomáscsökkentő meghatározása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-495">**SqlUserDefinedReducer** is an optional attribute for a user-defined reducer definition.</span></span> <span data-ttu-id="dd70e-496">Ez a toodefine IsRecursive tulajdonság használatban van.</span><span class="sxs-lookup"><span data-stu-id="dd70e-496">It's used toodefine IsRecursive property.</span></span>

* <span data-ttu-id="dd70e-497">logikai IsRecursive</span><span class="sxs-lookup"><span data-stu-id="dd70e-497">bool     IsRecursive</span></span>    
* <span data-ttu-id="dd70e-498">**Igaz** = annak jelzése, hogy a nyomáscsökkentő idempotent</span><span class="sxs-lookup"><span data-stu-id="dd70e-498">**true**  = Indicates whether this Reducer is idempotent</span></span>

<span data-ttu-id="dd70e-499">hello fő programozhatóság objektumok **bemeneti** és **kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="dd70e-499">hello main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="dd70e-500">hello bemeneti objektum használt tooenumerate bemeneti sor.</span><span class="sxs-lookup"><span data-stu-id="dd70e-500">hello input object is used tooenumerate input rows.</span></span> <span data-ttu-id="dd70e-501">Kimeneti használt tooset kimeneti sorok tevékenység csökkentése miatt.</span><span class="sxs-lookup"><span data-stu-id="dd70e-501">Output is used tooset output rows as a result of reducing activity.</span></span>

<span data-ttu-id="dd70e-502">Bemeneti sor számbavétel hello használjuk `Row.Get` metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-502">For input rows enumeration, we use hello `Row.Get` method.</span></span>

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

<span data-ttu-id="dd70e-503">hello paramétere hello `Row.Get` metódus egy oszlopot, amely hello megnevezése `PRODUCE` hello osztálya `REDUCE` alapszintű hello U-SQL parancsfájl utasítást.</span><span class="sxs-lookup"><span data-stu-id="dd70e-503">hello parameter for hello `Row.Get` method is a column that's passed as part of hello `PRODUCE` class of hello `REDUCE` statement of hello U-SQL base script.</span></span> <span data-ttu-id="dd70e-504">Igazolnia kell, hogy toouse hello megfelelő adattípus itt is.</span><span class="sxs-lookup"><span data-stu-id="dd70e-504">We need toouse hello correct data type here as well.</span></span>

<span data-ttu-id="dd70e-505">A kimenet, használja a hello `output.Set` metódust.</span><span class="sxs-lookup"><span data-stu-id="dd70e-505">For output, use hello `output.Set` method.</span></span>

<span data-ttu-id="dd70e-506">Fontos, hogy egyéni nyomáscsökkentő kimenetek értékek kizárólag a meghatározott hello toounderstand `output.Set` metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="dd70e-506">It is important toounderstand that custom reducer only outputs values that are defined with hello `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", guid);
```

<span data-ttu-id="dd70e-507">hello tényleges nyomáscsökkentő kimeneti kiváltásáról meghívásával `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="dd70e-507">hello actual reducer output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="dd70e-508">Az alábbiakban egy nyomáscsökkentő példa látható:</span><span class="sxs-lookup"><span data-stu-id="dd70e-508">Following is a reducer example:</span></span>

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

<span data-ttu-id="dd70e-509">Ilyenkor használati eset hello nyomáscsökkentő átugorja a sorokat egy üres felhasználónévvel.</span><span class="sxs-lookup"><span data-stu-id="dd70e-509">In this use-case scenario, hello reducer is skipping rows with an empty user name.</span></span> <span data-ttu-id="dd70e-510">Minden egyes sorára sorhalmaz minden szükséges oszlop beolvassa, ezután hello felhasználónév hossza hello kiértékeli.</span><span class="sxs-lookup"><span data-stu-id="dd70e-510">For each row in rowset, it reads each required column, then evaluates hello length of hello user name.</span></span> <span data-ttu-id="dd70e-511">Hello tényleges sor azt kimenete, csak ha érték hossza 0-nál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="dd70e-511">It outputs hello actual row only if user name value length is more than 0.</span></span>

<span data-ttu-id="dd70e-512">Az alábbiakban olvashat egy egyéni nyomáscsökkentő használó alap U-SQL-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="dd70e-512">Following is base U-SQL script that uses a custom reducer:</span></span>

```
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    too@output_file 
    USING Outputters.Text();
```
