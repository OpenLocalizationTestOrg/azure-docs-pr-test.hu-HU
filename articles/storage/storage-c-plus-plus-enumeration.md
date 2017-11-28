---
title: "a Storage ügyféloldali kódtára a C++ hello Azure Storage-erőforrások aaaList |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello listázásakor C++ tooenumerate tárolók, blobok, Microsoft Azure Storage ügyféloldali kódtár API-k várólisták, táblákat és entitásokat."
documentationcenter: .net
services: storage
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: 33563639-2945-4567-9254-bc4a7e80698f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.openlocfilehash: d20a86688938704c24339aa9a1145786783fea5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="list-azure-storage-resources-in-c"></a><span data-ttu-id="b7e5b-103">A c++ Azure Storage-erőforrások felsorolása</span><span class="sxs-lookup"><span data-stu-id="b7e5b-103">List Azure Storage resources in C++</span></span>
<span data-ttu-id="b7e5b-104">Listaelem műveletek kulcs toomany fejlesztési forgatókönyvek az Azure Storage esetében.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-104">Listing operations are key toomany development scenarios with Azure Storage.</span></span> <span data-ttu-id="b7e5b-105">Ez a cikk ismerteti, hogyan toomost objektumok az Azure Storage hello listázása megadott hello Microsoft Azure Storage ügyféloldali kódtára a C++ API-k használatával hatékonyan számbavétele.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-105">This article describes how toomost efficiently enumerate objects in Azure Storage using hello listing APIs provided in hello Microsoft Azure Storage Client Library for C++.</span></span>

> [!NOTE]
> <span data-ttu-id="b7e5b-106">Ez az útmutató célja hello Azure Storage ügyféloldali kódtára a C++ verzió 2.x, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="b7e5b-106">This guide targets hello Azure Storage Client Library for C++ version 2.x, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

<span data-ttu-id="b7e5b-107">a Storage ügyféloldali kódtára hello módszerek toolist vagy az Azure Storage lekérdezés objektumok különböző biztosít.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-107">hello Storage Client Library provides a variety of methods toolist or query objects in Azure Storage.</span></span> <span data-ttu-id="b7e5b-108">Ez a cikk a következő forgatókönyvek hello foglalkozik:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-108">This article addresses hello following scenarios:</span></span>

* <span data-ttu-id="b7e5b-109">Egy fiók tárolók listája</span><span class="sxs-lookup"><span data-stu-id="b7e5b-109">List containers in an account</span></span>
* <span data-ttu-id="b7e5b-110">A tároló vagy virtuális blob directory lista blobok</span><span class="sxs-lookup"><span data-stu-id="b7e5b-110">List blobs in a container or virtual blob directory</span></span>
* <span data-ttu-id="b7e5b-111">Egy fiók lista várólisták</span><span class="sxs-lookup"><span data-stu-id="b7e5b-111">List queues in an account</span></span>
* <span data-ttu-id="b7e5b-112">Egy fiók listáját táblákat</span><span class="sxs-lookup"><span data-stu-id="b7e5b-112">List tables in an account</span></span>
* <span data-ttu-id="b7e5b-113">A tábla lekérdezési entitások</span><span class="sxs-lookup"><span data-stu-id="b7e5b-113">Query entities in a table</span></span>

<span data-ttu-id="b7e5b-114">Mindkét módszerhez látható osztálypéldányt használatával különböző helyzetek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-114">Each of these methods is shown using different overloads for different scenarios.</span></span>

## <a name="asynchronous-versus-synchronous"></a><span data-ttu-id="b7e5b-115">Aszinkron és szinkron</span><span class="sxs-lookup"><span data-stu-id="b7e5b-115">Asynchronous versus synchronous</span></span>
<span data-ttu-id="b7e5b-116">Mivel a Storage ügyféloldali kódtára a C++ hello épül hello [C++ REST könyvtár](https://github.com/Microsoft/cpprestsdk), azt eleve támogatja az aszinkron műveletek használatával [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span><span class="sxs-lookup"><span data-stu-id="b7e5b-116">Because hello Storage Client Library for C++ is built on top of hello [C++ REST library](https://github.com/Microsoft/cpprestsdk), we inherently support asynchronous operations by using [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span></span> <span data-ttu-id="b7e5b-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-117">For example:</span></span>

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

<span data-ttu-id="b7e5b-118">Szinkron műveletek burkolása hello megfelelő aszinkron műveletek:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-118">Synchronous operations wrap hello corresponding asynchronous operations:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

<span data-ttu-id="b7e5b-119">Ha több szálkezelési alkalmazáshoz vagy szolgáltatáshoz dolgozik, azt javasoljuk, hogy használja-e hello aszinkron API-k létre szál toocall hello szinkronizálási API-k, amely jelentős hatással van a teljesítményre, hanem közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-119">If you are working with multiple threading applications or services, we recommend that you use hello async APIs directly instead of creating a thread toocall hello sync APIs, which significantly impacts your performance.</span></span>

## <a name="segmented-listing"></a><span data-ttu-id="b7e5b-120">Szegmentált listázása</span><span class="sxs-lookup"><span data-stu-id="b7e5b-120">Segmented listing</span></span>
<span data-ttu-id="b7e5b-121">felhőbeli tárolóhely hello méretezési szegmentált listaelem igényel.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-121">hello scale of cloud storage requires segmented listing.</span></span> <span data-ttu-id="b7e5b-122">Lehet például egy millió blobot, amely egy Azure blob-tároló vagy egy Azure-tábla egymilliárd entitások keresztül.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-122">For example, you can have over a million blobs in an Azure blob container or over a billion entities in an Azure Table.</span></span> <span data-ttu-id="b7e5b-123">Ezek a a nem elméleti számokat, de a tényleges felhasználói használati esetekben.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-123">These are not theoretical numbers, but real customer usage cases.</span></span>

<span data-ttu-id="b7e5b-124">Éppen ezért nem célszerű toolist egyetlen válasz objektumot.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-124">It is therefore impractical toolist all objects in a single response.</span></span> <span data-ttu-id="b7e5b-125">Ehelyett az objektumok lapozás listázhatja.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-125">Instead, you can list objects using paging.</span></span> <span data-ttu-id="b7e5b-126">API-k listázása hello mindegyike rendelkezik egy *szegmentált* túlterhelést.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-126">Each of hello listing APIs has a *segmented* overload.</span></span>

<span data-ttu-id="b7e5b-127">hello válasz szegmentált listázási művelet tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-127">hello response for a segmented listing operation includes:</span></span>

* <span data-ttu-id="b7e5b-128"><i>_segment</i>, a hívást toohello API listázása az adott eredmények hello készletét tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-128"><i>_segment</i>, which contains hello set of results returned for a single call toohello listing API.</span></span>
* <span data-ttu-id="b7e5b-129">*continuation_token*, amely átadott toohello következő hívás rendelés tooget hello következő oldalra.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-129">*continuation_token*, which is passed toohello next call in order tooget hello next page of results.</span></span> <span data-ttu-id="b7e5b-130">Ha nincsenek további eredmények tooreturn, hello folytatási kód értéke null.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-130">When there are no more results tooreturn, hello continuation token is null.</span></span>

<span data-ttu-id="b7e5b-131">Például egy tipikus hívás toolist összes BLOB a tárolóban lévő hasonlíthat hello a következő kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-131">For example, a typical call toolist all blobs in a container may look like hello following code snippet.</span></span> <span data-ttu-id="b7e5b-132">hello kód is elérhető a [minták](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span><span class="sxs-lookup"><span data-stu-id="b7e5b-132">hello code is available in our [samples](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span></span>

```cpp
// List blobs in hello blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_diretory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

<span data-ttu-id="b7e5b-133">Vegye figyelembe, hogy egy adott eredmények hello száma szabályozhatók hello paraméter *max_results* a hello túlterhelése minden API-t, például:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-133">Note that hello number of results returned in a page can be controlled by hello parameter *max_results* in hello overload of each API, for example:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

<span data-ttu-id="b7e5b-134">Ha nem adja meg a hello *max_results* paraméter, a hello alapértelmezett mentése too5000 eredmények maximális értékét eredmény abban az esetben egy oldalra.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-134">If you do not specify hello *max_results* parameter, hello default maximum value of up too5000 results is returned in a single page.</span></span>

<span data-ttu-id="b7e5b-135">Vegye figyelembe azt is, hogy egy Azure Table storage elleni vissza a lekérdezés egyetlen bejegyzést sem, vagy a hello hello értéknél kevesebb bejegyzést *max_results* paraméter, amely a megadott, még akkor is, ha hello folytatási kód nem üres.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-135">Also note that a query against Azure Table storage may return no records, or fewer records than hello value of hello *max_results* parameter that you specified, even if hello continuation token is not empty.</span></span> <span data-ttu-id="b7e5b-136">Egyik oka lehet, hogy hello lekérdezés nem hajtható végre, öt másodpercben.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-136">One reason might be that hello query could not complete in five seconds.</span></span> <span data-ttu-id="b7e5b-137">Mindaddig, amíg hello folytatási kód nem üres, hello lekérdezés továbbra is, és a kódot kell feltételezi azt szegmens eredmények hello méretét.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-137">As long as hello continuation token is not empty, hello query should continue, and your code should not assume hello size of segment results.</span></span>

<span data-ttu-id="b7e5b-138">hello kódolási a legtöbb esetben mintát van szegmentált listázása, amely biztosítja, hogy listázása vagy lekérdezése explicit előrehaladását, és hogyan hello szolgáltatás válaszol-e az tooeach kérelem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-138">hello recommended coding pattern for most scenarios is segmented listing, which provides explicit progress of listing or querying, and how hello service responds tooeach request.</span></span> <span data-ttu-id="b7e5b-139">Különösen a C++-alkalmazások vagy szolgáltatások alacsonyabb szintű ellenőrzése folyamatban listázása hello segíthet vezérlő memória és teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-139">Particularly for C++ applications or services, lower-level control of hello listing progress may help control memory and performance.</span></span>

## <a name="greedy-listing"></a><span data-ttu-id="b7e5b-140">Mohó listázása</span><span class="sxs-lookup"><span data-stu-id="b7e5b-140">Greedy listing</span></span>
<span data-ttu-id="b7e5b-141">A Storage ügyféloldali kódtára a C++ hello korábbi verzióiban (előzetes verzió 0.5.0 és korábbi) nem szegmentált átjáróra a listában API-k táblák és a várólisták, mint például a következő hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-141">Earlier versions of hello Storage Client Library for C++ (versions 0.5.0 Preview and earlier) included non-segmented listing APIs for tables and queues, as in hello following example:</span></span>

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

<span data-ttu-id="b7e5b-142">Ezek a módszerek, szegmentált API-k burkolók volt megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-142">These methods were implemented as wrappers of segmented APIs.</span></span> <span data-ttu-id="b7e5b-143">Minden válasz szegmentált listaelem hello kód fűzött hello eredmények tooa vektor, és adott vissza az összes eredményt, miután hello teljes tárolók beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-143">For each response of segmented listing, hello code appended hello results tooa vector and returned all results after hello full containers were scanned.</span></span>

<span data-ttu-id="b7e5b-144">Ez a megközelítés lehet, hogy működik, amikor hello storage-fiók vagy a táblázat tartalmazza a kis számú objektum.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-144">This approach might work when hello storage account or table contains a small number of objects.</span></span> <span data-ttu-id="b7e5b-145">Azonban hello objektumok számának növekedése, hello számára szükséges memória nő korlátozás nélkül, mert a memóriában marad minden eredmény.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-145">However, with an increase in hello number of objects, hello memory required could increase without limit, because all results remained in memory.</span></span> <span data-ttu-id="b7e5b-146">Egy listázási művelet során melyik hello hívó előrehaladásával kapcsolatos információ nem volt nagyon hosszú időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-146">One listing operation can take a very long time, during which hello caller had no information about its progress.</span></span>

<span data-ttu-id="b7e5b-147">Ezek mohó hello SDK API-k listáját nem létezik a C#, Java, vagy JavaScript Node.js környezet hello.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-147">These greedy listing APIs in hello SDK do not exist in C#, Java, or hello JavaScript Node.js environment.</span></span> <span data-ttu-id="b7e5b-148">tooavoid hello lehetséges problémák ezen mohó API-k használatával, hogy eltávolította azokat verziójában 0.6.0 előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-148">tooavoid hello potential issues of using these greedy APIs, we removed them in version 0.6.0 Preview.</span></span>

<span data-ttu-id="b7e5b-149">Ha a kód hívja meg ezek mohó API-kat:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-149">If your code is calling these greedy APIs:</span></span>

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

<span data-ttu-id="b7e5b-150">Módosítania kell a kódot, majd toouse hello szegmentált API-k listázása:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-150">Then you should modify your code toouse hello segmented listing APIs:</span></span>

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

<span data-ttu-id="b7e5b-151">Hello megadásával *max_results* hello szegmens paraméter, akkor is el tudnak osztani hello kérelmek száma és memória kihasználtsága toomeet teljesítménnyel kapcsolatos szempontok az alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-151">By specifying hello *max_results* parameter of hello segment, you can balance between hello numbers of requests and memory usage toomeet performance considerations for your application.</span></span>

<span data-ttu-id="b7e5b-152">Emellett ha most használ szegmentált listaelem API-kat, de hello adat tárolása egy helyi gyűjtési "mohó" Style, is javasoljuk, hogy refactor-e a kód toohandle adatainak tárolásához egy helyi gyűjtemény gondosan méretekben.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-152">Additionally, if you're using segmented listing APIs, but store hello data in a local collection in a "greedy" style, we also strongly recommend that you refactor your code toohandle storing data in a local collection carefully at scale.</span></span>

## <a name="lazy-listing"></a><span data-ttu-id="b7e5b-153">Lusta listázása</span><span class="sxs-lookup"><span data-stu-id="b7e5b-153">Lazy listing</span></span>
<span data-ttu-id="b7e5b-154">Bár mohó listaelem lehetséges problémák kezeléséhez következik be, célszerű a Ha nem túl sok objektum hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-154">Although greedy listing raised potential issues, it is convenient if there are not too many objects in hello container.</span></span>

<span data-ttu-id="b7e5b-155">Ha a C# vagy Oracle Java SDK-k is használ, hello enumerálható programozási modellel, a Lusta stílusú listázása, ahol hello adatok bizonyos eltolásnál csak beolvassa szükség esetén kínál, amelyek tisztában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-155">If you're also using C# or Oracle Java SDKs, you should be familiar with hello Enumerable programming model, which offers a lazy-style listing, where hello data at a certain offset is only fetched if it is required.</span></span> <span data-ttu-id="b7e5b-156">A c++-ban hello iterátor alapú sablont is hasonló megközelítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-156">In C++, hello iterator-based template also provides a similar approach.</span></span>

<span data-ttu-id="b7e5b-157">A tipikus Lusta felsorolást API-t használó **list_blobs** például dolgozunk:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-157">A typical lazy listing API, using **list_blobs** as an example, looks like this:</span></span>

```cpp
list_blob_item_iterator list_blobs() const;
```

<span data-ttu-id="b7e5b-158">Egy tipikus kódrészletet, amely hello Lusta listaelem mintát használ nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-158">A typical code snippet that uses hello lazy listing pattern might look like this:</span></span>

```cpp
// List blobs in hello blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

<span data-ttu-id="b7e5b-159">Vegye figyelembe, hogy Lusta átjáróra a listában csak a szinkron módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-159">Note that lazy listing is only available in synchronous mode.</span></span>

<span data-ttu-id="b7e5b-160">Mohó listaelem képest, Lusta listázása kér le adatokat, csak szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-160">Compared with greedy listing, lazy listing fetches data only when necessary.</span></span> <span data-ttu-id="b7e5b-161">Hello színfalak azt kér le adatokat az Azure Storage csak akkor, ha a következő iterátor hello áthelyezi a következő szegmens.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-161">Under hello covers, it fetches data from Azure Storage only when hello next iterator moves into next segment.</span></span> <span data-ttu-id="b7e5b-162">Ezért kötött méretű memória használata vezérli, és hello művelet gyors.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-162">Therefore, memory usage is controlled with a bounded size, and hello operation is fast.</span></span>

<span data-ttu-id="b7e5b-163">Lusta listaelem API-k szerepelnek hello a Storage ügyféloldali kódtára a C++ 2.2.0 verzióban.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-163">Lazy listing APIs are included in hello Storage Client Library for C++ in version 2.2.0.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b7e5b-164">Összegzés</span><span class="sxs-lookup"><span data-stu-id="b7e5b-164">Conclusion</span></span>
<span data-ttu-id="b7e5b-165">Ebben a cikkben tárgyalt API-k felsoroló különböző objektumoknál hello a Storage ügyféloldali kódtára a C++ osztálypéldányt.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-165">In this article, we discussed different overloads for listing APIs for various objects in hello Storage Client Library for C++ .</span></span> <span data-ttu-id="b7e5b-166">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="b7e5b-166">toosummarize:</span></span>

* <span data-ttu-id="b7e5b-167">Aszinkron API-kat is erősen ajánlott több szálkezelési forgatókönyv szerint.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-167">Async APIs are strongly recommended under multiple threading scenarios.</span></span>
* <span data-ttu-id="b7e5b-168">Legtöbb esetben ajánlott szegmentált listázása.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-168">Segmented listing is recommended for most scenarios.</span></span>
* <span data-ttu-id="b7e5b-169">Lusta listaelem hello könyvtár egy kényelmes burkoló szinkron forgatókönyvekben megadva.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-169">Lazy listing is provided in hello library as a convenient wrapper in synchronous scenarios.</span></span>
* <span data-ttu-id="b7e5b-170">Mohó listaelem nem ajánlott, és hello könyvtár el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-170">Greedy listing is not recommended and has been removed from hello library.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7e5b-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7e5b-171">Next steps</span></span>
<span data-ttu-id="b7e5b-172">C++ az Azure Storage- és ügyféloldali kódtár kapcsolatos további tudnivalókért tekintse meg a következő erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-172">For more information about Azure Storage and Client Library for C++, see hello following resources.</span></span>

* [<span data-ttu-id="b7e5b-173">Hogyan toouse Blob Storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="b7e5b-173">How toouse Blob Storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="b7e5b-174">Hogyan toouse Table Storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="b7e5b-174">How toouse Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="b7e5b-175">Hogyan toouse C++ a Queue Storage</span><span class="sxs-lookup"><span data-stu-id="b7e5b-175">How toouse Queue Storage from C++</span></span>](storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="b7e5b-176">Az Azure Storage ügyféloldali kódtára a C++ API-JÁNAK dokumentációja esetén.</span><span class="sxs-lookup"><span data-stu-id="b7e5b-176">Azure Storage Client Library for C++ API documentation.</span></span>](http://azure.github.io/azure-storage-cpp/)
* [<span data-ttu-id="b7e5b-177">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="b7e5b-177">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="b7e5b-178">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="b7e5b-178">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)

