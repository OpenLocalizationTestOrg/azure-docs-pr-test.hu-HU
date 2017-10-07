---
title: "az Application Insights segítségével Felhőszolgáltatások aaaTroubleshoot |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot felhőszolgáltatás állít ki az Application Insights tooprocess Azure Diagnostics adatai alapján."
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 972924d9e6d1fe33d5c19b006d482de52ffb0ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Hibaelhárítás az Application Insights segítségével Cloud Services
A [Azure SDK 2.8](https://azure.microsoft.com/downloads/) Azure diagnostics bővítmény 1.5-ös, a küldhet Azure diagnosztikai adatokat a felhőalapú szolgáltatás, és közvetlenül tooApplication Insights. hello Azure Diagnostics által gyűjtött naplók&mdash;többek között az alkalmazásnaplók, a Windows eseménynaplóiban keresse meg, az ETW-naplók és a teljesítményszámlálók&mdash;tooApplication Insights küldhetők el. Majd jelenítheti meg ezeket az információkat a hello Application Insights portál felhasználói felületén. Használhatja hello Application Insights SDK tooget betekintést metrikák és a naplófájlokat, amelyek az alkalmazás, valamint hello rendszer- és infrastruktúra-szintű származó Azure Diagnostics származik.

## <a name="configure-azure-diagnostics-toosend-data-tooapplication-insights"></a>Azure Diagnostics toosend adatok tooApplication Insights konfigurálása
Kövesse a lépéseket tooset fel a felhőalapú szolgáltatás projekt toosend Azure diagnosztikai adatokat tooApplication Insights.

1. A Visual Studio Solution Explorerben kattintson a jobb gombbal egy szerepkört, és válassza ki **tulajdonságok** tooopen hello szerepkör Tervező.

    ![Solution Explorer szerepkör tulajdonságai][1]

2. A hello **diagnosztika** hello szerepkör designer, jelölje be hello szakasza **elküldeni a diagnosztikai adatok tooApplication Insights** lehetőséget.

    ![Szerepkör designer küldjön diagnosztikai adatokat tooapplication insights][2]

3. Válassza ki hello Application Insights-erőforrást, amelyet toosend hello Azure diagnosztikai adatokat hello párbeszédpanelen ugrik fel. hello párbeszédpanel lehetővé teszi egy meglévő Application Insights-erőforrást az előfizetésből tooselect vagy toomanually adja meg az Application Insights-erőforrás instrumentation kulcsa. Application Insights-erőforrás létrehozásával kapcsolatos további információkért lásd: [hozzon létre egy új Application Insights-erőforrást](../application-insights/app-insights-create-new-resource.md).

    ![Válassza ki az application insights-erőforrás][3]

    Miután hozzáadta a hello Application Insights-erőforrást, hello instrumentation kulcs az adott erőforrás hello nevű szolgáltatás konfigurációs beállításként tárolási **APPINSIGHTS_INSTRUMENTATIONKEY**. A konfigurációs beállítást, az egyes szolgáltatáskonfiguráció vagy a környezet módosíthatja. toodo Igen, válasszon egy másik konfigurációs hello **szolgáltatáskonfiguráció** listában, és adja meg, hogy a konfiguráció új instrumentation kulcsot.

    ![Válassza ki a szolgáltatás konfigurációja][4]

    Hello **APPINSIGHTS_INSTRUMENTATIONKEY** konfigurációs beállítást használja a Visual Studio tooconfigure hello diagnosztika bővítmény erőforrásadatokkal hello megfelelő Application Insights közzététele során. hello konfigurációs beállítás kényelmes módja a meghatározásának különböző szolgáltatáskonfiguráció különböző instrumentation kulcsokat. Visual Studio meg fog beállítás fordítása, és helyezze be hello diagnosztikai bővítmény nyilvános konfigurációja során hello közzétételi folyamat. toosimplify hello folyamatán hello diagnosztika bővítmény a PowerShell-lel, hello csomag kimenetét a Visual Studio hello nyilvános konfigurációs XML-kód hello megfelelő Application Insights instrumentation kulccsal is tartalmaz. hello nyilvános konfigurációs fájljainak hello bővítmények mappában jönnek létre, és kövesse a hello mintát *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. A PowerShell-alapú telepítések használhatja ezt a mintát toomap konfigurációs tooa szerepkörönként.

4) tooconfigure Azure diagnostics toosend hello Azure diagnostics ügynök tooApplication Insights által gyűjtött teljesítményszámlálók és a hiba szintű naplók engedélyezése hello **elküldeni a diagnosztikai adatok tooApplication Insights** lehetőséget. 

    Ha azt szeretné, hogy toofurther milyen adatok küldési tooApplication Insights, manuálisan kell szerkeszteni hello *diagnostics.wadcfgx* fájl az egyes szerepkörökhöz. Lásd: [konfigurálása Azure Diagnostics toosend adatok tooApplication Insights](#configure-azure-diagnostics-to-send-data-to-application-insights) hello konfigurációs manuális frissítésével kapcsolatos további toolearn.

Ha hello felhőszolgáltatás konfigurált toosend Azure diagnosztikai adatokat tooapplication insights, ezután telepítheti azt tooAzure általában meggyőződött arról, hogy hello Azure diagnostics bővítmény engedélyezve van. További információkért lásd: [közzététele a felhőalapú szolgáltatás, a Visual Studio használatával](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Az Azure diagnosztikai adatainak megtekintése az Application Insights
hello Azure diagnosztikai telemetriai adatokat jeleníti meg az Application Insights-erőforrás a felhőalapú szolgáltatás számára megadott hello.

Az Azure diagnostics napló képezi le tooApplication elemzések fogalmakat az alábbi módszerek:

* Teljesítményszámlálók, egyéni metrikákat az Application Insightsban jelennek meg.
* Windows eseménynaplóiban keresse meg a nyomkövetési adatokat és az Application Insightsban egyéni események jelennek meg.
* Alkalmazás naplóit, ETW naplók és a diagnosztika infrastruktúra naplók jelennek meg az Application Insights nyomkövetési adatokat.

az Application Insightsban Azure diagnosztikai adatainak tooview hello következő módokon:

* Használjon [metrikaböngésző](../application-insights/app-insights-metrics-explorer.md) toovisualize bármilyen egyéni teljesítményszámlálókat, vagy a Windows Eseménynapló eseményeinek különböző típusú száma.

    ![Egyéni metrikák a Metrikaböngészőben][5]

* Használjon [keresési](../application-insights/app-insights-diagnostic-search.md) toosearch hello nyomkövetési naplók Azure Diagnostics által küldött között. Például ha a nem kezelt kivételt okozott hello szerepkör toocrash és újrahasznosítását, hello kivétel kapcsolatos információkat jeleníti meg hello *alkalmazás* csatornája *Windows-Eseménynapló*. Használja a keresési toolook hello Windows eseménynaplóban, és teljes Veremkivonat hello hello kivétel toohelp keresés hello problémának az oka hello beolvasása.

    ![Keresési nyomkövetések][6]

## <a name="next-steps"></a>Következő lépések
* [Adja hozzá a hello Application Insights SDK tooyour felhőszolgáltatás](../application-insights/app-insights-cloudservices.md) kérelmeket, kivételek, függőségeit és bármilyen egyéni telemetriai adat az alkalmazásról toosend adatait. Hello Azure diagnosztikai adatokat, ezt az információt kaphat a az alkalmazás és a rendszer, amely teljes rálátást együtt az összes hello ugyanahhoz a Application Insights-erőforráshoz.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
