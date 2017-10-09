---
title: "aaaAzure CDN áttekintése |} Microsoft Docs"
description: "Ismerje meg, milyen hello Azure tartalom Delivery Network (CDN) van, és hogyan toouse azt toodeliver a tartalmak nagy sávszélességű blobok és a statikus tartalom gyorsítótárazása révén."
services: cdn
documentationcenter: 
author: smcevoy
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/08/2017
ms.author: v-semcev
ms.openlocfilehash: e0230a6e107969b845985f2f4d357bf93cd40d42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-content-delivery-network-cdn"></a>Hello Azure Content Delivery Network (CDN) áttekintése
> [!NOTE]
> Ez a dokumentum ismerteti, milyen hello Azure Content Delivery Network (CDN) van, hogyan működik és minden egyes Azure CDN termék hello funkcióit.  Ha tooskip szeretné ezeket az információkat, és hogyan egyenes tooa oktatóanyag lépjen a CDN-végpontok toocreate lásd: [az Azure CDN](cdn-create-new-endpoint.md).  Ha azt szeretné, hogy a jelenlegi CDN-csomóponti helyek listája toosee, [Azure CDN POP-helyek](cdn-pop-locations.md).
> 
> 

hello Azure tartalom Delivery Network (CDN) gyorsítótárba helyezi azt a statikus webtartalmakat stratégiailag kiválasztott helyeken tooprovide maximális átviteli sebesség tartalom toousers kézbesítéséhez.  hello CDN tartalmak nagy sávszélességű kézbesítéséhez között hello world hello tartalom fizikai csomópontokon gyorsítótárazásával globális megoldást kínál a fejlesztők. 

hello CDN toocache webhely eszközök használatának hello előnyei a következők:

* Jobb teljesítmény és felhasználói élmény a végfelhasználók számára, különösen akkor, ha tooload tartalom alkalmazásokat használ, amelyben a kiszolgálóval végzett több adatváltásra is szükséges.
* Nagy méretű méretezési toobetter leíró azonnali nagy terhelés, például a termék hello elején indítsa el az eseményt.
* Felhasználói kérelmek elosztásával és a tartalom peremhálózati kiszolgálókról, amelyet kevesebb adatforgalom toohello forrása.

## <a name="how-it-works"></a>Működés
![A CDN áttekintése](./media/cdn-overview/cdn-overview.png)

1. A felhasználó (Anna) fájlt (más néven objektumot) kérelmez egy speciális tartománynevet (pl. `<endpointname>.azureedge.net`) tartalmazó URL-cím használatával.  DNS hello kérelem toohello legjobban teljesítő pont nyújtó jelenléti (POP) helyre irányítja.  Általában ez a hello toohello-felhasználó földrajzilag legközelebb elhelyezkedő jelenléti pont.
2. Ha hello POP hello peremhálózati kiszolgálóinak gyorsítótárában nem rendelkezik hello fájl, a hello peremhálózati kiszolgáló lekéri a hello fájl hello a forrásból.  hello forrás lehet az Azure Web Apps, Azure Cloud Service, Azure Storage-fiók vagy bármilyen nyilvánosan elérhető webkiszolgáló.
3. hello forrás visszaküldi hello fájl toohello biztonsági kiszolgálón, beleértve a hello fájl idő élettartamát (TTL) leíró nem kötelező HTTP-fejlécek.
4. hello peremhálózati kiszolgáló gyorsítótárazza a hello fájlt, és hello fájl toohello eredeti kérelmezőnek (Anna) adja vissza.  hello fájl hello TTL lejárata marad hello peremhálózati kiszolgáló gyorsítótárában.  Ha hello forrás nem ad meg Élettartamot, hello alapértelmezett élettartam (TTL) hét nap.
5. További felhasználók előfordulhat, hogy akkor kérelem hello azonos fájlt, azonos URL-címet használ, és is irányított toothat azonos POP.
6. Hello TTL hello fájl még nem járt le, ha hello biztonsági kiszolgáló hello fájl hello gyorsítótárból adja vissza.  Az eredmény nagyobb sebesség, dinamikusabb működés, vagyis összességében jobb felhasználói élmény.

## <a name="azure-cdn-features"></a>Az Azure CDN szolgáltatásai
Három Azure CDN termék áll rendelkezésre: az **Akamai Azure CDN Standard**, a **Verizon Azure CDN Standard** és a **Verizon Azure CDN Premium**.  hello alábbi táblázat az egyes termékek hello szolgáltatásokat.

|  | Akamai Standard | Verizon Standard | Verizon Premium |
| --- | --- | --- | --- |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Teljesítménnyel kapcsolatos szolgáltatások és optimalizálási lehetőségek__ |
| [Dinamikus helygyorsítás](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&amp;#x2713;;**  | **&amp;#x2713;;** | **&amp;#x2713;;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Dinamikus helygyorsítás – Adaptív képtömörítés](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&amp;#x2713;;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Dinamikus helygyorsítás – Előzetes objektumbetöltés](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&amp;#x2713;;**  |  |  |
| [Online streamelés optimalizálása](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&amp;#x2713;;**  | \* |  \* |
| [Nagyméretű fájlok optimalizálása](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&amp;#x2713;;**  | \* |  \* |
| [Globális kiszolgálói terheléselosztás (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Gyors végleges törlés](cdn-purge-endpoint.md) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Objektumok előzetes betöltése](cdn-preload-endpoint.md) | |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Lekérdezési sztringek gyorsítótárazása](cdn-query-string.md) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| Kettős verem (IPv4/IPv6) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [HTTP/2-támogatás](cdn-http2.md) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Biztonság__ |
| HTTPS-támogatás CDN-végponttal |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Egyéni tartományi HTTPS](cdn-custom-ssl.md) | |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Egyéni tartománynevek támogatása](cdn-map-content-to-custom-domain.md) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Geoszűrés](cdn-restrict-access-by-country.md) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Jogkivonat-hitelesítés](cdn-token-auth.md)|  |  |**&amp;#x2713;;**| 
| [Védelem DDOS-támadások ellen](https://www.us-cert.gov/ncas/tips/ST04-015) |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Elemzések és jelentéskészítés__ |
| [Egyszerűsített analitika](cdn-analyze-usage-patterns.md) | **&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Speciális HTTP-jelentések](cdn-advanced-http-reports.md) | | |**&amp;#x2713;;** |
| [Valós idejű statisztikák](cdn-real-time-stats.md) | | |**&amp;#x2713;;** |
| [Valós idejű riasztások](cdn-real-time-alerts.md) | | |**&amp;#x2713;;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Könnyű használat__ |
| Egyszerű integráció az Azure-szolgáltatásokkal – például a [Storage](cdn-create-a-storage-account-with-cdn.md), a [Cloud Services](cdn-cloud-service-with-cdn.md), a [Web Apps](../app-service-web/app-service-web-tutorial-content-delivery-network.md) és a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) szolgáltatással. |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| Felügyelet [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) vagy [PowerShell](cdn-manage-powershell.md) használatával. |**&amp;#x2713;;** |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [Testreszabható, szabályalapú tartalomkézbesítési motor](cdn-rules-engine.md) | | |**&amp;#x2713;;** |
| Gyorsítótár-/fejlécbeállítások (a [szabálymotorral](cdn-rules-engine.md)) | | |**&amp;#x2713;;** |
| URL-átirányítás/átírás (a [szabálymotorral](cdn-rules-engine.md)) | | |**&amp;#x2713;;** |
| Mobileszközökre vonatkozó szabályok (a [szabálymotorral](cdn-rules-engine.md)) | | |**&amp;#x2713;;** |

\* A Verizon támogatja a nagy méretű fájlok és médiatartalmak küldését általános webes kézbesítésen keresztül.


> [!TIP]
> Van olyan szolgáltatás, az Azure CDN toosee szeretné?  [Küldjön visszajelzést!](https://feedback.azure.com/forums/169397-cdn) 
> 
> 

## <a name="next-steps"></a>Következő lépések
a CDN, használatába tooget lásd: [az Azure CDN](cdn-create-new-endpoint.md).

Ha Ön már CDN-ügyfél, kezelheti a CDN-végpontokat keresztül hello [Microsoft Azure-portálon](https://portal.azure.com) vagy [PowerShell](cdn-manage-powershell.md).

toosee CDN hello a művelet, tekintse meg a hello [videót a Build 2016 munkamenet](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Megtudhatja, hogyan tooautomate Azure CDN rendelkező [.NET](cdn-app-dev-net.md) vagy [Node.js](cdn-app-dev-node.md).

Díjszabási információkért tekintse meg [A tartalomkézbesítési hálózat (CDN) díjszabása](https://azure.microsoft.com/pricing/details/cdn/) című cikket.

