---
title: "az Azure Application Gateway aaaSSL házirendek – áttekintés |} Microsoft Docs"
description: "Tudnivalók Azure Application Gateway lehetővé teszi a tooconfigure SSL-házirend"
services: application gateway
documentationcenter: na
author: amsriva
manager: 
editor: 
tags: azure resource manager
ms.service: application gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure services
ms.date: 08/03/2017
ms.author: amsriva
ms.openlocfilehash: 80dbc437da3dba8a558c518ecdbb5927a11a2ed9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-ssl-policy-overview"></a>Alkalmazás átjáró SSL házirendek – áttekintés

Alkalmazás átjáró lehetővé teszi toocentralize SSL-tanúsítványok kezelését, így csökkentheti a titkosítási és visszafejtési járó többletterhelést egy háttér-kiszolgálófarm. A központi SSL kezelésére is lehetővé teszi a toospecify központi SSL házirend alkalmas tooyour szervezeti biztonsági követelményeinek. Ez segít felel meg a megfelelőségi követelményeket, valamint biztonsági irányelvek és ajánlott eljárások.

SSL-házirend vezérelhető, hogy az SSL protokoll verziója, valamint a titkosító csomagok és egy SSL-kézfogás során használt titkosításokat hello sorrendben tartalmazza. Az Alkalmazásátjáró felé kínál a kétféle módszer tooallow ügyfelek toocontrol SSL-házirend, előre definiált vagy egyéni szabályzatot.

## <a name="predefined-ssl-policy"></a>Előre definiált SSL-házirend

Alkalmazásátjáró három előre meghatározott biztonsági házirend tartozik. Egyetlen ezen házirendek tooget hello megfelelő szintű biztonságot konfigurálhatja az átjárót. hello házirendek nevének hello évhez és hónaphoz konfigurálva volt szerint vannak feliratozva. Minden egyes házirend ajánlatok különböző SSL protokoll verziói és titkosító csomagok. Toouse hello legújabb SSL házirend tooensure hello ajánlott SSL biztonsági ajánlott. Alkalmazásátjáró ma lehetővé teszi a felhasználók toochoose az egyik hello a következő előre meghatározott.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Tulajdonság  |Érték  |
|---|---|
|Név     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Alapértelmezett| Igaz (Ha nincs előre definiált szabályzattal meg van adva) |
|Titkosítási készletek     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
  ### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Tulajdonság  |Érték  |
|   ---      |  ---       |
|Név     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_0        |
|Alapértelmezett| False (Hamis) |
|Titkosítási készletek     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Tulajdonság  |Érték  |
|---|---|
|Név     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_2        |
|Alapértelmezett| False (Hamis) |
|Titkosítási készletek     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-ssl-policy"></a>Egyéni SSL-házirend

Ha egy előre megadott SSL-házirend toobe az igényeinek megfelelően konfigurálni kell, akkor futnia kell a saját egyéni SSL-házirend meghatározásához. Ebben az esetben akkor hello minimális SSL protokoll verziója toosupport teljes szabályozhatják, valamint a támogatott titkosítócsomagok és a prioritásuk szerinti sorrendben történik.
 
Az SSL protokoll verziója:

* Az SSL 2.0 és 3.0 alapértelmezés szerint minden alkalmazásátjárón le van tiltva. Ezen protokoll verziói esetében nem konfigurálhatók.
* Egyéni SSL házirend definition ad meg a következő három protokollok verzióként hello minimális SSL protokoll az átjáró TLSv1_0, TLSv1_1, TLSv1_2 hello bármelyik tooselect lehetőséget.
* Ha nincs SSL-házirend van beállítva mind a három (TLSv1_0, TLSv1_1, TLSv1_2) engedélyezve van.

Titkosítási csomagok:

Alkalmazásátjáró, amelyből az egyéni házirend választhatók titkosító csomagok a következő hello támogatja. hello prioritási sorrendben során SSL-egyeztetést hello hello titkosító csomag sorrendje határozza meg.

Rendelkezésre álló titkosító csomagok:

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

## <a name="next-steps"></a>Következő lépések

[Alkalmazásátjáró SSL-házirend konfigurálása](application-gateway-configure-ssl-policy-powershell.md)
