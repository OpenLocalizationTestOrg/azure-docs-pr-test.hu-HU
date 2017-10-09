---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 16b098198674c49567d720d0c01d984cb763ed8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes"></a>Kibocsátási megjegyzések

## <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Javítsa ki, amelyek ritkán fordulhat elő, amikor a program meghívja crash `EngagementAgentUtils.isInDedicatedEngagementProcess`, amely is használják hello `EngagementApplication` osztály.

## <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8 támogatása (korábbi verzióiban hello SDK Android 8 nem fog működni).
* Nincs több függőség támogatási függvénytára.
* Távolítsa el `EngagementFragmentActivity` osztály.
* Esedékes túl[háttér végrehajtási korlátozások](https://developer.android.com/preview/features/background.html) Android 8, a háttér-naplók előfordulhat, hogy később, amíg hello felhasználó hello eszköz kommunikál, ez hatással lesz a leküldéses kampány **kézbesített** és **Rendszerértesítő jelenik meg** Ha hello eszköz alvó volt folyamatban késleltetett statisztika (hello értesítési továbbra is megjelenik, fog gyűrű és mozog hibák nélkül valós időben).
* Esedékes túl[háttér hely korlátok](https://developer.android.com/preview/features/background-location-limits.html), háttérben hello valós idejű hely nem fog frissülni gyakran Android 8.

## <a name="424-03302017"></a>4.2.4 (03/30/2017)
* Javítsa ki az alkalmazáson belüli értesítés szöveg színét az Android 7 toobe hello azonos régebbi Android verzióként.

## <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Nincsenek további Wi-Fi zárolása.
* Javítsa ki a holtpont getDeviceId előtt init (a hiba 4.2.0 bevezetett) hívásakor.

## <a name="422-05172016"></a>4.2.2 (05/17/2016)
* Jobb stabilitás.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)
* Biztonsági: tiltsa le a webes nézet helyi fájl hozzáférést.
* Biztonsági: távolítsa el `EngagementPreferenceActivity` elavult, és nem biztonságos osztályt `PreferenceActivity` osztály.
* Biztonsági: tevékenységek is reach dokumentált toouse `exported="false"`, ez a jelző is használható a korábbi SDK-verziókban.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)
* hello SDK MIT most licencbe.
* Lehetővé teszi egyéni eszköz azonosító megadása SDK inicializálása során.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)
* Jobb stabilitás.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)
* Jobb stabilitás.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)
* Jobb stabilitás.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)
* Jobb stabilitás.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)
* Jobb stabilitás.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)
* Új engedélyezési modelljének Android M kezeléséhez.
* Segítségével mostantól beállíthatja hely funkciók használata helyett futásidőben `AndroidManifest.xml`.
* Javítsa ki az engedély Programhiba: Ha `ACCESS_FINE_LOCATION`, majd `ACCESS_COARSE_LOCATION` már nincs rá szükség.
* Jobb stabilitás.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)
* Belső protokoll toomake elemzések és leküldéses megbízhatóbb változik.
* Natív leküldéses (GCM/ADM) most is szolgál az alkalmazáson belüli értesítések, konfigurálnia kell a hello hitelesítő adatokat a natív leküldéses kampány bármilyen típusú.
* Javítsa ki a nagy vonalakban tekinti értesítési: után leküldött éppen megjelenített csak 10 egység voltak.
* Hárítsa el a hiba a webes nézet: a hivatkozásra kattintva is végrehajtása hello alapértelmezett műveleti URL-cím.
* Hárítsa el a ritka összeomlási kapcsolódó toolocal tárolók kezelése.
* Javítsa ki a dinamikus konfigurációkezelés karakterlánc.
* Frissítse a végfelhasználói LICENCSZERZŐDÉST.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)
* Az Azure Mobile Engagement eredeti kiadás
* a kapcsolódási karakterlánc konfigurációs appId konfigurációs cseréli le.
* API toosend és a tetszőleges XMPP-üzeneteket fogadjon tetszőleges XMPP-entitások.
* API toosend eltávolítva és eszközök közötti üzeneteket fogadni.
* Biztonsági fejlesztések.
* Google Play és SmartAd követési eltávolítva.

