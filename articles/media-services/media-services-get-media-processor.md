---
title: "egy media feldolgozó használt aaaHow tooCreate .NET hello Azure Media Services SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy adathordozó processzor összetevő tooencode formátum konvertálni, titkosítani, illetve visszafejteni a médiatartalom Azure Media Services. Kódminták C# nyelven íródtak, és a Media Services SDK hello használata a .NET-hez."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: f133565cc1321d366013f17302adc8bc7585b251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Útmutató: a Media Processzorpéldány beolvasása
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Áttekintés
A Media Services media processzort összetevő, amely kezeli a különleges feldolgozási feladat, például a kódolása titkosítása vagy visszafejtése médiatartalom az átalakítás formátumban. Általában létrehozásakor media processzort egy feladat tooencode létrehozásakor, titkosítására, illetve hello formátum médiatartalom konvertálni.

## <a name="azure-media-processors"></a>Az Azure media processzor 

a következő témakör hello sorolja fel az adathordozó processzorok:

* [Kódolási media processzor](scenarios-and-availability.md#encoding-media-processors)
* [Elemzés media processzor](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Media processzor beolvasása

a következő módszert mutat be hogyan hello tooget egy media processzorpéldány. hello Kódpélda feltételezi, hogy a modul szintű változó nevű hello használata **_context** tooreference hello kiszolgálói környezetbe hello szakaszban leírt módon [hogyan: csatlakozás tooMedia programozott módon szolgáltatások](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy hogyan nyissa meg a media processzorpéldány tooget toohello [hogyan tooEncode egy eszköz](media-services-dotnet-encode-with-media-encoder-standard.md) témakör, amely bemutatja, hogyan toouse hello Media Encoder Standard tooencode egy eszköz.

