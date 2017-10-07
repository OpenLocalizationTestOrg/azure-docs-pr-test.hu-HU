---
title: "jelentéskészítési lehetőségek az Azure Mobile Engagement Android SDK aaaAdvanced"
description: "Ismerteti, hogyan toodo speciális az Azure Mobile Engagement Android SDK jelentéskészítési toocapture elemzés"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 5c8f4ea36c54715f4e09fd43c96132c15019a71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a>Speciális hibajelentési az Engagement Android rendszeren
> [!div class="op_single_selector"]
> * [Univerzális Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Ez a témakör az Android alkalmazás további jelentéskészítési forgatókönyvekhez. E beállítások toohello app hello létrehozott alkalmazhat [bevezetés](mobile-engagement-android-get-started.md) oktatóanyag.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

hello oktatóanyag elvégzésének szándékosan közvetlen és egyszerű, de nincs speciális lehetősége választható.

## <a name="modifying-your-activity-classes"></a>Módosítja a `Activity` osztályok
A hello [bevezető oktatóanyag](mobile-engagement-android-get-started.md), minden toodo kellett toomake volt a `*Activity` alosztályok öröklik a megfelelő hello `Engagement*Activity` osztályok. Például, ha az örökölt tevékenység kiterjesztett `ListActivity`, hogy könnyebben kiterjesztése `EngagementListActivity`.

> [!IMPORTANT]
> Használata esetén `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy semmilyen hívásban túl`requestWindowFeature(...);` túl a hello hívása előtt végrehajtott`super.onCreate(...);`, különben a összeomlási történik.
> 
> 

Ezeket az osztályokat az hello található `src` mappa, és a projektben másolja őket. hello osztályokat is az hello **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Másodlagos módszer: hívja `startActivity()` és `endActivity()` manuálisan
Ha nem, vagy nem toooverload a `Activity` osztályokat, akkor is inkább kezdő és záró a tevékenységek hello meghívásával `EngagementAgent`tartozó módszerek közvetlenül.

> [!IMPORTANT]
> Android SDK hello soha nem hívja a hello `endActivity()` metódust, akkor is, ha hello alkalmazás bezárása (Android, az alkalmazások soha nem Lezáródnak). Így *magas* toocall hello ajánlott `startActivity()` hello metódust `onResume` a visszahívási *minden* tevékenységek, és hello `endActivity()` hello metódus `onPause()` a visszahívási *összes* a tevékenységeket. Ez a hello csak úgy toobe meg arról, hogy munkamenetek nem szivárognak. Ha a munkamenet kiszivárgott, hello Engagement szolgáltatás soha nem bontja a kapcsolatot hello Engagement háttérrendszeréhez (mivel hello szolgáltatás továbbra is csatlakoztatva marad mindaddig, amíg függőben egy munkamenet).
> 
> 

Például:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Ebben a példában az hasonló toohello `EngagementActivity` osztály és annak változataihoz, amelynek forráskód megadott hello `src` mappát.

## <a name="using-applicationoncreate"></a>Application.onCreate() használatával
A kód helyez `Application.onCreate()` és más alkalmazásban visszahívások az alkalmazás folyamatok, többek között a hello Engagement szolgáltatás futtatásához. Nem kívánt hatásai, nem szükséges memória-kiosztásokat és hello bevonási folyamat, például lehet, vagy ismétlődő szórási fogadók vagy szolgáltatások.

Ha Ön felülbírálása `Application.onCreate()`, azt javasoljuk, hogy a következő kódrészletet a hello elején hello a `Application.onCreate()` függvény:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Mindent ugyanaz a hello `Application.onTerminate()`, `Application.onLowMemory()`, és `Application.onConfigurationChanged(...)`.

Emellett kibővítheti `EngagementApplication` helyett kiterjesztése `Application`: hello visszahívási `Application.onCreate()` hello folyamat ellenőrzése és hívások `Application.onApplicationProcessCreate()` csak ha hello aktuális folyamat nem hello egy üzemeltetési hello Engagement szolgáltatás, hello ugyanazok a szabályok vonatkoznak a hello más visszahívások.

## <a name="tags-in-hello-androidmanifestxml-file"></a>Címkék hello AndroidManifest.xml fájlban
Hello szolgáltatás címkében hello AndroidManifest.xml fájlban hello `android:label` attribútum lehetővé teszi a toochoose hello neve hello Engagement szolgáltatás tooend felhasználók telefont hello "Futó szolgáltatások" képernyőjén megjelenő. Javasoljuk, hogy ha az attribútum túl`"<Your application name>Service"` (például `"AcmeFunGameService"`).

Adja meg hello `android:process` attribútum biztosítja, hogy hello Engagement szolgáltatásának futtatásakor a saját folyamat (Engagement futtató ugyanezt a folyamatot, az alkalmazás lehetővé teszi a fő/felhasználói felület szálán potenciálisan kevésbé rugalmas hello).

## <a name="building-with-proguard"></a>A ProGuard felépítése
Ha ProGuard az alkalmazáscsomag, kell tookeep bizonyos osztályokat. A következő konfigurációs részlet hello használhatja:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
