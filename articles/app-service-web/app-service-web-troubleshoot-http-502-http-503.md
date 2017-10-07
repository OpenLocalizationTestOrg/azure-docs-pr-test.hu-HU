---
title: "502-es aaaFix hibás átjáró, a szolgáltatás nem érhető el 503-as |} Microsoft Docs"
description: "Végezzen hibaelhárítást a 502 Hibás átjáró hibák és 503-as szolgáltatás nem érhető el az Azure App Service-ben futtatott webalkalmazás."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "502 Hibás átjáró 503-as szolgáltatás nem érhető el, 503-as hiba, 502-es hiba"
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: d9d8dcddaac930967a2e8d2bfd8cad09e6824c17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>A "502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" az Azure web apps a HTTP-hibák elhárítása
"502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" tárolva a webalkalmazás gyakori hibáinak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Ez a cikk segítséget nyújt a hibák elhárítása.

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl is az Azure támogatási incidens. Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , majd kattintson a **támogatás**.

## <a name="symptom"></a>Jelenség
Ha a felhasználó toohello webes alkalmazás, egy HTTP adja vissza "502 Hibás átjáró" hiba vagy a HTTP "503-as szolgáltatás nem érhető el" hiba.

## <a name="cause"></a>Ok
Ez a probléma gyakran alkalmazás szintű problémákat, például:

* hosszú ideig kérelmek
* nagy memória/CPU-t használó alkalmazások
* az alkalmazás összeomló tooan kivétel miatt.

## <a name="troubleshooting-steps-toosolve-502-bad-gateway-and-503-service-unavailable-errors"></a>Hibaelhárítási lépéseket toosolve "502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" hiba
Hibaelhárítás három különböző feladatokat, egymás utáni sorrendben osztható:

1. [Figyelje meg, és figyelheti az alkalmazások viselkedését](#observe)
2. [Adatok gyűjtése](#collect)
3. [Hello probléma elhárítása érdekében](#mitigate)

[App Service Web Apps](/services/app-service/web/) minden lépésnél különböző lehetőséget biztosít.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Figyelje meg, és figyelheti az alkalmazások viselkedését
#### <a name="track-service-health"></a>Nyomon követése szolgáltatásának állapota
A Microsoft Azure publicizes minden alkalommal, amikor a szolgáltatás megszakadásának vagy a teljesítmény romlását van. Hello szolgáltatás állapotának hello követheti a hello [Azure Portal](https://portal.azure.com/). További információkért lásd: [szolgáltatás állapotát nyomon](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>A webes alkalmazás figyelése
Ezzel a beállítással toofind, ha az alkalmazás problémáit. A webalkalmazás panelen kattintson a hello **kérések és hibák követése** csempére. Hello **metrika** panelen láthatja, adhat hozzá minden hello metrikákat.

Hello metrikákat, érdemes a webalkalmazás toomonitor vannak

* Munkakészlet átlagos memória
* Átlagos válaszidő
* CPU-idő
* Memória munkakészlete
* Kérelmek

![a figyelő a webalkalmazás 502 Hibás átjáró és 503-as szolgáltatás nem érhető el a HTTP-hibák megoldása felé](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

További információkért lásd:

* [Az Azure App Service Web-alkalmazások figyelése](web-sites-monitor.md)
* [Riasztási értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Adatok gyűjtése
#### <a name="use-hello-azure-app-service-support-portal"></a>Hello Azure App Service támogatási portál használata
Web Apps biztosít hello képességét tootroubleshoot problémák kapcsolódó tooyour webalkalmazás HTTP naplókat, eseménynaplók, folyamat memóriaképek és több megtekintésével. Végezheti el az összes ezt az információt a támogatási portálon, a **http://&lt;az alkalmazás neve >.scm.azurewebsites.net/Support**

hello Azure App Service-támogatás portal nyújt három különálló lapok toosupport hello három lépést a gyakori hibaelhárítási használatára:

1. Figyelje meg a jelenlegi viselkedése
2. Diagnosztikai adatok gyűjtése és futtató hello beépített elemzőkkel elemzése
3. Csökkentése

Ha most hello hiba történik, kattintson a **elemzés** > **diagnosztika** > **diagnosztizálása most** toocreate diagnosztikai munkamenet, amely összegyűjti a HTTP jelentkezik, az Eseménynapló, memóriát, kiírt fájlok, a PHP hibanaplókat és a PHP feldolgozni a jelentést.

Miután hello adatgyűjtés, emellett futtatása egy hello adatokon és is biztosítanak egy HTML-jelentést.

Abban az esetben, ha azt szeretné, toodownload hello adatok, alapértelmezés szerint, akkor legyen tárolva hello D:\home\data\DaaS mappában.

Hello Azure App Service támogatási portálon további információkért lásd: [új frissítések tooSupport hely bővítmény Azure webhelyekhez](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-hello-kudu-debug-console"></a>Hello Kudu hibakereső konzol használata
A hibakereső konzol hibakeresés, felfedezése, fájlok, valamint JSON végpontokat a környezettel kapcsolatos információk beolvasásakor feltöltése használható webes alkalmazásokat tartalmaz. Ez a lehetőség hello *Kudu konzol* vagy hello *SCM irányítópult* a webalkalmazás.

Érhető el ehhez az irányítópulthoz toohello hivatkozást fog **https://&lt;az alkalmazás neve >.scm.azurewebsites.net/**.

A Kudu biztosítja hello, amit a következők:

* az alkalmazás környezeti beállítások
* naplófolyamot
* diagnosztikai memóriakép
* a hibakeresési konzol, ahol futtathatja a Powershell-parancsmagok és alapvető DOS parancsok.

A Kudu egy másik fontos része, hogy de, abban az esetben, ha az alkalmazás első-alkalommal kivételek szűrész, használhatja a Kudu hello SysInternals eszköz Procdump toocreate memóriaképeket. Ezek memóriaképek pillanatképek hello folyamat, és gyakran segítségével bonyolultabb webalkalmazásokba elhárítása.

A Kudu szolgáltatásainak további információkért lásd: [Azure Websitesra online eszközök kell tudni](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a>3. Hello probléma elhárítása érdekében
#### <a name="scale-hello-web-app"></a>Skála hello webalkalmazás
Az Azure App Service a teljesítmény növelése és a teljesítményt, módosíthatja hello méretezési, amelyen az alkalmazást futtat. A webes alkalmazás vertikális felskálázásával magában foglalja a két kapcsolódó műveletek: az App Service csomag tooa magasabb tarifacsomagra vált, és bizonyos beállítások konfigurálása után magasabb tarifacsomagra toohello rendelkezik váltott módosítása.

A méretezés további információkért lásd: [egy webalkalmazás skálázása az Azure App Service](web-sites-scale.md).

Ezenkívül lehetősége van toorun egynél több példány az alkalmazást. Ez nem csak nyújt további feldolgozási képesség, de is lehetővé teszi bizonyos hibatűrést. Ha hello folyamat leáll egy példányán, hello más példányt fog továbbra is küldött kérelmek.

Hello toobe manuális vagy automatikus skálázás állíthatja be.

#### <a name="use-autoheal"></a>Elindulásáról használata
Elindulásáról hello (ilyen például a konfigurációs módosításokat, kérelmek, memória-alapú korlátok vagy hello idő szükséges tooexecute kérelmet) kiválasztott beállítások alapján az alkalmazás munkavégző folyamata újraindul. Legtöbbször ennek hello újrahasznosítást hello folyamat hello leggyorsabb módon toorecover a probléma. Mindig újraindíthatja hello webalkalmazást az közvetlenül belül hello Azure portálon, bár elindulásáról lesz automatikusan elvégezze ezt meg. Toodo szüksége néhány eseményindítók hozzáadása a webalkalmazáshoz hello legfelső szintű web.config fájlban. Vegye figyelembe, hogy ezek a beállítások akkor működnek a hello azonos módon akkor is, ha az alkalmazás nem egy .net egyet.

További információkért lásd: [automatikus javítás Azure webhelyek](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-hello-web-app"></a>Hello a webalkalmazás újraindítása
Ez gyakran a legegyszerűbb módja toorecover hello az egyszeri hibák. A hello [Azure Portal](https://portal.azure.com/), a webalkalmazás panelen hello beállítások toostop van, vagy indítsa újra az alkalmazást.

 ![Indítsa újra az alkalmazást toosolve HTTP-hibák 502 Hibás átjáró és 503-as szolgáltatás nem érhető el](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

A webalkalmazás Azure Powershell használatával is kezelheti. További információ: [Az Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md).

