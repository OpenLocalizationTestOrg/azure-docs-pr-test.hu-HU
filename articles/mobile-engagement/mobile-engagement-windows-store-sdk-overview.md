---
title: "Mobile Engagement Windows Universal SDK-integráció aaaAzure |} Microsoft Docs"
description: "Windows Universal-integráció SDK és Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: 2f88e58adb349a2a4eb43b0f182f99b3e8b8cfd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Az Azure Mobile Engagement Windows Universal SDK-integráció
Ez a dokumentum ismerteti az összes hello integrációs és konfigurációs lehetőségek hello Azure Mobile Engagement univerzális Windows SDK érhető el.

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elindítása előtt először végezze el a [15 perces oktatóanyag](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Speciális funkciók
### <a name="reporting-features"></a>Jelentési szolgáltatások
Ezeket a szolgáltatásokat is hozzáadhat:

1. [Speciális jelentéskészítési beállítások](mobile-engagement-windows-store-advanced-reporting.md)
2. [Speciális konfigurációs beállítások](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Értesítések
[Hogyan toointegrate Reach (értesítések) az univerzális Windows-alkalmazás](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Címke terv végrehajtására:
[Hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API-címkézés](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a>Kibocsátási megjegyzések
### <a name="341-11032016"></a>3.4.1 (11/03/2016)

* Jobb stabilitás.

A korábbi, lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-windows-store-release-notes.md)

## <a name="upgrade-procedures"></a>Frissítési eljárások
Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Ha hello SDK a különböző verzióiban nem talált, előfordulhat, hogy toofollow számos eljárást. Lásd: hello teljes [frissítése eljárások](mobile-engagement-windows-store-upgrade-procedure.md). Például ha telepít át 0.10.1 hello kövesse toofirst rendelkezik too0.11.0 "0.9.0-s a too0.10.1" eljárást akkor hello "0.10.1 a too0.11.0" eljárás.

### <a name="from-330-too340"></a>A 3.3.0 too3.4.0
#### <a name="test-logs"></a>Vizsgálati naplók
Hello SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve. toocustomize, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello értékek hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a>Erőforrások
hello Reach átfedő javult. Hello SDK NuGet csomag erőforrások része.

Hello SDK toohello új verzióját a frissítés során választhat tookeep kíván-e a hello a meglévő fájlok vagy nem átfedő mappa az erőforrások:

* Ha hello előző átfedő működik-e meg, vagy hello integrációhoz `WebView` elemek manuálisan, majd dönthet úgy is tookeep fájlok, a program kilép, továbbra is működni fognak.
* tooupdate toohello új átmeneti területre, teljes név felülírandó hello `overlay` a hello újat hello SDK csomagban található erőforrások mappát (UWP-alkalmazások: hello frissítés után letölthető hello új átfedő mappát a(z) % USERPROFILE %\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> A testreszabott hello korábbi verziójához hello új átfedő használatával felülírja.
> 
> 

### <a name="upgrade-from-older-versions"></a>A régebbi verziók frissítése
Lásd: [eljárások frissítése](mobile-engagement-windows-store-upgrade-procedure.md)

