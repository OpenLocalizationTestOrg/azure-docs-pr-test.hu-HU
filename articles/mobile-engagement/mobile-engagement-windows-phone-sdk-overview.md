---
title: Mobile Engagement Windows Phone Silverlight SDK Overview aaaAzure |} Microsoft Docs
description: "Hello Windows Phone Silverlight SDK for Azure Mobile Engagement áttekintése"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: ff2febed2202127e0538373ebbabe674993ce39d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK Overview és Azure Mobile Engagement
Kezdje itt tooget hello részletek hogyan toointegrate Azure Mobile Engagement a Windows Phone Silverlight-alkalmazást. Ha azt szeretné, hogy toogive azt egy try először győződjön meg arról, hogy elvégezte a [15 perc oktatóanyag](mobile-engagement-windows-phone-get-started.md).

Kattintson a toosee hello [SDK tartalom](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Integráció eljárások
1. Kezdje itt: [hogyan toointegrate a Mobile Engagement a Windows Phone Silverlight-alkalmazásokban](mobile-engagement-windows-phone-integrate-engagement.md)
2. Az értesítések: [hogyan toointegrate Reach (értesítések) a Windows Phone Silverlight-alkalmazásokban](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Megvalósítási terv címkét: [hogyan toouse hello speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Kibocsátási megjegyzések
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
Hello részét *MicrosoftAzure.MobileEngagement* Nuget-csomag **v3.4.1**

* Jobb stabilitás.

Korábbi verziójához lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Frissítési eljárások
Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban. Lásd: hello teljes [frissítése eljárások](mobile-engagement-windows-phone-upgrade-procedure.md). Például ha telepít át 0.10.1 hello kövesse toofirst rendelkezik too0.11.0 "0.9.0-s a too0.10.1" eljárást akkor hello "0.10.1 a too0.11.0" eljárás.

### <a name="from-200-too330"></a>A 2.0.0 too3.3.0
#### <a name="test-logs"></a>Vizsgálati naplók
Hello SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve. toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>A régebbi verziók frissítése
Lásd: [eljárások frissítése](mobile-engagement-windows-phone-upgrade-procedure.md)

