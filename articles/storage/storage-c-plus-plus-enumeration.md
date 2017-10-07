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
# <a name="list-azure-storage-resources-in-c"></a>A c++ Azure Storage-erőforrások felsorolása
Listaelem műveletek kulcs toomany fejlesztési forgatókönyvek az Azure Storage esetében. Ez a cikk ismerteti, hogyan toomost objektumok az Azure Storage hello listázása megadott hello Microsoft Azure Storage ügyféloldali kódtára a C++ API-k használatával hatékonyan számbavétele.

> [!NOTE]
> Ez az útmutató célja hello Azure Storage ügyféloldali kódtára a C++ verzió 2.x, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

a Storage ügyféloldali kódtára hello módszerek toolist vagy az Azure Storage lekérdezés objektumok különböző biztosít. Ez a cikk a következő forgatókönyvek hello foglalkozik:

* Egy fiók tárolók listája
* A tároló vagy virtuális blob directory lista blobok
* Egy fiók lista várólisták
* Egy fiók listáját táblákat
* A tábla lekérdezési entitások

Mindkét módszerhez látható osztálypéldányt használatával különböző helyzetek kezelésére.

## <a name="asynchronous-versus-synchronous"></a>Aszinkron és szinkron
Mivel a Storage ügyféloldali kódtára a C++ hello épül hello [C++ REST könyvtár](https://github.com/Microsoft/cpprestsdk), azt eleve támogatja az aszinkron műveletek használatával [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Példa:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Szinkron műveletek burkolása hello megfelelő aszinkron műveletek:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Ha több szálkezelési alkalmazáshoz vagy szolgáltatáshoz dolgozik, azt javasoljuk, hogy használja-e hello aszinkron API-k létre szál toocall hello szinkronizálási API-k, amely jelentős hatással van a teljesítményre, hanem közvetlenül.

## <a name="segmented-listing"></a>Szegmentált listázása
felhőbeli tárolóhely hello méretezési szegmentált listaelem igényel. Lehet például egy millió blobot, amely egy Azure blob-tároló vagy egy Azure-tábla egymilliárd entitások keresztül. Ezek a a nem elméleti számokat, de a tényleges felhasználói használati esetekben.

Éppen ezért nem célszerű toolist egyetlen válasz objektumot. Ehelyett az objektumok lapozás listázhatja. API-k listázása hello mindegyike rendelkezik egy *szegmentált* túlterhelést.

hello válasz szegmentált listázási művelet tartalmazza:

* <i>_segment</i>, a hívást toohello API listázása az adott eredmények hello készletét tartalmazó.
* *continuation_token*, amely átadott toohello következő hívás rendelés tooget hello következő oldalra. Ha nincsenek további eredmények tooreturn, hello folytatási kód értéke null.

Például egy tipikus hívás toolist összes BLOB a tárolóban lévő hasonlíthat hello a következő kódrészletet. hello kód is elérhető a [minták](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

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

Vegye figyelembe, hogy egy adott eredmények hello száma szabályozhatók hello paraméter *max_results* a hello túlterhelése minden API-t, például:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

Ha nem adja meg a hello *max_results* paraméter, a hello alapértelmezett mentése too5000 eredmények maximális értékét eredmény abban az esetben egy oldalra.

Vegye figyelembe azt is, hogy egy Azure Table storage elleni vissza a lekérdezés egyetlen bejegyzést sem, vagy a hello hello értéknél kevesebb bejegyzést *max_results* paraméter, amely a megadott, még akkor is, ha hello folytatási kód nem üres. Egyik oka lehet, hogy hello lekérdezés nem hajtható végre, öt másodpercben. Mindaddig, amíg hello folytatási kód nem üres, hello lekérdezés továbbra is, és a kódot kell feltételezi azt szegmens eredmények hello méretét.

hello kódolási a legtöbb esetben mintát van szegmentált listázása, amely biztosítja, hogy listázása vagy lekérdezése explicit előrehaladását, és hogyan hello szolgáltatás válaszol-e az tooeach kérelem ajánlott. Különösen a C++-alkalmazások vagy szolgáltatások alacsonyabb szintű ellenőrzése folyamatban listázása hello segíthet vezérlő memória és teljesítmény.

## <a name="greedy-listing"></a>Mohó listázása
A Storage ügyféloldali kódtára a C++ hello korábbi verzióiban (előzetes verzió 0.5.0 és korábbi) nem szegmentált átjáróra a listában API-k táblák és a várólisták, mint például a következő hello tartalmazza:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Ezek a módszerek, szegmentált API-k burkolók volt megvalósítva. Minden válasz szegmentált listaelem hello kód fűzött hello eredmények tooa vektor, és adott vissza az összes eredményt, miután hello teljes tárolók beolvasásakor.

Ez a megközelítés lehet, hogy működik, amikor hello storage-fiók vagy a táblázat tartalmazza a kis számú objektum. Azonban hello objektumok számának növekedése, hello számára szükséges memória nő korlátozás nélkül, mert a memóriában marad minden eredmény. Egy listázási művelet során melyik hello hívó előrehaladásával kapcsolatos információ nem volt nagyon hosszú időt is igénybe vehet.

Ezek mohó hello SDK API-k listáját nem létezik a C#, Java, vagy JavaScript Node.js környezet hello. tooavoid hello lehetséges problémák ezen mohó API-k használatával, hogy eltávolította azokat verziójában 0.6.0 előzetes verzió.

Ha a kód hívja meg ezek mohó API-kat:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Módosítania kell a kódot, majd toouse hello szegmentált API-k listázása:

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

Hello megadásával *max_results* hello szegmens paraméter, akkor is el tudnak osztani hello kérelmek száma és memória kihasználtsága toomeet teljesítménnyel kapcsolatos szempontok az alkalmazás között.

Emellett ha most használ szegmentált listaelem API-kat, de hello adat tárolása egy helyi gyűjtési "mohó" Style, is javasoljuk, hogy refactor-e a kód toohandle adatainak tárolásához egy helyi gyűjtemény gondosan méretekben.

## <a name="lazy-listing"></a>Lusta listázása
Bár mohó listaelem lehetséges problémák kezeléséhez következik be, célszerű a Ha nem túl sok objektum hello tárolóban.

Ha a C# vagy Oracle Java SDK-k is használ, hello enumerálható programozási modellel, a Lusta stílusú listázása, ahol hello adatok bizonyos eltolásnál csak beolvassa szükség esetén kínál, amelyek tisztában kell lennie. A c++-ban hello iterátor alapú sablont is hasonló megközelítését ismerteti.

A tipikus Lusta felsorolást API-t használó **list_blobs** például dolgozunk:

```cpp
list_blob_item_iterator list_blobs() const;
```

Egy tipikus kódrészletet, amely hello Lusta listaelem mintát használ nézhet ki:

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

Vegye figyelembe, hogy Lusta átjáróra a listában csak a szinkron módban érhető el.

Mohó listaelem képest, Lusta listázása kér le adatokat, csak szükség esetén. Hello színfalak azt kér le adatokat az Azure Storage csak akkor, ha a következő iterátor hello áthelyezi a következő szegmens. Ezért kötött méretű memória használata vezérli, és hello művelet gyors.

Lusta listaelem API-k szerepelnek hello a Storage ügyféloldali kódtára a C++ 2.2.0 verzióban.

## <a name="conclusion"></a>Összegzés
Ebben a cikkben tárgyalt API-k felsoroló különböző objektumoknál hello a Storage ügyféloldali kódtára a C++ osztálypéldányt. toosummarize:

* Aszinkron API-kat is erősen ajánlott több szálkezelési forgatókönyv szerint.
* Legtöbb esetben ajánlott szegmentált listázása.
* Lusta listaelem hello könyvtár egy kényelmes burkoló szinkron forgatókönyvekben megadva.
* Mohó listaelem nem ajánlott, és hello könyvtár el lett távolítva.

## <a name="next-steps"></a>Következő lépések
C++ az Azure Storage- és ügyféloldali kódtár kapcsolatos további tudnivalókért tekintse meg a következő erőforrások hello.

* [Hogyan toouse Blob Storage-ának C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Hogyan toouse Table Storage-ának C++](storage-c-plus-plus-how-to-use-tables.md)
* [Hogyan toouse C++ a Queue Storage](storage-c-plus-plus-how-to-use-queues.md)
* [Az Azure Storage ügyféloldali kódtára a C++ API-JÁNAK dokumentációja esetén.](http://azure.github.io/azure-storage-cpp/)
* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)
* [Az Azure Storage dokumentációja](https://azure.microsoft.com/documentation/services/storage/)

