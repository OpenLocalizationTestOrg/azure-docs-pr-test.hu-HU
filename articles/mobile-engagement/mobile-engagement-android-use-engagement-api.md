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
# <a name="how-toouse-hello-engagement-api-on-android"></a><span data-ttu-id="9f2fe-103">Hogyan tooUse hello Engagement API Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="9f2fe-103">How tooUse hello Engagement API on Android</span></span>
<span data-ttu-id="9f2fe-104">Ez a dokumentum egy bővítmény toohello dokumentum [speciális jelentéskészítési lehetőségek a Mobile Engagement Android SDK](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-104">This document is an add-on toohello document [Advanced Reporting options for Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span></span> <span data-ttu-id="9f2fe-105">A mélység adatait hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái biztosít.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-105">It provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="9f2fe-106">Vegye figyelembe, hogy ha csak szeretné Engagement tooreport munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás, majd hello legegyszerűbb módja a rendszer minden toomake a `Activity` alosztályokat örökölhet hello megfelelő `EngagementActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-106">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `Activity` sub-classes inherit from hello corresponding `EngagementActivity` class.</span></span>

<span data-ttu-id="9f2fe-107">Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementActivity` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-107">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementActivity` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="9f2fe-108">hello Engagement API által biztosított hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-108">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="9f2fe-109">Ez az osztály példánya lekérhetők hívó hello `EngagementAgent.getInstance(Context)` statikus metódus (vegye figyelembe, hogy hello `EngagementAgent` objektumot adott vissza az Egypéldányos).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-109">An instance of this class can be retrieved by calling hello `EngagementAgent.getInstance(Context)` static method (note that hello `EngagementAgent` object returned is a singleton).</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="9f2fe-110">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="9f2fe-110">Engagement concepts</span></span>
<span data-ttu-id="9f2fe-111">hello következő részekből pontosítsa közös hello [Mobile Engagement fogalmait](mobile-engagement-concepts.md), hello Android platformhoz.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md), for hello Android platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="9f2fe-112">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="9f2fe-112">`Session` and `Activity`</span></span>
<span data-ttu-id="9f2fe-113">Ha hello felhasználó több mint néhány másodpercen belül két között tétlen marad *tevékenységek*, majd a sorozatát *tevékenységek* van szétosztva két különböző *munkamenetek*.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-113">If hello user stays more than a few seconds idle between two *activities*, then his sequence of *activities* is split in two distinct *sessions*.</span></span> <span data-ttu-id="9f2fe-114">Ezek néhány másodpercig hello "munkamenet időkorlátja" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-114">These few seconds are called hello "session timeout".</span></span>

<span data-ttu-id="9f2fe-115">Egy *tevékenység* általában egy képernyő hello alkalmazás, amely toosay hello társított *tevékenység* kezdődik, amikor hello képernyő jelenik meg, és leállítja az üdvözlő képernyőt bezárásakor: Ez a hello eset, amikor hello Engagement SDK integrálva van a hello segítségével `EngagementActivity` osztályok.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-115">An *activity* is usually associated with one screen of hello application, that is toosay hello *activity* starts when hello screen is displayed and stops when hello screen is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementActivity` classes.</span></span>

<span data-ttu-id="9f2fe-116">De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-116">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="9f2fe-117">Így toosplit egy adott képernyő több sub részek tooget kapcsolatos további részletekért hello használata ezen a képernyőn (például milyen gyakran tooknown és mennyi ideig párbeszédpanelek használt ezen a képernyőn belül).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-117">This allows toosplit a given screen in several sub parts tooget more details about hello usage of this screen (for example tooknown how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="9f2fe-118">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="9f2fe-118">Reporting Activities</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9f2fe-119">Nem kell tooreport tevékenységek, például a hello használata ebben a szakaszban leírt `EngagementActivity` osztály és annak változataihoz, ahogy hello hogyan tooIntegrate Engagement Android dokumentum.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-119">You don't need tooreport activities like described in this section if you are using hello `EngagementActivity` class and its variants as explained in hello How tooIntegrate Engagement on Android document.</span></span>
> 
> 

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="9f2fe-120">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="9f2fe-120">User starts a new Activity</span></span>
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing hello current activity is required for Reach toodisplay in-app notifications, passing null will postpone such announcements and polls.

<span data-ttu-id="9f2fe-121">Toocall kell `startActivity()` minden alkalommal hello felhasználói tevékenység változik.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-121">You need toocall `startActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="9f2fe-122">hello első hívás toothis függvény új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-122">hello first call toothis function starts a new user session.</span></span>

<span data-ttu-id="9f2fe-123">Ez a funkció csak az egyes tevékenységek a legjobb hely toocall hello `onResume` visszahívás.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-123">hello best place toocall this function is on each activity `onResume` callback.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="9f2fe-124">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="9f2fe-124">User ends his current Activity</span></span>
            EngagementAgent.getInstance(this).endActivity();

<span data-ttu-id="9f2fe-125">Toocall kell `endActivity()` legalább egyszer amikor hello felhasználó befejezi az utolsó tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-125">You need toocall `endActivity()` at least once when hello user finishes his last activity.</span></span> <span data-ttu-id="9f2fe-126">Ebben értesíti az Engagement SDK-t, hogy a hello felhasználó jelenleg inaktív, és, hogy a felhasználói munkamenet hello toobe kell lezárt egyszer hello munkamenet időkorlátja lejár hello (ha meghívja a `startActivity()` előtt hello munkamenet időkorlátja lejár, a hello munkamenet egyszerűen folytatva).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-126">This informs hello Engagement SDK that hello user is currently idle, and that hello user session need toobe closed once hello session timeout will expire (if you call `startActivity()` before hello session timeout expires, hello session is simply resumed).</span></span>

<span data-ttu-id="9f2fe-127">Ez a funkció csak az egyes tevékenységek a legjobb hely toocall hello `onPause` visszahívás.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-127">hello best place toocall this function is on each activity `onPause` callback.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="9f2fe-128">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="9f2fe-128">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="9f2fe-129">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="9f2fe-129">Session events</span></span>
<span data-ttu-id="9f2fe-130">Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-130">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

<span data-ttu-id="9f2fe-131">**További adatok nélkül. példa:**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-131">**Example without extra data:**</span></span>

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

<span data-ttu-id="9f2fe-132">**Példa a további adatokat:**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-132">**Example with extra data:**</span></span>

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

### <a name="standalone-events"></a><span data-ttu-id="9f2fe-133">Önálló események</span><span class="sxs-lookup"><span data-stu-id="9f2fe-133">Standalone Events</span></span>
<span data-ttu-id="9f2fe-134">Ellenkező toosession események, a kívül egy munkamenet környezetében hello önálló események is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-134">Contrary toosession events, standalone events can occur outside of hello context of a session.</span></span>

<span data-ttu-id="9f2fe-135">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-135">**Example:**</span></span>

<span data-ttu-id="9f2fe-136">Tegyük fel, hogy a szórásos receiver kiváltásakor bekövetkező tooreport események:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-136">Suppose you want tooreport events occurring when a broadcast receiver is triggered:</span></span>

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a><span data-ttu-id="9f2fe-137">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="9f2fe-137">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="9f2fe-138">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="9f2fe-138">Session errors</span></span>
<span data-ttu-id="9f2fe-139">Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-139">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

<span data-ttu-id="9f2fe-140">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-140">**Example:**</span></span>

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

### <a name="standalone-errors"></a><span data-ttu-id="9f2fe-141">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="9f2fe-141">Standalone errors</span></span>
<span data-ttu-id="9f2fe-142">Ellenkező toosession hibák önálló hibák adódhatnak kívül hello egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-142">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

<span data-ttu-id="9f2fe-143">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-143">**Example:**</span></span>

<span data-ttu-id="9f2fe-144">hello következő példa bemutatja, hogyan tooreport hiba történt, amikor hello memória válik alacsony hello telefonján, az alkalmazás folyamat közben fut-e.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-144">hello following example shows how tooreport an error whenever hello memory becomes low on hello phone while your application process is running.</span></span>

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a><span data-ttu-id="9f2fe-145">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="9f2fe-145">Reporting Jobs</span></span>
### <a name="example"></a><span data-ttu-id="9f2fe-146">Példa</span><span class="sxs-lookup"><span data-stu-id="9f2fe-146">Example</span></span>
<span data-ttu-id="9f2fe-147">Tegyük fel, hogy a bejelentkezési folyamat tooreport hello időtartama:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-147">Suppose you want tooreport hello duration of your login process:</span></span>

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

### <a name="report-errors-during-a-job"></a><span data-ttu-id="9f2fe-148">A feladat során hibák jelentése</span><span class="sxs-lookup"><span data-stu-id="9f2fe-148">Report Errors during a Job</span></span>
<span data-ttu-id="9f2fe-149">Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-149">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="9f2fe-150">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-150">**Example:**</span></span>

<span data-ttu-id="9f2fe-151">Tegyük fel, hogy azt szeretné, hogy tooreport során, akkor hiba bejelentkezési folyamat:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-151">Suppose you want tooreport an error during you login process:</span></span>

<span data-ttu-id="9f2fe-152">[...] nyilvános "void" signIn (helyi környezet,...) {</span><span class="sxs-lookup"><span data-stu-id="9f2fe-152">[...] public void signIn(Context context, ...) {</span></span>

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

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="9f2fe-153">A feladat során jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="9f2fe-153">Reporting Events during a job</span></span>
<span data-ttu-id="9f2fe-154">Események lehet feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-154">Events can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="9f2fe-155">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9f2fe-155">**Example:**</span></span>

<span data-ttu-id="9f2fe-156">Tegyük fel, közösségi hálózata, és egy feladat tooreport hello teljes ideje mely hello felhasználói pedig csatlakoztatott toohello server használjuk.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-156">Suppose we have a social network, and we use a job tooreport hello total time during which hello user is connected toohello server.</span></span> <span data-ttu-id="9f2fe-157">hello felhasználói is maradhat háttérben akkor is, ha azt egy másik alkalmazás használja, vagy ha hello phone alvó állapotban van, így nincs munkamenet sincs.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-157">hello user can stay connected in background even when he's using another application or when hello phone is sleeping, so there is no session.</span></span>

<span data-ttu-id="9f2fe-158">hello felhasználói fogadhat üzeneteket az ismerősök, ez az esemény feladat.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-158">hello user can receive messages from his friends, this is a job event.</span></span>

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

## <a name="extra-parameters"></a><span data-ttu-id="9f2fe-159">További paraméterek</span><span class="sxs-lookup"><span data-stu-id="9f2fe-159">Extra parameters</span></span>
<span data-ttu-id="9f2fe-160">Lehet, hogy az adatokat tetszőleges csatolt tooevents, hibákat, tevékenységeket és feladatokat.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-160">Arbitrary data can be attached tooevents, errors, activities and jobs.</span></span>

<span data-ttu-id="9f2fe-161">Ezek az adatok szervezhetők, Android tartozó csomagot osztály használ (ténylegesen, akkor működik, mint az Android leképezések kiegészítő paraméterekkel).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-161">This data can be structured, it uses Android's Bundle class (actually, it works like extra parameters in Android Intents).</span></span> <span data-ttu-id="9f2fe-162">Vegye figyelembe, hogy a csomag egyik Gyermekszoftver-tömb, vagy egy másik köteg példányokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-162">Note that a Bundle can contain arrays or another Bundle instances.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f2fe-163">Ha parcelable vagy szerializálható paraméterek be, ellenőrizze, hogy azok `toString()` metódus megvalósított tooreturn emberek számára olvasható karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-163">If you put in parcelable or serializable parameters, make sure their `toString()` method is implemented tooreturn a human-readable string.</span></span> <span data-ttu-id="9f2fe-164">Szerializálható nem átmeneti, amelyek nem szerializálható mezőket tartalmazó osztályokat megkönnyítő Android összeomlási hívásakor lesz`bundle.putSerializable("key",value);`</span><span class="sxs-lookup"><span data-stu-id="9f2fe-164">Serializable classes that contain non transient fields that are not serializable will make Android crash when you will call `bundle.putSerializable("key",value);`</span></span>
> 
> [!WARNING]
> <span data-ttu-id="9f2fe-165">A további paraméterek ritka tömbök használata nem támogatott, ez azt jelenti, hogy nem szerializálható tömbként.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-165">Sparse arrays in extra parameters are not supported, that is, it won't be serialized as an array.</span></span> <span data-ttu-id="9f2fe-166">Meg kell alakíthatja át őket a szabványos tömbök további paraméterek használatához.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-166">You should convert them into standard arrays before using it in extra parameters.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="9f2fe-167">Példa</span><span class="sxs-lookup"><span data-stu-id="9f2fe-167">Example</span></span>
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="9f2fe-168">Korlátok</span><span class="sxs-lookup"><span data-stu-id="9f2fe-168">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="9f2fe-169">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="9f2fe-169">Keys</span></span>
<span data-ttu-id="9f2fe-170">Minden kulcs hello `Bundle` meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-170">Each key in hello `Bundle` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="9f2fe-171">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-171">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="9f2fe-172">Méret</span><span class="sxs-lookup"><span data-stu-id="9f2fe-172">Size</span></span>
<span data-ttu-id="9f2fe-173">Kiegészítő funkciók korlátozva túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-173">Extras are limited too**1024** characters per call (once encoded in JSON by hello Engagement service).</span></span>

<span data-ttu-id="9f2fe-174">Hello az előző példában hello toohello server elküldött JSON 58 karakter hosszú:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-174">In hello previous example, hello JSON sent toohello server is 58 characters long:</span></span>

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="9f2fe-175">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="9f2fe-175">Reporting Application Information</span></span>
<span data-ttu-id="9f2fe-176">Manuálisan jelentheti a nyomkövetési adatokat (vagy más alkalmazás egyedi információt) hello `sendAppInfo()` függvény.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-176">You can manually report tracking information (or any other application specific information) using hello `sendAppInfo()` function.</span></span>

<span data-ttu-id="9f2fe-177">Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="9f2fe-177">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="9f2fe-178">Esemény kiegészítő funkciók, például hello köteg osztály használt tooabstract alkalmazással kapcsolatos információk, vegye figyelembe, hogy tömbállandó vagy részterv bundles minősül, egyszerű karakterlánc (JSON-szerializálás).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-178">Like event extras, hello Bundle class is used tooabstract application information, note that arrays or sub-bundles will be treated as flat strings (using JSON serialization).</span></span>

### <a name="example"></a><span data-ttu-id="9f2fe-179">Példa</span><span class="sxs-lookup"><span data-stu-id="9f2fe-179">Example</span></span>
<span data-ttu-id="9f2fe-180">Ez a kód a minta toosend felhasználói nemét és születési dátumot:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-180">Here is a code sample toosend user gender and birthdate:</span></span>

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="9f2fe-181">Korlátok</span><span class="sxs-lookup"><span data-stu-id="9f2fe-181">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="9f2fe-182">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="9f2fe-182">Keys</span></span>
<span data-ttu-id="9f2fe-183">Minden kulcs hello `Bundle` meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-183">Each key in hello `Bundle` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="9f2fe-184">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-184">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="9f2fe-185">Méret</span><span class="sxs-lookup"><span data-stu-id="9f2fe-185">Size</span></span>
<span data-ttu-id="9f2fe-186">Alkalmazásadatok esetén egyre korlátozódik túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="9f2fe-186">Application information are limited too**1024** characters per call (once encoded in JSON by hello Engagement service).</span></span>

<span data-ttu-id="9f2fe-187">Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:</span><span class="sxs-lookup"><span data-stu-id="9f2fe-187">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"expiration":"2016-12-07","status":"premium"}
