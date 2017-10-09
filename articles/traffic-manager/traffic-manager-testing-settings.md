---
title: "aaaVerify Azure Traffic Manager-beállítások |} Microsoft Docs"
description: "Ez a cikk segít a Traffic Manager beállításainak ellenőrzése"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 2180b640-596e-4fb2-be59-23a38d606d12
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: c670be6cf55e140c7ab63d5d526de08e14774d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="verify-traffic-manager-settings"></a>A Traffic Manager beállításainak ellenőrzése

tootest a Traffic Manager beállítások toohave több, különböző helyeken, ahol a tesztek futtatása az ügyfelek van szüksége. Ezt követően is betöltheti hello végpontok le egyenként, a Traffic Manager-profil.

* Állítsa be úgy hello DNS-élettartam értéke alacsony, módosításának propagálása gyorsan (például az 30 másodperc).
* Hello ismeri az Azure felhőszolgáltatások és webhelyek teszteli hello-profil IP-címét.
* Eszközök, amelyek lehetővé teszik a DNS neve tooan IP-címekhez, és megjeleníti ezt a címet használja.

Toosee, hogy hello DNS-nevek feloldása tooIP címek hello végpont a profil ellenőrzése hello nevek hello forgalom-útválasztási módszert hello Traffic Manager-profil definiált konzisztens módon oldja fel. Hello eszközök például **nslookup** vagy **átrágniuk** tooresolve DNS-neveket.

hello alábbi példák segítséget nyújtanak a Traffic Manager-profil tesztelése.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Ellenőrizze az nslookup és ipconfig használatát Windows Traffic Manager-profil

1. Nyissa meg rendszergazdaként egy parancs vagy a Windows PowerShell parancssorába.
2. Típus `ipconfig /flushdns` tooflush hello DNS-gyorsítótárban.
3. Gépelje be: `nslookup <your Traffic Manager domain name>`. Például a következő ellenőrzések hello tartománynév hello előtaggal rendelkező parancs hello *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Általános eredményt jeleníti meg a következő információ hello:

    + hello DNS-nevét és IP-cím hello DNS kiszolgáló, amely nem érhető el, tooresolve a Traffic Manager szolgáltatásbeli tartománynevére.
    + hello Traffic Manager szolgáltatásbeli tartománynevére beírt hello parancssorban "nslookup" után, és hello IP cím toowhich hello Traffic Manager-tartományra mutat. hello második IP-cím hello fontos egy toocheck. Hello felhőszolgáltatások és webhelyek hello Traffic Manager-profil teszteli az egyik nyilvános virtuális IP-cím (VIP) címet meg kell felelnie.

## <a name="how-tootest-hello-failover-traffic-routing-method"></a>Hogyan tootest hello feladatátvételi forgalom-útválasztási módszer

1. Hagyja végpontjai fel.
2. DNS-feloldás a vállalata tartománynevét, nslookup vagy egy hasonló segédprogram segítségével az egyetlen ügyfél használatával kérelmet.
3. Győződjön meg arról, hogy hello feloldani az IP-cím megegyezik hello elsődleges végpont.
4. Állítsa le az elsődleges végpont, vagy távolítsa el a fájl figyelése, hogy le van állítva a Traffic Manager úgy, hogy az alkalmazás hello értelmezi hello.
5. Várjon, amíg a Traffic Manager-profil hello hello DNS idő élettartamát (TTL), valamint további két percet. Például ha a DNS-élettartam 300 másodpercen (5 percen), meg kell várnia a hét perc.
6. Ürítse ki a DNS-ügyfél gyorsítótárába, a kérelem DNS névfeloldásban nslookup parancs segítségével. A Windows rendszerben ürítse ki a DNS-gyorsítótár hello ipconfig/flushdns paranccsal.
7. Győződjön meg arról, hogy hello feloldani az IP-cím megegyezik a másodlagos végponti.
8. Ismételje meg a hello folyamatot, mindegyik végpont pedig leáll. Győződjön meg arról, hogy hello DNS hello lista hello következő végpontjának hello IP-címét adja vissza. Minden végpontok nem működnek, amikor újra kell beszerezniük hello elsődleges végpont hello IP-címe.

## <a name="how-tootest-hello-weighted-traffic-routing-method"></a>Hogyan tootest hello súlyozott forgalom-útválasztási módszert

1. Hagyja végpontjai fel.
2. DNS-feloldás a vállalata tartománynevét, nslookup vagy egy hasonló segédprogram segítségével az egyetlen ügyfél használatával kérelmet.
3. Győződjön meg arról, hogy hello feloldani az IP-cím egyezik a végpontokat.
4. Ürítse ki a DNS-ügyfél gyorsítótárába, és ismételje meg a 2. és 3 végpontok. Megtekintheti az adott vissza az egyes a végpontok különböző IP-címeket.

## <a name="how-tootest-hello-performance-traffic-routing-method"></a>Hogyan tootest hello teljesítmény forgalom-útválasztási módszer

tooeffectively tesztelje a teljesítményt forgalom-útválasztási módszert, hello világ különböző pontjain lévő ügyfelek kell rendelkeznie. Létrehozhat ügyfelek különböző Azure régiókban, amely használt tootest a szolgáltatásokhoz. Ha egy globális hálózata, távolról hello világ többi részén tooclients bejelentkezés és ott a tesztek futtatása.

Azt is megteheti, nincsenek ingyenes, webalapú DNS-címkeresés és elmerülne a rendszer az elérhető szolgáltatások. Néhány esetben eszközöket adjon meg hello képességét toocheck DNS-név feloldása hello világ különböző helyekről. Hajtsa végre a Keresés a "DNS-címkeresés" példákat. Külső szolgáltatásokat, mint a Gomez vagy Előadásbeli lehet, hogy a profilok terjeszti forgalom várt módon használt tooconfirm.

## <a name="next-steps"></a>Következő lépések

* [A Traffic Manager forgalom-útválasztási módszerei](traffic-manager-routing-methods.md)
* [A Traffic Manager teljesítményével kapcsolatos megfontolások](traffic-manager-performance-considerations.md)
* [A Traffic Manager csökkentett teljesítményének elhárítása](traffic-manager-troubleshooting-degraded.md)
