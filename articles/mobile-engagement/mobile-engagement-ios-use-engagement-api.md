---
title: "aaaHow tooUse hello Engagement API IOS rendszerű eszközökön"
description: "Legújabb iOS SDK - tooUse hogyan Engagement API hello iOS"
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
ms.openlocfilehash: 7fb9b95ad319cf3b1e2de81b5d6aee5b30266069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-ios"></a><span data-ttu-id="484d2-103">Hogyan tooUse hello IOS Engagement API</span><span class="sxs-lookup"><span data-stu-id="484d2-103">How tooUse hello Engagement API on iOS</span></span>
<span data-ttu-id="484d2-104">Ez a dokumentum hogyan egy bővítmény toohello dokumentum tooIntegrate Engagement IOS: hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái mélysége adatait a biztosít.</span><span class="sxs-lookup"><span data-stu-id="484d2-104">This document is an add-on toohello document How tooIntegrate Engagement on iOS: it provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="484d2-105">Ne feledje, hogy ha munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás csak szeretné Engagement tooreport majd hello legegyszerűbb módja van toomake az egyéni `UIViewController` örökölt objektumok megfelelő hello `EngagementViewController` osztály .</span><span class="sxs-lookup"><span data-stu-id="484d2-105">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your custom `UIViewController` objects inherit from hello corresponding `EngagementViewController` class.</span></span>

<span data-ttu-id="484d2-106">Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementViewController` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.</span><span class="sxs-lookup"><span data-stu-id="484d2-106">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementViewController` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="484d2-107">hello Engagement API által biztosított hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="484d2-107">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="484d2-108">Ez az osztály példánya lekérhetők hívó hello `[EngagementAgent shared]` statikus metódus (vegye figyelembe, hogy hello `EngagementAgent` objektumot adott vissza az Egypéldányos).</span><span class="sxs-lookup"><span data-stu-id="484d2-108">An instance of this class can be retrieved by calling hello `[EngagementAgent shared]` static method (note that hello `EngagementAgent` object returned is a singleton).</span></span>

<span data-ttu-id="484d2-109">Mielőtt API-k hívja, hello `EngagementAgent` hello metódus meghívásával objektumot inicializálni kell`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span><span class="sxs-lookup"><span data-stu-id="484d2-109">Before any API calls, hello `EngagementAgent` object must be initialized by calling hello method `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="484d2-110">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="484d2-110">Engagement concepts</span></span>
<span data-ttu-id="484d2-111">hello következő részekből pontosítsa közös hello [Mobile Engagement fogalmait](mobile-engagement-concepts.md) hello iOS platformra.</span><span class="sxs-lookup"><span data-stu-id="484d2-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for hello iOS platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="484d2-112">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="484d2-112">`Session` and `Activity`</span></span>
<span data-ttu-id="484d2-113">Egy *tevékenység* általában egy képernyő hello alkalmazás, amely toosay hello társított *tevékenység* kezdődik, amikor hello képernyő jelenik meg, és leállítja az üdvözlő képernyőt bezárásakor: Ez a hello eset, amikor hello Engagement SDK integrálva van a hello segítségével `EngagementViewController` osztályok.</span><span class="sxs-lookup"><span data-stu-id="484d2-113">An *activity* is usually associated with one screen of hello application, that is toosay hello *activity* starts when hello screen is displayed and stops when hello screen is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementViewController` classes.</span></span>

<span data-ttu-id="484d2-114">De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="484d2-114">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="484d2-115">Így toosplit egy adott képernyő több sub részek tooget kapcsolatos további részletekért hello használata ezen a képernyőn (például milyen gyakran tooknown és mennyi ideig párbeszédpanelek használt ezen a képernyőn belül).</span><span class="sxs-lookup"><span data-stu-id="484d2-115">This allows toosplit a given screen in several sub parts tooget more details about hello usage of this screen (for example tooknown how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="484d2-116">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="484d2-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="484d2-117">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="484d2-117">User starts a new Activity</span></span>
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

<span data-ttu-id="484d2-118">Toocall kell `startActivity()` minden alkalommal hello felhasználói tevékenység változik.</span><span class="sxs-lookup"><span data-stu-id="484d2-118">You need toocall `startActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="484d2-119">hello első hívás toothis függvény új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="484d2-119">hello first call toothis function starts a new user session.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="484d2-120">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="484d2-120">User ends his current Activity</span></span>
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> <span data-ttu-id="484d2-121">Meg kell **soha** hívja meg a függvényt önállóan, kivéve, ha azt szeretné, hogy az alkalmazás több munkamenet toosplit használatát: toothis függvény volna end hívása hello azonnal, a jelenlegi munkamenet, így a későbbi hívása túl`startActivity()`volna indítson egy újat.</span><span class="sxs-lookup"><span data-stu-id="484d2-121">You should **NEVER** call this function by yourself, except if you want toosplit one use of your application into several sessions: a call toothis function would end hello current session immediately, so, a subsequent call too`startActivity()` would start a new session.</span></span> <span data-ttu-id="484d2-122">Ez a funkció automatikusan hívja hello SDK az alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="484d2-122">This function is automatically called by hello SDK when your application is closed.</span></span>
> 
> 

## <a name="reporting-events"></a><span data-ttu-id="484d2-123">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="484d2-123">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="484d2-124">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="484d2-124">Session events</span></span>
<span data-ttu-id="484d2-125">Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.</span><span class="sxs-lookup"><span data-stu-id="484d2-125">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

<span data-ttu-id="484d2-126">**További adatok nélkül. példa:**</span><span class="sxs-lookup"><span data-stu-id="484d2-126">**Example without extra data:**</span></span>

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

<span data-ttu-id="484d2-127">**Példa a további adatokat:**</span><span class="sxs-lookup"><span data-stu-id="484d2-127">**Example with extra data:**</span></span>

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

### <a name="standalone-events"></a><span data-ttu-id="484d2-128">Önálló események</span><span class="sxs-lookup"><span data-stu-id="484d2-128">Standalone events</span></span>
<span data-ttu-id="484d2-129">Ellentétes toosession események, különálló esemény egy munkamenet környezetében hello kívül is használható.</span><span class="sxs-lookup"><span data-stu-id="484d2-129">Contrary toosession events, standalone events can be used outside of hello context of a session.</span></span>

<span data-ttu-id="484d2-130">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-130">**Example:**</span></span>

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a><span data-ttu-id="484d2-131">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="484d2-131">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="484d2-132">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="484d2-132">Session errors</span></span>
<span data-ttu-id="484d2-133">Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="484d2-133">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

<span data-ttu-id="484d2-134">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-134">**Example:**</span></span>

    /** hello user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* hello user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="484d2-135">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="484d2-135">Standalone errors</span></span>
<span data-ttu-id="484d2-136">Ellentétes toosession hibák, önálló hibák kívül hello egy munkamenet környezetében használható.</span><span class="sxs-lookup"><span data-stu-id="484d2-136">Contrary toosession errors, standalone errors can be used outside of hello context of a session.</span></span>

<span data-ttu-id="484d2-137">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-137">**Example:**</span></span>

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a><span data-ttu-id="484d2-138">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="484d2-138">Reporting Jobs</span></span>
<span data-ttu-id="484d2-139">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-139">**Example:**</span></span>

<span data-ttu-id="484d2-140">Tegyük fel, hogy a bejelentkezési folyamat tooreport hello időtartama:</span><span class="sxs-lookup"><span data-stu-id="484d2-140">Suppose you want tooreport hello duration of your login process:</span></span>

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

### <a name="report-errors-during-a-job"></a><span data-ttu-id="484d2-141">A feladat során hibák jelentése</span><span class="sxs-lookup"><span data-stu-id="484d2-141">Report Errors during a Job</span></span>
<span data-ttu-id="484d2-142">Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="484d2-142">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="484d2-143">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-143">**Example:**</span></span>

<span data-ttu-id="484d2-144">Tegyük fel, hogy tooreport hiba történt a bejelentkezési folyamat során:</span><span class="sxs-lookup"><span data-stu-id="484d2-144">Suppose you want tooreport an error during your login process:</span></span>

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try toosign in */
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

### <a name="events-during-a-job"></a><span data-ttu-id="484d2-145">A feladat során események</span><span class="sxs-lookup"><span data-stu-id="484d2-145">Events during a job</span></span>
<span data-ttu-id="484d2-146">Események lehet feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="484d2-146">Events can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="484d2-147">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-147">**Example:**</span></span>

<span data-ttu-id="484d2-148">Tegyük fel, közösségi hálózata, és egy feladat tooreport hello teljes ideje mely hello felhasználói pedig csatlakoztatott toohello server használjuk.</span><span class="sxs-lookup"><span data-stu-id="484d2-148">Suppose we have a social network, and we use a job tooreport hello total time during which hello user is connected toohello server.</span></span> <span data-ttu-id="484d2-149">hello felhasználói fogadhat üzeneteket az ismerősök, ez az esemény feladat.</span><span class="sxs-lookup"><span data-stu-id="484d2-149">hello user can receive messages from his friends, this is a job event.</span></span>

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

## <a name="extra-parameters"></a><span data-ttu-id="484d2-150">További paraméterek</span><span class="sxs-lookup"><span data-stu-id="484d2-150">Extra parameters</span></span>
<span data-ttu-id="484d2-151">Lehet, hogy az adatokat tetszőleges csatolt tooevents, hibákat, tevékenységeket és feladatokat.</span><span class="sxs-lookup"><span data-stu-id="484d2-151">Arbitrary data can be attached tooevents, errors, activities and jobs.</span></span>

<span data-ttu-id="484d2-152">Ezek az adatok szervezhetők, iOS tartozó NSDictionary osztályt használja.</span><span class="sxs-lookup"><span data-stu-id="484d2-152">This data can be structured, it uses iOS's NSDictionary class.</span></span>

<span data-ttu-id="484d2-153">Vegye figyelembe, hogy a kiegészítő funkciók tartalmazhatnak `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` vagy más `NSDictionary` példányok.</span><span class="sxs-lookup"><span data-stu-id="484d2-153">Note that extras can contain `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` or other `NSDictionary` instances.</span></span>

> [!NOTE]
> <span data-ttu-id="484d2-154">hello kiegészítő paraméterrel szerializált a JSON-ban.</span><span class="sxs-lookup"><span data-stu-id="484d2-154">hello extra parameter is serialized in JSON.</span></span> <span data-ttu-id="484d2-155">Ha azt szeretné, hogy toopass különböző objektumok, mint hello fent leírtaktól, meg kell valósítani hello metódus az osztály a következő:</span><span class="sxs-lookup"><span data-stu-id="484d2-155">If you want toopass different objects than hello ones described above, you must implement hello following method in your class:</span></span>
> 
> <span data-ttu-id="484d2-156">-(NSString*) JSONRepresentation;</span><span class="sxs-lookup"><span data-stu-id="484d2-156">-(NSString*)JSONRepresentation;</span></span>
> 
> <span data-ttu-id="484d2-157">hello metódus az objektum JSON-ábrázolását kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="484d2-157">hello method should return a JSON representation of your object.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="484d2-158">Példa</span><span class="sxs-lookup"><span data-stu-id="484d2-158">Example</span></span>
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a><span data-ttu-id="484d2-159">Korlátok</span><span class="sxs-lookup"><span data-stu-id="484d2-159">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="484d2-160">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="484d2-160">Keys</span></span>
<span data-ttu-id="484d2-161">Minden kulcs hello `NSDictionary` meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="484d2-161">Each key in hello `NSDictionary` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="484d2-162">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="484d2-162">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="484d2-163">Méret</span><span class="sxs-lookup"><span data-stu-id="484d2-163">Size</span></span>
<span data-ttu-id="484d2-164">Kiegészítő funkciók korlátozva túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement ügynök).</span><span class="sxs-lookup"><span data-stu-id="484d2-164">Extras are limited too**1024** characters per call (once encoded in JSON by hello Engagement agent).</span></span>

<span data-ttu-id="484d2-165">Hello az előző példában hello toohello server elküldött JSON 58 karakter hosszú:</span><span class="sxs-lookup"><span data-stu-id="484d2-165">In hello previous example, hello JSON sent toohello server is 58 characters long:</span></span>

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="484d2-166">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="484d2-166">Reporting Application Information</span></span>
<span data-ttu-id="484d2-167">Manuálisan jelentheti a nyomkövetési adatokat (vagy más alkalmazás egyedi információt) hello `sendAppInfo:` függvény.</span><span class="sxs-lookup"><span data-stu-id="484d2-167">You can manually report tracking information (or any other application specific information) using hello `sendAppInfo:` function.</span></span>

<span data-ttu-id="484d2-168">Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="484d2-168">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="484d2-169">Például a esemény kiegészítő funkciók, hello `NSDictionary` osztály használt tooabstract alkalmazással kapcsolatos információk, vegye figyelembe, hogy tömbállandó vagy alárendelt szótárak minősül, egyszerű karakterlánc (JSON-szerializálás).</span><span class="sxs-lookup"><span data-stu-id="484d2-169">Like event extras, hello `NSDictionary` class is used tooabstract application information, note that arrays or sub-dictionaries will be treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="484d2-170">**Példa**</span><span class="sxs-lookup"><span data-stu-id="484d2-170">**Example:**</span></span>

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a><span data-ttu-id="484d2-171">Korlátok</span><span class="sxs-lookup"><span data-stu-id="484d2-171">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="484d2-172">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="484d2-172">Keys</span></span>
<span data-ttu-id="484d2-173">Minden kulcs hello `NSDictionary` meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="484d2-173">Each key in hello `NSDictionary` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="484d2-174">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="484d2-174">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="484d2-175">Méret</span><span class="sxs-lookup"><span data-stu-id="484d2-175">Size</span></span>
<span data-ttu-id="484d2-176">Alkalmazásadatok esetén egyre korlátozódik túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement ügynök).</span><span class="sxs-lookup"><span data-stu-id="484d2-176">Application information are limited too**1024** characters per call (once encoded in JSON by hello Engagement agent).</span></span>

<span data-ttu-id="484d2-177">Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:</span><span class="sxs-lookup"><span data-stu-id="484d2-177">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
