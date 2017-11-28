---
title: "Az Azure Mobile Engagement hibaelhárítási útmutatója - API-k"
description: "Hibaelhárítás az Azure Mobile Engagement - API-k útmutatók"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3efc8a52-2b74-4917-b887-815ae8277474
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: piyushjo
ms.openlocfilehash: a7ae0a83046f2d67b790f672dcd3ae261987357a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-api-issues"></a><span data-ttu-id="257de-103">Az API-problémák hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="257de-103">Troubleshooting guide for API issues</span></span>
<span data-ttu-id="257de-104">A következőkben lehetséges problémák merülhetnek fel a rendszergazdák hogyan működnek együtt az Azure Mobile Engagement az API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="257de-104">The following are possible issues you may encounter with how administrators interact with Azure Mobile Engagement via the APIs.</span></span>

## <a name="syntax-issues"></a><span data-ttu-id="257de-105">Szintaxis problémák</span><span class="sxs-lookup"><span data-stu-id="257de-105">Syntax issues</span></span>
### <a name="issue"></a><span data-ttu-id="257de-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="257de-106">Issue</span></span>
* <span data-ttu-id="257de-107">Szintaktikai hibákat a API-t (vagy váratlan viselkedést) használatával.</span><span class="sxs-lookup"><span data-stu-id="257de-107">Syntax Errors using the API (or unexpected behavior).</span></span>

### <a name="causes"></a><span data-ttu-id="257de-108">Okok</span><span class="sxs-lookup"><span data-stu-id="257de-108">Causes</span></span>
* <span data-ttu-id="257de-109">Szintaxis problémák:</span><span class="sxs-lookup"><span data-stu-id="257de-109">Syntax issues:</span></span>
  * <span data-ttu-id="257de-110">Győződjön meg arról, hogy ellenőrizze a szintaxist, az adott API segítségével győződjön meg arról, hogy a beállítás lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="257de-110">Make sure to check the Syntax of the specific API you are using to confirm that the option is available.</span></span>
  * <span data-ttu-id="257de-111">Általános hiba az API-használati hathat Reach API és a leküldéses API-val (a legtöbb feladatot kell elvégezni helyett a leküldéses API Reach API-val).</span><span class="sxs-lookup"><span data-stu-id="257de-111">A common issue with API usage is to confuse the Reach API and the Push API (most tasks should be performed with the Reach API instead of the Push API).</span></span> 
  * <span data-ttu-id="257de-112">SDK-integrációval és API-használati egy másik gyakori probléma, hogy megzavarják a SDK és az API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="257de-112">Another common issue with SDK integration and API usage is to confuse the SDK Key and the API Key.</span></span>
  * <span data-ttu-id="257de-113">Parancsfájlok, amelyek kapcsolódnak az API-kat kell adatküldés legalább 10 percenként, vagy a kapcsolat időtúllépés (különösen a figyelő API parancsfájlok figyeli az adatokat a közös) lesz.</span><span class="sxs-lookup"><span data-stu-id="257de-113">Scripts that connect to the APIs need to send data at least every 10 minutes or the connection will time out (especially common in Monitor API scripts listening for data).</span></span> <span data-ttu-id="257de-114">Időtúllépések megelőzése érdekében a parancsfájl egy XMPP ping 10 percenként küld a munkamenet életben a kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="257de-114">To prevent timeouts, have your script send an XMPP ping every 10 minutes to keep the session alive with the server.</span></span>

### <a name="see-also"></a><span data-ttu-id="257de-115">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="257de-115">See also</span></span>
* <span data-ttu-id="257de-116">[API-JÁNAK dokumentációja][Link 4]</span><span class="sxs-lookup"><span data-stu-id="257de-116">[API Documentation][Link 4]</span></span>
* [<span data-ttu-id="257de-117">XMPP protokoll adatai</span><span class="sxs-lookup"><span data-stu-id="257de-117">XMPP Protocol Info</span></span>](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a><span data-ttu-id="257de-118">Nem sikerült az API-t használja ugyanazt a műveletet az Azure Mobile Engagement felhasználói felület érhető el</span><span class="sxs-lookup"><span data-stu-id="257de-118">Unable to use the API to perform the same action available in the Azure Mobile Engagement UI</span></span>
### <a name="issue"></a><span data-ttu-id="257de-119">Probléma</span><span class="sxs-lookup"><span data-stu-id="257de-119">Issue</span></span>
* <span data-ttu-id="257de-120">Egy műveletet, amely az Azure Mobile Engagement felhasználói felület működik a kapcsolódó Azure Mobile Engagement API nem működik.</span><span class="sxs-lookup"><span data-stu-id="257de-120">An action that works from the Azure Mobile Engagement UI doesn't work from the related Azure Mobile Engagement API.</span></span>

### <a name="causes"></a><span data-ttu-id="257de-121">Okok</span><span class="sxs-lookup"><span data-stu-id="257de-121">Causes</span></span>
* <span data-ttu-id="257de-122">Erősítse meg, hogy elvégezhető ugyanaz a művelet az Azure Mobile Engagement felhasználói felület jeleníti meg, hogy helyesen integrálva van a szolgáltatás az Azure Mobile Engagement az SDK-t.</span><span class="sxs-lookup"><span data-stu-id="257de-122">Confirming that you can perform the same action from the Azure Mobile Engagement UI shows that you have correctly integrated this feature of Azure Mobile Engagement with the SDK.</span></span>

### <a name="see-also"></a><span data-ttu-id="257de-123">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="257de-123">See also</span></span>
* <span data-ttu-id="257de-124">[Felhasználói felületének dokumentációja][Link 1]</span><span class="sxs-lookup"><span data-stu-id="257de-124">[UI Documentation][Link 1]</span></span>

## <a name="error-messages"></a><span data-ttu-id="257de-125">Hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="257de-125">Error Messages</span></span>
### <a name="issue"></a><span data-ttu-id="257de-126">Probléma</span><span class="sxs-lookup"><span data-stu-id="257de-126">Issue</span></span>
* <span data-ttu-id="257de-127">A hibakódok a futási időben, vagy a naplókban megjelenő API használatával.</span><span class="sxs-lookup"><span data-stu-id="257de-127">Error codes using the API displayed at runtime or in logs.</span></span>

### <a name="causes"></a><span data-ttu-id="257de-128">Okok</span><span class="sxs-lookup"><span data-stu-id="257de-128">Causes</span></span>
* <span data-ttu-id="257de-129">Ez egy összetett lista közös API állapot kódok számok hivatkozást és a előzetes hibaelhárítás céljából:</span><span class="sxs-lookup"><span data-stu-id="257de-129">Here is a composite list of common API status codes numbers for reference and preliminary troubleshooting:</span></span>
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a><span data-ttu-id="257de-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="257de-130">See also</span></span>
* <span data-ttu-id="257de-131">[Részletes hibaüzenetek az egyes adott API - API dokumentációját][Link 4]</span><span class="sxs-lookup"><span data-stu-id="257de-131">[API Documentation - for detailed errors on each specific API][Link 4]</span></span>

## <a name="silent-failures"></a><span data-ttu-id="257de-132">Csendes hibák</span><span class="sxs-lookup"><span data-stu-id="257de-132">Silent failures</span></span>
### <a name="issue"></a><span data-ttu-id="257de-133">Probléma</span><span class="sxs-lookup"><span data-stu-id="257de-133">Issue</span></span>
* <span data-ttu-id="257de-134">Nincs hibaüzenet jelenik meg a futási időben, vagy a naplókban az API-művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="257de-134">API action fails with no error message displayed at runtime or in logs.</span></span>

### <a name="causes"></a><span data-ttu-id="257de-135">Okok</span><span class="sxs-lookup"><span data-stu-id="257de-135">Causes</span></span>
* <span data-ttu-id="257de-136">Sok elem a rendszer letiltja az Azure Mobile Engagement felhasználói felületén, ha azok nincsenek megfelelően, integrált, de fog csendes az API-n, a, ne felejtse el tesztelni ugyanazokat a funkció a megjelenítéséhez, ha működik a felhasználói Felületről.</span><span class="sxs-lookup"><span data-stu-id="257de-136">Many items will be disabled in the Azure Mobile Engagement UI if they aren't integrated correctly, but will fail silently from the API, so remember to test the same functionality from the UI to see if it works.</span></span>
* <span data-ttu-id="257de-137">Az Azure Mobile Engagement és az Azure Mobile Engagement próbál használni, számos további beállításokat kell külön-külön integrálni az alkalmazás az SDK-val külön lépéseket, mielőtt azokat.</span><span class="sxs-lookup"><span data-stu-id="257de-137">Azure Mobile Engagement, and many advanced features of Azure Mobile Engagement you are attempting to use, need to be individually integrated into your app with the SDK as separate steps before you can use them.</span></span>

### <a name="see-also"></a><span data-ttu-id="257de-138">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="257de-138">See also</span></span>
* <span data-ttu-id="257de-139">[Hibaelhárítási útmutató - SDK][Link 25]</span><span class="sxs-lookup"><span data-stu-id="257de-139">[Troubleshooting Guide - SDK][Link 25]</span></span>

<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

