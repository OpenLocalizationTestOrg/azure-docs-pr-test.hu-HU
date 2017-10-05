---
title: "A bevonási API használata a Windows Phone Silverlight"
description: "A bevonási API használata a Windows Phone Silverlight"
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
ms.openlocfilehash: ec8b6c13ea052c8063dfde4321cdd286ab6cb817
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a><span data-ttu-id="8e988-103">A bevonási API használata a Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="8e988-103">How to Use the Engagement API on Windows Phone Silverlight</span></span>
<span data-ttu-id="8e988-104">Ez a dokumentum az bővítménye a dokumentum [a Windows Phone Silverlight-alkalmazást a Mobile Engagement integrálása](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="8e988-104">This document is an add-on to the document [How to integrate Mobile Engagement in your Windows Phone Silverlight app](mobile-engagement-windows-phone-integrate-engagement.md).</span></span> <span data-ttu-id="8e988-105">A mélység részletei jelentés az alkalmazás statisztikái az Engagement API használatával biztosít.</span><span class="sxs-lookup"><span data-stu-id="8e988-105">It provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="8e988-106">Ha csak a bevonási jelenti a munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás szeretné, akkor a legegyszerűbb módja, hogy minden a `PhoneApplicationPage` alosztályokat örökli a `EngagementPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="8e988-106">If you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `PhoneApplicationPage` sub-classes inherit from the `EngagementPage` class.</span></span>

<span data-ttu-id="8e988-107">Ha szeretne többet, például ha adott eseményeket, hibákat és feladatok, jelentenie, vagy ha a jelentés az alkalmazás tevékenységei eltérő módon, mint az egyik valósult meg, hogy a `EngagementPage` osztályokat, akkor használja az Engagement API szükséges.</span><span class="sxs-lookup"><span data-stu-id="8e988-107">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementPage` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="8e988-108">A bevonási API által biztosított a `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="8e988-108">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="8e988-109">Azokat a módszereket használatával tudja elérni `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="8e988-109">You can access to those methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="8e988-110">Akkor is, ha az ügynök modul inicializálása nem történt meg, az API-t minden egyes hívásakor késleltetve, és végrehajtja újra, amikor az ügynök nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="8e988-110">Even if the agent module has not been initialized, each call to the API is deferred, and will be executed again when the agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="8e988-111">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="8e988-111">Engagement concepts</span></span>
<span data-ttu-id="8e988-112">A következő részek a Mobile Engagement fogalmait a Windows Phone platform szűkítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8e988-112">The following parts refine the Mobile Engagement Concepts for the Windows Phone platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="8e988-113">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="8e988-113">`Session` and `Activity`</span></span>
<span data-ttu-id="8e988-114">Egy *tevékenység* tartozik általában az alkalmazás egy oldalnyi, azaz a *tevékenység* kezdődik, amikor az oldal jelenik meg, és leállítja a lap bezárásakor: Ez a helyzet, amikor az Engagement SDK használatával integrálva van a `EngagementPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="8e988-114">An *activity* is usually associated with one page of the application, that is to say the *activity* starts when the page is displayed and stops when the page is closed: this is the case when the Engagement SDK is integrated by using the `EngagementPage` class.</span></span>

<span data-ttu-id="8e988-115">De *tevékenységek* is szabályozhatja manuálisan az Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="8e988-115">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="8e988-116">Ez lehetővé teszi több sub részből további részletes információkat az ezen a lapon (például hogy milyen gyakran ismert, és mennyi ideig párbeszédpanelek ezen a lapon belül használt) használatát egy adott lap felosztása.</span><span class="sxs-lookup"><span data-stu-id="8e988-116">This allows to split a given page in several sub parts to get more details about the usage of this page (for example to known how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="8e988-117">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="8e988-117">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="8e988-118">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="8e988-118">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-119">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-119">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-120">Meg kell hívnia `StartActivity()` minden egyes alkalommal, amikor a felhasználói tevékenység módosul.</span><span class="sxs-lookup"><span data-stu-id="8e988-120">You need to call `StartActivity()` each time the user activity changes.</span></span> <span data-ttu-id="8e988-121">Ez a függvény az első hívás új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="8e988-121">The first call to this function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e988-122">Az SDK automatikusan a EndActivity metódust hívja, ha az alkalmazás le van zárva.</span><span class="sxs-lookup"><span data-stu-id="8e988-122">The SDK automatically call the EndActivity method when the application is closed.</span></span> <span data-ttu-id="8e988-123">Ebből kifolyólag javasoljuk a StartActivity metódus meghívására, amikor a tevékenység, a felhasználó módosítása, és soha EndActivity metódus hívható, mert a metódus hívása az aktuális munkamenet be kell fejezni kényszeríti.</span><span class="sxs-lookup"><span data-stu-id="8e988-123">Thus, it is HIGHLY recommended to call the StartActivity method whenever the activity of the user change, and to NEVER call the EndActivity method, since calling this method forces the current session to be ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="8e988-124">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-124">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="8e988-125">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="8e988-125">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-126">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-126">Reference</span></span>
            void EndActivity()

<span data-ttu-id="8e988-127">Meg kell hívnia `EndActivity()` legalább egyszer amikor a felhasználó befejezi az utolsó tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8e988-127">You need to call `EndActivity()` at least once when the user finishes his last activity.</span></span> <span data-ttu-id="8e988-128">Ebben értesíti az Engagement SDK-t, hogy a felhasználó jelenleg inaktív, valamint, hogy a felhasználói munkamenet kell egyszer lezárni a munkamenet időkorlátja lejár (ha meghívja a `StartActivity()` előtt a munkamenet időkorlátja lejár, a munkamenet egyszerűen továbbra is).</span><span class="sxs-lookup"><span data-stu-id="8e988-128">This informs the Engagement SDK that the user is currently idle, and that the user session need to be closed once the session timeout will expire (if you call `StartActivity()` before the session timeout expires, the session is simply continued).</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-129">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-129">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="8e988-130">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="8e988-130">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="8e988-131">Feladat indítása</span><span class="sxs-lookup"><span data-stu-id="8e988-131">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-132">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-132">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-133">A feladatot használhatja egy meghatározott időtartamra vonatkozóan: bizonyos feladatok követése céljából.</span><span class="sxs-lookup"><span data-stu-id="8e988-133">You can use the job to track certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-134">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-134">Example</span></span>
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="8e988-135">Egy feladat vége</span><span class="sxs-lookup"><span data-stu-id="8e988-135">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-136">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-136">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="8e988-137">Amint egy tevékenység követi nyomon a feladat le lett állítva, meg kell a EndJob metódus hívható meg a feladat úgy, hogy megadja a feladat neve.</span><span class="sxs-lookup"><span data-stu-id="8e988-137">As soon as a task tracked by a job has been terminated, you should call the EndJob method for this job, by supplying the job name.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-138">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-138">Example</span></span>
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="8e988-139">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="8e988-139">Reporting Events</span></span>
<span data-ttu-id="8e988-140">A következő három típusú eseményeket:</span><span class="sxs-lookup"><span data-stu-id="8e988-140">There is three types of events :</span></span>

* <span data-ttu-id="8e988-141">Önálló események</span><span class="sxs-lookup"><span data-stu-id="8e988-141">Standalone events</span></span>
* <span data-ttu-id="8e988-142">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="8e988-142">Session events</span></span>
* <span data-ttu-id="8e988-143">Feladat-események</span><span class="sxs-lookup"><span data-stu-id="8e988-143">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="8e988-144">Önálló események</span><span class="sxs-lookup"><span data-stu-id="8e988-144">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-145">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-145">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-146">Önálló események is előfordulhatnak kívül egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="8e988-146">Standalone events can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-147">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-147">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="8e988-148">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="8e988-148">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-149">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-149">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-150">A munkamenet során a felhasználó által végrehajtott műveletek jelentésére általában használhatók munkamenet események.</span><span class="sxs-lookup"><span data-stu-id="8e988-150">Session events are usually used to report the actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-151">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-151">Example</span></span>
<span data-ttu-id="8e988-152">**Adatok: nélkül**</span><span class="sxs-lookup"><span data-stu-id="8e988-152">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="8e988-153">**Adatokkal:**</span><span class="sxs-lookup"><span data-stu-id="8e988-153">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="8e988-154">Feladat-események</span><span class="sxs-lookup"><span data-stu-id="8e988-154">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-155">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-155">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-156">Feladat események általában a felhasználó által a feldolgozás során végrehajtott műveletek jelentésére használt.</span><span class="sxs-lookup"><span data-stu-id="8e988-156">Job events are usually used to report the actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-157">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-157">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="8e988-158">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="8e988-158">Reporting Errors</span></span>
<span data-ttu-id="8e988-159">A következő három típusú hibák:</span><span class="sxs-lookup"><span data-stu-id="8e988-159">There is three types of errors :</span></span>

* <span data-ttu-id="8e988-160">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="8e988-160">Standalone errors</span></span>
* <span data-ttu-id="8e988-161">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="8e988-161">Session errors</span></span>
* <span data-ttu-id="8e988-162">Hibák: feladathiba</span><span class="sxs-lookup"><span data-stu-id="8e988-162">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="8e988-163">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="8e988-163">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-164">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-164">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-165">Munkamenet hibák ellentétesen önálló felléphetnek a kívül egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="8e988-165">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-166">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-166">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="8e988-167">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="8e988-167">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-168">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-168">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-169">Munkamenet a hibák általában használhatók a a munkamenet során a felhasználót érintő hibák jelentését.</span><span class="sxs-lookup"><span data-stu-id="8e988-169">Session errors are usually used to report the errors impacting the user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-170">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-170">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="8e988-171">Hibák: feladathiba</span><span class="sxs-lookup"><span data-stu-id="8e988-171">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-172">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-172">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="8e988-173">Hibák a jelenlegi felhasználói munkamenet való helyett egy futó feladat kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="8e988-173">Errors can be related to a running job instead of being related to the current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-174">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-174">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="8e988-175">Jelentéskészítési összeomlik</span><span class="sxs-lookup"><span data-stu-id="8e988-175">Reporting Crashes</span></span>
<span data-ttu-id="8e988-176">Az ügynök az összeomlások kezelésére két módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="8e988-176">The agent provides two methods to deal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="8e988-177">Kivétel küldése</span><span class="sxs-lookup"><span data-stu-id="8e988-177">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-178">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-178">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="8e988-179">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-179">Example</span></span>
<span data-ttu-id="8e988-180">Bármikor kivétel meghívásával küldheti el:</span><span class="sxs-lookup"><span data-stu-id="8e988-180">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="8e988-181">Egy nem kötelező paraméter segítségével állítsa le a engagement munkamenet, mint a összeomlási egyszerre.</span><span class="sxs-lookup"><span data-stu-id="8e988-181">You can also use an optional parameter to terminate the engagement session at the same time than sending the crash.</span></span> <span data-ttu-id="8e988-182">Ehhez hívja meg:</span><span class="sxs-lookup"><span data-stu-id="8e988-182">To do so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="8e988-183">Ha így tesz, a munkamenet és feladatok csak az összeomlás elküldése után megszűnik.</span><span class="sxs-lookup"><span data-stu-id="8e988-183">If you do that, the session and jobs will be closed just after sending the crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="8e988-184">Nem kezelt kivétel küldése</span><span class="sxs-lookup"><span data-stu-id="8e988-184">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="8e988-185">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-185">Reference</span></span>
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

<span data-ttu-id="8e988-186">Engagement küldése nem kezelt kivételek módszert is biztosít.</span><span class="sxs-lookup"><span data-stu-id="8e988-186">Engagement also provides a method to send unhandled exceptions.</span></span> <span data-ttu-id="8e988-187">Ez különösen akkor hasznos, ha a silverlight UnhandledException eseménykezelő használni.</span><span class="sxs-lookup"><span data-stu-id="8e988-187">This is especially useful when used inside the silverlight UnhandledException event handler.</span></span>

<span data-ttu-id="8e988-188">Ezt a módszert fog **mindig** a bevonási munkamenet és feladatok meghívott után leáll.</span><span class="sxs-lookup"><span data-stu-id="8e988-188">This method will **ALWAYS** terminate the engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="8e988-189">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-189">Example</span></span>
<span data-ttu-id="8e988-190">Használhatja a saját UnhandledException eseménykezelő megvalósítása (különösen akkor, ha le van tiltva az automatikus összeomlási jelentéskészítési funkció bekapcsolása).</span><span class="sxs-lookup"><span data-stu-id="8e988-190">You can use it to implement your own UnhandledException handler (especially if you have disabled the automatic crash reporting feature of Engagement).</span></span> <span data-ttu-id="8e988-191">Például a `Application_UnhandledException` metódusában a `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="8e988-191">For example, in the `Application_UnhandledException` method of the `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a><span data-ttu-id="8e988-192">OnActivated</span><span class="sxs-lookup"><span data-stu-id="8e988-192">OnActivated</span></span>
### <a name="reference"></a><span data-ttu-id="8e988-193">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-193">Reference</span></span>
            void OnActivated(ActivatedEventArgs e)

<span data-ttu-id="8e988-194">Amikor a felhasználók megnyitják előre, egy alkalmazást, másik lapra után az inaktív egy esemény jelenik meg, az operációs rendszer megkísérli az alkalmazás alvó állapotba helyezni.</span><span class="sxs-lookup"><span data-stu-id="8e988-194">When the user navigates forward, away from an application, after the Deactivated event is raised, the operating system will attempt to put the application into a dormant state.</span></span> <span data-ttu-id="8e988-195">Az alkalmazás akkor törlésre.</span><span class="sxs-lookup"><span data-stu-id="8e988-195">Then, the application is Tombstoning.</span></span> <span data-ttu-id="8e988-196">A folyamat a rendszer megszakítja a kérelmet, de az alkalmazás és az alkalmazáson belül az egyes lapokon állapotára vonatkozó néhány adat megőrződik.</span><span class="sxs-lookup"><span data-stu-id="8e988-196">In this process an application is terminated but some data about the state of the application and the individual pages within the application is preserved.</span></span>

<span data-ttu-id="8e988-197">Helyezze be, hogy `EngagementAgent.Instance.OnActivated(e)` a a `Application_Activated` az App.xaml.cs fájlt, az Engagement ügynök alaphelyzetbe állítani, ha az alkalmazás már a törlésre kijelölt metódust.</span><span class="sxs-lookup"><span data-stu-id="8e988-197">You have to insert `EngagementAgent.Instance.OnActivated(e)` in the `Application_Activated` method from the App.xaml.cs file to reset the Engagement Agent when the application has been Tombstoned.</span></span>

### <a name="example"></a><span data-ttu-id="8e988-198">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-198">Example</span></span>
            // Inside your App.xaml.cs file

            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a><span data-ttu-id="8e988-199">Eszközazonosító</span><span class="sxs-lookup"><span data-stu-id="8e988-199">Device Id</span></span>
            String GetDeviceId()

<span data-ttu-id="8e988-200">A metódus hívása kaphat az engagement-eszközazonosító.</span><span class="sxs-lookup"><span data-stu-id="8e988-200">You can get the engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="8e988-201">Kiegészítő funkciók paraméterek</span><span class="sxs-lookup"><span data-stu-id="8e988-201">Extras parameters</span></span>
<span data-ttu-id="8e988-202">Egy esemény, a hiba, a tevékenység vagy a feladat tetszőleges adatok csatolható.</span><span class="sxs-lookup"><span data-stu-id="8e988-202">Arbitrary data can be attached to an event, an error, an activity or a job.</span></span> <span data-ttu-id="8e988-203">Ezen adatok segítségével dictionary szervezhetők.</span><span class="sxs-lookup"><span data-stu-id="8e988-203">These data can be structured using a dictionary.</span></span> <span data-ttu-id="8e988-204">Kulcsok és értékek bármilyen típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="8e988-204">Keys and values can be of any type.</span></span>

<span data-ttu-id="8e988-205">Kiegészítő funkciók adatokat, ha be szeretné szúrni a saját típusát a kiegészítő funkciók fel kell vennie egy típus adategyezmény szerializálva vannak.</span><span class="sxs-lookup"><span data-stu-id="8e988-205">Extras data are serialized so if you want to insert your own type in extras you have to add a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="8e988-206">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-206">Example</span></span>
<span data-ttu-id="8e988-207">Azt a hozzon létre egy új osztályt "Személy".</span><span class="sxs-lookup"><span data-stu-id="8e988-207">We create a new class "Person".</span></span>

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

<span data-ttu-id="8e988-208">Ezt követően adunk hozzá egy `Person` egy további példányt.</span><span class="sxs-lookup"><span data-stu-id="8e988-208">Then, we will add a `Person` instance to an extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="8e988-209">Ha más típusú objektumokat, ellenőrizze, hogy azok ToString() metódus megvalósítása emberi olvasható karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8e988-209">If you put other types of objects, make sure their ToString() method is implemented to return a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="8e988-210">Korlátok</span><span class="sxs-lookup"><span data-stu-id="8e988-210">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="8e988-211">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="8e988-211">Keys</span></span>
<span data-ttu-id="8e988-212">Az objektum minden egyes kulcsot meg kell egyeznie a következő reguláris kifejezésnek:</span><span class="sxs-lookup"><span data-stu-id="8e988-212">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="8e988-213">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="8e988-213">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="8e988-214">Méret</span><span class="sxs-lookup"><span data-stu-id="8e988-214">Size</span></span>
<span data-ttu-id="8e988-215">Kiegészítő funkciók korlátozva **1024** hívás karakteres.</span><span class="sxs-lookup"><span data-stu-id="8e988-215">Extras are limited to **1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="8e988-216">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="8e988-216">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="8e988-217">Referencia</span><span class="sxs-lookup"><span data-stu-id="8e988-217">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="8e988-218">A SendAppInfo() használatával információkat (vagy más alkalmazás egyedi információt) követési függvény manuálisan jelentést.</span><span class="sxs-lookup"><span data-stu-id="8e988-218">You can manually report tracking information (or any other application specific information) using the SendAppInfo() function.</span></span>

<span data-ttu-id="8e988-219">Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: a megadott kulcs csak a legutóbbi értékét az adott eszköz megmarad.</span><span class="sxs-lookup"><span data-stu-id="8e988-219">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="8e988-220">Esemény kiegészítő funkciók, például egy szótár használata\<objektum, objektum\> informations csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="8e988-220">Like event extras, use a Dictionary\<object, object\> to attach informations.</span></span>

### <a name="example"></a><span data-ttu-id="8e988-221">Példa</span><span class="sxs-lookup"><span data-stu-id="8e988-221">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="8e988-222">Korlátok</span><span class="sxs-lookup"><span data-stu-id="8e988-222">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="8e988-223">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="8e988-223">Keys</span></span>
<span data-ttu-id="8e988-224">Az objektum minden egyes kulcsot meg kell egyeznie a következő reguláris kifejezésnek:</span><span class="sxs-lookup"><span data-stu-id="8e988-224">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="8e988-225">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="8e988-225">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="8e988-226">Méret</span><span class="sxs-lookup"><span data-stu-id="8e988-226">Size</span></span>
<span data-ttu-id="8e988-227">Alkalmazásadatok esetén egyre korlátozódik **1024** hívás karakteres.</span><span class="sxs-lookup"><span data-stu-id="8e988-227">Application information are limited to **1024** characters per call.</span></span>

<span data-ttu-id="8e988-228">Az előző példában a a kiszolgálónak küldött JSON-ja 44 karakter:</span><span class="sxs-lookup"><span data-stu-id="8e988-228">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a><span data-ttu-id="8e988-229">Naplózás</span><span class="sxs-lookup"><span data-stu-id="8e988-229">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="8e988-230">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8e988-230">Enable Logging</span></span>
<span data-ttu-id="8e988-231">Az SDK beállítható úgy, hogy az IDE-konzolon a vizsgálati naplók eredményez.</span><span class="sxs-lookup"><span data-stu-id="8e988-231">The SDK can be configured to produce test logs in the IDE console.</span></span>
<span data-ttu-id="8e988-232">Ezek a naplók alapértelmezés szerint nincsenek aktiválva.</span><span class="sxs-lookup"><span data-stu-id="8e988-232">These logs are not activated by default.</span></span> <span data-ttu-id="8e988-233">Ez testreszabásához frissítse a tulajdonságát `EngagementAgent.Instance.TestLogEnabled` egy érhetők el az érték a `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="8e988-233">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
