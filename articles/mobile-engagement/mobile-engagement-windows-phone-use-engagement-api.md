---
title: aaaHow tooUse hello Engagement API a Windows Phone Silverlight
description: Hogyan tooUse hello Engagement API a Windows Phone Silverlight
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ae2ba2e8-f75b-4dee-a164-a7dd65d35a23
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1e84be95cc910be7f1227b4ae60eb483a1939284
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-phone-silverlight"></a><span data-ttu-id="ad9e5-103">Hogyan tooUse hello Engagement API a Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="ad9e5-103">How tooUse hello Engagement API on Windows Phone Silverlight</span></span>
<span data-ttu-id="ad9e5-104">Ez a dokumentum egy bővítmény toohello dokumentum [hogyan a Windows Phone Silverlight-alkalmazást a Mobile Engagement toointegrate](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="ad9e5-104">This document is an add-on toohello document [How toointegrate Mobile Engagement in your Windows Phone Silverlight app](mobile-engagement-windows-phone-integrate-engagement.md).</span></span> <span data-ttu-id="ad9e5-105">A mélység adatait hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái biztosít.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-105">It provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="ad9e5-106">Ha munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás csak szeretné Engagement tooreport, akkor a legegyszerűbb hello eltérő módon minden toomake a `PhoneApplicationPage` alosztályokat örökölhet hello `EngagementPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-106">If you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `PhoneApplicationPage` sub-classes inherit from hello `EngagementPage` class.</span></span>

<span data-ttu-id="ad9e5-107">Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementPage` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-107">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementPage` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="ad9e5-108">hello Engagement API által biztosított hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-108">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="ad9e5-109">Toothose módszerek keresztül érheti el `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-109">You can access toothose methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="ad9e5-110">Akkor is, ha hello ügynök modul nem lett inicializálva, minden hívás toohello API késleltetve, és végrehajtja újra elérhető hello ügynök esetén.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-110">Even if hello agent module has not been initialized, each call toohello API is deferred, and will be executed again when hello agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="ad9e5-111">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="ad9e5-111">Engagement concepts</span></span>
<span data-ttu-id="ad9e5-112">a következő részek hello hello Mobile Engagement fogalmait hello Windows Phone platform szűkítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-112">hello following parts refine hello Mobile Engagement Concepts for hello Windows Phone platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="ad9e5-113">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="ad9e5-113">`Session` and `Activity`</span></span>
<span data-ttu-id="ad9e5-114">Egy *tevékenység* általában egy oldal, amely toosay hello hello alkalmazás társított *tevékenység* kezdődik, amikor hello oldal jelenik meg, és hello lap bezárásakor leáll: hello eset, amikor hello Hello segítségével integrált engagement SDK `EngagementPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-114">An *activity* is usually associated with one page of hello application, that is toosay hello *activity* starts when hello page is displayed and stops when hello page is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementPage` class.</span></span>

<span data-ttu-id="ad9e5-115">De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-115">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="ad9e5-116">Így toosplit egy adott oldal több sub részek tooget kapcsolatos további részletekért hello használata ezen a lapon (például milyen gyakran tooknown és mennyi ideig párbeszédpanelek használt ezen a lapon belül).</span><span class="sxs-lookup"><span data-stu-id="ad9e5-116">This allows toosplit a given page in several sub parts tooget more details about hello usage of this page (for example tooknown how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="ad9e5-117">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="ad9e5-117">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="ad9e5-118">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="ad9e5-118">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-119">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-119">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-120">Toocall kell `StartActivity()` minden alkalommal hello felhasználói tevékenység változik.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-120">You need toocall `StartActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="ad9e5-121">hello első hívás toothis függvény új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-121">hello first call toothis function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ad9e5-122">hello SDK automatikusan metódushívás hello EndActivity hello alkalmazás bezárásakor.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-122">hello SDK automatically call hello EndActivity method when hello application is closed.</span></span> <span data-ttu-id="ad9e5-123">Ebből kifolyólag javasoljuk toocall hello StartActivity metódus amikor hello felhasználó módosítása és hello EndActivity metódus a metódus hívása óta kényszeríti hello aktuális munkamenet toobe tooNEVER hívás hello tevékenység befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-123">Thus, it is HIGHLY recommended toocall hello StartActivity method whenever hello activity of hello user change, and tooNEVER call hello EndActivity method, since calling this method forces hello current session toobe ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="ad9e5-124">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-124">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="ad9e5-125">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="ad9e5-125">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-126">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-126">Reference</span></span>
            void EndActivity()

<span data-ttu-id="ad9e5-127">Toocall kell `EndActivity()` legalább egyszer amikor hello felhasználó befejezi az utolsó tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-127">You need toocall `EndActivity()` at least once when hello user finishes his last activity.</span></span> <span data-ttu-id="ad9e5-128">Ebben értesíti az Engagement SDK-t, hogy a hello felhasználó jelenleg inaktív, és, hogy a felhasználói munkamenet hello toobe kell lezárt egyszer hello munkamenet időkorlátja lejár hello (ha meghívja a `StartActivity()` hello munkamenet időkorlátja lejár, mielőtt hello munkamenet egyszerűen továbbra is).</span><span class="sxs-lookup"><span data-stu-id="ad9e5-128">This informs hello Engagement SDK that hello user is currently idle, and that hello user session need toobe closed once hello session timeout will expire (if you call `StartActivity()` before hello session timeout expires, hello session is simply continued).</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-129">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-129">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="ad9e5-130">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="ad9e5-130">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="ad9e5-131">Feladat indítása</span><span class="sxs-lookup"><span data-stu-id="ad9e5-131">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-132">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-132">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-133">Hello feladat tootrack: bizonyos feladatokat is használhat egy meghatározott időtartamra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-133">You can use hello job tootrack certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-134">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-134">Example</span></span>
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="ad9e5-135">Egy feladat vége</span><span class="sxs-lookup"><span data-stu-id="ad9e5-135">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-136">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-136">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="ad9e5-137">Amint egy tevékenység követi nyomon a feladat le lett állítva, meg kell hívnia hello EndJob metódust a feladat úgy, hogy megadja a hello feladat neve.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-137">As soon as a task tracked by a job has been terminated, you should call hello EndJob method for this job, by supplying hello job name.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-138">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-138">Example</span></span>
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="ad9e5-139">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="ad9e5-139">Reporting Events</span></span>
<span data-ttu-id="ad9e5-140">A következő három típusú eseményeket:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-140">There is three types of events :</span></span>

* <span data-ttu-id="ad9e5-141">Önálló események</span><span class="sxs-lookup"><span data-stu-id="ad9e5-141">Standalone events</span></span>
* <span data-ttu-id="ad9e5-142">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="ad9e5-142">Session events</span></span>
* <span data-ttu-id="ad9e5-143">Feladat-események</span><span class="sxs-lookup"><span data-stu-id="ad9e5-143">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="ad9e5-144">Önálló események</span><span class="sxs-lookup"><span data-stu-id="ad9e5-144">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-145">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-145">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-146">Önálló események is előfordulhatnak kívül hello egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-146">Standalone events can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-147">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-147">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="ad9e5-148">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="ad9e5-148">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-149">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-149">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-150">Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-150">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-151">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-151">Example</span></span>
<span data-ttu-id="ad9e5-152">**Adatok: nélkül**</span><span class="sxs-lookup"><span data-stu-id="ad9e5-152">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="ad9e5-153">**Adatokkal:**</span><span class="sxs-lookup"><span data-stu-id="ad9e5-153">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="ad9e5-154">Feladat-események</span><span class="sxs-lookup"><span data-stu-id="ad9e5-154">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-155">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-155">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-156">Feladat eseményeket általában használt tooreport hello műveletek során a feladat felhasználó által végrehajtott is.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-156">Job events are usually used tooreport hello actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-157">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-157">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="ad9e5-158">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="ad9e5-158">Reporting Errors</span></span>
<span data-ttu-id="ad9e5-159">A következő három típusú hibák:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-159">There is three types of errors :</span></span>

* <span data-ttu-id="ad9e5-160">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="ad9e5-160">Standalone errors</span></span>
* <span data-ttu-id="ad9e5-161">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="ad9e5-161">Session errors</span></span>
* <span data-ttu-id="ad9e5-162">Hibák: feladathiba</span><span class="sxs-lookup"><span data-stu-id="ad9e5-162">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="ad9e5-163">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="ad9e5-163">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-164">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-164">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-165">Ellenkező toosession hibák önálló hibák adódhatnak kívül hello egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-165">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-166">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-166">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="ad9e5-167">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="ad9e5-167">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-168">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-168">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-169">Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-169">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-170">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-170">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="ad9e5-171">Hibák: feladathiba</span><span class="sxs-lookup"><span data-stu-id="ad9e5-171">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-172">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-172">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="ad9e5-173">Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-173">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-174">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-174">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="ad9e5-175">Jelentéskészítési összeomlik</span><span class="sxs-lookup"><span data-stu-id="ad9e5-175">Reporting Crashes</span></span>
<span data-ttu-id="ad9e5-176">hello ügynök biztosít két módszer toodeal összeomlik.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-176">hello agent provides two methods toodeal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="ad9e5-177">Kivétel küldése</span><span class="sxs-lookup"><span data-stu-id="ad9e5-177">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-178">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-178">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="ad9e5-179">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-179">Example</span></span>
<span data-ttu-id="ad9e5-180">Bármikor kivétel meghívásával küldheti el:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-180">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="ad9e5-181">Is használhat egy nem kötelező paraméter tooterminate hello engagement munkamenet hello ugyanannyi időt vesz igénybe, mint hello összeomlási küldésekor.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-181">You can also use an optional parameter tooterminate hello engagement session at hello same time than sending hello crash.</span></span> <span data-ttu-id="ad9e5-182">Ebben az esetben hívható toodo:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-182">toodo so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="ad9e5-183">Ha így tesz, a hello munkamenet és feladatok csak hello összeomlási elküldése után megszűnik.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-183">If you do that, hello session and jobs will be closed just after sending hello crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="ad9e5-184">Nem kezelt kivétel küldése</span><span class="sxs-lookup"><span data-stu-id="ad9e5-184">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="ad9e5-185">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-185">Reference</span></span>
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

<span data-ttu-id="ad9e5-186">Engagement is biztosít a metódus nem kezelt toosend kivételek.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-186">Engagement also provides a method toosend unhandled exceptions.</span></span> <span data-ttu-id="ad9e5-187">Ez különösen akkor hasznos, ha hello silverlight UnhandledException eseménykezelő használni.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-187">This is especially useful when used inside hello silverlight UnhandledException event handler.</span></span>

<span data-ttu-id="ad9e5-188">Ezt a módszert fog **mindig** hello engagement munkamenet és feladatok meghívott után leáll.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-188">This method will **ALWAYS** terminate hello engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="ad9e5-189">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-189">Example</span></span>
<span data-ttu-id="ad9e5-190">Használat tooimplement saját UnhandledException kezelő (különösen akkor, ha a jelentéskészítési funkció bekapcsolása hello automatikus összeomlási le van tiltva).</span><span class="sxs-lookup"><span data-stu-id="ad9e5-190">You can use it tooimplement your own UnhandledException handler (especially if you have disabled hello automatic crash reporting feature of Engagement).</span></span> <span data-ttu-id="ad9e5-191">Például a hello `Application_UnhandledException` hello metódusában `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-191">For example, in hello `Application_UnhandledException` method of hello `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a><span data-ttu-id="ad9e5-192">OnActivated</span><span class="sxs-lookup"><span data-stu-id="ad9e5-192">OnActivated</span></span>
### <a name="reference"></a><span data-ttu-id="ad9e5-193">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-193">Reference</span></span>
            void OnActivated(ActivatedEventArgs e)

<span data-ttu-id="ad9e5-194">Amikor hello felhasználók megnyitják továbbítás, egy alkalmazást, másik lapra hello inaktív egy esemény jelenik meg, miután hello operációs rendszer megpróbál tooput hello alkalmazás alvó állapotba.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-194">When hello user navigates forward, away from an application, after hello Deactivated event is raised, hello operating system will attempt tooput hello application into a dormant state.</span></span> <span data-ttu-id="ad9e5-195">Ezt követően a hello alkalmazás kijelölése törlésre.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-195">Then, hello application is Tombstoning.</span></span> <span data-ttu-id="ad9e5-196">A folyamat a rendszer megszakítja a kérelmet, de hello alkalmazás és az egyes lapok hello hello alkalmazáson belül hello állapotával kapcsolatos néhány adat megőrződik.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-196">In this process an application is terminated but some data about hello state of hello application and hello individual pages within hello application is preserved.</span></span>

<span data-ttu-id="ad9e5-197">Tooinsert rendelkezik `EngagementAgent.Instance.OnActivated(e)` a hello `Application_Activated` hello App.xaml.cs fájlt tooreset hello Engagement ügynök amikor hello alkalmazás lett a törlésre kijelölt metódust.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-197">You have tooinsert `EngagementAgent.Instance.OnActivated(e)` in hello `Application_Activated` method from hello App.xaml.cs file tooreset hello Engagement Agent when hello application has been Tombstoned.</span></span>

### <a name="example"></a><span data-ttu-id="ad9e5-198">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-198">Example</span></span>
            // Inside your App.xaml.cs file

            // Code tooexecute when hello application is activated (brought tooforeground)
            // This code will not execute when hello application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a><span data-ttu-id="ad9e5-199">Eszközazonosító</span><span class="sxs-lookup"><span data-stu-id="ad9e5-199">Device Id</span></span>
            String GetDeviceId()

<span data-ttu-id="ad9e5-200">A metódus hívása kaphat hello engagement-eszközazonosító.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-200">You can get hello engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="ad9e5-201">Kiegészítő funkciók paraméterek</span><span class="sxs-lookup"><span data-stu-id="ad9e5-201">Extras parameters</span></span>
<span data-ttu-id="ad9e5-202">Lehet, hogy az adatokat tetszőleges csatolt tooan esemény, a hiba, a tevékenység vagy a feladat.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-202">Arbitrary data can be attached tooan event, an error, an activity or a job.</span></span> <span data-ttu-id="ad9e5-203">Ezen adatok segítségével dictionary szervezhetők.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-203">These data can be structured using a dictionary.</span></span> <span data-ttu-id="ad9e5-204">Kulcsok és értékek bármilyen típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-204">Keys and values can be of any type.</span></span>

<span data-ttu-id="ad9e5-205">Kiegészítő funkciók adatok szerializálva vannak, így ha azt szeretné, tooinsert a saját típusát, a kiegészítő funkciók kell tooadd egy adategyezmény ehhez a típushoz.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-205">Extras data are serialized so if you want tooinsert your own type in extras you have tooadd a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="ad9e5-206">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-206">Example</span></span>
<span data-ttu-id="ad9e5-207">Azt a hozzon létre egy új osztályt "Személy".</span><span class="sxs-lookup"><span data-stu-id="ad9e5-207">We create a new class "Person".</span></span>

            using System.Runtime.Serialization;

            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

<span data-ttu-id="ad9e5-208">Ezt követően adunk hozzá egy `Person` példány tooan extra.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-208">Then, we will add a `Person` instance tooan extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="ad9e5-209">Ha más típusú objektumokat, győződjön meg arról a ToString() módszer megvalósított tooreturn emberi olvasható karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-209">If you put other types of objects, make sure their ToString() method is implemented tooreturn a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="ad9e5-210">Korlátok</span><span class="sxs-lookup"><span data-stu-id="ad9e5-210">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="ad9e5-211">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="ad9e5-211">Keys</span></span>
<span data-ttu-id="ad9e5-212">Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-212">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="ad9e5-213">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="ad9e5-213">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="ad9e5-214">Méret</span><span class="sxs-lookup"><span data-stu-id="ad9e5-214">Size</span></span>
<span data-ttu-id="ad9e5-215">Kiegészítő funkciók korlátozva túl**1024** hívás karakteres.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-215">Extras are limited too**1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="ad9e5-216">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="ad9e5-216">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="ad9e5-217">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad9e5-217">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="ad9e5-218">Követés hello SendAppInfo() függvény használatával információkat (vagy egyéb alkalmazás konkrét információkat) kézzel jelentést.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-218">You can manually report tracking information (or any other application specific information) using hello SendAppInfo() function.</span></span>

<span data-ttu-id="ad9e5-219">Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-219">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="ad9e5-220">Esemény kiegészítő funkciók, például egy szótár használata\<objektum, objektum\> tooattach informations.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-220">Like event extras, use a Dictionary\<object, object\> tooattach informations.</span></span>

### <a name="example"></a><span data-ttu-id="ad9e5-221">Példa</span><span class="sxs-lookup"><span data-stu-id="ad9e5-221">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="ad9e5-222">Korlátok</span><span class="sxs-lookup"><span data-stu-id="ad9e5-222">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="ad9e5-223">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="ad9e5-223">Keys</span></span>
<span data-ttu-id="ad9e5-224">Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-224">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="ad9e5-225">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="ad9e5-225">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="ad9e5-226">Méret</span><span class="sxs-lookup"><span data-stu-id="ad9e5-226">Size</span></span>
<span data-ttu-id="ad9e5-227">Alkalmazásadatok esetén egyre korlátozódik túl**1024** hívás karakteres.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-227">Application information are limited too**1024** characters per call.</span></span>

<span data-ttu-id="ad9e5-228">Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-228">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a><span data-ttu-id="ad9e5-229">Naplózás</span><span class="sxs-lookup"><span data-stu-id="ad9e5-229">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="ad9e5-230">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ad9e5-230">Enable Logging</span></span>
<span data-ttu-id="ad9e5-231">hello SDK konfigurált tooproduce vizsgálati naplók hello IDE konzolon lehet.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-231">hello SDK can be configured tooproduce test logs in hello IDE console.</span></span>
<span data-ttu-id="ad9e5-232">Ezek a naplók alapértelmezés szerint nincsenek aktiválva.</span><span class="sxs-lookup"><span data-stu-id="ad9e5-232">These logs are not activated by default.</span></span> <span data-ttu-id="ad9e5-233">toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="ad9e5-233">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
