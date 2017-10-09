---
title: aaaHow tooUse hello Engagement API Android rendszeren
description: "Legújabb Android SDK - hogyan tooUse hello Engagement API Android rendszeren"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 09b62659-82ae-4a55-8784-fca0b6b22eaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: e0b2d484616c0c7874e77c5283d94c3063949ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-android"></a>Hogyan tooUse hello Engagement API Android rendszeren
Ez a dokumentum egy bővítmény toohello dokumentum [speciális jelentéskészítési lehetőségek a Mobile Engagement Android SDK](mobile-engagement-android-advanced-reporting.md). A mélység adatait hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái biztosít.

Vegye figyelembe, hogy ha csak szeretné Engagement tooreport munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás, majd hello legegyszerűbb módja a rendszer minden toomake a `Activity` alosztályokat örökölhet hello megfelelő `EngagementActivity` osztály.

Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementActivity` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.

hello Engagement API által biztosított hello `EngagementAgent` osztály. Ez az osztály példánya lekérhetők hívó hello `EngagementAgent.getInstance(Context)` statikus metódus (vegye figyelembe, hogy hello `EngagementAgent` objektumot adott vissza az Egypéldányos).

## <a name="engagement-concepts"></a>Engagement – fogalmak
hello következő részekből pontosítsa közös hello [Mobile Engagement fogalmait](mobile-engagement-concepts.md), hello Android platformhoz.

### <a name="session-and-activity"></a>`Session` és `Activity`
Ha hello felhasználó több mint néhány másodpercen belül két között tétlen marad *tevékenységek*, majd a sorozatát *tevékenységek* van szétosztva két különböző *munkamenetek*. Ezek néhány másodpercig hello "munkamenet időkorlátja" nevezzük.

Egy *tevékenység* általában egy képernyő hello alkalmazás, amely toosay hello társított *tevékenység* kezdődik, amikor hello képernyő jelenik meg, és leállítja az üdvözlő képernyőt bezárásakor: Ez a hello eset, amikor hello Engagement SDK integrálva van a hello segítségével `EngagementActivity` osztályok.

De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával. Így toosplit egy adott képernyő több sub részek tooget kapcsolatos további részletekért hello használata ezen a képernyőn (például milyen gyakran tooknown és mennyi ideig párbeszédpanelek használt ezen a képernyőn belül).

## <a name="reporting-activities"></a>Jelentéskészítési tevékenység
> [!IMPORTANT]
> Nem kell tooreport tevékenységek, például a hello használata ebben a szakaszban leírt `EngagementActivity` osztály és annak változataihoz, ahogy hello hogyan tooIntegrate Engagement Android dokumentum.
> 
> 

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing hello current activity is required for Reach toodisplay in-app notifications, passing null will postpone such announcements and polls.

Toocall kell `startActivity()` minden alkalommal hello felhasználói tevékenység változik. hello első hívás toothis függvény új felhasználói munkamenet indítása.

Ez a funkció csak az egyes tevékenységek a legjobb hely toocall hello `onResume` visszahívás.

### <a name="user-ends-his-current-activity"></a>Felhasználói karakterlánccal végződik-e aktuális tevékenysége
            EngagementAgent.getInstance(this).endActivity();

Toocall kell `endActivity()` legalább egyszer amikor hello felhasználó befejezi az utolsó tevékenység. Ebben értesíti az Engagement SDK-t, hogy a hello felhasználó jelenleg inaktív, és, hogy a felhasználói munkamenet hello toobe kell lezárt egyszer hello munkamenet időkorlátja lejár hello (ha meghívja a `startActivity()` előtt hello munkamenet időkorlátja lejár, a hello munkamenet egyszerűen folytatva).

Ez a funkció csak az egyes tevékenységek a legjobb hely toocall hello `onPause` visszahívás.

## <a name="reporting-events"></a>Jelentési eseményeket
### <a name="session-events"></a>Munkamenet-események
Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.

**További adatok nélkül. példa:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Példa a további adatokat:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Önálló események
Ellenkező toosession események, a kívül egy munkamenet környezetében hello önálló események is előfordulhatnak.

**Példa**

Tegyük fel, hogy a szórásos receiver kiváltásakor bekövetkező tooreport események:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a>Hibát jelentett
### <a name="session-errors"></a>Munkamenet-hibák
Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.

**Példa**

            /** hello user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* hello user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Önálló hibák
Ellenkező toosession hibák önálló hibák adódhatnak kívül hello egy munkamenet környezetében.

**Példa**

hello következő példa bemutatja, hogyan tooreport hiba történt, amikor hello memória válik alacsony hello telefonján, az alkalmazás folyamat közben fut-e.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a>Feladatok jelentése
### <a name="example"></a>Példa
Tegyük fel, hogy a bejelentkezési folyamat tooreport hello időtartama:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>A feladat során hibák jelentése
Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.

**Példa**

Tegyük fel, hogy azt szeretné, hogy tooreport során, akkor hiba bejelentkezési folyamat:

[...] nyilvános "void" signIn (helyi környezet,...) {

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try toosign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report hello error tooEngagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>A feladat során jelentési eseményeket
Események lehet feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.

**Példa**

Tegyük fel, közösségi hálózata, és egy feladat tooreport hello teljes ideje mely hello felhasználói pedig csatlakoztatott toohello server használjuk. hello felhasználói is maradhat háttérben akkor is, ha azt egy másik alkalmazás használja, vagy ha hello phone alvó állapotban van, így nincs munkamenet sincs.

hello felhasználói fogadhat üzeneteket az ismerősök, ez az esemény feladat.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

## <a name="extra-parameters"></a>További paraméterek
Lehet, hogy az adatokat tetszőleges csatolt tooevents, hibákat, tevékenységeket és feladatokat.

Ezek az adatok szervezhetők, Android tartozó csomagot osztály használ (ténylegesen, akkor működik, mint az Android leképezések kiegészítő paraméterekkel). Vegye figyelembe, hogy a csomag egyik Gyermekszoftver-tömb, vagy egy másik köteg példányokat tartalmazhat.

> [!IMPORTANT]
> Ha parcelable vagy szerializálható paraméterek be, ellenőrizze, hogy azok `toString()` metódus megvalósított tooreturn emberek számára olvasható karakterláncnak. Szerializálható nem átmeneti, amelyek nem szerializálható mezőket tartalmazó osztályokat megkönnyítő Android összeomlási hívásakor lesz`bundle.putSerializable("key",value);`
> 
> [!WARNING]
> A további paraméterek ritka tömbök használata nem támogatott, ez azt jelenti, hogy nem szerializálható tömbként. Meg kell alakíthatja át őket a szabványos tömbök további paraméterek használatához.
> 
> 

### <a name="example"></a>Példa
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Korlátok
#### <a name="keys"></a>Kulcsok
Minden kulcs hello `Bundle` meg kell egyeznie a következő reguláris kifejezésnek hello:

`^[a-zA-Z][a-zA-Z_0-9]*`

Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).

#### <a name="size"></a>Méret
Kiegészítő funkciók korlátozva túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement szolgáltatás).

Hello az előző példában hello toohello server elküldött JSON 58 karakter hosszú:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Jelentéskészítési alkalmazással kapcsolatos adatok
Manuálisan jelentheti a nyomkövetési adatokat (vagy más alkalmazás egyedi információt) hello `sendAppInfo()` függvény.

Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét.

Esemény kiegészítő funkciók, például hello köteg osztály használt tooabstract alkalmazással kapcsolatos információk, vegye figyelembe, hogy tömbállandó vagy részterv bundles minősül, egyszerű karakterlánc (JSON-szerializálás).

### <a name="example"></a>Példa
Ez a kód a minta toosend felhasználói nemét és születési dátumot:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Korlátok
#### <a name="keys"></a>Kulcsok
Minden kulcs hello `Bundle` meg kell egyeznie a következő reguláris kifejezésnek hello:

`^[a-zA-Z][a-zA-Z_0-9]*`

Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).

#### <a name="size"></a>Méret
Alkalmazásadatok esetén egyre korlátozódik túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement szolgáltatás).

Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:

            {"expiration":"2016-12-07","status":"premium"}
