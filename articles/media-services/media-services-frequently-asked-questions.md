---
title: "Gyakori kérdések a Media Services aaaAzure |} Microsoft Docs"
description: "Gyakori kérdések (GYIK)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 6d48a5c1291f3c2559d8445921d571718d0a0a6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions"></a>Gyakori kérdések

Ez a cikk foglalkozik hello Azure Media Services (AMS) felhasználói Közösség által kiváltott gyakran ismételt kérdések.

## <a name="general-ams-faqs"></a>Általános AMS – gyakori kérdések
K: hogyan, méretezhető, indexelő?

V: hello szolgáltatás számára fenntartott egység vannak hello azonos kódolás és indexelési feladatok. Kövesse az utasításokat [hogyan tooScale kódoláshoz fenntartott egységek](media-services-scale-media-processing-overview.md). **Megjegyzés:** , hogy az indexelő teljesítmény nem érinti a fenntartott egység típusát.

K: I feltöltött, kódolt és videó közzé. Mi lehet hello OK hello videó nem tölt meg toostream azt?

V: egyik leggyakoribb oka, nem rendelkezik, amelyből a hello tooplayback kívánt streamvégpontra hello hello **futtató** állapotát.  

K: feladatokat lehet elvégezni egy élő adatfolyam összeállítás?

A: az élő adatfolyamok összeállítás jelenleg nem érhető el Azure Media Services, ezért létre kell toopre-állítható össze a számítógépen.

K: használhatok Azure CDN élő adatfolyam-továbbítási?

V: Media Services támogatja az Azure CDN integrációja (további információkért lásd: [hogyan tooManage adatfolyam-továbbítási végpontok Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)).  Live streaming CDN is használhatja. Az Azure Media Services Smooth Streaming, HLS és MPEG-DASH kimenetek biztosít. Ezek a formátumok adatok átvitele a HTTP Protokollt használja, és a HTTP-gyorsítótárazás előnyök. Az élő adatfolyam tényleges videó/hang osztott toofragments és az egyes töredék beolvasása gyorsítótárazza a CDN. Csak adatok igényeinek toobe frissíteni az hello jegyzék adatai. CDN rendszeresen frissülnek a jegyzék adatokat.

K: Does Azure Media services támogatja a tárolni lemezképeket?

V: Ha csupán toostore JPEG vagy PNG-fájlok, az Azure Blob Storage legyen. Nincs juttatás tooputting őket a Media Services fiók, kivéve, ha azt szeretné, hogy azokat a videó vagy a hang eszközök társított tookeep van. Vagy ha lehetséges, hogy a szükséges toouse hello lemezképek, a hello videókódoló átfedések. Media Encoder Standard felirataként Képek videók felett, és, hogy mi felsorolja JPEG és PNG támogatott formátumok bemeneti. További információkért lásd: [létrehozása átfedések](media-services-advanced-encoding-with-mes.md#overlay).

K: hogyan tudja másolni a Media Services-fiók egy tooanother eszközök.

A: a Media Services-fiók egy tooanother használata a .NET, toocopy eszközök használata [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) hello elérhető kiterjesztésmetódus [Azure Media Services .NET SDK-bővítmények](https://github.com/Azure/azure-sdk-for-media-services-extensions/) tárházba. További információkért lásd: [ez](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum szál.

K: milyen hello támogatott karaktereket az AMS használatakor a fájlok elnevezési?

V: Media Services hello hello IAssetFile.Name tulajdonság értékének használja, amikor a hello adatfolyam-tartalmat (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett. hello értékének hello **neve** tulajdonság nem lehet hello következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Emellett csak lehet egy "." hello fájlnévkiterjesztés.

K: hogyan tooconnect használatával REST?

V: kapcsolatos információk hogyan tooconnect toohello AMS API-ról: [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI. 

K: hogyan lehet videó elforgatása hello kódolási folyamat során.

V: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) elforgatási szög által 90/180 vagy 270 támogatja. hello alapértelmezés lesz az "Auto", ahol az megpróbál toodetect hello Elforgatás metaadatok hello bejövő MP4/MOV fájlban, és ellensúlyozza a azt. Adja meg a következőket hello **források** elem tooone hello json készleteket definiált [Itt](media-services-mes-presets-overview.md):

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
