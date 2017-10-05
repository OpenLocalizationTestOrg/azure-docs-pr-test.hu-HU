---
title: "A bevonási API használata IOS rendszerű eszközökön"
description: "Legújabb iOS SDK - IOS rendszerű eszközökön a bevonási API használatával"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a31424da98205e97bdf57010cccfd044360f03dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-ios"></a><span data-ttu-id="3cce5-103">A bevonási API használata IOS rendszerű eszközökön</span><span class="sxs-lookup"><span data-stu-id="3cce5-103">How to Use the Engagement API on iOS</span></span>
<span data-ttu-id="3cce5-104">Ez a dokumentum az bővítménye a dokumentum hogyan integrálhatja Engagement IOS: a mélység részletei jelentés az alkalmazás statisztikái az Engagement API használatával biztosít.</span><span class="sxs-lookup"><span data-stu-id="3cce5-104">This document is an add-on to the document How to Integrate Engagement on iOS: it provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="3cce5-105">Ne feledje, hogy ha Engagement jelenti a munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás csak szeretne, majd a legegyszerűbb módja annak, hogy az egyéni `UIViewController` objektumok öröklik a megfelelő `EngagementViewController` osztály.</span><span class="sxs-lookup"><span data-stu-id="3cce5-105">Keep in mind that if you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your custom `UIViewController` objects inherit from the corresponding `EngagementViewController` class.</span></span>

<span data-ttu-id="3cce5-106">Ha szeretne többet, például ha adott eseményeket, hibákat és feladatok, jelentenie, vagy ha a jelentés az alkalmazás tevékenységei eltérő módon, mint az egyik valósult meg, hogy a `EngagementViewController` osztályokat, akkor használja az Engagement API szükséges.</span><span class="sxs-lookup"><span data-stu-id="3cce5-106">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementViewController` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="3cce5-107">A bevonási API által biztosított a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="3cce5-107">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="3cce5-108">Ez az osztály példánya meghívásával lehet beolvasni a `[EngagementAgent shared]` statikus metódus (vegye figyelembe, hogy a `EngagementAgent` objektumot adott vissza az Egypéldányos).</span><span class="sxs-lookup"><span data-stu-id="3cce5-108">An instance of this class can be retrieved by calling the `[EngagementAgent shared]` static method (note that the `EngagementAgent` object returned is a singleton).</span></span>

<span data-ttu-id="3cce5-109">Minden API-hívások előtt az `EngagementAgent` objektumot inicializálni kell a metódus meghívásával.`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span><span class="sxs-lookup"><span data-stu-id="3cce5-109">Before any API calls, the `EngagementAgent` object must be initialized by calling the method `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="3cce5-110">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="3cce5-110">Engagement concepts</span></span>
<span data-ttu-id="3cce5-111">A következő részekből finomíthatja a közös [Mobile Engagement fogalmait](mobile-engagement-concepts.md) az iOS platformra.</span><span class="sxs-lookup"><span data-stu-id="3cce5-111">The following parts refine the common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for the iOS platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="3cce5-112">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="3cce5-112">`Session` and `Activity`</span></span>
<span data-ttu-id="3cce5-113">Egy *tevékenység* tartozik általában egy képernyő az alkalmazás, azaz a *tevékenység* kezdődik, amikor a képernyő jelenik meg, és leállítja a képernyő bezárásakor: Ez a helyzet, amikor az Engagement SDK használatával integrálva van a `EngagementViewController` osztályok.</span><span class="sxs-lookup"><span data-stu-id="3cce5-113">An *activity* is usually associated with one screen of the application, that is to say the *activity* starts when the screen is displayed and stops when the screen is closed: this is the case when the Engagement SDK is integrated by using the `EngagementViewController` classes.</span></span>

<span data-ttu-id="3cce5-114">De *tevékenységek* is szabályozhatja manuálisan az Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="3cce5-114">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="3cce5-115">Ez lehetővé teszi több sub pontján további részletes információkat az ezen a képernyőn (például hogy milyen gyakran ismert, és mennyi ideig párbeszédpanelek ezen a képernyőn belül használt) használatát egy adott képernyő felosztása.</span><span class="sxs-lookup"><span data-stu-id="3cce5-115">This allows to split a given screen in several sub parts to get more details about the usage of this screen (for example to known how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="3cce5-116">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="3cce5-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="3cce5-117">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="3cce5-117">User starts a new Activity</span></span>
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

<span data-ttu-id="3cce5-118">Meg kell hívnia `startActivity()` minden egyes alkalommal, amikor a felhasználói tevékenység módosul.</span><span class="sxs-lookup"><span data-stu-id="3cce5-118">You need to call `startActivity()` each time the user activity changes.</span></span> <span data-ttu-id="3cce5-119">Ez a függvény az első hívás új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="3cce5-119">The first call to this function starts a new user session.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="3cce5-120">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="3cce5-120">User ends his current Activity</span></span>
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> <span data-ttu-id="3cce5-121">Meg kell **soha** hív, ez a funkció önállóan, kivéve ha szeretné osztani több munkamenet az alkalmazás használatát: a függvény hívása úgy, azonnal, a jelenlegi munkamenet volna end későbbi hívása `startActivity()` volna indítson egy újat.</span><span class="sxs-lookup"><span data-stu-id="3cce5-121">You should **NEVER** call this function by yourself, except if you want to split one use of your application into several sessions: a call to this function would end the current session immediately, so, a subsequent call to `startActivity()` would start a new session.</span></span> <span data-ttu-id="3cce5-122">Ez a funkció automatikusan hívja az SDK az alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="3cce5-122">This function is automatically called by the SDK when your application is closed.</span></span>
> 
> 

## <a name="reporting-events"></a><span data-ttu-id="3cce5-123">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="3cce5-123">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="3cce5-124">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="3cce5-124">Session events</span></span>
<span data-ttu-id="3cce5-125">A munkamenet során a felhasználó által végrehajtott műveletek jelentésére általában használhatók munkamenet események.</span><span class="sxs-lookup"><span data-stu-id="3cce5-125">Session events are usually used to report the actions performed by a user during his session.</span></span>

<span data-ttu-id="3cce5-126">**További adatok nélkül. példa:**</span><span class="sxs-lookup"><span data-stu-id="3cce5-126">**Example without extra data:**</span></span>

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

<span data-ttu-id="3cce5-127">**Példa a további adatokat:**</span><span class="sxs-lookup"><span data-stu-id="3cce5-127">**Example with extra data:**</span></span>

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="3cce5-128">Önálló események</span><span class="sxs-lookup"><span data-stu-id="3cce5-128">Standalone events</span></span>
<span data-ttu-id="3cce5-129">Ellentétesen munkamenet események különálló esemény egy munkamenet környezetében kívül is használható.</span><span class="sxs-lookup"><span data-stu-id="3cce5-129">Contrary to session events, standalone events can be used outside of the context of a session.</span></span>

<span data-ttu-id="3cce5-130">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-130">**Example:**</span></span>

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a><span data-ttu-id="3cce5-131">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="3cce5-131">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="3cce5-132">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="3cce5-132">Session errors</span></span>
<span data-ttu-id="3cce5-133">Munkamenet a hibák általában használhatók a a munkamenet során a felhasználót érintő hibák jelentését.</span><span class="sxs-lookup"><span data-stu-id="3cce5-133">Session errors are usually used to report the errors impacting the user during his session.</span></span>

<span data-ttu-id="3cce5-134">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-134">**Example:**</span></span>

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="3cce5-135">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="3cce5-135">Standalone errors</span></span>
<span data-ttu-id="3cce5-136">Munkamenet hibák ellentétesen önálló hibák kívül egy munkamenet környezetében használható.</span><span class="sxs-lookup"><span data-stu-id="3cce5-136">Contrary to session errors, standalone errors can be used outside of the context of a session.</span></span>

<span data-ttu-id="3cce5-137">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-137">**Example:**</span></span>

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a><span data-ttu-id="3cce5-138">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="3cce5-138">Reporting Jobs</span></span>
<span data-ttu-id="3cce5-139">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-139">**Example:**</span></span>

<span data-ttu-id="3cce5-140">Tegyük fel, hogy szeretne jelentést készíteni a bejelentkezési folyamat időtartama:</span><span class="sxs-lookup"><span data-stu-id="3cce5-140">Suppose you want to report the duration of your login process:</span></span>

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="3cce5-141">A feladat során hibák jelentése</span><span class="sxs-lookup"><span data-stu-id="3cce5-141">Report Errors during a Job</span></span>
<span data-ttu-id="3cce5-142">Hibák a jelenlegi felhasználói munkamenet való helyett egy futó feladat kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="3cce5-142">Errors can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="3cce5-143">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-143">**Example:**</span></span>

<span data-ttu-id="3cce5-144">Tegyük fel, hogy a bejelentkezési folyamat során hiba jelentését:</span><span class="sxs-lookup"><span data-stu-id="3cce5-144">Suppose you want to report an error during your login process:</span></span>

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a><span data-ttu-id="3cce5-145">A feladat során események</span><span class="sxs-lookup"><span data-stu-id="3cce5-145">Events during a job</span></span>
<span data-ttu-id="3cce5-146">Események egy futó feladat helyett a jelenlegi felhasználói munkamenet való kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="3cce5-146">Events can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="3cce5-147">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-147">**Example:**</span></span>

<span data-ttu-id="3cce5-148">Tegyük fel, közösségi hálózata, és a teljes idő, ameddig a felhasználó csatlakozik-e a kiszolgáló egy feladatot, amely a jelentés használjuk.</span><span class="sxs-lookup"><span data-stu-id="3cce5-148">Suppose we have a social network, and we use a job to report the total time during which the user is connected to the server.</span></span> <span data-ttu-id="3cce5-149">A felhasználó fogadhat üzeneteket az ismerősök, ez az esemény feladat.</span><span class="sxs-lookup"><span data-stu-id="3cce5-149">The user can receive messages from his friends, this is a job event.</span></span>

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a><span data-ttu-id="3cce5-150">További paraméterek</span><span class="sxs-lookup"><span data-stu-id="3cce5-150">Extra parameters</span></span>
<span data-ttu-id="3cce5-151">Tetszőleges adatok csatolható események, hibákat, tevékenységeket és feladatokat.</span><span class="sxs-lookup"><span data-stu-id="3cce5-151">Arbitrary data can be attached to events, errors, activities and jobs.</span></span>

<span data-ttu-id="3cce5-152">Ezek az adatok szervezhetők, iOS tartozó NSDictionary osztályt használja.</span><span class="sxs-lookup"><span data-stu-id="3cce5-152">This data can be structured, it uses iOS's NSDictionary class.</span></span>

<span data-ttu-id="3cce5-153">Vegye figyelembe, hogy a kiegészítő funkciók tartalmazhatnak `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` vagy más `NSDictionary` példányok.</span><span class="sxs-lookup"><span data-stu-id="3cce5-153">Note that extras can contain `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` or other `NSDictionary` instances.</span></span>

> [!NOTE]
> <span data-ttu-id="3cce5-154">A kiegészítő paraméterrel szerializált a JSON-ban.</span><span class="sxs-lookup"><span data-stu-id="3cce5-154">The extra parameter is serialized in JSON.</span></span> <span data-ttu-id="3cce5-155">Ha a különböző objektumok, mint a fent leírt átadni kívánt, meg kell valósítani a következő metódust az osztályban:</span><span class="sxs-lookup"><span data-stu-id="3cce5-155">If you want to pass different objects than the ones described above, you must implement the following method in your class:</span></span>
> 
> <span data-ttu-id="3cce5-156">-(NSString*) JSONRepresentation;</span><span class="sxs-lookup"><span data-stu-id="3cce5-156">-(NSString*)JSONRepresentation;</span></span>
> 
> <span data-ttu-id="3cce5-157">A módszer az objektum JSON-ábrázolását kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="3cce5-157">The method should return a JSON representation of your object.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="3cce5-158">Példa</span><span class="sxs-lookup"><span data-stu-id="3cce5-158">Example</span></span>
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a><span data-ttu-id="3cce5-159">Korlátok</span><span class="sxs-lookup"><span data-stu-id="3cce5-159">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="3cce5-160">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="3cce5-160">Keys</span></span>
<span data-ttu-id="3cce5-161">Az egyes kulcsok a `NSDictionary` meg kell egyeznie a következő reguláris kifejezésnek:</span><span class="sxs-lookup"><span data-stu-id="3cce5-161">Each key in the `NSDictionary` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="3cce5-162">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="3cce5-162">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="3cce5-163">Méret</span><span class="sxs-lookup"><span data-stu-id="3cce5-163">Size</span></span>
<span data-ttu-id="3cce5-164">Kiegészítő funkciók korlátozva **1024** karakter / hívás (egyszer kódolású a JSON-ban az Engagement ügynök).</span><span class="sxs-lookup"><span data-stu-id="3cce5-164">Extras are limited to **1024** characters per call (once encoded in JSON by the Engagement agent).</span></span>

<span data-ttu-id="3cce5-165">Az előző példában a a kiszolgálónak küldött JSON-ja 58 karakter:</span><span class="sxs-lookup"><span data-stu-id="3cce5-165">In the previous example, the JSON sent to the server is 58 characters long:</span></span>

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="3cce5-166">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="3cce5-166">Reporting Application Information</span></span>
<span data-ttu-id="3cce5-167">Manuálisan jelentheti a nyomkövetési adatokat (vagy más alkalmazás egyedi információt) használatával a `sendAppInfo:` függvény.</span><span class="sxs-lookup"><span data-stu-id="3cce5-167">You can manually report tracking information (or any other application specific information) using the `sendAppInfo:` function.</span></span>

<span data-ttu-id="3cce5-168">Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: a megadott kulcs csak a legutóbbi értékét az adott eszköz megmarad.</span><span class="sxs-lookup"><span data-stu-id="3cce5-168">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="3cce5-169">Esemény kiegészítő funkciók, például a `NSDictionary` osztály absztrakt alkalmazással kapcsolatos információk, vegye figyelembe, hogy tömbök vagy alárendelt szótárak minősül, egyszerű karakterlánc (JSON-szerializálás) szolgál.</span><span class="sxs-lookup"><span data-stu-id="3cce5-169">Like event extras, the `NSDictionary` class is used to abstract application information, note that arrays or sub-dictionaries will be treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="3cce5-170">**Példa**</span><span class="sxs-lookup"><span data-stu-id="3cce5-170">**Example:**</span></span>

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a><span data-ttu-id="3cce5-171">Korlátok</span><span class="sxs-lookup"><span data-stu-id="3cce5-171">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="3cce5-172">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="3cce5-172">Keys</span></span>
<span data-ttu-id="3cce5-173">Az egyes kulcsok a `NSDictionary` meg kell egyeznie a következő reguláris kifejezésnek:</span><span class="sxs-lookup"><span data-stu-id="3cce5-173">Each key in the `NSDictionary` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="3cce5-174">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="3cce5-174">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="3cce5-175">Méret</span><span class="sxs-lookup"><span data-stu-id="3cce5-175">Size</span></span>
<span data-ttu-id="3cce5-176">Alkalmazásadatok esetén egyre korlátozódik **1024** karakter / hívás (egyszer kódolású a JSON-ban az Engagement ügynök).</span><span class="sxs-lookup"><span data-stu-id="3cce5-176">Application information are limited to **1024** characters per call (once encoded in JSON by the Engagement agent).</span></span>

<span data-ttu-id="3cce5-177">Az előző példában a a kiszolgálónak küldött JSON-ja 44 karakter:</span><span class="sxs-lookup"><span data-stu-id="3cce5-177">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
