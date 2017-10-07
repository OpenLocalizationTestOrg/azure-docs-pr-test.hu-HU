---
title: "aaaAzure Media Services kódolási hibakódok |} Microsoft Docs"
description: "Ez a témakör felsorolja a abban az esetben, ha hiba történt a feladat a végrehajtás kódolás hello során visszatérő hibakódok..."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: b69b6abee797c40c9b8b8f23bf2398273c170e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encoding-error-codes"></a>Kódolási hibakódok

hello következő táblázat sorolja fel, ha hiba történt a feladat a végrehajtás kódolás hello során visszatérő hibakódok.  tooget hiba részletes adatait, a .NET-kódot, a hello használata [hiba részletei](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) osztály. tooget hiba részletes adatait, a többi kódban hello használata [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API-t.

| ErrorDetail.Code | Hiba lehetséges okai |
| --- | --- |
| Ismeretlen |Hello feladat végrehajtása közben ismeretlen hiba |
| ErrorDownloadingInputAssetMalformedContent |Kategória hibák, amely lefedi a hibák például rossz fájlneveket, nulla hosszúságú fájlok, a hibás bemeneti eszköz letöltése a formázza és így tovább. |
| ErrorDownloadingInputAssetServiceFailure |Kategória hibák hello szolgáltatás oldalán - letöltése közben hálózati vagy tárolási hibák példa problémákat ismerteti. |
| ErrorParsingConfiguration |A hibák kategória ahol feladat <see cref="MediaTask.PrivateData"/> (konfiguráció) érvénytelen, például hello konfigurációja nem érvényes rendszerrel előre, vagy érvénytelen XML-kódot tartalmaz. |
| ErrorExecutingTaskMalformedContent |Ahol problémák belül hello bemeneti médiafájlok hiba következtében hello feladat hello végrehajtása során hibák kategóriáját. |
| ErrorExecutingTaskUnsupportedFormat |Kategória hibák, ahol hello media feldolgozója nem képes feldolgozni a megadott hello fájlok - adathordozó formázása nem támogatott, vagy nem felel meg a konfigurációs hello. Például próbált tooproduce egy eszköz, amelynek csak videó csak kimenete |
| ErrorProcessingTask |Más hibák media processzor hello kategória észlel, amelyek nem kapcsolódó toocontent hello feladat hello feldolgozása során. |
| ErrorUploadingOutputAsset |A hibák hello kimeneti adategységen feltöltésekor kategória |
| ErrorCancelingTask |A hibák toocover hibákat megkísérlésekor. a feladat toocancel hello kategória |
| TransientError |Kategória hibák toocover átmeneti problémákat (például) ideiglenes hálózati probléma az Azure Storage szolgáltatással) |

a hello tooget súgó **Media Services** team, nyissa meg a [támogatja a jegy](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Speciális feladatokra kódolási Media Encoder Standard készletek testreszabásával.](media-services-custom-mes-presets-with-dotnet.md)
* [Kvóták és korlátozások](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
