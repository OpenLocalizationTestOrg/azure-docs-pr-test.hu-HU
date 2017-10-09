---
title: "AAA \"(.NET API - Azure Search) index létrehozása |} Microsoft dokumentumok\""
description: "Index létrehozása kódban hello Azure Search .NET SDK használatával."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 7fa4030b8c3565bc02b1d6bb4426331657cf3a5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-net-sdk"></a>Hozzon létre egy Azure Search-index hello .NET SDK használatával
> [!div class="op_single_selector"]
> * [Áttekintés](search-what-is-an-index.md)
> * [Portál](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Ez a cikk végigvezeti az Azure Search létrehozásának folyamatán hello [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) hello segítségével [Azure Search .NET SDK](https://aka.ms/search-sdk).

Már az útmutató követése és az index létrehozása előtt [létre kell hoznia egy Azure Search szolgáltatást](search-create-service-portal.md).

> [!NOTE]
> A cikkben szereplő összes példakód C# nyelven van megírva. Hello teljes forráskód található [a Githubon](http://aka.ms/search-dotnet-howto). Hello is olvashat [Azure Search .NET SDK](search-howto-dotnet-sdk.md) egy részletesebb lépésein végighaladva hello mintakódot az.


## <a name="identify-your-azure-search-services-admin-api-key"></a>Azonosítsa az Azure Search szolgáltatás rendszergazdai API-kulcsát
Most, hogy létrehozta az Azure Search szolgáltatást, csaknem készen áll tooissue kérelmek a szolgáltatásvégponton hello .NET SDK használatával. Először szüksége lesz egy hello adminisztrációs api-kulcsok lett létrehozva, tooobtain hello keresőszolgáltatáshoz. hello .NET SDK minden kérelem tooyour szolgáltatás elküld az api-kulcsot. Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.

1. toofind a szolgáltatás api-kulcsokat, jelentkezzen be a toohello [Azure-portálon](https://portal.azure.com/)
2. Nyissa meg tooyour Azure Search szolgáltatás paneljét
3. Kattintson a hello "Kulcsok" ikonra

A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.

* Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások. Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.
* A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.

Index létrehozása hello célokra is használhatja az elsődleges vagy másodlagos adminisztrátori kulcsot.

<a name="CreateSearchServiceClient"></a>

## <a name="create-an-instance-of-hello-searchserviceclient-class"></a>Hello SearchServiceClient osztály példányának létrehozása
Azure Search .NET SDK használatával toostart hello, szüksége lesz a toocreate hello példányának `SearchServiceClient` osztály. Ez az osztály több konstruktorral rendelkezik. hello kívánt veszi a keresőszolgáltatása nevét és egy `SearchCredentials` objektumot használ paraméterként. A `SearchCredentials` becsomagolja az API-kulcsot.

az alábbi hello kód létrehoz egy új `SearchServiceClient` hello keresőszolgáltatás nevének, valamint hello alkalmazás konfigurációs fájljában tárolt api-kulcs értékeket használja (`appsettings.json` hello hello esetében [mintaalkalmazás](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`SearchServiceClient``Indexes`tulajdonsággal rendelkezik. Ez a tulajdonság kell toocreate, a listában, a frissítés, vagy törölje az Azure Search-indexek összes hello módszert biztosít.

> [!NOTE]
> Hello `SearchServiceClient` osztály kapcsolatok tooyour keresési szolgáltatás kezeli. A sorrend tooavoid túl sok kapcsolat megnyitásának, meg kell tooshare egyetlen példányát `SearchServiceClient` lehetőleg az alkalmazásban. A módszerek ilyen megosztás szálbiztos tooenable vannak.
> 
> 

<a name="DefineIndex"></a>

## <a name="define-your-azure-search-index"></a>Az Azure Search-index meghatározása
A hívást toohello `Indexes.Create` metódus létrehozza az indexet. Ez a módszer egy `Index` objektumot használ paraméterként, amely meghatározza az Azure Search-indexet. Toocreate van szüksége egy `Index` objektumra, és inicializálja az alábbiak szerint:

1. Set hello `Name` hello tulajdonságának `Index` toohello objektumnév az index.
2. Set hello `Fields` hello tulajdonságának `Index` objektum tooan tömbje `Field` objektumok. hello legegyszerűbb módja toocreate hello `Field` objektumok hívó hello szerint van `FieldBuilder.BuildForType` módszer, átadja egy modellosztállyal hello típusú paraméter. A modell osztály tulajdonságai toohello mezők az index. Ez lehetővé teszi a keresési index tooinstances a modell osztály toobind dokumentumok.

> [!NOTE]
> Ha nem tervezi toouse egy modellosztállyal, továbbra is megadhat az index létrehozásával `Field` közvetlenül objektumokat. Megadhatja, hogy hello neve hello mező toohello konstruktor, hello adattípust (vagy karakterláncmezők esetében az elemzőnek) együtt. Más tulajdonságokat is beállíthat, például: `IsSearchable`, `IsFilterable` stb.
>
>

Fontos, hogy a keresés felhasználói élményt és az üzleti igényeket figyelembe venni az index tervezésekor, mivel minden egyes mezőt hozzá kell rendelni hello [megfelelő attribútumokhoz](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Ezek a tulajdonságok határozzák melyik keresési funkciók (szűrés, értékkorlátozás, rendezés, teljes szöveges keresés stb.) alkalmazni toowhich mezőket. Bármely tulajdonság nincs explicit módon megadva, hello `Field` osztály alapértelmezés szerint toodisabling hello megfelelő keresési funkciót, kivéve, ha kifejezetten engedélyezi.

A fenti példában az indexnek a „hotels” nevet adtuk, és a mezőket egy modellosztály segítségével definiáltuk. Minden egyes hello modellosztállyal tulajdonságnak hello hello megfelelő mezőjét keresési kapcsolatos viselkedését meghatározó attribútumok. hello modellosztállyal a következők:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// hello SerializePropertyNamesAsCamelCase attribute is defined in hello Azure Search .NET SDK.
// It ensures that Pascal-case property names in hello model class are mapped toocamel-case
// field names in hello index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Hello az attribútumoknak ahogyan szerintünk használni fogják őket egy alkalmazásban alapján választottuk ki. Például, akkor valószínű, hogy a szállodákat kereső személyeket érdekelni fogják kulcsszóra kapott találatok hello `description` mezőt, így engedélyezzük a teljes szöveges keresés mező hello hozzáadásával `IsSearchable` toohello attribútum `Description` tulajdonság.

Vegye figyelembe, hogy pontosan egy mezőt az indexben típusú `string` hello kijelölt hello *kulcs* mező hello hozzáadásával `Key` attribútum (lásd: `HotelId` a fenti példa hello).

a fenti hello Indexdefiníció nyelvi elemzőt használ a hello `description_fr` mezőben, mert az adott toostore francia szöveg. Lásd: [hello nyelvi támogatás című](https://docs.microsoft.com/rest/api/searchservice/Language-support) valamint hello vonatkozó [blogbejegyzés](https://azure.microsoft.com/blog/language-support-in-azure-search/) nyelvi elemzőkkel kapcsolatos további információk.

> [!NOTE]
> Alapértelmezés szerint a modell osztály egyes tulajdonságai hello neve hello megfelelő mező hello index hello neve lesz. Ha toomap összes tulajdonság nevek toocamel eset mező nevét, jelölje meg a hello hello osztályt `SerializePropertyNamesAsCamelCase` attribútum. Ha azt szeretné, hogy toomap tooa másik nevet, használhatja a hello `JsonProperty` attribútum például hello `DescriptionFr` fenti tulajdonság. Hello `JsonProperty` attribútum elsőbbséget élvez hello `SerializePropertyNamesAsCamelCase` attribútum.
> 
> 

Most, hogy meghatároztuk a modellosztályt, már egyszerűen létrehozhatunk egy indexdefiníciót:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="create-hello-index"></a>Hello index létrehozása
Most, hogy már rendelkezik egy inicializált `Index` objektum meghívásával egyszerűen létrehozhat hello index `Indexes.Create` a a `SearchServiceClient` objektum:

```csharp
serviceClient.Indexes.Create(definition);
```

A kérelem sikeres hello módszer általában visszaadható. Például érvénytelen egy paraméter hello kérelemmel probléma merül fel, ha hello metódus kivételhibát `CloudException`.

Amikor végzett az index és a kívánt toodelete azt, csak hívja hello `Indexes.Delete` metódust a `SearchServiceClient`. Ez az például hello "Hotels nevű" index következőképpen törölhető:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> hello példakódot ebben a cikkben az egyszerűség kedvéért hello hello Azure Search .NET SDK szinkron módszereit használja. Javasoljuk, hogy a saját alkalmazások tookeep hello aszinkron metódusok használja őket a méretezhető és rugalmas. Például a hello példák fent akkor használhat, `CreateAsync` és `DeleteAsync` helyett `Create` és `Delete`.
> 
> 

## <a name="next-steps"></a>Következő lépések
Miután létrehozta az Azure Search-index, lehetővé válik túl[feltöltse a tartalmát hello indexbe](search-what-is-data-import.md) , és megkezdje az adatok keresését.

