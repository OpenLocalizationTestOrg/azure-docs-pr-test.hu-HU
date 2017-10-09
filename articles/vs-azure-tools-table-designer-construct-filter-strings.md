---
title: "aaaConstructing szűrőkarakterláncokban hello tábla Designer |} Microsoft Docs"
description: "Hello tábla Designer szűrőkarakterláncokban létrehozása"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 48b38d27b97936064daa875e41881d51546bc11f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="constructing-filter-strings-for-hello-table-designer"></a><span data-ttu-id="aaf19-103">A tábla Designer hello hozhat létre, Szűrőkarakterláncokban</span><span class="sxs-lookup"><span data-stu-id="aaf19-103">Constructing Filter Strings for hello Table Designer</span></span>
## <a name="overview"></a><span data-ttu-id="aaf19-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="aaf19-104">Overview</span></span>
<span data-ttu-id="aaf19-105">egy Azure-tábla megjelenő adatok toofilter hello Visual Studio **tábla Designer**, egy szűrési karakterláncot létrehozni, és írja be a hello szűrő mezőbe.</span><span class="sxs-lookup"><span data-stu-id="aaf19-105">toofilter data in an Azure table that is displayed in hello Visual Studio **Table Designer**, you construct a filter string and enter it into hello filter field.</span></span> <span data-ttu-id="aaf19-106">hello szűrő karakterlánc-formátum: határozzák meg a WCF Data Services hello és hasonló tooa SQL WHERE záradék, de toohello Table szolgáltatás HTTP-kérelem keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="aaf19-106">hello filter string syntax is defined by hello WCF Data Services and is similar tooa SQL WHERE clause, but is sent toohello Table service via an HTTP request.</span></span> <span data-ttu-id="aaf19-107">Hello **tábla Designer** leírók hello meg a kívánt tulajdonság értéke Igen toofilter megfelelő kódolás, hello tulajdonságnév, összehasonlító operátor feltétel érték csak akkor kell meg és szükség esetén hello lévő logikai operátor szűrése a mező.</span><span class="sxs-lookup"><span data-stu-id="aaf19-107">hello **Table Designer** handles hello proper encoding for you, so toofilter on a desired property value, you need only enter hello property name, comparison operator, criteria value, and optionally, Boolean operator in hello filter field.</span></span> <span data-ttu-id="aaf19-108">Nem kell tooinclude hello $filter lekérdezési lehetőség, mintha volt hozhat létre egy URL-cím tooquery hello tábla keresztül hello [Storage szolgáltatások REST API-referencia](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span><span class="sxs-lookup"><span data-stu-id="aaf19-108">You do not need tooinclude hello $filter query option as you would if you were constructing a URL tooquery hello table via hello [Storage Services REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span></span>

<span data-ttu-id="aaf19-109">hello a WCF Data Services hello alapuló [protokollhoz adatok](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span><span class="sxs-lookup"><span data-stu-id="aaf19-109">hello WCF Data Services are based on hello [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span></span> <span data-ttu-id="aaf19-110">Talál részletes információt hello szűrő rendszer lekérdezési lehetőség (**$filter**), lásd: hello [URI egyezmények OData specifikáció](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span><span class="sxs-lookup"><span data-stu-id="aaf19-110">For details on hello filter system query option (**$filter**), see hello [OData URI Conventions specification](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span></span>

## <a name="comparison-operators"></a><span data-ttu-id="aaf19-111">Összehasonlító operátorok</span><span class="sxs-lookup"><span data-stu-id="aaf19-111">Comparison Operators</span></span>
<span data-ttu-id="aaf19-112">hello következő logikai operátorok támogatja az összes tulajdonság esetében:</span><span class="sxs-lookup"><span data-stu-id="aaf19-112">hello following logical operators are supported for all property types:</span></span>

| <span data-ttu-id="aaf19-113">Logikai operátor</span><span class="sxs-lookup"><span data-stu-id="aaf19-113">Logical operator</span></span> | <span data-ttu-id="aaf19-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="aaf19-114">Description</span></span> | <span data-ttu-id="aaf19-115">Példa szűrési karakterláncot</span><span class="sxs-lookup"><span data-stu-id="aaf19-115">Example filter string</span></span> |
| --- | --- | --- |
| <span data-ttu-id="aaf19-116">EQ</span><span class="sxs-lookup"><span data-stu-id="aaf19-116">eq</span></span> |<span data-ttu-id="aaf19-117">Egyenlő</span><span class="sxs-lookup"><span data-stu-id="aaf19-117">Equal</span></span> |<span data-ttu-id="aaf19-118">Város eq "Redmond"</span><span class="sxs-lookup"><span data-stu-id="aaf19-118">City eq 'Redmond'</span></span> |
| <span data-ttu-id="aaf19-119">gt</span><span class="sxs-lookup"><span data-stu-id="aaf19-119">gt</span></span> |<span data-ttu-id="aaf19-120">Nagyobb mint</span><span class="sxs-lookup"><span data-stu-id="aaf19-120">Greater than</span></span> |<span data-ttu-id="aaf19-121">Árlista gt 20</span><span class="sxs-lookup"><span data-stu-id="aaf19-121">Price gt 20</span></span> |
| <span data-ttu-id="aaf19-122">GE</span><span class="sxs-lookup"><span data-stu-id="aaf19-122">ge</span></span> |<span data-ttu-id="aaf19-123">Nagyobb vagy egyenlő túl</span><span class="sxs-lookup"><span data-stu-id="aaf19-123">Greater than or equal too</span></span>|<span data-ttu-id="aaf19-124">Árlista ge 10</span><span class="sxs-lookup"><span data-stu-id="aaf19-124">Price ge 10</span></span> |
| <span data-ttu-id="aaf19-125">lt</span><span class="sxs-lookup"><span data-stu-id="aaf19-125">lt</span></span> |<span data-ttu-id="aaf19-126">Kisebb mint</span><span class="sxs-lookup"><span data-stu-id="aaf19-126">Less than</span></span> |<span data-ttu-id="aaf19-127">Árlista lt 20</span><span class="sxs-lookup"><span data-stu-id="aaf19-127">Price lt 20</span></span> |
| <span data-ttu-id="aaf19-128">le</span><span class="sxs-lookup"><span data-stu-id="aaf19-128">le</span></span> |<span data-ttu-id="aaf19-129">Kisebb vagy egyenlő</span><span class="sxs-lookup"><span data-stu-id="aaf19-129">Less than or equal</span></span> |<span data-ttu-id="aaf19-130">Árlista le 100</span><span class="sxs-lookup"><span data-stu-id="aaf19-130">Price le 100</span></span> |
| <span data-ttu-id="aaf19-131">Ne</span><span class="sxs-lookup"><span data-stu-id="aaf19-131">ne</span></span> |<span data-ttu-id="aaf19-132">Nem egyenlő</span><span class="sxs-lookup"><span data-stu-id="aaf19-132">Not equal</span></span> |<span data-ttu-id="aaf19-133">Város ne "London"</span><span class="sxs-lookup"><span data-stu-id="aaf19-133">City ne 'London'</span></span> |
| <span data-ttu-id="aaf19-134">és</span><span class="sxs-lookup"><span data-stu-id="aaf19-134">and</span></span> |<span data-ttu-id="aaf19-135">És</span><span class="sxs-lookup"><span data-stu-id="aaf19-135">And</span></span> |<span data-ttu-id="aaf19-136">Árlista le 200 és ár gt 3.5</span><span class="sxs-lookup"><span data-stu-id="aaf19-136">Price le 200 and Price gt 3.5</span></span> |
| <span data-ttu-id="aaf19-137">vagy</span><span class="sxs-lookup"><span data-stu-id="aaf19-137">or</span></span> |<span data-ttu-id="aaf19-138">Vagy</span><span class="sxs-lookup"><span data-stu-id="aaf19-138">Or</span></span> |<span data-ttu-id="aaf19-139">Árlista le 3.5-ös vagy ár gt 200</span><span class="sxs-lookup"><span data-stu-id="aaf19-139">Price le 3.5 or Price gt 200</span></span> |
| <span data-ttu-id="aaf19-140">nem</span><span class="sxs-lookup"><span data-stu-id="aaf19-140">not</span></span> |<span data-ttu-id="aaf19-141">nem</span><span class="sxs-lookup"><span data-stu-id="aaf19-141">Not</span></span> |<span data-ttu-id="aaf19-142">nem isAvailable</span><span class="sxs-lookup"><span data-stu-id="aaf19-142">not isAvailable</span></span> |

<span data-ttu-id="aaf19-143">Egy szűrési karakterláncot térített hello következő szabályok fontosak:</span><span class="sxs-lookup"><span data-stu-id="aaf19-143">When constructing a filter string, hello following rules are important:</span></span>

* <span data-ttu-id="aaf19-144">Hello logikai operátorok toocompare tulajdonság tooa értéket használja.</span><span class="sxs-lookup"><span data-stu-id="aaf19-144">Use hello logical operators toocompare a property tooa value.</span></span> <span data-ttu-id="aaf19-145">Ne feledje, hogy nem lehetséges toocompare tooa dinamikus tulajdonságérték; hello kifejezés oldalán állandónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aaf19-145">Note that it is not possible toocompare a property tooa dynamic value; one side of hello expression must be a constant.</span></span>
* <span data-ttu-id="aaf19-146">Minden hello szűrési karakterláncot részei kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="aaf19-146">All parts of hello filter string are case-sensitive.</span></span>
* <span data-ttu-id="aaf19-147">hello hello állandó értéknek kell lennie ahhoz, hogy hello szűrő tooreturn érvényes eredmények hello tulajdonság írja ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="aaf19-147">hello constant value must be of hello same data type as hello property in order for hello filter tooreturn valid results.</span></span> <span data-ttu-id="aaf19-148">További információ a támogatott tulajdonságtípus: [ismertetése hello tábla szolgáltatás adatmodell](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span><span class="sxs-lookup"><span data-stu-id="aaf19-148">For more information about supported property types, see [Understanding hello Table Service Data Model](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span></span>

## <a name="filtering-on-string-properties"></a><span data-ttu-id="aaf19-149">Szűrés karakterlánc tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="aaf19-149">Filtering on String Properties</span></span>
<span data-ttu-id="aaf19-150">Karakterlánc-tulajdonságainál szűrésekor idézőjelek hello szövegkonstans egyetlen.</span><span class="sxs-lookup"><span data-stu-id="aaf19-150">When you filter on string properties, enclose hello string constant in single quotation marks.</span></span>

<span data-ttu-id="aaf19-151">Példa szűrők követően hello hello **PartitionKey** és **RowKey** ; további nem kulcs tulajdonságai Tulajdonságok sikerült is hozzá kell adni toohello szűrési karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="aaf19-151">hello following example filters on hello **PartitionKey** and **RowKey** properties; additional non-key properties could also be added toohello filter string:</span></span>

    PartitionKey eq 'Partition1' and RowKey eq '00001'

<span data-ttu-id="aaf19-152">Tegye minden szűrőkifejezés kerek zárójeleket tartalmazhatnak, de nincs rá szükség:</span><span class="sxs-lookup"><span data-stu-id="aaf19-152">You can enclose each filter expression in parentheses, although it is not required:</span></span>

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

<span data-ttu-id="aaf19-153">Vegye figyelembe, hogy hello Table szolgáltatás nem támogatja a helyettesítő karakteres lekérdezések, és nem támogatják a tábla Designer hello vagy.</span><span class="sxs-lookup"><span data-stu-id="aaf19-153">Note that hello Table service does not support wildcard queries, and they are not supported in hello Table Designer either.</span></span> <span data-ttu-id="aaf19-154">Azonban előtag megfelelő hello kívánt előtag alapján összehasonlító operátorok használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="aaf19-154">However, you can perform prefix matching by using comparison operators on hello desired prefix.</span></span> <span data-ttu-id="aaf19-155">hello alábbi példa entitásokat ad vissza az "A" hello betűvel kezdődő vezetéknév tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="aaf19-155">hello following example returns entities with a LastName property beginning with hello letter 'A':</span></span>

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a><span data-ttu-id="aaf19-156">A numerikus tulajdonságok szűrése</span><span class="sxs-lookup"><span data-stu-id="aaf19-156">Filtering on Numeric Properties</span></span>
<span data-ttu-id="aaf19-157">az egész szám vagy egy lebegőpontos szám toofilter adható meg hello idézőjelek nélkül.</span><span class="sxs-lookup"><span data-stu-id="aaf19-157">toofilter on an integer or floating-point number, specify hello number without quotation marks.</span></span>

<span data-ttu-id="aaf19-158">Ez a példa egy kora tulajdonsággal, amelynek értéke nagyobb, mint 30 összes entitásokat ad vissza:</span><span class="sxs-lookup"><span data-stu-id="aaf19-158">This example returns all entities with an Age property whose value is greater than 30:</span></span>

    Age gt 30

<span data-ttu-id="aaf19-159">Ez a példa egy AmountDue tulajdonsággal, amelynek értéke összes entitásokat ad vissza kisebb vagy egyenlő too100.25:</span><span class="sxs-lookup"><span data-stu-id="aaf19-159">This example returns all entities with an AmountDue property whose value is less than or equal too100.25:</span></span>

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a><span data-ttu-id="aaf19-160">A logikai tulajdonságok szűrése</span><span class="sxs-lookup"><span data-stu-id="aaf19-160">Filtering on Boolean Properties</span></span>
<span data-ttu-id="aaf19-161">toofilter egy logikai értéket adja meg **igaz** vagy **hamis** idézőjelek nélkül.</span><span class="sxs-lookup"><span data-stu-id="aaf19-161">toofilter on a Boolean value, specify **true** or **false** without quotation marks.</span></span>

<span data-ttu-id="aaf19-162">hello alábbi példa entitásokat ad vissza az összes ahol hello IsActive tulajdonság túl beállítása**igaz**:</span><span class="sxs-lookup"><span data-stu-id="aaf19-162">hello following example returns all entities where hello IsActive property is set too**true**:</span></span>

    IsActive eq true

<span data-ttu-id="aaf19-163">A szűrőkifejezés hello logikai operátor anélkül is írhat.</span><span class="sxs-lookup"><span data-stu-id="aaf19-163">You can also write this filter expression without hello logical operator.</span></span> <span data-ttu-id="aaf19-164">A következő példa hello, hello Table szolgáltatás is visszaadható entitásokhoz IsActive esetén **igaz**:</span><span class="sxs-lookup"><span data-stu-id="aaf19-164">In hello following example, hello Table service will also return all entities where IsActive is **true**:</span></span>

    IsActive

<span data-ttu-id="aaf19-165">minden entitás, ahol IsActive értéke "false", használhatja hello nem tooreturn operátor:</span><span class="sxs-lookup"><span data-stu-id="aaf19-165">tooreturn all entities where IsActive is false, you can use hello not operator:</span></span>

    not IsActive

## <a name="filtering-on-datetime-properties"></a><span data-ttu-id="aaf19-166">Dátum és idő tulajdonságai szűrése</span><span class="sxs-lookup"><span data-stu-id="aaf19-166">Filtering on DateTime Properties</span></span>
<span data-ttu-id="aaf19-167">egy DateTime értéket toofilter adja meg a hello **datetime** kulcsszót, hello dátum/idő állandó szimpla idézőjelek között szerepel.</span><span class="sxs-lookup"><span data-stu-id="aaf19-167">toofilter on a DateTime value, specify hello **datetime** keyword, followed by hello date/time constant in single quotation marks.</span></span> <span data-ttu-id="aaf19-168">hello dátum/idő állandó kombinált UTC formátumban kell megadni, a [formázás DateTime tulajdonságértékek](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span><span class="sxs-lookup"><span data-stu-id="aaf19-168">hello date/time constant must be in combined UTC format, as described in [Formatting DateTime Property Values](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span></span>

<span data-ttu-id="aaf19-169">a következő példa hello entitásokat ad vissza hello CustomerSince tulajdonság esetén egyenlő tooJuly 10, 2008:</span><span class="sxs-lookup"><span data-stu-id="aaf19-169">hello following example returns entities where hello CustomerSince property is equal tooJuly 10, 2008:</span></span>

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
