---
title: a CORS Azure CDN aaaUsing |} Microsoft Docs
description: Ismerje meg, hogyan toouse hello Azure Content Delivery Network (CDN) toowith Cross-Origin Resource Sharing (CORS).
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c743b56c32a2d3aacc9a77094cb87d61b95d2f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-cdn-with-cors"></a>Az Azure CDN a CORS használatával
## <a name="what-is-cors"></a>Mi az a CORS?
A CORS (Cross eredetű erőforrások megosztása) egy HTTP-szolgáltatás, amely lehetővé teszi egy webalkalmazást az egyik tartomány tooaccess erőforrásainak fut egy másik tartományban. Rendelés tooreduce hello lehetőségét többhelyes parancsfájlok futtatására, a minden modern webböngésző néven ismert biztonsági korlátozások megvalósítása [azonos eredetű házirend](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Ez megakadályozza, hogy a hívó API-k egy másik tartományban származó weblap.  A CORS egy biztonságos módon tooallow egy eredeti adatforrással (hello forrástartományt) toocall API-k az eredeti biztosít.

## <a name="how-it-works"></a>Működés
A CORS kérelmek két különböző *egyszerű kérelmek* és *összetett kérelmeket.*

### <a name="for-simple-requests"></a>Egyszerű kérelmeknél:

1. hello böngésző küldi hello CORS kérelem további **származási** HTTP-kérés fejlécének. hello a fejléc értéke hello származási hello szülő lapon, amely hello kombinációja típusúként van definiálva szolgáltatott *protokoll* *tartomány,* és *port.*  Amikor egy lapot https://www.contoso.com próbál tooaccess hello fabrikam.com forrása a felhasználó adatai, a következő fejléc hello küldheti toofabrikam.com:

   `Origin: https://www.contoso.com`

2. hello server hello következő jelenhetnek meg:

   * Egy **hozzáférés-vezérlési-engedélyezése-forrás** jelző a származási hely engedélyezett válaszában fejléc. Példa:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Egy HTTP-hibakód: például a 403-as, ha hello server nem engedélyezi a hello eltérő eredetű kérelem hello származási fejléc ellenőrzése után

   * Egy **hozzáférés-vezérlési-engedélyezése-forrás** helyettesítő karakter, amely lehetővé teszi minden eredet fejléc:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Összetett kérelmeknél:

Egy összetett vonatkozó kérés, mert a CORS kérelem hello böngésző esetén szükséges toosend egy *ellenőrzési kérés* (azaz egy előzetes mintavételt) hello tényleges CORS kérelem elküldése előtt. hello ellenőrzési kérés hello server engedélyt kér Ha hello eredeti CORS kérelem lépne, és van egy `OPTIONS` toohello kérelem URL-CÍMÉRE.

> [!TIP]
> A CORS-adatfolyamok és közös nehézségek további részletekért tekintse meg a hello [tooCORS útmutató REST API-kat tartalmaz](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Helyettesítő karakteres vagy egyetlen forrás forgatókönyvek
Az Azure CDN CORS automatikusan együttműködve nincs további konfigurálásra, ha hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc értéke toowildcard (*) vagy egy egyetlen forrása.  hello CDN hello első válasz gyorsítótárazni fog, és később fogja használni a hello azonos fejléc.

Ha kérelmek már végzett toohello CDN előzetes tooCORS a hello beállítása a forrás, lesz szükség toopurge tartalmat a végpont tartalom tooreload hello hello tartalom **hozzáférés-vezérlési-engedélyezése-forrás** fejléc.

## <a name="multiple-origin-scenarios"></a>Több forrás forgatókönyv
Ha módosítania kell a CORS engedélyezett eredetet toobe listának tooallow, részek lesznek egy kicsit bonyolultabb. hello probléma akkor fordul elő, amikor gyorsítótárazza a hello CDN hello **hozzáférés-vezérlési-engedélyezése-forrás** hello első CORS forrás fejléc.  Ha egy másik CORS származási későbbi kérést, hello CDN teljesíti a gyorsítótárazott hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc, amelyek nem egyeznek.  Nincsenek számos módon toocorrect ez.

### <a name="azure-cdn-premium-from-verizon"></a>Prémium szintű Azure CDN a Verizontól
legjobb módja tooenable hello Ez az toouse **verizon Azure CDN Premium**, amely azt mutatja, néhány speciális funkcióinak. 

Túl kell[hozzon létre egy szabályt](cdn-rules-engine.md) toocheck hello **származási** fejléc hello kérésre.  Ha egy érvényes forrás, a szabály állítja be hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc a következő hello kérelemben megadott hello forrása.  Ha hello származási megadott hello **származási** fejléc nem engedélyezett, a szabály el kell hagynia hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc, aminek következtében hello böngésző tooreject hello kérelem. 

Nincsenek két módon toodo ez hello szabályok motor.  Mindkét esetben hello **hozzáférés-vezérlési-engedélyezése-forrás** hello fájl forráskiszolgálóról fejléc teljesen figyelmen kívül hagyva, hello CDN szabálymotor teljes körű kezelése hello CORS források engedélyezett.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Minden érvényes eredet egy reguláris kifejezések
Ebben az esetben létre fog hozni egy reguláris kifejezés, amely tartalmazza az összes hello források tooallow szeretne: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Verizon Azure CDN** használ [Perl kompatibilis reguláris kifejezések](http://pcre.org/) , a reguláris kifejezések motor.  Egy eszköz, például használhatja [reguláris kifejezések 101](https://regex101.com/) toovalidate a reguláris kifejezés.  Vegye figyelembe, hogy hello "/" karakter érvényes reguláris kifejezések, és nem szükséges escape-karaktersorozatot toobe, azonban ez a karakter escape ajánlott eljárás, és néhány regex érvényesség-ellenőrzők által várt.
> 
> 

Hello reguláris kifejezésnek megfelelő, ha a szabály felülírja, hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc (ha van ilyen) hello forrás a hello kérelmet küldött a hello a forrásból.  Azt is megteheti további CORS fejlécek, például a **hozzáférés-vezérlési-engedélyezése-metódusok**.

![A reguláris kifejezéssel szabályok – példa](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Az egyes származási kérelmek fejléc szabályt.
Reguláris kifejezések, hanem ehelyett hozzon létre a külön szabály minden forrás hello segítségével tooallow kívánja **kérelem fejléc helyettesítő** [feltételének](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Mint hello reguláris kifejezés módszerrel hello szabályok motor különálló beállítása hello CORS fejlécek. 

![Szabályok reguláris kifejezés nélkül – példa](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> Hello a fenti példában a hello hello helyettesítő karakter használatát * hello szabályok motor toomatch közli a HTTP és HTTPS.
> 
> 

### <a name="azure-cdn-standard"></a>Az Azure CDN Standard
Az Azure CDN Standard profilok hello csak a több források hello helyettesítő származási hello használata nélkül mechanizmus tooallow toouse [lekérdezési karakterláncok gyorsítótárazása](cdn-query-string.md).  Tooenable lekérdezési karakterláncának beállítása szükséges hello CDN-végpontot, és ezután használja az egy egyedi lekérdezési karakterlánc minden olyan engedélyezett tartományhoz érkező kérelmeket. Ez azt eredményezi, hogy hello CDN gyorsítótárazás egy külön objektum minden egyes egyedi lekérdezési karakterlánc. Ez a módszer nem ideális, azonban több példányát hello okoz, ugyanazon fájl a gyorsítótárazott hello CDN.  

