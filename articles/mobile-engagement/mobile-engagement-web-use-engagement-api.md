---
title: Mobile Engagement webes SDK API-k aaaAzure |} Microsoft Docs
description: "Azure Mobile Engagement hello legújabb frissítésekről és hello webes SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: ec1261d6ad573b8c3ad6d5f616ab7bbe560d6fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-mobile-engagement-api-in-a-web-application"></a><span data-ttu-id="9d192-103">A webalkalmazás az Azure Mobile Engagement API hello használata</span><span class="sxs-lookup"><span data-stu-id="9d192-103">Use hello Azure Mobile Engagement API in a web application</span></span>
<span data-ttu-id="9d192-104">Ez a dokumentum egy hozzáadása toohello dokumentumot, amely azt ismerteti, hogyan túl[integrálja a Mobile Engagement egy webalkalmazásban](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="9d192-104">This document is an addition toohello document that tells you how too[integrate Mobile Engagement in a web application](mobile-engagement-web-integrate-engagement.md).</span></span> <span data-ttu-id="9d192-105">Hogyan toouse hello Azure Mobile Engagement API tooreport kapcsolatos részletes adatokat biztosít az alkalmazás statisztikái.</span><span class="sxs-lookup"><span data-stu-id="9d192-105">It provides in-depth details about how toouse hello Azure Mobile Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="9d192-106">hello Mobile Engagement API által biztosított hello `engagement.agent` objektum.</span><span class="sxs-lookup"><span data-stu-id="9d192-106">hello Mobile Engagement API is provided by hello `engagement.agent` object.</span></span> <span data-ttu-id="9d192-107">az alapértelmezett Azure Mobile Engagement webes SDK alias hello `engagement`.</span><span class="sxs-lookup"><span data-stu-id="9d192-107">hello default Azure Mobile Engagement Web SDK alias is `engagement`.</span></span> <span data-ttu-id="9d192-108">Ez az alias hello SDK konfigurációból is definiálja újra.</span><span class="sxs-lookup"><span data-stu-id="9d192-108">You can redefine this alias from hello SDK configuration.</span></span>

## <a name="mobile-engagement-concepts"></a><span data-ttu-id="9d192-109">Mobile Engagement – fogalmak</span><span class="sxs-lookup"><span data-stu-id="9d192-109">Mobile Engagement concepts</span></span>
<span data-ttu-id="9d192-110">hello következő részekből pontosítsa közös [Mobile Engagement – fogalmak](mobile-engagement-concepts.md) hello webes platform jöhet létre.</span><span class="sxs-lookup"><span data-stu-id="9d192-110">hello following parts refine common [Mobile Engagement concepts](mobile-engagement-concepts.md) for hello web platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="9d192-111">`Session` és `Activity`</span><span class="sxs-lookup"><span data-stu-id="9d192-111">`Session` and `Activity`</span></span>
<span data-ttu-id="9d192-112">Hello felhasználó két tevékenység között csak néhány másodpercig inaktív marad, ha hello felhasználói műveletek sorozata alapján oszlik, két, különböző munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="9d192-112">If hello user stays idle for more than a few seconds between two activities, hello user's sequence of activities is split into two distinct sessions.</span></span> <span data-ttu-id="9d192-113">Ezek néhány másodpercig hello munkamenet időkorlátja nevezzük.</span><span class="sxs-lookup"><span data-stu-id="9d192-113">These few seconds are called hello session timeout.</span></span>

<span data-ttu-id="9d192-114">Ha a webes alkalmazás önmagában nem deklarálható felhasználói tevékenységek hello vége (hívó hello által `engagement.agent.endActivity` függvény), hello a Mobile Engagement-kiszolgáló automatikusan lejárnak hello felhasználói munkamenet hello alkalmazáslap bezárása után három percen belül.</span><span class="sxs-lookup"><span data-stu-id="9d192-114">If your web application doesn't declare hello end of user activities by itself (by calling hello `engagement.agent.endActivity` function), hello Mobile Engagement server automatically expires hello user session within three minutes after hello application page is closed.</span></span> <span data-ttu-id="9d192-115">Ez a lehetőség hello server munkamenet időkorlátja.</span><span class="sxs-lookup"><span data-stu-id="9d192-115">This is called hello server session timeout.</span></span>

### `Crash`
<span data-ttu-id="9d192-116">Nem kezelt JavaScript-kivételeknek az automatizált jelentések nem jönnek létre, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="9d192-116">Automated reports of uncaught JavaScript exceptions are not created by default.</span></span> <span data-ttu-id="9d192-117">Összeomlások jelentést azonban hello segítségével manuálisan `sendCrash` (lásd hello összeomlik a jelentéskészítő) működik.</span><span class="sxs-lookup"><span data-stu-id="9d192-117">However, you can report crashes manually by using hello `sendCrash` function (see hello section on reporting crashes).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="9d192-118">Jelentéskészítési tevékenység</span><span class="sxs-lookup"><span data-stu-id="9d192-118">Reporting activities</span></span>
<span data-ttu-id="9d192-119">Amikor a felhasználó elindít egy új tevékenységet és hello felhasználói addig tart, amíg a jelenlegi tevékenység hello felhasználói tevékenység Reporting tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9d192-119">Reporting on user activity includes when a user starts a new activity, and when hello user ends hello current activity.</span></span>

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="9d192-120">Felhasználó elindítja az új tevékenység</span><span class="sxs-lookup"><span data-stu-id="9d192-120">User starts a new activity</span></span>
    engagement.agent.startActivity("MyUserActivity");

<span data-ttu-id="9d192-121">Toocall kell `startActivity()` minden felhasználói tevékenység változik.</span><span class="sxs-lookup"><span data-stu-id="9d192-121">You need toocall `startActivity()` each time user activity changes.</span></span> <span data-ttu-id="9d192-122">hello első hívás toothis függvény új felhasználói munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="9d192-122">hello first call toothis function starts a new user session.</span></span>

### <a name="user-ends-hello-current-activity"></a><span data-ttu-id="9d192-123">Felhasználói hello aktuális tevékenység befejeződik</span><span class="sxs-lookup"><span data-stu-id="9d192-123">User ends hello current activity</span></span>
    engagement.agent.endActivity();

<span data-ttu-id="9d192-124">Toocall kell `endActivity()` legalább egyszer amikor hello felhasználó befejezi a legutolsó tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9d192-124">You need toocall `endActivity()` at least once when hello user finishes their last activity.</span></span> <span data-ttu-id="9d192-125">Ebben értesíti hello Mobile Engagement webes SDK-t, hogy hello felhasználó jelenleg inaktív, valamint, hogy a felhasználói munkamenet hello toobe kell bezárása után hello munkamenet időkorlátja lejár.</span><span class="sxs-lookup"><span data-stu-id="9d192-125">This informs hello Mobile Engagement Web SDK that hello user is currently idle, and that hello user session needs toobe closed after hello session timeout expires.</span></span> <span data-ttu-id="9d192-126">Ha meghívja a `startActivity()` előtt hello munkamenet időkorlátja lejár, a hello munkamenet egyszerűen folytatódik.</span><span class="sxs-lookup"><span data-stu-id="9d192-126">If you call `startActivity()` before hello session timeout expires, hello session is simply resumed.</span></span>

<span data-ttu-id="9d192-127">Mivel a nem megbízható hívásakor hello navigator ablak lezárt, akkor gyakran nehéz vagy lehetetlen toocatch hello vége felhasználói tevékenységet egy webes környezeten belül.</span><span class="sxs-lookup"><span data-stu-id="9d192-127">Because there's no reliable call for when hello navigator window is closed, it's often difficult or impossible toocatch hello end of user activities inside a web environment.</span></span> <span data-ttu-id="9d192-128">Hogy miért van hello a kiszolgáló automatikusan lejárnak a Mobile Engagement hello felhasználói munkamenet hello alkalmazáslap bezárása után három percen belül.</span><span class="sxs-lookup"><span data-stu-id="9d192-128">That's why hello Mobile Engagement server automatically expires hello user session within three minutes after hello application page is closed.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="9d192-129">Jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="9d192-129">Reporting events</span></span>
<span data-ttu-id="9d192-130">Jelentési események foglalkozik, munkamenet és önálló események.</span><span class="sxs-lookup"><span data-stu-id="9d192-130">Reporting on events covers session events and standalone events.</span></span>

### <a name="session-events"></a><span data-ttu-id="9d192-131">Munkamenet-események</span><span class="sxs-lookup"><span data-stu-id="9d192-131">Session events</span></span>
<span data-ttu-id="9d192-132">Munkamenet-események általában használt tooreport hello műveletek hello felhasználói munkamenet során a felhasználó által végrehajtott.</span><span class="sxs-lookup"><span data-stu-id="9d192-132">Session events usually are used tooreport hello actions performed by a user during hello user's session.</span></span>

<span data-ttu-id="9d192-133">**További adatok nélkül. példa:**</span><span class="sxs-lookup"><span data-stu-id="9d192-133">**Example without extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

<span data-ttu-id="9d192-134">**Példa a további adatokat:**</span><span class="sxs-lookup"><span data-stu-id="9d192-134">**Example with extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="9d192-135">Önálló események</span><span class="sxs-lookup"><span data-stu-id="9d192-135">Standalone events</span></span>
<span data-ttu-id="9d192-136">Munkamenet események eltérően önálló események egy munkamenet környezetében hello kívül is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="9d192-136">Unlike session events, standalone events can occur outside hello context of a session.</span></span>

<span data-ttu-id="9d192-137">Ezzel ``engagement.agent.sendEvent`` helyett ``engagement.agent.sendSessionEvent``.</span><span class="sxs-lookup"><span data-stu-id="9d192-137">For that, use ``engagement.agent.sendEvent`` instead of ``engagement.agent.sendSessionEvent``.</span></span>

## <a name="reporting-errors"></a><span data-ttu-id="9d192-138">Hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="9d192-138">Reporting errors</span></span>
<span data-ttu-id="9d192-139">Hibákkal Reporting munkamenet hibák és önálló hibákat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9d192-139">Reporting on errors covers session errors and standalone errors.</span></span>

### <a name="session-errors"></a><span data-ttu-id="9d192-140">Munkamenet-hibák</span><span class="sxs-lookup"><span data-stu-id="9d192-140">Session errors</span></span>
<span data-ttu-id="9d192-141">Munkamenet általában olyan használt tooreport hello hibákat tartalmaznak, amelyek hatással vannak az hello felhasználói hello felhasználói munkamenet során.</span><span class="sxs-lookup"><span data-stu-id="9d192-141">Session errors usually are used tooreport hello errors that have an impact on hello user during hello user's session.</span></span>

<span data-ttu-id="9d192-142">**További adatok nélkül. példa:**</span><span class="sxs-lookup"><span data-stu-id="9d192-142">**Example without extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

<span data-ttu-id="9d192-143">**Példa a további adatokat:**</span><span class="sxs-lookup"><span data-stu-id="9d192-143">**Example with extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="9d192-144">Önálló hibák</span><span class="sxs-lookup"><span data-stu-id="9d192-144">Standalone errors</span></span>
<span data-ttu-id="9d192-145">Munkamenet hibák eltérően a önálló hibák adódhatnak kívül hello egy munkamenet környezetében.</span><span class="sxs-lookup"><span data-stu-id="9d192-145">Unlike session errors, standalone errors can occur outside hello context of a session.</span></span>

<span data-ttu-id="9d192-146">Ezzel `engagement.agent.sendError` helyett `engagement.agent.sendSessionError`.</span><span class="sxs-lookup"><span data-stu-id="9d192-146">For that, use `engagement.agent.sendError` instead of `engagement.agent.sendSessionError`.</span></span>

## <a name="reporting-jobs"></a><span data-ttu-id="9d192-147">Feladatok jelentése</span><span class="sxs-lookup"><span data-stu-id="9d192-147">Reporting jobs</span></span>
<span data-ttu-id="9d192-148">Jelentés a jelentéskészítési hibák és események, a feladatok során felmerülő, és az összeomlások feladatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9d192-148">Reporting on jobs covers reporting errors and events that occur during a job, and reporting crashes.</span></span>

<span data-ttu-id="9d192-149">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9d192-149">**Example:**</span></span>

<span data-ttu-id="9d192-150">Ha azt szeretné, hogy egy AJAX-kérelem toomonitor, hello következő használja:</span><span class="sxs-lookup"><span data-stu-id="9d192-150">If you want toomonitor an AJAX request, you'd use hello following:</span></span>

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a><span data-ttu-id="9d192-151">A feladat során hibát jelentett</span><span class="sxs-lookup"><span data-stu-id="9d192-151">Reporting errors during a job</span></span>
<span data-ttu-id="9d192-152">Hibák lehetnek kapcsolódó tooa helyett toohello jelenlegi felhasználói munkamenet feladat futtatásával.</span><span class="sxs-lookup"><span data-stu-id="9d192-152">Errors can be related tooa running job instead of toohello current user session.</span></span>

<span data-ttu-id="9d192-153">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9d192-153">**Example:**</span></span>

<span data-ttu-id="9d192-154">Ha azt szeretné, hogy tooreport hibát adott vissza, ha egy AJAX-kérelem sikertelen:</span><span class="sxs-lookup"><span data-stu-id="9d192-154">If you want tooreport an error if an AJAX request fails:</span></span>

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="9d192-155">A feladat során jelentési eseményeket</span><span class="sxs-lookup"><span data-stu-id="9d192-155">Reporting events during a job</span></span>
<span data-ttu-id="9d192-156">Események lehet feladat futtatásával helyett toohello jelenlegi felhasználói munkamenet köszönhetően toohello kapcsolódó tooa `engagement.agent.sendJobEvent` függvény.</span><span class="sxs-lookup"><span data-stu-id="9d192-156">Events can be related tooa running job instead of toohello current user session, thanks toohello `engagement.agent.sendJobEvent` function.</span></span>

<span data-ttu-id="9d192-157">Ez a funkció ugyanúgy működik, mint `engagement.agent.sendJobError`.</span><span class="sxs-lookup"><span data-stu-id="9d192-157">This function works exactly like `engagement.agent.sendJobError`.</span></span>

### <a name="reporting-crashes"></a><span data-ttu-id="9d192-158">Összeomlások Reporting</span><span class="sxs-lookup"><span data-stu-id="9d192-158">Reporting crashes</span></span>
<span data-ttu-id="9d192-159">Használjon hello `sendCrash` függvény tooreport manuálisan összeomlik.</span><span class="sxs-lookup"><span data-stu-id="9d192-159">Use hello `sendCrash` function tooreport crashes manually.</span></span>

<span data-ttu-id="9d192-160">Hello `crashid` argumentum egy karakterlánc, amely azonosítja a összeomlási hello típusú.</span><span class="sxs-lookup"><span data-stu-id="9d192-160">hello `crashid` argument is a string that identifies hello type of crash.</span></span>
<span data-ttu-id="9d192-161">Hello `crash` általában argumentum hello Veremkivonat a hello összeomlási karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="9d192-161">hello `crash` argument usually is hello stack trace of hello crash as a string.</span></span>

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a><span data-ttu-id="9d192-162">További paraméterek</span><span class="sxs-lookup"><span data-stu-id="9d192-162">Extra parameters</span></span>
<span data-ttu-id="9d192-163">Csatolhat tetszőleges adatok tooan esemény, a hiba, a tevékenység vagy a feladatot.</span><span class="sxs-lookup"><span data-stu-id="9d192-163">You can attach arbitrary data tooan event, error, activity, or job.</span></span>

<span data-ttu-id="9d192-164">lehet, hogy az hello adatokat bármely JSON-objektum (de nem tömb vagy egyszerű típusú).</span><span class="sxs-lookup"><span data-stu-id="9d192-164">hello data can be any JSON object (but not an array or primitive type).</span></span>

<span data-ttu-id="9d192-165">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9d192-165">**Example:**</span></span>

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="9d192-166">Korlátok</span><span class="sxs-lookup"><span data-stu-id="9d192-166">Limits</span></span>
<span data-ttu-id="9d192-167">Tooextra paraméterek vonatkozó határértékeket hello területein reguláris kifejezéseket a kulcsokat, értéktípusokat és méretét.</span><span class="sxs-lookup"><span data-stu-id="9d192-167">Limits that apply tooextra parameters are in hello areas of regular expressions for keys, value types, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="9d192-168">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="9d192-168">Keys</span></span>
<span data-ttu-id="9d192-169">Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="9d192-169">Each key in hello object must match hello following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="9d192-170">Ez azt jelenti, hogy a kulcsokat legalább egy betűvel kell kezdődnie követ betűk, számok és aláhúzásjelek (\_).</span><span class="sxs-lookup"><span data-stu-id="9d192-170">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="values"></a><span data-ttu-id="9d192-171">Értékek</span><span class="sxs-lookup"><span data-stu-id="9d192-171">Values</span></span>
<span data-ttu-id="9d192-172">Értékek: korlátozott toostring, számát, és logikai típusát.</span><span class="sxs-lookup"><span data-stu-id="9d192-172">Values are limited toostring, number, and Boolean types.</span></span>

#### <a name="size"></a><span data-ttu-id="9d192-173">Méret</span><span class="sxs-lookup"><span data-stu-id="9d192-173">Size</span></span>
<span data-ttu-id="9d192-174">Kiegészítő funkciók korlátozva too1, 024 karakteres hívás (miután a Mobile Engagement webes SDK hello kódolja a JSON-ban).</span><span class="sxs-lookup"><span data-stu-id="9d192-174">Extras are limited too1,024 characters per call (after hello Mobile Engagement Web SDK encodes it in JSON).</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="9d192-175">Jelentéskészítési alkalmazással kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="9d192-175">Reporting application information</span></span>
<span data-ttu-id="9d192-176">Manuálisan jelentheti a nyomkövetési adatokat (vagy bármely más alkalmazás-specifikus adatait) hello segítségével `sendAppInfo()` függvény.</span><span class="sxs-lookup"><span data-stu-id="9d192-176">You can manually report tracking information (or any other application-specific information) by using hello `sendAppInfo()` function.</span></span>

<span data-ttu-id="9d192-177">Vegye figyelembe, hogy ezt az információt elküldi Növekményesen.</span><span class="sxs-lookup"><span data-stu-id="9d192-177">Note that this information can be sent incrementally.</span></span> <span data-ttu-id="9d192-178">Csak hello legújabb egy adott kulcs értékét egy adott eszközhöz tárolni fogja.</span><span class="sxs-lookup"><span data-stu-id="9d192-178">Only hello latest value for a specific key will be kept for a specific device.</span></span>

<span data-ttu-id="9d192-179">Esemény kiegészítő funkciók, például a bármely JSON objektum tooabstract alkalmazással kapcsolatos adatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9d192-179">Like event extras, you can use any JSON object tooabstract application information.</span></span> <span data-ttu-id="9d192-180">Vegye figyelembe, hogy a tömb, vagy az alárendelt objektumok számít-e egyszerű karakterlánc (JSON-szerializálás).</span><span class="sxs-lookup"><span data-stu-id="9d192-180">Note that arrays or sub-objects are treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="9d192-181">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9d192-181">**Example:**</span></span>

<span data-ttu-id="9d192-182">Íme egy kódminta küldő hello felhasználói nemét és születési dátuma:</span><span class="sxs-lookup"><span data-stu-id="9d192-182">Here is a code sample for sending hello user's gender and birth date:</span></span>

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a><span data-ttu-id="9d192-183">Korlátok</span><span class="sxs-lookup"><span data-stu-id="9d192-183">Limits</span></span>
<span data-ttu-id="9d192-184">Tooapplication információk korlátokkal vannak hello területein reguláris kifejezéseket a kulcsokat és méretét.</span><span class="sxs-lookup"><span data-stu-id="9d192-184">Limits that apply tooapplication information are in hello areas of regular expressions for keys, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="9d192-185">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="9d192-185">Keys</span></span>
<span data-ttu-id="9d192-186">Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:</span><span class="sxs-lookup"><span data-stu-id="9d192-186">Each key in hello object must match hello following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="9d192-187">Ez azt jelenti, hogy a kulcsokat legalább egy betűvel kell kezdődnie követ betűk, számok és aláhúzásjelek (\_).</span><span class="sxs-lookup"><span data-stu-id="9d192-187">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="9d192-188">Méret</span><span class="sxs-lookup"><span data-stu-id="9d192-188">Size</span></span>
<span data-ttu-id="9d192-189">Az alkalmazásadatok korlátozott too1, 024 karakteres hívás (miután a Mobile Engagement webes SDK hello kódolja a JSON-ban).</span><span class="sxs-lookup"><span data-stu-id="9d192-189">Application information is limited too1,024 characters per call (after hello Mobile Engagement Web SDK encodes it in JSON).</span></span>

<span data-ttu-id="9d192-190">A fenti példa hello hello JSON küldeni toohello server 44 karakter:</span><span class="sxs-lookup"><span data-stu-id="9d192-190">In hello preceding example, hello JSON sent toohello server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
