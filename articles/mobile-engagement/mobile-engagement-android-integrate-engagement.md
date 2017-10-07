---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4f79936ea0fa6102023dec2b4682032a4a81fa9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-android"></a>Hogyan tooIntegrate Engagement Android rendszeren
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Engagement elemzés és figyelési funkciók az Android-alkalmazás.

> [!IMPORTANT]
> A minimális Android SDK API-szintet kell lennie, 10 vagy újabb rendszer (Android 2.3.3 vagy újabb).
> 
> 

a lépéseket követve hello a naplók elegendő tooactivates hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok. hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális a Mobile Engagement az Android API szerinti címkézését](mobile-engagement-android-use-engagement-api.md) óta ezek a statisztikákat a függő alkalmazást.

## <a name="embed-hello-engagement-sdk-and-service-into-your-android-project"></a>Hello Engagement SDK-t és a szolgáltatás beágyazása az Android-projekt
Letöltési hello Android SDK [Itt](https://aka.ms/vq9mfn) beolvasása `mobile-engagement-VERSION.jar` és hello is elhelyezheti `libs` az Android-projekt mappájából (Ha még nem létezik, a hello függvénytárak mappa létrehozása).

> [!IMPORTANT]
> Ha ProGuard az alkalmazáscsomag, kell tookeep bizonyos osztályokat. A következő konfigurációs részlet hello használhatja:
> 
> -nyilvános osztály tartsa * android.os.IInterface bővíti-osztály com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {megtartása
> 
> <methods>; }
> 
> 

A bevonási kapcsolati karakterlánc megadásához hívó hello metódus hello indítója tevékenység a következő:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

hello kapcsolati karakterlánc az alkalmazás Azure-portálon jelenik meg.

* Ha hiányzik, adja hozzá az alábbi Android engedélyek hello (előtt hello `<application>` címke):
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* Adja hozzá a következő szakasz hello (közötti hello `<application>` és `</application>` címkék):
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* Változás `<Your application name>` által az alkalmazás hello nevét.

> [!TIP]
> Hello `android:label` attribútum lehetővé teszi a toochoose hello neve hello Engagement szolgáltatás toohello végfelhasználók hello "Futó szolgáltatások" képernyőjén telefont fog megjelenni. Az ajánlott tooset ezt az attribútumot túl`"<Your application name>Service"` (pl. `"AcmeFunGameService"`).
> 
> 

Adja meg hello `android:process` attribútum biztosítja, hogy hello Engagement szolgáltatás folyamatban (Engagement futtató ugyanezt a folyamatot, az alkalmazás biztosítják a fő/felhasználói felület szálán potenciálisan kevésbé rugalmas hello) fog futni.

> [!NOTE]
> A kód helyez `Application.onCreate()` és más alkalmazás visszahívások fog futni az alkalmazás folyamatok, többek között a hello Engagement szolgáltatáshoz. (Például nem szükséges memória-kiosztásokat és szálak hello bevonási folyamat, ismétlődő szórási fogadók vagy szolgáltatások) nemkívánatos hatásai lehetnek.
> 
> 

Ha akkor bírálja felül `Application.onCreate()`, a következő kódrészletet a hello elején ajánlott tooadd hello a `Application.onCreate()` függvény:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Mindent ugyanaz a hello `Application.onTerminate()`, `Application.onLowMemory()` és `Application.onConfigurationChanged(...)`.

Emellett kibővítheti `EngagementApplication` helyett kiterjesztése `Application`: hello visszahívási `Application.onCreate()` hello folyamat ellenőrzése és hívások `Application.onApplicationProcessCreate()` csak ha hello aktuális folyamat nem hello egy üzemeltetési hello Engagement szolgáltatás, hello ugyanazok a szabályok vonatkoznak a hello más visszahívások.

## <a name="basic-reporting"></a>Alapvető jelentéskészítési
### <a name="recommended-method-overload-your-activity-classes"></a>Ajánlott módszer: túlterhelés a `Activity` osztályok
Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, csak rendelkezik az összes toomake a `*Activity` alosztályokat örökölhet hello megfelelő `Engagement*Activity` osztályok (pl. Ha kiterjeszti az örökölt tevékenység `ListActivity`, ellenőrizze az általa bővített `EngagementListActivity`).

**Nélkül Engagement:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Az Engagement:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> Használata esetén `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy semmilyen hívásban túl`requestWindowFeature(...);` túl a hello hívása előtt végrehajtott`super.onCreate(...);`, ellenkező esetben a összeomlási történik.
> 
> 

Ezeket az osztályokat az hello található `src` mappa, és a projektben másolja őket. hello osztályokat is az hello **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Másodlagos módszer: hívja `startActivity()` és `endActivity()` manuálisan
Ha nem, vagy nem toooverload a `Activity` osztályok, ehelyett elindításához és a tevékenységek befejezése meghívásával `EngagementAgent`tartozó módszerek közvetlenül.

> [!IMPORTANT]
> Android SDK hello soha nem hívja a hello `endActivity()` metódust, akkor is, ha hello alkalmazás bezárása (Android, az alkalmazások valójában soha nem Lezáródnak). Így *magas* toocall hello ajánlott `startActivity()` hello metódust `onResume` a visszahívási *minden* tevékenységek, és hello `endActivity()` hello metódus `onPause()` a visszahívási *összes* a tevékenységeket. Ez a hello csak úgy toobe meg arról, hogy a munkamenetek szivárgását nem lehet. Egy munkamenet kiszivárgott, ha hello Engagement szolgáltatás soha nem leválasztja a hello Engagement háttérrendszerből (mivel hello szolgáltatás továbbra is csatlakoztatva marad mindaddig, amíg függőben egy munkamenet).
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

A Példa nagyon hasonló toohello `EngagementActivity` osztály és annak változataihoz, amelynek forráskód megadott hello `src` mappát.

## <a name="test"></a>Tesztelés
Most ellenőrizze, hogy az integráció a mobilalkalmazás emulátor vagy eszköz a és ellenőrzése, hogy a munkamenet hello figyelő lapon történő regisztrálást.

hello következő szakaszokban opcionálisak.

## <a name="location-reporting"></a>Helyi jelentéskészítés
Ha azt szeretné, hogy a helyek toobe jelentett, szüksége tooadd konfigurációs néhány sornyi (közötti hello `<application>` és `</application>` címkék).

### <a name="lazy-area-location-reporting"></a>Helymeghatározást
Helymeghatározást lehetővé teszi, hogy tooreport hello ország, valamint régió és Helység társított toodevices. A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján). hello eszköz terület munkamenetenként legfeljebb egyszer jelenti. hello GPS a rendszer nem használja, és ezáltal az ilyen típusú hely jelentés nagyon kevés (nem toosay nem) hello akkumulátor hatása.

Jelentett területek a következők felhasználók, munkamenetek, kapcsolódó események és hibák földrajzi statisztikája használt toocompute. Ezek használhatók a Reach-kampányokat feltételként.

tooenable Lusta terület helye reporting hello konfigurációs korábban az itt ismertetett eljárás segítségével is elvégezhető:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Tovább használhatja vagy ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazásban.

### <a name="real-time-location-reporting"></a>Valós idejű hely jelentéskészítési
Valós idejű hely jelentéskészítési lehetővé teszi, hogy tooreport hello szélességi és hosszúsági társított toodevices. Alapértelmezés szerint a hely jelentéskészítési csak használja a hálózati helyek (cella azonosító vagy Wi-Fi alapján), és hello reporting csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben).

Valós idejű helyek *nem* toocompute statisztika használt. A csak célja a geokerítések valós idejű tooallow hello használata \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.

tooenable valós idejű helye reporting hello konfigurációs korábban az itt ismertetett eljárás segítségével is elvégezhető:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Tovább használhatja vagy ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazásban.

#### <a name="gps-based-reporting"></a>GPS-alapú jelentés
Alapértelmezés szerint valós idejű helyét jelentő csak funkció alapú hálózati helyek. tooenable hello GPS alapuló (amelyek pontos sokkal) helyek, hello konfigurációs objektum használja:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Háttér-jelentés
Alapértelmezés szerint valós idejű hely jelentéskészítési az csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben). a háttérben, használjon hello konfigurációs objektum is reporting tooenable hello:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Hello alkalmazás fut a háttérben, amikor csak alapú hálózati helyek küld jelentést, még akkor is, ha engedélyezte a hello GPS.
> 
> 

hello háttér hely jelentés le lesz állítva, ha hello felhasználó az eszköz újraindul, az automatikus újraindítás rendszerindítás toomake is hozzáadhat:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Ha hiányoznak a következő engedély tooadd hello is szüksége lesz:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M-engedélyek
Az Android M-től kezdődően egyes engedélyek futásidőben felügyelt, és felhasználói jóváhagyás szükséges.

hello futásidejű engedélyek ki lesz kapcsolva a új alkalmazások alapértelmezés szerint ha Android API-szintet 23 célozhat meg. Ellenkező esetben azt lesz kapcsolva alapértelmezés szerint.

hello felhasználói is engedélyezi/letiltja ezeket az engedélyeket hello eszköz beállítások menüjében. Engedélyek rendszer menüből kikapcsolása használhatatlanná teszi a háttérfolyamatot hello alkalmazás, ez a rendszer viselkedését, és nincs hatással van a háttérben képességét tooreceive leküldéses.

A Mobile Engagement hello környezetében futásidőben jóváhagyást igénylő hello engedélyek a következők:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* `WRITE_EXTERNAL_STORAGE`(csak akkor, ha ez egy 23 Android API-szintet célzó)

hello külső tárhelyen csak Reach nagy vonalakban tekinti a szolgáltatás használatos. Ha talál kérni a felhasználókat, az engedély toobe zavaró, eltávolíthatja azt ha azt csak a Mobile Engagement, de a nagy vonalakban tekinti funkció letiltása hello költségekkel.

Hello hely szolgáltatások egy szabványos rendszer párbeszédpanelen engedélyek toouser igényeljen. Hello felhasználói jóvá, ha szüksége van-e tootell ``EngagementAgent`` tootake (egyéb hello módosítás felül lesz feldolgozott hello következő hello felhasználó elindítja hello alkalmazása) valós idejű figyelembe módosítani.

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
         * Request location permission, but this won't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want tookeep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a>Speciális jelentéskészítés
Nem kötelező, ha tooreport adott eseményeket, hibákat és feladatok, kell toouse hello Engagement API keresztül hello hello módszerek `EngagementAgent` osztály. Ez az osztály egy objektumának lehet hívó hello által retreived `EngagementAgent.getInstance()` statikus metódus.

hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók és részleteit a hello hogyan tooUse az Engagement API-t az Android (valamint hello hello műszaki dokumentációját az `EngagementAgent` osztály).

## <a name="advanced-configuration-in-androidmanifestxml"></a>Speciális konfigurációs (az AndroidManifest.xml)
### <a name="wake-locks"></a>Ébresztési zárolások feloldása
Ha azt szeretné, hogy statisztika küldött Wi-Fi használata esetén valós időben, vagy le van tiltva üdvözlő képernyőt toobe, adja hozzá a következő opcionális engedély hello:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Összeomlási jelentést
Ha azt szeretné, hogy toodisable összeomlási jelentések, adja hozzá ezt a (közötti hello `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Küszöbérték-kapacitásnövelés
Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben. Ha az alkalmazás naplók nagyon gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (Ez a lehetőség hello "kapacitásnövelés mód"). toodo Igen, adja hozzá ezt a (közötti hello `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

hello kapacitásnövelés mód némileg növeli hello akkumulátor élettartama hello Engagement figyelő azonban hatással van: az összes munkamenetek és feladatok is lesznek kerekített toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható). Az ajánlott toouse egy mint 30000 (30s) kapacitásnövelés küszöbértéket.

### <a name="session-timeout"></a>Munkamenet időkorlátja
Alapértelmezés szerint a munkamenet befejeződött a 10 egység az utolsó tevékenység (amely általában hello lenyomásával Home esetén, vagy a kulcs, biztonsági beállítás hello üresjárati telefonos vagy egy másik alkalmazásban átugorja) hello vége után. Ez a tooavoid munkamenet valószínűségét minden alkalommal hello felhasználói lépjen ki, és térjen vissza a toohello alkalmazás nagyon gyorsan (amely a hiba akkor fordulhat elő ő átvételéhez egy kép, amikor ellenőrizheti egy értesítés, stb.). Érdemes lehet toomodify ezt a paramétert. toodo Igen, adja hozzá ezt a (közötti hello `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Naplózási jelentések letiltásához
### <a name="using-a-method-call"></a>Metódushívások használatával
Ha azt szeretné, hogy a naplók küldése Engagement toostop, hívása:

            EngagementAgent.getInstance(context).setEnabled(false);

Ez a hívás az állandó: egy megosztott beállításokat tartalmazó fájlt használja.

Ha Engagement aktív Ez a függvény hívásakor, 1 perces hello szolgáltatás toostop is eltarthat. Azonban ez nem indítsa el a hello szolgáltatást minden hello hello alkalmazás következő indításakor.

Engedélyezheti újra reporting működik az azonos hello meghívásával napló `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integráció saját`PreferenceActivity`
Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.

A beállítások fájlt (hello kívánt mód) Engagement toouse konfigurálhatja a hello `AndroidManifest.xml` fájlt `application meta-data`:

* Hello `engagement:agent:settings:name` kulcsa használt toodefine hello hello közös beállításokat fájl nevét.
* Hello `engagement:agent:settings:mode` kulcsa használt toodefine hello mód hello közös beállításokat fájl, hello használja ugyanazt a módot és a a `PreferenceActivity`. hello módban kell átadni egy számot: használatakor a konstans jelzők kombinációja a kódban, ellenőrizze a hello teljes értékét.

Bevonási mindig használjon hello `engagement:key` belül hello beállításokat tartalmazó fájlt ezzel a beállítással kezelésére szolgáló logikai kulcs.

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

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
