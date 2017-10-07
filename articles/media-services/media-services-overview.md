---
title: "aaaAzure Media Services áttekintése |} Microsoft Docs"
description: "Ezen témakör áttekintést nyújt az Azure Media Services szolgáltatásairól"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 81f9f4d9ff75effea30c10fd09449e9d2025f377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-overview"></a>Az Azure Media Services áttekintése 

Microsoft Azure Media Services szolgáltatás felhőalapú egy bővíthető platform, amely lehetővé teszi a fejlesztők toobuild méretezhető médiafelügyeleti és -továbbítási alkalmazások. A Media Services alapjai a REST API-k, amelyek lehetővé teszik toosecurely feltöltési, tárolásához, kódolása és video- vagy tartalom csomag mind igény szerinti és élő adatfolyam-továbbítási kézbesítési toovarious ügyfelek (például TV, számítógépek és mobileszközök).

A Media Services használatával teljes, átfogó munkafolyamatokat hozhat létre. Másik lehetőségként toouse külső összetevőket a munkafolyamatok bizonyos részeihez. Például egy harmadik féltől származó kódolóval végezhet kódolást. Ezután a Media Services használatával intézheti a feltöltést, a védelmet, a csomagolást és a továbbítást.

Válassza ki a toostream élő tartalom, vagy tartalom igény biztosíthat. hello témakör ezenkívül hivatkozásokat is tartalmaz, tooother kapcsolódó témakörökre.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
Az AMS képzési terveket itt tekintheti meg:

* [AMS élő adatfolyam-továbbítási munkafolyamat](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [AMS igényalapú streaming-munkafolyamat](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Előfeltételek

az Azure Media Services toostart hello következő szükséges:

* Egy Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com).
* Egy Azure Media Services-fiók. További információ: [Fiók létrehozása](media-services-portal-create-account.md)
* (Választható:) A fejlesztési környezet beállítása Válassza ki, hogy a .NET vagy a REST API fejlesztési környezetet kívánja-e használni. További információ: [Környezet beállítása](media-services-dotnet-how-to-use.md)

    Emellett ismerje meg, hogyan túl[tooAMS API programozott módon való kapcsolódás](media-services-use-aad-auth-to-access-ams-api.md).
* Elindított állapotú standard vagy prémium szintű streamvégpont.  További információ: [Streamvégpontok felügyelete](media-services-portal-manage-streaming-endpoints.md)

## <a name="sdks-and-tools"></a>SDK-k és eszközök

Media Services-megoldások toobuild használhatja:

* [Media Services REST API](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Hello elérhető ügyfél SDK-k közül:
    * [.NET-keretrendszerhez készült Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services),
    * [Javához készült Azure SDK](https://github.com/Azure/azure-sdk-for-java),
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
    * [Node.js-hez készült Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Ez a Node.js SDK nem Microsoft által készített verziója. Ez karbantartását egy Közösség végzi, és jelenleg nem rendelkezik a 100 %-ában használhassák az hello AMS API-k).
* Meglévő eszközök:
    * [Azure Portal](https://portal.azure.com/)
    * [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Az Azure Media Services Explorer (AMSE) egy Winforms/C#-alkalmazás Windows rendszerre)

## <a name="concepts-and-overview"></a>Alapfogalmak és áttekintés
Az Azure Media Services alapfogalmaiért lásd: [Fogalmak](media-services-concepts.md)

Egy hogyan-tooseries tooall hello fő összetevőit Azure Media Services szolgáltatásban, lásd: [Azure Media Services részletes oktatóprogramjai](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Az adatsorozat fogalmak áttekintést rendelkezik, és hello AMSE eszköz toodemonstrate AMS feladatok használ. Az AMSE eszköz egy Windows-eszköz. Az eszköz támogatja a legtöbb érhet el programozott módon való hello feladatot [.NET-keretrendszerhez készült AMS SDK](https://github.com/Azure/azure-sdk-for-media-services), [Javához készült Azure SDK](https://github.com/Azure/azure-sdk-for-java), vagy [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Támogatott helyzetek és a Media Services rendelkezésre állása az egyes adatközpontokban

Részletes információkért lásd az [AMS-forgatókönyvek és a funkciók és szolgáltatások az egyes adatközpontokban történő rendelkezésre állását](scenarios-and-availability.md) ismertető cikket.

## <a name="service-level-agreement-sla"></a>Szolgáltatói szerződés (SLA)

* A Media Services kódolási funkciójához szükséges REST API-tranzakciókhoz 99,9%-os rendelkezésre állást garantálunk.
* A streamelési funkciók szolgáltatási kéréseinek sikeres kiszolgálására 99,9%-os rendelkezésre állást garantálunk a létező médiatartalmak esetében egy standard vagy prémium szintű streamvégpont megvásárlásakor.
* Az élő csatornák esetében garantáljuk, hogy fut a csatornák lesz külső kapcsolatot hello idő legalább 99,9 %.
* A Content Protection esetében garantáljuk, hogy ában sikeresen teljesítjük kulcs kérelmek legalább 99,9 %-os hello idő.
* Az indexelő esetében sikeresen szolgálja, hogy az egy kódolási feldolgozott indexelési kéréseket hello idő 99,9 % egység.

További információ: [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/)

Rendelkezésre állás biztosításához az adatközpontok kapcsolatos információkért lásd: hello [Avaiability](scenarios-and-availability.md#availability) szakasz.

## <a name="support"></a>Támogatás

[Az Azure-támogatási szolgáltatások](https://azure.microsoft.com/support/options/) támogatási lehetőségeket biztosítanak az Azure szolgáltatásaival, köztük a Media Services szolgáltatással kapcsolatban.

## <a name="provide-feedback"></a>Visszajelzés küldése

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
