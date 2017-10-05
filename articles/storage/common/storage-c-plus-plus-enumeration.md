---
title: "Azure Storage-erőforrások a Storage ügyféloldali kódtára a C++ listázása |} Microsoft Docs"
description: "Ismerje meg, a listaelem API-k használata a Microsoft Azure Storage ügyféloldali kódtára a C++ tárolók, blobok, várólisták, táblákat és entitásokat enumerálása."
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
ms.openlocfilehash: 9844412739f4f6f95416f81347f0f2eeeca62bea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="list-azure-storage-resources-in-c"></a><span data-ttu-id="684a2-103">A c++ Azure Storage-erőforrások felsorolása</span><span class="sxs-lookup"><span data-stu-id="684a2-103">List Azure Storage resources in C++</span></span>
<span data-ttu-id="684a2-104">Listaelem műveletek kulcsot az Azure Storage számos fejlesztési forgatókönyvek esetében.</span><span class="sxs-lookup"><span data-stu-id="684a2-104">Listing operations are key to many development scenarios with Azure Storage.</span></span> <span data-ttu-id="684a2-105">Ez a cikk ismerteti a leghatékonyabban az Azure Storage az átjáróra a listában megadott a Microsoft Azure Storage ügyféloldali kódtára a C++ API-k használata az objektumok számbavétele.</span><span class="sxs-lookup"><span data-stu-id="684a2-105">This article describes how to most efficiently enumerate objects in Azure Storage using the listing APIs provided in the Microsoft Azure Storage Client Library for C++.</span></span>

> [!NOTE]
> <span data-ttu-id="684a2-106">Ez az útmutató az Azure Storage ügyféloldali kódtára a C++ verzióját célozza 2.x, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="684a2-106">This guide targets the Azure Storage Client Library for C++ version 2.x, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

<span data-ttu-id="684a2-107">A Storage ügyféloldali kódtára számos módszer az Azure Storage lista vagy a lekérdezés objektumhoz biztosít.</span><span class="sxs-lookup"><span data-stu-id="684a2-107">The Storage Client Library provides a variety of methods to list or query objects in Azure Storage.</span></span> <span data-ttu-id="684a2-108">Ez a cikk foglalkozik a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="684a2-108">This article addresses the following scenarios:</span></span>

* <span data-ttu-id="684a2-109">Egy fiók tárolók listája</span><span class="sxs-lookup"><span data-stu-id="684a2-109">List containers in an account</span></span>
* <span data-ttu-id="684a2-110">A tároló vagy virtuális blob directory lista blobok</span><span class="sxs-lookup"><span data-stu-id="684a2-110">List blobs in a container or virtual blob directory</span></span>
* <span data-ttu-id="684a2-111">Egy fiók lista várólisták</span><span class="sxs-lookup"><span data-stu-id="684a2-111">List queues in an account</span></span>
* <span data-ttu-id="684a2-112">Egy fiók listáját táblákat</span><span class="sxs-lookup"><span data-stu-id="684a2-112">List tables in an account</span></span>
* <span data-ttu-id="684a2-113">A tábla lekérdezési entitások</span><span class="sxs-lookup"><span data-stu-id="684a2-113">Query entities in a table</span></span>

<span data-ttu-id="684a2-114">Mindkét módszerhez látható osztálypéldányt használatával különböző helyzetek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="684a2-114">Each of these methods is shown using different overloads for different scenarios.</span></span>

## <a name="asynchronous-versus-synchronous"></a><span data-ttu-id="684a2-115">Aszinkron és szinkron</span><span class="sxs-lookup"><span data-stu-id="684a2-115">Asynchronous versus synchronous</span></span>
<span data-ttu-id="684a2-116">Mivel a Storage ügyféloldali kódtára a C++ épül, a a [C++ REST könyvtár](https://github.com/Microsoft/cpprestsdk), azt eleve támogatja az aszinkron műveletek használatával [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span><span class="sxs-lookup"><span data-stu-id="684a2-116">Because the Storage Client Library for C++ is built on top of the [C++ REST library](https://github.com/Microsoft/cpprestsdk), we inherently support asynchronous operations by using [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span></span> <span data-ttu-id="684a2-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="684a2-117">For example:</span></span>

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

<span data-ttu-id="684a2-118">Szinkron műveletek burkolja a megfelelő aszinkron műveletek:</span><span class="sxs-lookup"><span data-stu-id="684a2-118">Synchronous operations wrap the corresponding asynchronous operations:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

<span data-ttu-id="684a2-119">Több szálkezelési alkalmazáshoz vagy szolgáltatáshoz dolgozik, azt javasoljuk, hogy az async API-k közvetlenül a szál létrehozása helyett hívására használt a szinkronizálás API-k, amely jelentős hatással van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="684a2-119">If you are working with multiple threading applications or services, we recommend that you use the async APIs directly instead of creating a thread to call the sync APIs, which significantly impacts your performance.</span></span>

## <a name="segmented-listing"></a><span data-ttu-id="684a2-120">Szegmentált listázása</span><span class="sxs-lookup"><span data-stu-id="684a2-120">Segmented listing</span></span>
<span data-ttu-id="684a2-121">A skála felhőbeli tárolóhely szükséges szegmentált listázása.</span><span class="sxs-lookup"><span data-stu-id="684a2-121">The scale of cloud storage requires segmented listing.</span></span> <span data-ttu-id="684a2-122">Lehet például egy millió blobot, amely egy Azure blob-tároló vagy egy Azure-tábla egymilliárd entitások keresztül.</span><span class="sxs-lookup"><span data-stu-id="684a2-122">For example, you can have over a million blobs in an Azure blob container or over a billion entities in an Azure Table.</span></span> <span data-ttu-id="684a2-123">Ezek a a nem elméleti számokat, de a tényleges felhasználói használati esetekben.</span><span class="sxs-lookup"><span data-stu-id="684a2-123">These are not theoretical numbers, but real customer usage cases.</span></span>

<span data-ttu-id="684a2-124">Éppen ezért nem célszerű egyetlen válasz összes objektumok listája.</span><span class="sxs-lookup"><span data-stu-id="684a2-124">It is therefore impractical to list all objects in a single response.</span></span> <span data-ttu-id="684a2-125">Ehelyett az objektumok lapozás listázhatja.</span><span class="sxs-lookup"><span data-stu-id="684a2-125">Instead, you can list objects using paging.</span></span> <span data-ttu-id="684a2-126">Az átjáróra a listában API-k mindegyike rendelkezik egy *szegmentált* túlterhelést.</span><span class="sxs-lookup"><span data-stu-id="684a2-126">Each of the listing APIs has a *segmented* overload.</span></span>

<span data-ttu-id="684a2-127">A válasz szegmentált listázási művelet tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="684a2-127">The response for a segmented listing operation includes:</span></span>

* <span data-ttu-id="684a2-128"><i>_segment</i>, amely azokat a egyetlen meghívása a listaelem API-t adott vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="684a2-128"><i>_segment</i>, which contains the set of results returned for a single call to the listing API.</span></span>
* <span data-ttu-id="684a2-129">*continuation_token*, amely a következő alkalommal meghívja a következő oldalra, hogy átadott.</span><span class="sxs-lookup"><span data-stu-id="684a2-129">*continuation_token*, which is passed to the next call in order to get the next page of results.</span></span> <span data-ttu-id="684a2-130">Nincs több eredmény esetén, a folytatási kód értéke null.</span><span class="sxs-lookup"><span data-stu-id="684a2-130">When there are no more results to return, the continuation token is null.</span></span>

<span data-ttu-id="684a2-131">Például minden, a tárolóban lévő blobok listázásához tipikus hívása a következő kódrészletet hasonlíthat.</span><span class="sxs-lookup"><span data-stu-id="684a2-131">For example, a typical call to list all blobs in a container may look like the following code snippet.</span></span> <span data-ttu-id="684a2-132">A kód is elérhető a [minták](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span><span class="sxs-lookup"><span data-stu-id="684a2-132">The code is available in our [samples](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span></span>

```cpp
// List blobs in the blob container
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

<span data-ttu-id="684a2-133">Vegye figyelembe, hogy a paraméter egy visszaadott eredmények száma szabályozhatók *max_results* az egyes API, például a túlterhelési:</span><span class="sxs-lookup"><span data-stu-id="684a2-133">Note that the number of results returned in a page can be controlled by the parameter *max_results* in the overload of each API, for example:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

<span data-ttu-id="684a2-134">Ha nem adja meg a *max_results* paraméter, az alapértelmezett legfeljebb 5000 eredmények maximális értékének eredmény abban az esetben egy oldalra.</span><span class="sxs-lookup"><span data-stu-id="684a2-134">If you do not specify the *max_results* parameter, the default maximum value of up to 5000 results is returned in a single page.</span></span>

<span data-ttu-id="684a2-135">Vegye figyelembe azt is, hogy egy Azure Table storage elleni vissza a lekérdezés egyetlen bejegyzést sem, vagy az értéknél kevesebb bejegyzést a *max_results* paraméter, amely a megadott, még akkor is, ha a folytatási kód nem üres.</span><span class="sxs-lookup"><span data-stu-id="684a2-135">Also note that a query against Azure Table storage may return no records, or fewer records than the value of the *max_results* parameter that you specified, even if the continuation token is not empty.</span></span> <span data-ttu-id="684a2-136">Egyik oka lehet, hogy a lekérdezés nem hajtható végre öt másodpercben.</span><span class="sxs-lookup"><span data-stu-id="684a2-136">One reason might be that the query could not complete in five seconds.</span></span> <span data-ttu-id="684a2-137">Mindaddig, amíg a folytatási kód nem üres, továbbra is a lekérdezés, és a kódot kell feltételezi azt szegmens eredmények méretét.</span><span class="sxs-lookup"><span data-stu-id="684a2-137">As long as the continuation token is not empty, the query should continue, and your code should not assume the size of segment results.</span></span>

<span data-ttu-id="684a2-138">A legtöbb esetben ajánlott kódolási mintát szegmentált van, listázása, amely biztosítja, hogy listázása vagy kérdez le, és hogyan reagál a a szolgáltatás minden egyes kérelem explicit előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="684a2-138">The recommended coding pattern for most scenarios is segmented listing, which provides explicit progress of listing or querying, and how the service responds to each request.</span></span> <span data-ttu-id="684a2-139">Különösen a C++-alkalmazások vagy szolgáltatások a listaelem folyamatban alacsonyabb szintű irányítását segíthet vezérlő memória és teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="684a2-139">Particularly for C++ applications or services, lower-level control of the listing progress may help control memory and performance.</span></span>

## <a name="greedy-listing"></a><span data-ttu-id="684a2-140">Mohó listázása</span><span class="sxs-lookup"><span data-stu-id="684a2-140">Greedy listing</span></span>
<span data-ttu-id="684a2-141">A Storage ügyféloldali kódtára a C++ korábbi verzióiban (előzetes verzió 0.5.0 és korábbi) része nem szegmentált átjáróra a listában API-k táblák és a várólisták, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="684a2-141">Earlier versions of the Storage Client Library for C++ (versions 0.5.0 Preview and earlier) included non-segmented listing APIs for tables and queues, as in the following example:</span></span>

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

<span data-ttu-id="684a2-142">Ezek a módszerek, szegmentált API-k burkolók volt megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="684a2-142">These methods were implemented as wrappers of segmented APIs.</span></span> <span data-ttu-id="684a2-143">Minden válasz szegmentált listaelem a kódot az eredmények hozzáfűzi a vektor, és adott vissza az összes eredményt, után a teljes tárolók beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="684a2-143">For each response of segmented listing, the code appended the results to a vector and returned all results after the full containers were scanned.</span></span>

<span data-ttu-id="684a2-144">Ez a megközelítés előfordulhat, hogy működnek, ha a tárfiók, vagy a tábla tartalmaz a kis számú objektum.</span><span class="sxs-lookup"><span data-stu-id="684a2-144">This approach might work when the storage account or table contains a small number of objects.</span></span> <span data-ttu-id="684a2-145">Azonban objektumok számának növekedése, a szükséges memória nő korlátozás nélkül, mert a memóriában marad minden eredmény.</span><span class="sxs-lookup"><span data-stu-id="684a2-145">However, with an increase in the number of objects, the memory required could increase without limit, because all results remained in memory.</span></span> <span data-ttu-id="684a2-146">Egy listázási művelet sok időt, amely alatt a hívó előrehaladásával kapcsolatos információ nem volt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="684a2-146">One listing operation can take a very long time, during which the caller had no information about its progress.</span></span>

<span data-ttu-id="684a2-147">Ezek mohó listázása az SDK API-k nem léteznek a C#, Java, vagy a JavaScript Node.js környezetben.</span><span class="sxs-lookup"><span data-stu-id="684a2-147">These greedy listing APIs in the SDK do not exist in C#, Java, or the JavaScript Node.js environment.</span></span> <span data-ttu-id="684a2-148">A lehetséges problémák ezen mohó API-k használatának elkerülése azt eltávolította azokat verziójában 0.6.0 előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="684a2-148">To avoid the potential issues of using these greedy APIs, we removed them in version 0.6.0 Preview.</span></span>

<span data-ttu-id="684a2-149">Ha a kód hívja meg ezek mohó API-kat:</span><span class="sxs-lookup"><span data-stu-id="684a2-149">If your code is calling these greedy APIs:</span></span>

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

<span data-ttu-id="684a2-150">Majd módosítania kell a kódot a szegmentált listaelem API-k használata:</span><span class="sxs-lookup"><span data-stu-id="684a2-150">Then you should modify your code to use the segmented listing APIs:</span></span>

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

<span data-ttu-id="684a2-151">Megadja a *max_results* szegmens paraméter, akkor is el tudnak osztani kérések számát és a memóriahasználat felel meg a teljesítménnyel kapcsolatos szempontok az alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="684a2-151">By specifying the *max_results* parameter of the segment, you can balance between the numbers of requests and memory usage to meet performance considerations for your application.</span></span>

<span data-ttu-id="684a2-152">Emellett ha használ szegmentált listaelem API-kat, de az adatok tárolása a helyi gyűjtés "mohó" Style, is javasoljuk, hogy refactor-e a kód adatainak tárolásához egy helyi gyűjtemény gondosan léptékű kezelésére.</span><span class="sxs-lookup"><span data-stu-id="684a2-152">Additionally, if you're using segmented listing APIs, but store the data in a local collection in a "greedy" style, we also strongly recommend that you refactor your code to handle storing data in a local collection carefully at scale.</span></span>

## <a name="lazy-listing"></a><span data-ttu-id="684a2-153">Lusta listázása</span><span class="sxs-lookup"><span data-stu-id="684a2-153">Lazy listing</span></span>
<span data-ttu-id="684a2-154">Bár mohó listaelem lehetséges problémák kezeléséhez következik be, célszerű a Ha nem túl sok objektum a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="684a2-154">Although greedy listing raised potential issues, it is convenient if there are not too many objects in the container.</span></span>

<span data-ttu-id="684a2-155">Ha a C# vagy Oracle Java SDK-k is használ, a Számbavehető programozási modellel, a Lusta stílusú listázása, ahol az adatok egy bizonyos eltolásnál csak beolvassa szükség esetén kínál, amelyek tisztában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="684a2-155">If you're also using C# or Oracle Java SDKs, you should be familiar with the Enumerable programming model, which offers a lazy-style listing, where the data at a certain offset is only fetched if it is required.</span></span> <span data-ttu-id="684a2-156">A c++-ban a iterátor alapú sablont is hasonló megközelítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="684a2-156">In C++, the iterator-based template also provides a similar approach.</span></span>

<span data-ttu-id="684a2-157">A tipikus Lusta felsorolást API-t használó **list_blobs** például dolgozunk:</span><span class="sxs-lookup"><span data-stu-id="684a2-157">A typical lazy listing API, using **list_blobs** as an example, looks like this:</span></span>

```cpp
list_blob_item_iterator list_blobs() const;
```

<span data-ttu-id="684a2-158">Egy tipikus kódrészletet, amely a Lusta listaelem mintát használ nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="684a2-158">A typical code snippet that uses the lazy listing pattern might look like this:</span></span>

```cpp
// List blobs in the blob container
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

<span data-ttu-id="684a2-159">Vegye figyelembe, hogy Lusta átjáróra a listában csak a szinkron módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="684a2-159">Note that lazy listing is only available in synchronous mode.</span></span>

<span data-ttu-id="684a2-160">Mohó listaelem képest, Lusta listázása kér le adatokat, csak szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="684a2-160">Compared with greedy listing, lazy listing fetches data only when necessary.</span></span> <span data-ttu-id="684a2-161">A színfalak azt kér le adatokat az Azure Storage csak akkor, ha a következő iterátor áthelyezi a következő szegmens.</span><span class="sxs-lookup"><span data-stu-id="684a2-161">Under the covers, it fetches data from Azure Storage only when the next iterator moves into next segment.</span></span> <span data-ttu-id="684a2-162">Ezért kötött méretű memória használata vezérli, és a művelet gyors.</span><span class="sxs-lookup"><span data-stu-id="684a2-162">Therefore, memory usage is controlled with a bounded size, and the operation is fast.</span></span>

<span data-ttu-id="684a2-163">Lusta listaelem API-k szerepelnek a Storage ügyféloldali kódtára a C++ 2.2.0 verzióban.</span><span class="sxs-lookup"><span data-stu-id="684a2-163">Lazy listing APIs are included in the Storage Client Library for C++ in version 2.2.0.</span></span>

## <a name="conclusion"></a><span data-ttu-id="684a2-164">Összegzés</span><span class="sxs-lookup"><span data-stu-id="684a2-164">Conclusion</span></span>
<span data-ttu-id="684a2-165">Ebben a cikkben tárgyalt API-k felsoroló különböző objektumoknál a Storage ügyféloldali kódtára a C++ osztálypéldányt.</span><span class="sxs-lookup"><span data-stu-id="684a2-165">In this article, we discussed different overloads for listing APIs for various objects in the Storage Client Library for C++ .</span></span> <span data-ttu-id="684a2-166">Összefoglalásképpen:</span><span class="sxs-lookup"><span data-stu-id="684a2-166">To summarize:</span></span>

* <span data-ttu-id="684a2-167">Aszinkron API-kat is erősen ajánlott több szálkezelési forgatókönyv szerint.</span><span class="sxs-lookup"><span data-stu-id="684a2-167">Async APIs are strongly recommended under multiple threading scenarios.</span></span>
* <span data-ttu-id="684a2-168">Legtöbb esetben ajánlott szegmentált listázása.</span><span class="sxs-lookup"><span data-stu-id="684a2-168">Segmented listing is recommended for most scenarios.</span></span>
* <span data-ttu-id="684a2-169">Lusta listázása a könyvtár egy kényelmes burkoló szinkron forgatókönyvekben megadva.</span><span class="sxs-lookup"><span data-stu-id="684a2-169">Lazy listing is provided in the library as a convenient wrapper in synchronous scenarios.</span></span>
* <span data-ttu-id="684a2-170">Mohó listaelem nem ajánlott, és a könyvtár el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="684a2-170">Greedy listing is not recommended and has been removed from the library.</span></span>

## <a name="next-steps"></a><span data-ttu-id="684a2-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="684a2-171">Next steps</span></span>
<span data-ttu-id="684a2-172">További információ Azure Storage- és ügyféloldali kódtára a C++ a következő forrásanyagokban talál.</span><span class="sxs-lookup"><span data-stu-id="684a2-172">For more information about Azure Storage and Client Library for C++, see the following resources.</span></span>

* [<span data-ttu-id="684a2-173">A C++ Blob Storage használata</span><span class="sxs-lookup"><span data-stu-id="684a2-173">How to use Blob Storage from C++</span></span>](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="684a2-174">A C++ Table Storage használata</span><span class="sxs-lookup"><span data-stu-id="684a2-174">How to use Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="684a2-175">A C++ Queue Storage használata</span><span class="sxs-lookup"><span data-stu-id="684a2-175">How to use Queue Storage from C++</span></span>](../storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="684a2-176">Az Azure Storage ügyféloldali kódtára a C++ API-JÁNAK dokumentációja esetén.</span><span class="sxs-lookup"><span data-stu-id="684a2-176">Azure Storage Client Library for C++ API documentation.</span></span>](http://azure.github.io/azure-storage-cpp/)
* [<span data-ttu-id="684a2-177">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="684a2-177">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="684a2-178">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="684a2-178">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)

