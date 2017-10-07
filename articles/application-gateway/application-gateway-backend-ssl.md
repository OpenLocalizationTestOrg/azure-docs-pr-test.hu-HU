---
title: aaaEnabling end tooend Azure Application Gateway SSL |} Microsoft Docs
description: "Ezen a lapon hello Alkalmazásátjáró end tooend támogatja az SSL áttekintést nyújt."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: c5cb398a1e7d9a08662a3120baad98edb5575917
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-end-tooend-ssl-with-application-gateway"></a>Záró tooend SSL-Titkosítást az Alkalmazásátjáró áttekintése

Alkalmazásátjáró támogatja az SSL-lezárást hello átjáróra, után toohello háttérkiszolgálók mely általában a forgalom titkosítás nélkül. Ez a funkció lehetővé teszi, hogy a webes kiszolgálók toobe unburdened a költséges titkosítási és visszafejtési terhelést. Azonban az egyes ügyfelek titkosítatlan kommunikáció toohello háttérkiszolgálók lehetőség nem elfogadható. Titkosítatlan kommunikáció toosecurity követelmények, a megfelelőségi követelmények oka lehet, vagy hello alkalmazás csak biztonságos kapcsolatot fogad el. Az ilyen alkalmazások alkalmazás-átjáró támogatja a záró tooend SSL titkosítást.

## <a name="overview"></a>Áttekintés

Záró tooend SSL lehetővé teszi a toosecurely továbbítja a bizalmas adatok toohello háttér továbbra is kihasználja a réteg 7 terheléselosztási funkciók előnyeit hello mely Alkalmazásátjáró biztosít titkosítja. Ezek a funkciók némelyike munkamenet cookie-alapú kapcsolat, az URL-alapú útválasztási, a támogatási helyek vagy képességét tooinject X - továbbított - alapú útválasztási * fejléceket.

Vége tooend SSL kommunikáció üzemmódban konfigurálásakor Alkalmazásátjáró hello SSL-munkameneteket hello átjáróra leáll, és a felhasználói forgalom visszafejti. Ezt követően végrehajtja hello konfigurált szabályok tooselect egy megfelelő háttér címkészletet példány tooroute forgalmat. Alkalmazásátjáró majd indít el egy új SSL kapcsolat toohello háttérkiszolgáló, és újból titkosítja az adatokat, mielőtt továbbítanák hello kérelem toohello háttér hello háttér kiszolgáló nyilvános kulcsú tanúsítvány használatával. SSL engedélyezve van, úgy, hogy protokollbeállítás BackendHTTPSetting tooHTTPS, majd pedig a végfelhasználók tooend tooa háttérkészlet alkalmazza. Tanúsítvány tooallow biztonságos kommunikációhoz hello háttérkészlet end tooend SSL engedélyezve van a háttér-kiszolgálónként kell konfigurálni.

![tooend ssl forgatókönyvek][1]

Ebben a példában a kérelmek TLS1.2 történő olyan irányított toobackend kiszolgálók Pool1 a záró tooend SSL használatával.

## <a name="end-tooend-ssl-and-whitelisting-of-certificates"></a>Befejezési tooend SSL és a tanúsítványok engedélyezése

Alkalmazásátjáró szerepel az engedélyezési listán hello Alkalmazásátjáró a tanúsítvány rendelkezik ismert háttér-példányok csak kommunikál. tooenable az engedélyezett tanúsítványok, kell feltöltenie hello háttér kiszolgálói tanúsítványok toohello Alkalmazásátjáró (nem hello főtanúsítvány) nyilvános kulcsa. Csak kapcsolatok tooknown és engedélyezési háttérkiszolgálókon majd engedélyezettek. fennmaradó háttérkiszolgálókon hello egy átjáró hibát eredményez. Az önaláírt tanúsítványok csupán tesztelési célokat szolgálnak, és nem ajánlottak éles számítási feladatokra. Ilyen tanúsítványának rendelkeznie kell a toobe engedélyezési hello alkalmazás átjáróval előző lépésekben előtt a felhasználásuk hello leírtak szerint.

## <a name="next-steps"></a>Következő lépések

Záró tooend SSL megismerését követően nyissa meg túl[end tooend SSL az Alkalmazásátjáró engedélyezése](application-gateway-end-to-end-ssl-powershell.md) egy alkalmazást átjáró, amely toocreate tooend SSL végén.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
