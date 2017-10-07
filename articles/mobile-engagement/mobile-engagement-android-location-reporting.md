---
title: "aaaLocation jelentéskészítést az Azure Mobile Engagement Android SDK"
description: "Ismerteti, hogyan jelentéskészítést az Azure Mobile Engagement Android SDK tooconfigure helye"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: c2cb097df2a77bee2d56ffe9509dc116548db408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Az Azure Mobile Engagement Android SDK Reporting helye
> [!div class="op_single_selector"]
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Ez a témakör ismerteti, hogyan jelentéskészítést az Android-alkalmazás az toodo helyét.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Helyi jelentéskészítés
Ha azt szeretné, hogy a helyek toobe jelentett, szüksége tooadd konfigurációs néhány sornyi (közötti hello `<application>` és `</application>` címkék).

### <a name="lazy-area-location-reporting"></a>Helymeghatározást
Helymeghatározást lehetővé teszi, hogy jelentéskészítési hello ország, régió és Helység szerint társított eszközök. A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján). hello eszköz terület munkamenetenként legfeljebb egyszer jelenti. hello GPS soha nem szolgál, és így ez a típus helye jelentés alacsony hatással van hello akkumulátor.

Jelentett területek a következők felhasználók munkamenetek, események és hibák földrajzi statisztikája használt toocompute. Ezek használhatók a Reach-kampányokat feltételként.

Engedélyezi a korábban az itt ismertetett eljárás hello konfiguráció reporting Lusta terület helye:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Meg kell a helyre vonatkozó engedély toospecify is. Ezt a kódot használja ``COARSE`` engedély:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ha az alkalmazás számára szükséges, akkor használhatja ``ACCESS_FINE_LOCATION`` helyette.

### <a name="real-time-location-reporting"></a>Valós idejű hely jelentéskészítési
Valós idejű hely jelentéskészítési lehetővé teszi, hogy jelentéskészítési hello szélességi és hosszúsági társított eszközök. Alapértelmezés szerint a hely jelentéskészítési típusú hálózati helyek, cella azonosító vagy Wi-Fi alapú csak használja. hello reporting csak aktív alkalmazás futásakor, hello előtérben (például egy munkamenetben).

Valós idejű helyek *nem* toocompute statisztika használt. A csak célja a geokerítések valós idejű tooallow hello használata \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.

tooenable valós idejű hely jelentéskészítési, adja hozzá a sort a kód toowhere hello Engagement kapcsolati karakterlánc beállítása hello indítója tevékenység. hello eredménye a következőhöz hasonló, hello:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need toospecify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS-alapú jelentés
Alapértelmezés szerint a valós idejű hely jelentéskészítési csak használja hálózati helyek. tooenable hello használata GPS-alapú helyeket, amelyek jobban pontos, hello konfigurációs objektum használja:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Háttér-jelentés
Alapértelmezés szerint a valós idejű hely jelentéskészítési csak aktív alkalmazás futásakor, hello előtérben (például egy munkamenetben). a háttérben, is reporting tooenable hello a konfigurációs objektum használja:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Hello alkalmazás fut a háttérben, amikor csak a hálózati helyek küld jelentést, még akkor is, ha engedélyezte a hello GPS.
> 
> 

Ha hello felhasználó az eszköz újraindul, hello háttér hely jelentés le van állítva. toomake automatikusan újraindítja a rendszerindítás, adja hozzá ezt a kódot.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M-engedélyek
Az Android M-től kezdődően egyes engedélyek futásidőben felügyelt, és felhasználói jóváhagyás szükséges.

Ha a cél Android API-szintet 23, hello futásidejű engedélyek ki vannak kapcsolva alapértelmezés szerint új alkalmazástelepítések. Ellenkező esetben ezek alapértelmezés szerint vannak kapcsolva.

Akkor is engedélyezi/letiltja ezeket az engedélyeket hello eszköz beállítások menüjében. Hello háttérfolyamatot hello alkalmazás, amely a rendszer viselkedését, és nincs hatással van a háttérben képességét tooreceive leküldéses hello rendszer menü engedélyek kikapcsolása használhatatlanná teszi.

A Mobile Engagement-hely jelentési hello környezetében futásidőben jóváhagyást igénylő hello engedélyek a következők:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

Egy szabványos rendszer párbeszédpanelen hello felhasználói engedélyek kéréséhez. Ha hello felhasználói jóvá, mondja el ``EngagementAgent`` tootake, amely valós idejű figyelembe változik. Ellenkező esetben a hello módosítása feldolgozott hello következő hello felhasználó elindítja hello alkalmazása.

Íme egy kódot minta toouse toorequest Alkalmazásengedélyek és előre hello eredmény tevékenységet, ha pozitív túl``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
