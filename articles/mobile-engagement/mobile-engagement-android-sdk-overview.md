---
title: "Azure Mobile Engagement SDK-integráció aaaAndroid"
description: "Ismerteti, hogyan toointegrate Azure Mobile Engagement SDK az Android-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0c63bfaf673abbda7ea498390f8282c43e2fb8df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Az Azure Mobile Engagement Android SDK-integráció
> [!div class="op_single_selector"]
> * [Univerzális Windows](mobile-engagement-windows-store-sdk-overview.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
> * [iOS](mobile-engagement-ios-sdk-overview.md)
> * [Android](mobile-engagement-android-sdk-overview.md)
> 
> 

Ez a dokumentum ismerteti az összes hello integrációs és konfigurációs lehetőségek érhető el az Azure Mobile Engagement Android SDK-t.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>A speciális funkciók
### <a name="reporting-features"></a>Jelentési szolgáltatások
Ezeket a szolgáltatásokat is hozzáadhat:

1. [Speciális jelentéskészítési beállítások](mobile-engagement-android-advanced-reporting.md)
2. [Hely jelentéskészítési lehetőségek](mobile-engagement-android-location-reporting.md)
3. [Speciális konfigurációs beállítások](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Értesítések:
[Hogyan toointegrate Reach (értesítések) a Android-alkalmazásokban](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [hogyan tooIntegrate a GCM használata a Mobile Engagement](mobile-engagement-android-gcm-integrate.md)
2. Amazon eszköz Messaging (ADM): [hogyan tooIntegrate ADM a Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Címke terv végrehajtására:
[Hogyan toouse hello speciális a Mobile Engagement az Android-alkalmazás szerinti címkézését API](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Javítsa ki, amelyek ritkán fordulhat elő, amikor a program meghívja crash `EngagementAgentUtils.isInDedicatedEngagementProcess`, amely is használják hello `EngagementApplication` osztály.

### <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8 támogatása (korábbi verzióiban hello SDK Android 8 nem fog működni).
* Nincs több függőség támogatási függvénytára.
* Távolítsa el `EngagementFragmentActivity` osztály.
* Esedékes túl[háttér végrehajtási korlátozások](https://developer.android.com/preview/features/background.html) Android 8, a háttér-naplók előfordulhat, hogy később, amíg hello felhasználó hello eszköz kommunikál, ez hatással lesz a leküldéses kampány **kézbesített** és **Rendszerértesítő jelenik meg** Ha hello eszköz alvó volt folyamatban késleltetett statisztika (hello értesítési továbbra is megjelenik, fog gyűrű és mozog hibák nélkül valós időben).
* Esedékes túl[háttér hely korlátok](https://developer.android.com/preview/features/background-location-limits.html), háttérben hello valós idejű hely nem fog frissülni gyakran Android 8.

Minden verzióiért lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Frissítési eljárások
Ha Ön már rendelkezik integrálva egy régebbi verzióját az SDK az alkalmazás, tekintse meg [frissítése eljárások](mobile-engagement-android-upgrade-procedure.md).

