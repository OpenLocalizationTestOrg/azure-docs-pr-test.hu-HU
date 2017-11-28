---
title: "aaaHow tooUse hello az univerzális Windows-Engagement API"
description: "Hogyan tooUse hello az univerzális Windows-Engagement API"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0256b839c28e4ef6c530106408d744038fa711ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-universal"></a><span data-ttu-id="41e22-103">Hogyan tooUse hello az univerzális Windows-Engagement API</span><span class="sxs-lookup"><span data-stu-id="41e22-103">How tooUse hello Engagement API on Windows Universal</span></span>
<span data-ttu-id="41e22-104">Ez a dokumentum egy bővítmény toohello dokumentum [hogyan tooIntegrate Engagement az univerzális Windows-](mobile-engagement-windows-store-integrate-engagement.md): hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái mélysége adatait a biztosít.</span><span class="sxs-lookup"><span data-stu-id="41e22-104">This document is an add-on toohello document [How tooIntegrate Engagement on Windows Universal](mobile-engagement-windows-store-integrate-engagement.md): it provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="41e22-105">Vegye figyelembe, hogy ha csak szeretné Engagement tooreport munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás, majd hello legegyszerűbb módja a rendszer minden toomake a `Page` alosztályokat örökölhet hello `EngagementPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="41e22-105">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `Page` sub-classes inherit from hello `EngagementPage` class.</span></span>

<span data-ttu-id="41e22-106">Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementPage` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.</span><span class="sxs-lookup"><span data-stu-id="41e22-106">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementPage` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="41e22-107">hello Engagement API által biztosított hello `EngagementAgent` osztály.</span><span class="sxs-lookup"><span data-stu-id="41e22-107">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="41e22-108">Toothose módszerek keresztül érheti el `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="41e22-108">You can access toothose methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="41e22-109">Akkor is, ha hello ügynök modul nem lett inicializálva, minden hívás toohello API késleltetve, és végrehajtja újra elérhető hello ügynök esetén.</span><span class="sxs-lookup"><span data-stu-id="41e22-109">Even if hello agent module has not been initialized, each call toohello API is deferred, and will be executed again when hello agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="41e22-110">Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="41e22-110">Engagement concepts</span></span>
<span data-ttu-id="41e22-111">hello következő részekből pontosítsa közös hello [Mobile Engagement fogalmait](mobile-engagement-concepts.md) hello univerzális Windows platform.</span><span class="sxs-lookup"><span data-stu-id="41e22-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for hello Windows Universal platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="41e22-112">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="41e22-112">`Session` and `Activity`</span></span>
<span data-ttu-id="41e22-113">Egy *tevékenység* általában egy oldal, amely toosay hello hello alkalmazás társított *tevékenység* kezdődik, amikor hello oldal jelenik meg, és hello lap bezárásakor leáll: hello eset, amikor hello Hello segítségével integrált engagement SDK `EngagementPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="41e22-113">An *activity* is usually associated with one page of hello application, that is toosay hello *activity* starts when hello page is displayed and stops when hello page is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementPage` class.</span></span>

<span data-ttu-id="41e22-114">De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="41e22-114">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="41e22-115">Ez lehetővé teszi egy adott oldal több sub részek tooget hello használata ezen a lapon (például milyen gyakran tooknow és mennyi ideig párbeszédpanelek ezen a lapon belül használt) kapcsolatos további részletekért toosplit.</span><span class="sxs-lookup"><span data-stu-id="41e22-115">This allows you toosplit a given page in several sub parts tooget more details about hello usage of this page (for example tooknow how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="41e22-116">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="41e22-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="41e22-117">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="41e22-117">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-118">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-118">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-119">Toocall kell `StartActivity()` minden alkalommal hello felhasználói tevékenység változik.</span><span class="sxs-lookup"><span data-stu-id="41e22-119">You need toocall `StartActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="41e22-120">hello első hívás toothis függvény új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="41e22-120">hello first call toothis function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41e22-121">hello SDK automatikusan hello EndActivity metódus meghívja a hello alkalmazás bezárásakor.</span><span class="sxs-lookup"><span data-stu-id="41e22-121">hello SDK automatically calls hello EndActivity method when hello application is closed.</span></span> <span data-ttu-id="41e22-122">Ebből kifolyólag javasoljuk toocall hello StartActivity metódus amikor hello tevékenység hello felhasználó módosítja, és hello EndActivity metódus a metódus hívása óta kényszeríti hello aktuális munkamenet toobe tooNEVER hívás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="41e22-122">Thus, it is HIGHLY recommended toocall hello StartActivity method whenever hello activity of hello user changes, and tooNEVER call hello EndActivity method, since calling this method forces hello current session toobe ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="41e22-123">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-123">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="41e22-124">Felhasználói karakterlánccal végződik-e aktuális tevékenysége</span><span class="sxs-lookup"><span data-stu-id="41e22-124">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-125">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-125">Reference</span></span>
            void EndActivity()

<span data-ttu-id="41e22-126">Ez befejezi a hello tevékenység és hello munkamenet.</span><span class="sxs-lookup"><span data-stu-id="41e22-126">This ends hello activity and hello session.</span></span> <span data-ttu-id="41e22-127">Ez a módszer nem célszerű hívni, csak ha valóban tudja, milyen végzett.</span><span class="sxs-lookup"><span data-stu-id="41e22-127">You should not call this method unless you really know what you're doing.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-128">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-128">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="41e22-129">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="41e22-129">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="41e22-130">Feladat indítása</span><span class="sxs-lookup"><span data-stu-id="41e22-130">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-131">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-131">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-132">Hello feladat tootrack: bizonyos feladatokat is használhat egy meghatározott időtartamra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="41e22-132">You can use hello job tootrack certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-133">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-133">Example</span></span>
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="41e22-134">Egy feladat vége</span><span class="sxs-lookup"><span data-stu-id="41e22-134">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-135">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-135">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="41e22-136">Amint egy tevékenység követi nyomon a feladat le lett állítva, meg kell hívnia hello EndJob metódust a feladat úgy, hogy megadja a hello feladat neve.</span><span class="sxs-lookup"><span data-stu-id="41e22-136">As soon as a task tracked by a job has been terminated, you should call hello EndJob method for this job, by supplying hello job name.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-137">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-137">Example</span></span>
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="41e22-138">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="41e22-138">Reporting Events</span></span>
<span data-ttu-id="41e22-139">A következő három típusú eseményeket:</span><span class="sxs-lookup"><span data-stu-id="41e22-139">There is three types of events :</span></span>

* <span data-ttu-id="41e22-140">Önálló események</span><span class="sxs-lookup"><span data-stu-id="41e22-140">Standalone events</span></span>
* <span data-ttu-id="41e22-141">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="41e22-141">Session events</span></span>
* <span data-ttu-id="41e22-142">Feladat-események</span><span class="sxs-lookup"><span data-stu-id="41e22-142">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="41e22-143">Önálló események</span><span class="sxs-lookup"><span data-stu-id="41e22-143">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-144">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-144">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-145">Önálló események is előfordulhatnak kívül hello egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="41e22-145">Standalone events can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-146">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-146">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="41e22-147">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="41e22-147">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-148">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-148">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-149">Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.</span><span class="sxs-lookup"><span data-stu-id="41e22-149">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-150">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-150">Example</span></span>
<span data-ttu-id="41e22-151">**Adatok: nélkül**</span><span class="sxs-lookup"><span data-stu-id="41e22-151">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="41e22-152">**Adatokkal:**</span><span class="sxs-lookup"><span data-stu-id="41e22-152">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="41e22-153">Feladat-események</span><span class="sxs-lookup"><span data-stu-id="41e22-153">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-154">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-154">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-155">Feladat eseményeket általában használt tooreport hello műveletek során a feladat felhasználó által végrehajtott is.</span><span class="sxs-lookup"><span data-stu-id="41e22-155">Job events are usually used tooreport hello actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-156">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-156">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="41e22-157">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="41e22-157">Reporting Errors</span></span>
<span data-ttu-id="41e22-158">Hibák három típusa van:</span><span class="sxs-lookup"><span data-stu-id="41e22-158">There are three types of errors :</span></span>

* <span data-ttu-id="41e22-159">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="41e22-159">Standalone errors</span></span>
* <span data-ttu-id="41e22-160">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="41e22-160">Session errors</span></span>
* <span data-ttu-id="41e22-161">Hibák: feladathiba</span><span class="sxs-lookup"><span data-stu-id="41e22-161">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="41e22-162">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="41e22-162">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-163">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-163">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-164">Ellenkező toosession hibák önálló hibák adódhatnak kívül hello egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="41e22-164">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-165">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-165">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="41e22-166">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="41e22-166">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-167">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-167">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-168">Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="41e22-168">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-169">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-169">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="41e22-170">Hibák: feladathiba</span><span class="sxs-lookup"><span data-stu-id="41e22-170">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-171">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-171">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="41e22-172">Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="41e22-172">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-173">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-173">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="41e22-174">Jelentéskészítési összeomlik</span><span class="sxs-lookup"><span data-stu-id="41e22-174">Reporting Crashes</span></span>
<span data-ttu-id="41e22-175">hello ügynök biztosít két módszer toodeal összeomlik.</span><span class="sxs-lookup"><span data-stu-id="41e22-175">hello agent provides two methods toodeal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="41e22-176">Kivétel küldése</span><span class="sxs-lookup"><span data-stu-id="41e22-176">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-177">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-177">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="41e22-178">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-178">Example</span></span>
<span data-ttu-id="41e22-179">Bármikor kivétel meghívásával küldheti el:</span><span class="sxs-lookup"><span data-stu-id="41e22-179">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="41e22-180">Is használhat egy nem kötelező paraméter tooterminate hello engagement munkamenet hello ugyanannyi időt vesz igénybe, mint hello összeomlási küldésekor.</span><span class="sxs-lookup"><span data-stu-id="41e22-180">You can also use an optional parameter tooterminate hello engagement session at hello same time than sending hello crash.</span></span> <span data-ttu-id="41e22-181">Ebben az esetben hívható toodo:</span><span class="sxs-lookup"><span data-stu-id="41e22-181">toodo so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="41e22-182">Ha így tesz, a hello munkamenet és feladatok csak hello összeomlási elküldése után megszűnik.</span><span class="sxs-lookup"><span data-stu-id="41e22-182">If you do that, hello session and jobs will be closed just after sending hello crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="41e22-183">Nem kezelt kivétel küldése</span><span class="sxs-lookup"><span data-stu-id="41e22-183">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="41e22-184">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-184">Reference</span></span>
            void SendCrash(Exception e)

<span data-ttu-id="41e22-185">Engagement is biztosít a metódus nem kezelt toosend kivételek, ha **LETILTOTT** Engagement automatikus **összeomlási** reporting.</span><span class="sxs-lookup"><span data-stu-id="41e22-185">Engagement also provides a method toosend unhandled exceptions if you have **DISABLED** Engagement automatic **crash** reporting.</span></span> <span data-ttu-id="41e22-186">Ez különösen akkor hasznos, ha az alkalmazás UnhandledException eseménykezelő hello használni.</span><span class="sxs-lookup"><span data-stu-id="41e22-186">This is especially useful when used inside hello application UnhandledException event handler.</span></span>

<span data-ttu-id="41e22-187">Ezt a módszert fog **mindig** hello engagement munkamenet és feladatok meghívott után leáll.</span><span class="sxs-lookup"><span data-stu-id="41e22-187">This method will **ALWAYS** terminate hello engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="41e22-188">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-188">Example</span></span>
<span data-ttu-id="41e22-189">Használat tooimplement saját UnhandledExceptionEventArgs kezelő.</span><span class="sxs-lookup"><span data-stu-id="41e22-189">You can use it tooimplement your own UnhandledExceptionEventArgs handler.</span></span> <span data-ttu-id="41e22-190">Adja hozzá például hello `Current_UnhandledException` hello metódusában `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="41e22-190">For example, add hello `Current_UnhandledException` method of hello `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

<span data-ttu-id="41e22-191">Az App.xaml.cs "Nyilvános App() megkeresése" hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="41e22-191">In App.xaml.cs in "Public App(){}" add:</span></span>

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a><span data-ttu-id="41e22-192">Eszközazonosító</span><span class="sxs-lookup"><span data-stu-id="41e22-192">Device Id</span></span>
            String EngagementAgent.Instance.GetDeviceId()

<span data-ttu-id="41e22-193">A metódus hívása kaphat hello engagement-eszközazonosító.</span><span class="sxs-lookup"><span data-stu-id="41e22-193">You can get hello engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="41e22-194">Kiegészítő funkciók paraméterek</span><span class="sxs-lookup"><span data-stu-id="41e22-194">Extras parameters</span></span>
<span data-ttu-id="41e22-195">Lehet, hogy az adatokat tetszőleges csatolt tooan esemény, a hiba, a tevékenység vagy a feladat.</span><span class="sxs-lookup"><span data-stu-id="41e22-195">Arbitrary data can be attached tooan event, an error, an activity or a job.</span></span> <span data-ttu-id="41e22-196">Ezen adatok segítségével dictionary szervezhetők.</span><span class="sxs-lookup"><span data-stu-id="41e22-196">These data can be structured using a dictionary.</span></span> <span data-ttu-id="41e22-197">Kulcsok és értékek bármilyen típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="41e22-197">Keys and values can be of any type.</span></span>

<span data-ttu-id="41e22-198">Kiegészítő funkciók adatok szerializálva vannak, így ha azt szeretné, tooinsert a saját típusát, a kiegészítő funkciók kell tooadd egy adategyezmény ehhez a típushoz.</span><span class="sxs-lookup"><span data-stu-id="41e22-198">Extras data are serialized so if you want tooinsert your own type in extras you have tooadd a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="41e22-199">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-199">Example</span></span>
<span data-ttu-id="41e22-200">Azt a hozzon létre egy új osztályt "Személy".</span><span class="sxs-lookup"><span data-stu-id="41e22-200">We create a new class "Person".</span></span>

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
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

<span data-ttu-id="41e22-201">Ezt követően adunk hozzá egy `Person` példány tooan extra.</span><span class="sxs-lookup"><span data-stu-id="41e22-201">Then, we will add a `Person` instance tooan extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="41e22-202">Ha más típusú objektumokat, győződjön meg arról a ToString() módszer megvalósított tooreturn emberi olvasható karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="41e22-202">If you put other types of objects, make sure their ToString() method is implemented tooreturn a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="41e22-203">Korlátok</span><span class="sxs-lookup"><span data-stu-id="41e22-203">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="41e22-204">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="41e22-204">Keys</span></span>
<span data-ttu-id="41e22-205">Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="41e22-205">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="41e22-206">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="41e22-206">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="41e22-207">Méret</span><span class="sxs-lookup"><span data-stu-id="41e22-207">Size</span></span>
<span data-ttu-id="41e22-208">Kiegészítő funkciók korlátozva túl**1024** hívás karakteres.</span><span class="sxs-lookup"><span data-stu-id="41e22-208">Extras are limited too**1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="41e22-209">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="41e22-209">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="41e22-210">Referencia</span><span class="sxs-lookup"><span data-stu-id="41e22-210">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="41e22-211">Követés hello SendAppInfo() függvény használatával információkat (vagy egyéb alkalmazás konkrét információkat) kézzel jelentést.</span><span class="sxs-lookup"><span data-stu-id="41e22-211">You can manually report tracking information (or any other application specific information) using hello SendAppInfo() function.</span></span>

<span data-ttu-id="41e22-212">Vegye figyelembe, hogy az adatküldés Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="41e22-212">Note that this data can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="41e22-213">Esemény kiegészítő funkciók, például egy szótár használata\<objektum, objektum\> tooattach adatokat.</span><span class="sxs-lookup"><span data-stu-id="41e22-213">Like event extras, use a Dictionary\<object, object\> tooattach data.</span></span>

### <a name="example"></a><span data-ttu-id="41e22-214">Példa</span><span class="sxs-lookup"><span data-stu-id="41e22-214">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="41e22-215">Korlátok</span><span class="sxs-lookup"><span data-stu-id="41e22-215">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="41e22-216">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="41e22-216">Keys</span></span>
<span data-ttu-id="41e22-217">Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="41e22-217">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="41e22-218">Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).</span><span class="sxs-lookup"><span data-stu-id="41e22-218">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="41e22-219">Méret</span><span class="sxs-lookup"><span data-stu-id="41e22-219">Size</span></span>
<span data-ttu-id="41e22-220">Az alkalmazásadatok korlátozódik túl**1024** hívás karakteres.</span><span class="sxs-lookup"><span data-stu-id="41e22-220">Application information is limited too**1024** characters per call.</span></span>

<span data-ttu-id="41e22-221">Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:</span><span class="sxs-lookup"><span data-stu-id="41e22-221">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a><span data-ttu-id="41e22-222">Naplózás</span><span class="sxs-lookup"><span data-stu-id="41e22-222">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="41e22-223">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="41e22-223">Enable Logging</span></span>
<span data-ttu-id="41e22-224">hello SDK konfigurált tooproduce vizsgálati naplók hello IDE konzolon lehet.</span><span class="sxs-lookup"><span data-stu-id="41e22-224">hello SDK can be configured tooproduce test logs in hello IDE console.</span></span>
<span data-ttu-id="41e22-225">Ezek a naplók alapértelmezés szerint nincsenek aktiválva.</span><span class="sxs-lookup"><span data-stu-id="41e22-225">These logs are not activated by default.</span></span> <span data-ttu-id="41e22-226">toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="41e22-226">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

