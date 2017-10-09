---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: df5c82812fe0a242eaa5df8c906030237215b7eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a>Frissítési eljárások
Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban. Például ha telepít át 1.4.0 hello kövesse toofirst rendelkezik too1.6.0 "1.4.0 a too1.5.0" eljárást akkor hello "1.5.0 a too1.6.0" eljárás.

Bármilyen hello verzióra frissít, hogy tooreplace hello `mobile-engagement-VERSION.jar` a hello újat.

## <a name="from-420-too421"></a>A 4.2.0 too4.2.1
Ez a lépés ténylegesen végezhető hello SDK bármely verzióját, hogy a biztonság fokozása Reach tevékenységek integrálásakor.

Most adja hozzá `exported="false"` tooall Reach tevékenységeket.

Reach tevékenységek most példához hasonló a `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

## <a name="from-400-too410"></a>A 4.0.0 too4.1.0
hello SDK most leíró új engedély modell Android m-alapú.

Ha hely használatát, vagy olvassa el nagy vonalakban tekinti értesítések [ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Ezenkívül toohello új engedély modell mostantól támogatjuk hely szolgáltatások konfigurálásának futásidőben.
Továbbra is kompatibilis helyhez hello jegyzék paraméterekkel dolgozunk, de most már elavult. remove hello következő szakaszában toouse futásidejű konfigurálását, a ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

olvassa el és [a frissíti, az eljárás](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse futásidejű konfigurálását helyette.

## <a name="from-300-too400"></a>A 3.0.0 too4.0.0
### <a name="native-push"></a>Natív leküldéssel
Natív leküldéses (GCM/ADM) most is szolgál az alkalmazáson belüli értesítések, konfigurálnia kell a hello hitelesítő adatokat a natív leküldéses kampány bármilyen típusú.

Ha nem már kövesse [ezzel az eljárással](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Reach integrációs módosítottuk ``AndroidManifest.xml``.

Cserélje le ezt:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

–

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Már létezik valószínűleg betöltése képernyő kattintva (a szöveges vagy webes tartalom) bejelentés vagy szavazásként.
Rendelkezik tooadd ez ezeket a 4.0.0 kampányok toowork:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Erőforrások
Új hello beágyazása `res/layout/engagement_loading.xml` fájlt a projektben.

## <a name="from-240-too300"></a>A 2.4.0 too3.0.0
hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba. Ha egy korábbi verziójáról telepít, tekintse át először hello Capptain webhely toomigrate too2.4.0, és ezután alkalmazza az eljárást követő hello.

> [!IMPORTANT]
> Capptain és a Mobile Engagement nem hello azonos szolgáltatások, és hello az alábbi eljárás csak Highlight hogyan toomigrate hello ügyfélalkalmazás. Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement-kiszolgálókról.
> 
> 

### <a name="jar-file"></a>JAR-fájlra
Cserélje le `capptain.jar` által `mobile-engagement-VERSION.jar` a a `libs` mappát.

### <a name="resource-files"></a>Erőforrás-fájlok
A Microsoft által biztosított minden erőforrásfájl (által meghatározott `capptain_`) toobe helyettesítése hello újakat (előtagként `engagement_`).

Ha testreszabta azokat a fájlokat, rendelkezik-e toore-hello új fájlok, a testreszabást alkalmazni **hello erőforrás-fájlokban szereplő összes hello azonosítónak is át lett nevezve**.

### <a name="application-id"></a>Alkalmazásazonosító
Most Engagement használja a kapcsolati karakterlánc tooconfigure hello SDK azonosítók például hello alkalmazás azonosítója.

Toouse rendelkezik `EngagementAgent.init` módszer a indítója tevékenységi ehhez hasonló:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

hello kapcsolati karakterlánc az alkalmazás Azure-portálon jelenik meg.

Távolítsa el az összes hívás túl`CapptainAgent.configure` , `EngagementAgent.init` Ez a módszer váltja fel.

Hello `appId` már nem konfigurálható segítségével `AndroidManifest.xml`.

Távolítsa el az ebben a szakaszban a `AndroidManifest.xml` Ha azt:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API
Minden hívás tooany az SDK Java-osztály rendelkezik toobe átnevezett; például `CapptainAgent.getInstance(this)` kell átnevezni `EngagementAgent.getInstance(this)`, `extends CapptainActivity` kell átnevezni `extends EngagementActivity` stb...

Ha alapértelmezett ügynök preferencia fájlok lettek integrálva, hello alapértelmezett neve mostantól `engagement.agent` és hello kulcs `engagement:agent`.

Webes közlemények létrehozásakor hello Javascript kötő mostantól `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Hiba történt a sok módosítást, hello szolgáltatást nem többé megosztva, és nagy mennyiségű fogadók nem exportálható többé.

hello szolgáltatás deklaráció mostantól egyszerűbb; Távolítsa el a hello leképezési szűrő és az összes metaadatot azon belül, és adja hozzá `exportable=false`.

Plusz átnevezett toouse engagement.

Ez most néz ki:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Ha azt szeretné, hogy tooenable vizsgálati naplókat, hello metaadatok most már toohello alkalmazás címke áthelyezni, és át lett nevezve:

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

Minden egyéb metaadatot csak át lett nevezve, hello teljes listája (természetesen átnevezése csak hello néhányat a meglévők közül használhat):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play és SmartAd követési el lett távolítva az SDK csak rendelkezik tooremove ez helyettesítő nélkül:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

hello Reach tevékenységek most deklarált ehhez hasonló:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

Ha egyéni Reach tevékenységeket, vagy van kell csak toochange hello leképezési műveletek toomatch `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` vagy `com.microsoft.azure.engagement.reach.intent.action.POLL`.

hello szórási fogadók átnevezték, valamint mostantól felvehet `exported=false`. Hello hello fogadók hello új meghatározásával, (természetesen átnevezése csak hello néhányat a meglévők közül használhat) teljes listája itt található:

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Követés fogadó eltávolították, így ez a szakasz kell tooremove:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Vegye figyelembe, hogy hello megvalósítását hello deklarációjában szórási fogadó **EngagementMessageReceiver** hello változásai `AndroidManifest.xml`. Ez mert hello API toosend és eltávolítás tetszőleges XMPP a tetszőleges XMPP entitásokból üzenetek és API toosend hello és eszközök közötti üzeneteket fogadni, hogy eltávolították. Ebből kifolyólag rendelkezik is a visszahívásokat a következő toodelete hello a **EngagementMessageReceiver** végrehajtására:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

és

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Törölje a semmilyen hívásban **EngagementAgent** számára:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

és

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard
Proguard konfigurációs is negatív hatással lehet rebranding, szabályok most hasznosítani például hello:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

