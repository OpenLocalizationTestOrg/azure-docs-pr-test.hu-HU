---
title: "aaaMedia műveletek REST API áttekintése |} Microsoft Docs"
description: "Media Services REST API – áttekintés"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b54f4d9123486d6cae832c9817688b0f3da5b401
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-operations-rest-api-overview"></a>A Media Services REST API áttekintése
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Hello **Media Services Operations REST** API feladatok, eszközök, hozzáférési házirendek és más műveletek olyan objektumokon, a Media Services-fiók létrehozására használt. További információkért lásd: [Media Services Operations REST API-referenciában](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

A Microsoft Azure Media Services egy olyan szolgáltatás, amely támogatja az OData-alapú HTTP-kérelmekre, és képes válaszolni részletes JSON vagy atom + pub. A Media Services tooAzure tervezési irányelveket megfelel, mivel nincs a szükséges minden egyes ügyfélnek kell használni, amikor tooMedia szolgáltatások kapcsolódás HTTP-fejléc készlettel, valamint a nem kötelező használható fejléc készlettel. hello következő részek a hello fejlécek és használhatja, ha HTTP-műveletek létrehozása a kérelmeket, és érkezik válasz, a Media Services.

Ez a témakör áttekintést nyújt a Media Services toouse REST v2.

## <a name="considerations"></a>Megfontolandó szempontok

a következő szempontok hello REST használata esetén alkalmazza.

* Entitások lekérdezésekor korlátozás van adja vissza egy időben, mert a nyilvános REST v2 korlátozza a lekérdezési eredmények too1000 eredmények 1000 entitások. Toouse kell **kihagyása** és **érvénybe** (.NET) / **felső** (REST) leírtak szerint [.NET példában](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) és [a REST API Példa](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Ha JSON használatával és toouse hello megadásával **__metadata** hello kérelem (például a csatolt objektum tooreferences) meg kell adni a hello kulcsszó **elfogadás** fejléc túl[részletes JSON formátumban ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (lásd a következő példa hello). OData nem értelmezi hello **__metadata** hello tulajdonság kérte, kivéve, ha tooverbose beállítása azt.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Media Services által támogatott szabványos HTTP-kérelem fejlécek
Minden hívás elvégezte a Media Services, a szükséges, meg kell adni a kérelem fejléc készlettel és is a választható fejléc készlettel érdemes lehet tooinclude. a következő tábla listák hello hello fejlécek szükséges:

| Fejléc | Típus | Érték |
| --- | --- | --- |
| Engedélyezés |Tulajdonosi |Tulajdonosi, amely hello csak elfogadható engedélyezési mechanizmus. hello érték hello hozzáférési jogkivonatot az ACS által biztosított is magában kell foglalnia. |
| x-ms-version |Decimális |2.11 |
| DataServiceVersion |Decimális |3.0 |
| MaxDataServiceVersion |Decimális |3.0 |

> [!NOTE]
> Mivel a Media Services OData tooexpose használja az alapul szolgáló eszköz metaadatok tárház REST API-k segítségével, hello DataServiceVersion és MaxDataServiceVersion fejlécek fel kell venni kérésének; azonban ha nem, majd jelenleg Media Services azt feltételezi, hogy hello DataServiceVersion használati érték 3.0.
> 
> 

hello az alábbiakban látható a választható fejléc készlettel:

| Fejléc | Típus | Érték |
| --- | --- | --- |
| Dátum |RFC 1123 dátuma |Hello kérés időbélyege |
| Fogadja el |Tartalom típusa |hello kért hello válasz hello alábbi content-type:<p> -az application/json; odata részletes =<p> -application/atom + xml<p> Válaszok lehet egy másik tartalomtípus egy blob adatlehívás, például ha a sikeres válasz tartalmazza hello blob adatfolyam hello hasznos. |
| Fogadja el-kódolás |A gzip, deflate |A GZIP és a DEFLATE kódolását, ha alkalmazható. Megjegyzés: Nagy erőforrások Media Services előfordulhat, hogy figyelmen kívül hagyja ezt a fejlécet, és vissza noncompressed adatokat. |
| Fogadja el nyelv |"hu", "es", és így tovább. |Meghatározza a hello válasz hello elsődleges nyelvét. |
| Fogadja el Charset |Például a "UTF-8" Charset típusa |Alapértelmezés az UTF-8. |
| X-HTTP-módszer |HTTP-metódus |Lehetővé teszi, hogy nem támogatják a HTTP-metódus PUT vagy DELETE toouse például tűzfalak vagy az ügyfelek ezekkel az eljárásokkal bújtatott GET hívást keresztül. |
| Tartalomtípus |Tartalom típusa |A PUT vagy POST kérelmek hello kérelemtörzset tartalomtípus. |
| ügyfél-azonosító |Karakterlánc |A hívó által megadott érték, amely azonosítja hello kérelmet kap. Ha meg van adva, ennek az értéknek szerepelni fog a hello válaszüzenetet módon toomap hello kérelmet. <p><p>**Fontos**<p>Értékeket kell felső határa 2096b (2-k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Media Services által támogatott szabványos HTTP-válaszfejléceket
hello az alábbiakban olvashatja a, attól függően, hogy hello erőforrás volt a kért tooyou visszaadott és tooperform kívánt művelet hello fejléc készlettel.

| Fejléc | Típus | Érték |
| --- | --- | --- |
| -azonosító |Karakterlánc |Hello aktuális műveletet, generált szolgáltatás egyedi azonosítója. |
| ügyfél-azonosító |Karakterlánc |Hello hívó hello eredeti kérelem, ha van ilyen megadott azonosító. |
| Dátum |RFC 1123 dátuma |hello dátum, hogy hello kérelem feldolgozása. |
| Tartalomtípus |Változó |hello tartalomtípus hello adott válasz törzse. |
| Tartalom kódolása |Változó |A gzip és a deflate szükség szerint. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Media Services által támogatott szabványos HTTP-műveletek
hello az alábbiakban látható, amely így a HTTP-kérelmek használható HTTP-műveletek teljes listáját:

| Művelet | Leírás |
| --- | --- |
| GET |Vissza az objektum aktuális érték hello. |
| POST |Egy megadott hello adatok alapján objektumot hoz létre, vagy elküldi a parancsot. |
| A PUT |A felváltja egy objektumot, vagy létrehoz egy elnevezett objektum (ha alkalmazható). |
| TÖRLÉSE |Törli az objektumot. |
| EGYESÍTÉS |Frissíti a meglévő objektum elnevezett tulajdonság módosításokkal. |
| HEAD |Az objektum GET választ metaadatokat adja vissza. |

## <a name="discover-media-services-model"></a>Media Services-modell felderítése
toomake Media Services entitások felfedezését, hello $metadata művelet használható. Tooretrieve lehetővé teszi az összes érvényes entitástípusok, entitás tulajdonságai, társítások, Funkciók, műveletek, és így tovább. hello következő példa bemutatja, hogyan tooconstruct hello URI: https://media.windows.net/API/$ metaadatok.

Kell hozzáfűzése "? api-version=2.x" hello Ha tooview hello metaadatok a böngészőben, vagy szeretné hello x-ms-version fejlécnek nem tartalmaznak a kérés URI toohello végét.

## <a name="connect-toomedia-services"></a>Connect tooMedia szolgáltatások

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

## <a name="next-steps"></a>Következő lépések

tooaccess AMS API-k, a többi, lásd: [használja az Azure AD hitelesítési tooaccess hello Azure Media Services API többi](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

