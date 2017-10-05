---
title: "A bevonási API használatával Android rendszeren"
description: "Legújabb Android SDK - az Engagement API használatával Android rendszeren"
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
ms.openlocfilehash: d353cd2fe47c54a0282cc5bb1b22b4a56e0cd82c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-android"></a><span data-ttu-id="b58ac-103">A bevonási API használatával Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="b58ac-103">How to Use the Engagement API on Android</span></span>
<span data-ttu-id="b58ac-104">Ez a dokumentum az bővítménye a dokumentum [speciális jelentéskészítési lehetőségek a Mobile Engagement Android SDK](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="b58ac-104">This document is an add-on to the document [Advanced Reporting options for Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span></span> <span data-ttu-id="b58ac-105">A mélység részletei jelentés az alkalmazás statisztikái az Engagement API használatával biztosít.</span><span class="sxs-lookup"><span data-stu-id="b58ac-105">It provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="b58ac-106">Ne feledje, hogy ha Engagement jelenti a munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás csak szeretne, majd a legegyszerűbb módja a rendszer minden ahhoz a `Activity` alosztályokat öröklik a megfelelő `EngagementActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="b58ac-106">Keep in mind that if you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `Activity` sub-classes inherit from the corresponding `EngagementActivity` class.</span></span>

<span data-ttu-id="b58ac-107">Ha szeretne többet, például ha adott eseményeket, hibákat és feladatok, jelentenie, vagy ha a jelentés az alkalmazás tevékenységei eltérő módon, mint az egyik valósult meg, hogy a `EngagementActivity` osztályokat, akkor használja az Engagement API szükséges.</span><span class="sxs-lookup"><span data-stu-id="b58ac-107">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementActivity` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="b58ac-108">A bevonási API által biztosított a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="b58ac-108">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="b58ac-109">Ez az osztály példánya meghívásával lehet beolvasni a `EngagementAgent.getInstance(Context)` statikus metódus (vegye figyelembe, hogy a `EngagementAgent` objektumot adott vissza az Egypéldányos).</span><span class="sxs-lookup"><span data-stu-id="b58ac-109">An instance of this class can be retrieved by calling the `EngagementAgent.getInstance(Context)` static method (note that the `EngagementAgent` object returned is a singleton).</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="b58ac-110">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="b58ac-110">Engagement concepts</span></span>
<span data-ttu-id="b58ac-111">A következő részekből finomíthatja a közös [Mobile Engagement fogalmait](mobile-engagement-concepts.md), az Android platformhoz.</span><span class="sxs-lookup"><span data-stu-id="b58ac-111">The following parts refine the common [Mobile Engagement Concepts](mobile-engagement-concepts.md), for the Android platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="b58ac-112">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="b58ac-112">`Session` and `Activity`</span></span>
<span data-ttu-id="b58ac-113">Ha a felhasználó több mint néhány másodpercen belül két között tétlen marad *tevékenységek*, majd a sorozatát *tevékenységek* van szétosztva két különböző *munkamenetek*.</span><span class="sxs-lookup"><span data-stu-id="b58ac-113">If the user stays more than a few seconds idle between two *activities*, then his sequence of *activities* is split in two distinct *sessions*.</span></span> <span data-ttu-id="b58ac-114">Ezek néhány másodpercet a "munkamenet időkorlátja" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="b58ac-114">These few seconds are called the "session timeout".</span></span>

<span data-ttu-id="b58ac-115">Egy *tevékenység* tartozik általában egy képernyő az alkalmazás, azaz a *tevékenység* kezdődik, amikor a képernyő jelenik meg, és leállítja a képernyő bezárásakor: Ez a helyzet, amikor az Engagement SDK használatával integrálva van a `EngagementActivity` osztályok.</span><span class="sxs-lookup"><span data-stu-id="b58ac-115">An *activity* is usually associated with one screen of the application, that is to say the *activity* starts when the screen is displayed and stops when the screen is closed: this is the case when the Engagement SDK is integrated by using the `EngagementActivity` classes.</span></span>

<span data-ttu-id="b58ac-116">De *tevékenységek* is szabályozhatja manuálisan az Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="b58ac-116">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="b58ac-117">Ez lehetővé teszi több sub pontján további részletes információkat az ezen a képernyőn (például hogy milyen gyakran ismert, és mennyi ideig párbeszédpanelek ezen a képernyőn belül használt) használatát egy adott képernyő felosztása.</span><span class="sxs-lookup"><span data-stu-id="b58ac-117">This allows to split a given screen in several sub parts to get more details about the usage of this screen (for example to known how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="b58ac-118">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="b58ac-118">Reporting Activities</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b58ac-119">Ebben a szakaszban ismertetett használata tevékenységek, például jelentés nem kell a `EngagementActivity` osztály és annak változataihoz Android dokumentumon integrálni engagement hogyan leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="b58ac-119">You don't need to report activities like described in this section if you are using the `EngagementActivity` class and its variants as explained in the How to Integrate Engagement on Android document.</span></span>
> 
> 

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="b58ac-120">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="b58ac-120">User starts a new Activity</span></span>
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

<span data-ttu-id="b58ac-121">Meg kell hívnia `startActivity()` minden egyes alkalommal, amikor a felhasználói tevékenység módosul.</span><span class="sxs-lookup"><span data-stu-id="b58ac-121">You need to call `startActivity()` each time the user activity changes.</span></span> <span data-ttu-id="b58ac-122">Ez a függvény az első hívás új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="b58ac-122">The first call to this function starts a new user session.</span></span>

<span data-ttu-id="b58ac-123">Ez a függvény hívása a legjobb hely van, minden tevékenység `onResume` visszahívás.</span><span class="sxs-lookup"><span data-stu-id="b58ac-123">The best place to call this function is on each activity `onResume` callback.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="b58ac-124">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="b58ac-124">User ends his current Activity</span></span>
            EngagementAgent.getInstance(this).endActivity();

<span data-ttu-id="b58ac-125">Meg kell hívnia `endActivity()` legalább egyszer amikor a felhasználó befejezi az utolsó tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b58ac-125">You need to call `endActivity()` at least once when the user finishes his last activity.</span></span> <span data-ttu-id="b58ac-126">Ebben értesíti az Engagement SDK-t, hogy a felhasználó jelenleg inaktív, valamint, hogy a felhasználói munkamenet kell egyszer lezárni a munkamenet időkorlátja lejár (ha meghívja a `startActivity()` előtt a munkamenet időkorlátja lejár, egyszerűen folytatni a munkamenetet).</span><span class="sxs-lookup"><span data-stu-id="b58ac-126">This informs the Engagement SDK that the user is currently idle, and that the user session need to be closed once the session timeout will expire (if you call `startActivity()` before the session timeout expires, the session is simply resumed).</span></span>

<span data-ttu-id="b58ac-127">Ez a függvény hívása a legjobb hely van, minden tevékenység `onPause` visszahívás.</span><span class="sxs-lookup"><span data-stu-id="b58ac-127">The best place to call this function is on each activity `onPause` callback.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="b58ac-128">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="b58ac-128">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="b58ac-129">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="b58ac-129">Session events</span></span>
<span data-ttu-id="b58ac-130">A munkamenet során a felhasználó által végrehajtott műveletek jelentésére általában használhatók munkamenet események.</span><span class="sxs-lookup"><span data-stu-id="b58ac-130">Session events are usually used to report the actions performed by a user during his session.</span></span>

<span data-ttu-id="b58ac-131">**További adatok nélkül. példa:**</span><span class="sxs-lookup"><span data-stu-id="b58ac-131">**Example without extra data:**</span></span>

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

<span data-ttu-id="b58ac-132">**Példa a további adatokat:**</span><span class="sxs-lookup"><span data-stu-id="b58ac-132">**Example with extra data:**</span></span>

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

### <a name="standalone-events"></a><span data-ttu-id="b58ac-133">Önálló események</span><span class="sxs-lookup"><span data-stu-id="b58ac-133">Standalone Events</span></span>
<span data-ttu-id="b58ac-134">Munkamenet események ellentétesen önálló események egy munkamenet környezetében kívül is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="b58ac-134">Contrary to session events, standalone events can occur outside of the context of a session.</span></span>

<span data-ttu-id="b58ac-135">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b58ac-135">**Example:**</span></span>

<span data-ttu-id="b58ac-136">Tegyük fel, a szórásos receiver kiváltásakor bekövetkező jelentés események:</span><span class="sxs-lookup"><span data-stu-id="b58ac-136">Suppose you want to report events occurring when a broadcast receiver is triggered:</span></span>

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a><span data-ttu-id="b58ac-137">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="b58ac-137">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="b58ac-138">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="b58ac-138">Session errors</span></span>
<span data-ttu-id="b58ac-139">Munkamenet a hibák általában használhatók a a munkamenet során a felhasználót érintő hibák jelentését.</span><span class="sxs-lookup"><span data-stu-id="b58ac-139">Session errors are usually used to report the errors impacting the user during his session.</span></span>

<span data-ttu-id="b58ac-140">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b58ac-140">**Example:**</span></span>

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a><span data-ttu-id="b58ac-141">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="b58ac-141">Standalone errors</span></span>
<span data-ttu-id="b58ac-142">Munkamenet hibák ellentétesen önálló felléphetnek a kívül egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="b58ac-142">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

<span data-ttu-id="b58ac-143">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b58ac-143">**Example:**</span></span>

<span data-ttu-id="b58ac-144">A következő példa bemutatja, hogyan hiba jelentését, amikor a memória válik alacsony a telefonján, az alkalmazás folyamat futása közben.</span><span class="sxs-lookup"><span data-stu-id="b58ac-144">The following example shows how to report an error whenever the memory becomes low on the phone while your application process is running.</span></span>

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a><span data-ttu-id="b58ac-145">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="b58ac-145">Reporting Jobs</span></span>
### <a name="example"></a><span data-ttu-id="b58ac-146">Példa</span><span class="sxs-lookup"><span data-stu-id="b58ac-146">Example</span></span>
<span data-ttu-id="b58ac-147">Tegyük fel, hogy szeretne jelentést készíteni a bejelentkezési folyamat időtartama:</span><span class="sxs-lookup"><span data-stu-id="b58ac-147">Suppose you want to report the duration of your login process:</span></span>

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="b58ac-148">A feladat során hibák jelentése</span><span class="sxs-lookup"><span data-stu-id="b58ac-148">Report Errors during a Job</span></span>
<span data-ttu-id="b58ac-149">Hibák a jelenlegi felhasználói munkamenet való helyett egy futó feladat kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="b58ac-149">Errors can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="b58ac-150">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b58ac-150">**Example:**</span></span>

<span data-ttu-id="b58ac-151">Tegyük fel, hogy során hiba bejelentkezési folyamat jelentést szeretne:</span><span class="sxs-lookup"><span data-stu-id="b58ac-151">Suppose you want to report an error during you login process:</span></span>

<span data-ttu-id="b58ac-152">[...] nyilvános "void" signIn (helyi környezet,...) {</span><span class="sxs-lookup"><span data-stu-id="b58ac-152">[...] public void signIn(Context context, ...) {</span></span>

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="b58ac-153">A feladat során jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="b58ac-153">Reporting Events during a job</span></span>
<span data-ttu-id="b58ac-154">Események egy futó feladat helyett a jelenlegi felhasználói munkamenet való kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="b58ac-154">Events can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="b58ac-155">**Példa**</span><span class="sxs-lookup"><span data-stu-id="b58ac-155">**Example:**</span></span>

<span data-ttu-id="b58ac-156">Tegyük fel, közösségi hálózata, és a teljes idő, ameddig a felhasználó csatlakozik-e a kiszolgáló egy feladatot, amely a jelentés használjuk.</span><span class="sxs-lookup"><span data-stu-id="b58ac-156">Suppose we have a social network, and we use a job to report the total time during which the user is connected to the server.</span></span> <span data-ttu-id="b58ac-157">A felhasználó is maradhat háttérben még akkor is, ha azt egy másik alkalmazás használja, vagy ha a telefonszám alvó állapotban van, így nincs munkamenet sincs.</span><span class="sxs-lookup"><span data-stu-id="b58ac-157">The user can stay connected in background even when he's using another application or when the phone is sleeping, so there is no session.</span></span>

<span data-ttu-id="b58ac-158">A felhasználó fogadhat üzeneteket az ismerősök, ez az esemény feladat.</span><span class="sxs-lookup"><span data-stu-id="b58ac-158">The user can receive messages from his friends, this is a job event.</span></span>

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

## <a name="extra-parameters"></a><span data-ttu-id="b58ac-159">További paraméterek</span><span class="sxs-lookup"><span data-stu-id="b58ac-159">Extra parameters</span></span>
<span data-ttu-id="b58ac-160">Tetszőleges adatok csatolható események, hibákat, tevékenységeket és feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b58ac-160">Arbitrary data can be attached to events, errors, activities and jobs.</span></span>

<span data-ttu-id="b58ac-161">Ezek az adatok szervezhetők, Android tartozó csomagot osztály használ (ténylegesen, akkor működik, mint az Android leképezések kiegészítő paraméterekkel).</span><span class="sxs-lookup"><span data-stu-id="b58ac-161">This data can be structured, it uses Android's Bundle class (actually, it works like extra parameters in Android Intents).</span></span> <span data-ttu-id="b58ac-162">Vegye figyelembe, hogy a csomag egyik Gyermekszoftver-tömb, vagy egy másik köteg példányokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b58ac-162">Note that a Bundle can contain arrays or another Bundle instances.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b58ac-163">Ha parcelable vagy szerializálható paraméterek be, ellenőrizze, hogy azok `toString()` metódus megvalósítása emberek számára olvasható karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="b58ac-163">If you put in parcelable or serializable parameters, make sure their `toString()` method is implemented to return a human-readable string.</span></span> <span data-ttu-id="b58ac-164">Szerializálható nem átmeneti, amelyek nem szerializálható mezőket tartalmazó osztályokat megkönnyítő Android összeomlási hívásakor lesz`bundle.putSerializable("key",value);`</span><span class="sxs-lookup"><span data-stu-id="b58ac-164">Serializable classes that contain non transient fields that are not serializable will make Android crash when you will call `bundle.putSerializable("key",value);`</span></span>
> 
> [!WARNING]
> <span data-ttu-id="b58ac-165">A további paraméterek ritka tömbök használata nem támogatott, ez azt jelenti, hogy nem szerializálható tömbként.</span><span class="sxs-lookup"><span data-stu-id="b58ac-165">Sparse arrays in extra parameters are not supported, that is, it won't be serialized as an array.</span></span> <span data-ttu-id="b58ac-166">Meg kell alakíthatja át őket a szabványos tömbök további paraméterek használatához.</span><span class="sxs-lookup"><span data-stu-id="b58ac-166">You should convert them into standard arrays before using it in extra parameters.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="b58ac-167">Példa</span><span class="sxs-lookup"><span data-stu-id="b58ac-167">Example</span></span>
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="b58ac-168">Korlátok</span><span class="sxs-lookup"><span data-stu-id="b58ac-168">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="b58ac-169">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="b58ac-169">Keys</span></span>
<span data-ttu-id="b58ac-170">Az egyes kulcsok a `Bundle` meg kell egyeznie a következő reguláris kifejezésnek:</span><span class="sxs-lookup"><span data-stu-id="b58ac-170">Each key in the `Bundle` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="b58ac-171">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="b58ac-171">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="b58ac-172">Méret</span><span class="sxs-lookup"><span data-stu-id="b58ac-172">Size</span></span>
<span data-ttu-id="b58ac-173">Kiegészítő funkciók korlátozva **1024** karakter / hívás (egyszer kódolású a JSON-ban az Engagement szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="b58ac-173">Extras are limited to **1024** characters per call (once encoded in JSON by the Engagement service).</span></span>

<span data-ttu-id="b58ac-174">Az előző példában a a kiszolgálónak küldött JSON-ja 58 karakter:</span><span class="sxs-lookup"><span data-stu-id="b58ac-174">In the previous example, the JSON sent to the server is 58 characters long:</span></span>

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="b58ac-175">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="b58ac-175">Reporting Application Information</span></span>
<span data-ttu-id="b58ac-176">Manuálisan jelentheti a nyomkövetési adatokat (vagy más alkalmazás egyedi információt) használatával a `sendAppInfo()` függvény.</span><span class="sxs-lookup"><span data-stu-id="b58ac-176">You can manually report tracking information (or any other application specific information) using the `sendAppInfo()` function.</span></span>

<span data-ttu-id="b58ac-177">Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: a megadott kulcs csak a legutóbbi értékét az adott eszköz megmarad.</span><span class="sxs-lookup"><span data-stu-id="b58ac-177">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="b58ac-178">Esemény kiegészítő funkciók, például a csomag osztály szolgál az alkalmazással kapcsolatos adatok absztrakt, vegye figyelembe, hogy tömbök vagy alárendelt kötegek minősül, egyszerű karakterlánc (JSON-szerializálás).</span><span class="sxs-lookup"><span data-stu-id="b58ac-178">Like event extras, the Bundle class is used to abstract application information, note that arrays or sub-bundles will be treated as flat strings (using JSON serialization).</span></span>

### <a name="example"></a><span data-ttu-id="b58ac-179">Példa</span><span class="sxs-lookup"><span data-stu-id="b58ac-179">Example</span></span>
<span data-ttu-id="b58ac-180">Ez a kódminta, a felhasználó nemét és születési dátumot küldését:</span><span class="sxs-lookup"><span data-stu-id="b58ac-180">Here is a code sample to send user gender and birthdate:</span></span>

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="b58ac-181">Korlátok</span><span class="sxs-lookup"><span data-stu-id="b58ac-181">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="b58ac-182">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="b58ac-182">Keys</span></span>
<span data-ttu-id="b58ac-183">Az egyes kulcsok a `Bundle` meg kell egyeznie a következő reguláris kifejezésnek:</span><span class="sxs-lookup"><span data-stu-id="b58ac-183">Each key in the `Bundle` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="b58ac-184">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="b58ac-184">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="b58ac-185">Méret</span><span class="sxs-lookup"><span data-stu-id="b58ac-185">Size</span></span>
<span data-ttu-id="b58ac-186">Alkalmazásadatok esetén egyre korlátozódik **1024** karakter / hívás (egyszer kódolású a JSON-ban az Engagement szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="b58ac-186">Application information are limited to **1024** characters per call (once encoded in JSON by the Engagement service).</span></span>

<span data-ttu-id="b58ac-187">Az előző példában a a kiszolgálónak küldött JSON-ja 44 karakter:</span><span class="sxs-lookup"><span data-stu-id="b58ac-187">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"expiration":"2016-12-07","status":"premium"}
