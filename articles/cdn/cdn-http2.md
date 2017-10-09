---
title: "Azure CDN aaaHTTP/2 támogatást |} Microsoft Docs"
description: "Ismerje meg a HTTP/2 és a CDN támogatása."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 2e5e5345e8cf5c40e080ebf18b4f13a239a5aac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="http2-support-in-azure-cdn"></a>Az Azure CDN HTTP/2-támogatás

A HTTP/2 egy nagyobb javítása tooHTTP/1.1\. Biztosít gyorsabban webes teljesítmény, a csökkentett válaszidő és a jobb felhasználói élmény, hello ismerős HTTP-metódus állapotkódok és szemantikáját megőrzésével. Bár a HTTP/2 HTTP és HTTPS tervezett toowork, számos ügyfél webböngésző csak támogatja a HTTP/2 TLS feletti.

###<a name="http2-benefits"></a>A HTTP/2 előnyei

a HTTP/2 hello előnyei a következők:

*   **Multiplexáló és feldolgozási**

    HTTP 1.1 több és több erőforrás-kérelmek több TCP-kapcsolatot igényel, és minden egyes kapcsolathoz teljesítményigény társítva van. A HTTP/2 lehetővé teszi, hogy több erőforrás toobe egyetlen TCP-kapcsolatot kért.

*   **Fejléc tömörítése**

    Hello HTTP-fejlécek szolgálatban erőforrások tömörítésével hello keresztülhaladnak a hálózaton idő jelentősen csökkent.

*   **Adatfolyam-függőségek**

    Az adatfolyam függőségek hello ügyfél tooindicate toohello kiszolgálót, amely az erőforrások prioritással rendelkezik.


##<a name="http2-browser-support"></a>A HTTP/2 webböngésző támogatása

Az összes ismertebb böngésző hello alkalmaztunk, hogy HTTP/2-támogatás az aktuális verzióban. Nem támogatott böngészőkkel automatikusan tartalék tooHTTP/1.1.

|Böngésző|Minimális verzió|
|-------------|------------|
|A Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

##<a name="enabling-http2-support-in-azure-cdn"></a>Az Azure CDN HTTP/2-támogatás engedélyezése

Jelenleg aktív a HTTP/2 támogatási **Akamai Azure CDN** és **Azure CDN Verizon** profilok. Az ügyfelek semmilyen további műveletet kell biztosítania.

##<a name="next-steps"></a>Következő lépések

toosee hello előnyei HTTP/2 működés közben, lásd: [ebben a bemutatóban a Akamai](https://http2.akamai.com/demo).

toolearn bővebben HTTP/2, látogasson el a következő erőforrások hello:

*   [A HTTP/2 specification-kezdőlap](https://http2.github.io/)
*   [Hivatalos HTTP/2 – gyakori kérdések](https://http2.github.io/faq/)
*   [Akamai HTTP/2-információk](https://http2.akamai.com/)

További információ az Azure CDN szolgáltatásai toolearn lásd: hello [Azure CDN áttekintése](https://azure.microsoft.com/documentation/articles/cdn-overview/).
