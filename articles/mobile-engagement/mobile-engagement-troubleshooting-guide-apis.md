---
title: "Mobile Engagement hibaelhárítási útmutatója - aaaAzure API-k"
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
ms.openlocfilehash: 5656b6f0f1aaf3e496a168c7cf09b307b9ab2a4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-api-issues"></a>Az API-problémák hibaelhárítási útmutató
Az alábbiakban hello lehetséges problémák merülhetnek fel a rendszergazdák együttműködését az Azure Mobile Engagement hello API-k használatával.

## <a name="syntax-issues"></a>Szintaxis problémák
### <a name="issue"></a>Probléma
* Szintaktikai hibákat hello API (vagy váratlan viselkedést) használatával.

### <a name="causes"></a>Okok
* Szintaxis problémák:
  * Győződjön meg arról, hogy toocheck hello szintaxisát hello adott API-t használ, amely lehetőséget hello tooconfirm érhető el.
  * Általános hiba az API-használati tooconfuse hello Reach API és hello leküldéses API (legtöbb feladatokat kell elvégezni hello Reach API hello leküldéses API helyett). 
  * Egy másik gyakori probléma SDK-integrációval és API-használati tooconfuse hello SDK kulcs, és az API-kulcs hello.
  * Parancsfájlok, amelyek kapcsolódnak az API-k toohello toosend adatokra van szükségük, legalább 10 percig vagy hello kapcsolat időtúllépése (különösen közös figyeli az adatokat a figyelő API parancsfájlokban). tooprevent időtúllépést, a parancsfájl küldése egy XMPP ping minden 10 perc tookeep hello munkamenet életben hello kiszolgálóval rendelkezik.

### <a name="see-also"></a>Lásd még:
* [API-JÁNAK dokumentációja][Link 4]
* [XMPP protokoll adatai](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-toouse-hello-api-tooperform-hello-same-action-available-in-hello-azure-mobile-engagement-ui"></a>Nem lehet toouse hello API tooperform hello azonos művelet hello Azure Mobile Engagement felhasználói felület érhető el
### <a name="issue"></a>Probléma
* Egy műveletet, amely az Azure Mobile Engagement felhasználói felület nem működik a hello hello Azure Mobile Engagement API-val kapcsolatos.

### <a name="causes"></a>Okok
* Erősítse meg, hogy elvégezhető-e azonos művelet hello Azure Mobile Engagement felhasználói felület megjeleníti, hogy helyesen integrált Azure Mobile Engagement az a funkciója hello hello SDK.

### <a name="see-also"></a>Lásd még:
* [Felhasználói felületének dokumentációja][Link 1]

## <a name="error-messages"></a>Hibaüzenetek
### <a name="issue"></a>Probléma
* A hibakódok futásidőben, vagy a naplókban megjelenő hello API használatával.

### <a name="causes"></a>Okok
* Ez egy összetett lista közös API állapot kódok számok hivatkozást és a előzetes hibaelhárítás céljából:
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from hello current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in hello response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). hello parameters provided toohello API or service are invalid. In this case, hello HTTP response will embed more details. Make sure tootest for hello MIME type of hello response as hello payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check hello AppID and SDK key).
        402        Billing lock. hello application is either off its quotas or is currently in a bad billing state.
        403        hello application is not enabled or hello specific API is disabled for this application.
        403        Unauthorized access toohello project or application, invalid access key (hello key must match hello one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), hello suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and hello campaign’s property named, manual Push must be set tootrue.
        403        hello email address is already associated tooanother account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        hello email address is not associated with an account. Please create or update hello account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying tooedit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for hello first time.
        409        Name already associated tooa different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or hello period is too large toobe displayed (hello server didn’t retrieve hello analytics because hello user request is for a period that is too large).
        503        Analytics not available yet (hello requested information is not computed yet for an application).
        504        hello server was not able toohandle your request in a reasonable time (if you make multiple calls tooan API very quickly, try toomake one call at a time and spread hello calls out over time).

### <a name="see-also"></a>Lásd még:
* [Részletes hibaüzenetek az egyes adott API - API dokumentációját][Link 4]

## <a name="silent-failures"></a>Csendes hibák
### <a name="issue"></a>Probléma
* Nincs hibaüzenet jelenik meg a futási időben, vagy a naplókban az API-művelet sikertelen lesz.

### <a name="causes"></a>Okok
* Sok elem letiltja az Azure Mobile Engagement felhasználói felületén Ha nincsenek megfelelően integrált, de értesítés nélkül sikertelen lesz a hello API, ezért ne feledje hello tootest hello ugyanezeket a funkciókat a hello felhasználói felület toosee használható-e.
* Az Azure Mobile Engagement, és az Azure Mobile Engagement toouse, külön-külön integrálva az alkalmazás hello SDK a különálló lépések őket használatba vétele előtt kell toobe próbált számos speciális szolgáltatás.

### <a name="see-also"></a>Lásd még:
* [Hibaelhárítási útmutató - SDK][Link 25]

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

