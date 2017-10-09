---
title: "Azure Mobile Engagement Android SDK aaaAdvanced konfigurációja"
description: "Speciális konfigurációs beállítások, beleértve az Azure Mobile Engagement Android SDK-val Android Manifest hello hello ismerteti"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 757abf362021fd018f444cae6305524623e77062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Az Azure Mobile Engagement Android SDK speciális konfiguráció
> [!div class="op_single_selector"]
> * [Univerzális Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
>
>

Ez az eljárás ismerteti, hogyan tooconfigure Azure Mobile Engagement Android-alkalmazások különböző konfigurációs beállításait.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Engedélykövetelmények
Néhány beállításhoz konkrét engedélyeket, amelyek az itt felsorolt hivatkozást, és beágyazott hello adott funkciójában szükségesek. Ezen engedélyek toohello AndroidManifest.xml a projekt hozzáadása előtt vagy után hello `<application>` címke.

hello engedély kód toolook hello következő, ahol hello megfelelő engedélyekkel az alábbi hello táblából kitöltése szükséges.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Engedély | Használata esetén |
| --- | --- |
| INTERNET |Kötelező. Alapszintű jelentéskészítéshez |
| ACCESS_NETWORK_STATE |Kötelező. Alapszintű jelentéskészítéshez |
| RECEIVE_BOOT_COMPLETED |Kötelező. tooshow mentése hello értesítések center eszköz újraindítását követően. |
| WAKE_LOCK |Ajánlott. Lehetővé teszi, hogy Wi-Fi használata esetén, vagy ha a képernyőn a rendszer nem végez adatgyűjtést |
| MOZOG |Választható. Lehetővé teszi, hogy vibráció értesítések fogadásakor. |
| DOWNLOAD_WITHOUT_NOTIFICATION |Választható. Lehetővé teszi, hogy Android nagy vonalakban tekinti értesítés |
| WRITE_EXTERNAL_STORAGE |Választható. Lehetővé teszi, hogy Android nagy vonalakban tekinti értesítés |
| ACCESS_COARSE_LOCATION |Választható. Lehetővé teszi a valós idejű hely jelentéskészítési |
| ACCESS_FINE_LOCATION |Választható. Lehetővé teszi, hogy helyét GPS-alapú jelentés |

Kezdődően az Android M [futás közben bizonyos engedélyek felügyelt](mobile-engagement-android-location-reporting.md#android-m-permissions).

Ha már használja a ``ACCESS_FINE_LOCATION``, akkor nem szükséges tooalso használja ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android konfigurációs beállítások
### <a name="crash-report"></a>Összeomlási jelentést
toodisable összeomlási jelentések, adja hozzá ezt a kódot közötti hello `<application>` és `</application>` címkék:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Küszöbérték-kapacitásnövelés
Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben. A jelentés alkalmazásnaplók gyakran változnak, esetén jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (más néven "nagy sebességű átvitel"). toodo Igen, adja hozzá ezt a kódot hello közötti `<application>` és `</application>` címkék:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Kapacitásnövelés mód némileg növeli a hello eszközakkumulátor élettartamának, azonban hatással van hello Engagement figyelő: minden munkamenetek és feladatok időtartama vannak kerekítve toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható). A kapacitásnövelés küszöbértéknek legfeljebb 30000 (30s) kell lennie.

### <a name="session-timeout"></a>Munkamenet időkorlátja
 Egy tevékenység által lenyomásával hello fejezheti **Home** vagy **vissza** kulcs beállítása hello üresjárati telefonos vagy egy másik alkalmazásban átugorja. Alapértelmezés szerint a munkamenet véget ér 10 másodperc után utolsó tevékenysége hello végét. Ezzel elkerülhető, hogy a munkamenet valószínűségét minden alkalommal hello felhasználó kilép, és adja vissza toohello alkalmazás gyorsan, amely is hiba akkor fordulhat elő, amikor a felhasználó hello szerzi be a képen egy értesítés, stb. Érdemes lehet toomodify ezt a paramétert. toodo Igen, adja hozzá ezt a kódot hello közötti `<application>` és `</application>` címkék:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Naplózási jelentések letiltásához
### <a name="using-a-method-call"></a>Metódushívások használatával
Ha azt szeretné, hogy a naplók küldése Engagement toostop, hívása:

    EngagementAgent.getInstance(context).setEnabled(false);

Ez a hívás az állandó: egy megosztott beállításokat tartalmazó fájlt használja.

Ha Engagement aktív Ez a függvény hívásakor, hello szolgáltatás toostop egy percig is eltarthat. Azonban ez nem indítsa el a hello szolgáltatást minden hello hello alkalmazás következő indításakor.

Engedélyezheti újra reporting működik az azonos hello meghívásával napló `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integráció saját`PreferenceActivity`
Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.

A beállítások fájlt (hello kívánt mód) Engagement toouse konfigurálhatja a hello `AndroidManifest.xml` fájlt `application meta-data`:

* Hello `engagement:agent:settings:name` kulcsa használt toodefine hello hello közös beállításokat fájl nevét.
* Hello `engagement:agent:settings:mode` kulcs hello közös beállításokat fájl használt toodefine hello módban. Használjon hello azonos és a mód a `PreferenceActivity`. hello módban kell átadni egy számot: használatakor a konstans jelzők kombinációja a kódban, ellenőrizze a hello teljes értékét.

Bevonási mindig használja hello `engagement:key` belül hello beállításokat tartalmazó fájlt ezzel a beállítással kezelésére szolgáló logikai kulcs.

Példa következő hello `AndroidManifest.xml` jeleníti meg az alapértelmezett értékek hello:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

Ezután hozzáadhatja a `CheckBoxPreference` a preferencia elrendezésben például hello egyet a következő:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
