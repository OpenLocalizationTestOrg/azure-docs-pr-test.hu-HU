---
title: "Azure CDN gyorsítótárazási házirend Azure Media Services aaaManage |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage gyorsítótárazási házirend Azure CDN szolgáltatás használata az Azure Media Services."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 4c7ee2922fcbb5727e10fbe44fc6e61b79f6917c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Azure CDN gyorsítótárazási házirend Azure Media Services kezelése
Azure Media Services biztosította HTTP alapú adaptív Streameléshez és progresszív letöltés. A HTTP-alapú adatfolyam kiválóan méretezhető, proxy és a CDN rétegek, valamint ügyféloldali gyorsítótárazás gyorsítótárazásának előnyt. Adatfolyam-végpontok nyújt általános adatfolyam képességek, valamint a gyorsítótár HTTP-fejlécek konfigurációját. Adatfolyam-végpontok állítja be a HTTP-Cache-Control: maximális-kor és Expires fejléc. További részleteket a HTTP-gyorsítótár fejléceket [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Alapértelmezett gyorsítótárazását fejlécek
Alapértelmezés szerint az adatfolyam-végpontok 3 nap gyorsítótár fejlécek az igényalapú streamelési adatok (tényleges media töredék/adattömbök) és manifest(playlist) alkalmazni. Az élő adatfolyamként történő streamvégpontok 3 nap gyorsítótár fejlécek adatok (tényleges media töredék/adattömbök) vonatkoznak, és 2 másodperc gyorsítótár manifest(playlist) kérelem fejléce. Élő program tooon igény szerinti (élő archív) kerül, majd igény szerinti adatfolyam-továbbítási gyorsítótár fejlécek vonatkoznak.

## <a name="azure-cdn-integration"></a>Azure CDN-integráció
Azure Media Services biztosította [CDN integrált](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) streaming-végpontok. A cache-control fejlécek alkalmazza a hello azonos módon adatfolyam-továbbítási végpontok tooCDN streamvégpontok engedélyezve van. Belső streaming beállított végpont gyorsítótár értékek toodefine hello élettartama hello Azure CDN által használt objektumok a gyorsítótárban, és használja arra is, az érték tooset hello kézbesítési gyorsítótár fejlécek. Engedélyezésekor CDN használatával streamvégpontok tooset kis gyorsítótár értékek nem ajánlott. Kis beállításértékek csökkentheti hello teljesítményét, és csökkentheti a CDN hello előnyeit. Nem engedélyezett tooset gyorsítótár fejlécek kisebb, mint a CDN 600 másodperc streamvégpontok engedélyezve van.

> [!IMPORTANT]
>Azure Media Services Azure CDN teljes integrálva van. Egyetlen kattintással összes hello elérhető Azure CDN szolgáltatók (Akamai és Verizon) tooyour adatfolyam-továbbítási végpontra, beleértve a CDN Standard és Premium termékek integrálhatja. További információkért tekintse meg a [közlemény](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> A végpont tooCDN streaming adatok díjak csak lekérdezi a letiltott hello CDN engedélyezése adatfolyam-továbbítási végpontra API-k vagy az Azure felügyeleti portálon adatfolyam végpont szakaszának használatával. Közvetlenül a CDN API-k vagy a portál szakaszban CDN-végpont létrehozása vagy manuális integrációs nem letiltja a hello adatok díjakat.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Gyorsítótár-fejlécek konfigurálása az Azure Media Services
Használhatja az Azure felügyeleti portálon vagy az Azure Media Services API-k tooconfigure gyorsítótár fejléc értékei.

1. tooconfigure gyorsítótár fejlécek felügyeleti portál tekintse meg a túl[hogyan tooManage adatfolyam-végpontok](../media-services/media-services-portal-manage-streaming-endpoints.md) szakasz konfigurálása hello Streaming Endpoint.
2. Azure Media Services REST API-t [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK-val [StreamingEndpointCacheControl tulajdonságok](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Gyorsítótár konfigurációs prioritási sorrendje
1. Az Azure Media Services konfigurált gyorsítótár érték felülbírálja az alapértelmezett érték.
2. Ha nincs kézi konfigurálásra, alapértelmezett értékek vonatkozik.
3. Alapértelmezés szerint 2 másodperc gyorsítótár fejlécek vonatkozik toolive függetlenül Azure Media vagy az Azure Storage manifest(playlist) streamelésre és az ennek az értéknek felülbírálása nem érhető el.

