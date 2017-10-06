---
title: "az Azure API Management-szabályozás aaaAdvanced kérelem"
description: "Megtudhatja, hogyan toocreate és rugalmas kvóta és sebessége korlátozza az Azure API-felügyeleti házirendek vonatkoznak."
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ac87f83118a37bd587fddf044e5c2d6fc2af9031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Az Azure API Management-szabályozás Speciális kérelem
Az Azure API Management kulcsfontosságú szerepet képes toothrottle bejövő kérelmek folyamatban. Vagy ellenőrző hello gyakorisága a kéréseket, és hello teljes kérelmek/adatokat továbbít, az API Management lehetővé teszi, hogy API szolgáltatók tooprotect az API-kat visszaélés, és hozzon létre külön API termék rétegekhez értékét.

## <a name="product-based-throttling"></a>A termék alapú sávszélesség-szabályozás
toodate, szabályozására hello sebessége korlátozott hatókörű toobeing tooa adott termék-előfizetéshez (lényegében egy kulcs) lett, meghatározott hello API Management publisher portálon. Ez akkor hasznos, ha hello API szolgáltató tooapply vonatkozó korlátozások feliratkozott toouse API hello fejlesztőknek, azonban ez nem működik, például az egyes végfelhasználói hello API-szabályozás. Lehetséges, hogy a hello fejlesztői alkalmazás tooconsume-egy felhasználóhoz hello teljes kvóta, majd előfordulhat, hogy más ügyfelek hello fejlesztő képes toouse hello alkalmazás folyamatban. Is több olyan ügyfelek, akik nagy mennyiségű kérést hozhat létre korlátozhatja a hozzáférést toooccasional felhasználók.

## <a name="custom-key-based-throttling"></a>Egyéni kulcs alapú sávszélesség-szabályozás
új hello [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek biztosítják a megoldás jóval rugalmasabb tootraffic vezérlő. Ezeket az új házirendeket toodefine kifejezések tooidentify hello kulcsok használt tootrack forgalom használati leendő engedélyezése. hello működésének ez easiest látható egy példa a. 

## <a name="ip-address-throttling"></a>IP-cím sávszélesség-szabályozás
hello alábbi házirendek korlátozása egyetlen ügyfél IP-cím tooonly 10 percenként összesen 1 000 000 hívások és 10 000 kilobájt havonta sávszélesség hívja. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Összes ügyfél a hello Internet egy egyedi IP-címet használja, ha ez a felhasználó használat korlátozása hatékony módszer lehet. Azonban ez nagyon valószínű, hogy több felhasználó fog-e a megosztás toothem elérése során hello Internet NAT-eszközön keresztül miatt egyetlen nyilvános IP-címet. Annak ellenére, hogy az API-k esetében, amelyek lehetővé teszik a nem hitelesített hozzáférést hello `IpAddress` hello-érdemes lehet.

## <a name="user-identity-throttling"></a>Felhasználói azonosító sávszélesség-szabályozás
Ha a felhasználó hitelesítése, akkor a sávszélesség-szabályozási kulcs hozható létre, amely egyedileg azonosít egy, amely adatok alapján felhasználó.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

Ebben a példában a Microsoft hello Authorization fejlécet kiolvasni, alakítsa át a túl`JWT` objektumra és hello token tooidentify hello felhasználó hello tulajdonos, és használhassa a kulcs hello sebességével. Ha a hello hello felhasználói identitás `JWT` , más hello egyik jogcímek majd, hogy az érték volt használható helyette.

## <a name="combined-policies"></a>Kombinált házirendek
Új szabályozása házirendekkel hello adja meg a szabályozási házirendek meglévő hello-nál nagyobb mértékben vezérelheti, bár nincs továbbra is érték kombinálásával mindkét szolgáltatást. Termékkulcs előfizetés általi szabályozás ([korlát hívás arányt a előfizetés](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [Set memóriahasználati kvóta előfizetéssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) használati szintek alapján úgy egy API monetizing tooenable remek módja. hello eszközzel kombinálva felhasználóbarát nyomtatott, hogy a felhasználó képes toothrottle kiegészítő és megakadályozza, hogy egy felhasználó viselkedésének hello élménye egy másik terhelése. 

## <a name="client-driven-throttling"></a>Ügyfél-alapú sávszélesség-szabályozás
Ha kulcs-szabályozás hello definiálva van a használatával egy [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx), akkor célszerű hello API szolgáltató, amely megválasztása a módját a sávszélesség-szabályozás hello hatókörébe tartozik-e. Azonban érdemes egy fejlesztő hogyan értékelje toocontrol korlátozzák a saját ügyfelek. Ez sikerült engedélyezni a hello API-szolgáltató egy egyéni fejlécet tooallow hello fejlesztői ügyfél alkalmazás toocommunicate hello kulcs toohello API szemben.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Ez lehetővé teszi a fejlesztői hello ügyfél alkalmazás toochoose toocreate hello sebességével kulcs módját. Egy kis nyújt az ügyfél a fejlesztők kulcsok toousers készleteinek hozzárendelésének és elforgatása hello kulcshasználat létrehozhat saját rétegek.

## <a name="summary"></a>Összefoglalás
Az Azure API Management sebesség és a sávszélesség-szabályozás tooboth ajánlat védelmét, és adja hozzá a tooyour API szolgáltatás biztosít. új sávszélesség-szabályozás egyéni szabályának szabályzatok hello engedélyezi, akkor pontosabban megbízhatatlanná tudják befolyásolni ezen házirendek tooenable az ügyfelek toobuild még élvezetesebbé alkalmazások. hello a cikkben szereplő példák bemutatják, ezeket az új házirendeket hello használata korlátozza az ügyfél IP-címeket, a felhasználói azonosító és a generált ügyfél értékek kulcsok gyártó szerint. Vannak azonban sok más részei üdvözlőüzenetére, például a felhasználói ügynök, az URL-cím elérési út töredék, az üzenet mérete használható.

## <a name="next-steps"></a>Következő lépések
Adja meg a visszajelzést hello disqus-beszélgetésben teheti szálon az ebben a témakörben. Nagyszerű toohear kapcsolatos egyéb lehetséges értékek, amelyek egy logikai választás a forgatókönyvekben lettek volna.

## <a name="watch-a-video-overview-of-these-policies"></a>A házirendek áttekintő videó megtekintése
Hello bővebben [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) a cikkben szereplő házirendek adjon tekintse meg a következő videó hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

