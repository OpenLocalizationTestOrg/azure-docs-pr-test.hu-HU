---
title: "Azure Application Insights aaaSet riasztások |} Microsoft Docs"
description: "Értesítés válaszidejű, kivételek, és egyéb teljesítmény vagy a webalkalmazás használatának változásairól."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e160cb173e68fda2e6d97f29da342c46b7ac4f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-alerts-in-application-insights"></a>Az Application Insightsban riasztások beállítása
[Az Azure Application Insights] [ start] riasztást küldhet, a teljesítmény vagy a használati metrikák a webalkalmazáson belüli toochanges. 

Az Application Insights figyeli az élő app egy [számos platformon] [ platforms] toohelp teljesítménnyel kapcsolatos problémák diagnosztizálásához és használati módokba.

Riasztások három fő típusba sorolhatók:

* **Metrika riasztások** jelzi, hogy ha egy metrika áthalad bizonyos időszakokra – például a válaszidőt, a kivételek száma, a CPU-használat vagy a lapmegtekintések egy küszöbértéket. 
* [**Webalkalmazás-tesztek** ] [ availability] mondja el, ha a webhely nem érhető el a hello interneten vagy lassan válaszol. [További][availability].
* [**Proaktív diagnosztika** ](app-insights-proactive-diagnostics.md) konfigurálása automatikusan toonotify szokatlan teljesítmény minták kapcsolatban.

Azt a cikkben metrika riasztások összpontosítanak.

## <a name="set-a-metric-alert"></a>A metrika-riasztások beállítása
Nyissa meg hello riasztási szabályok panelen, majd használja hello Hozzáadás gomb. 

![Hello riasztási szabályok paneljén válassza riasztási hozzáadása. Erőforrás toomeasure hello beállítani az alkalmazás, hello riasztás nevét adja meg, és válassza ki a metrika.](./media/app-insights-alerts/01-set-metric.png)

* Mielőtt hello hello erőforrás más tulajdonságainak beállítása. **Válassza ki a "(összetevők)" hello erőforrás** Ha azt szeretné, hogy a teljesítmény vagy a használati metrikák tooset ki riasztást.
* toohello riasztás rendelt hello név hello erőforráscsoportban (ne csak az alkalmazás) egyedinek kell lennie.
* Lehet, amelyben kéri tooenter hello küszöbérték gondos toonote hello egység.
* Ha bejelöli a "... E-mail tulajdonosok" hello mezőben, riasztások hozzáférés toothis erőforráscsoport, aki e-mailek tooeveryone által küldött. Ez beállítva fel, tooexpand toohello adja hozzá [erőforráscsoportba vagy előfizetésbe](app-insights-resources-roles-access-control.md) (nem hello erőforrás).
* "További e-mailek" adja meg, ha toothose személyek vagy csoportok (-e ellenőrzött hello "e-mail-tulajdonosok..." mezőben) értesítések küldése. 
* Állítsa be a [webhook cím](../monitoring-and-diagnostics/insights-webhooks-alerts.md) Ha meg van adva egy webes alkalmazás, amely tooalerts válaszol. A neve egyaránt hello riasztás aktiválásakor, és ha meg van oldva. (De vegye figyelembe, hogy jelenleg lekérdezési paraméterek nem továbbítja a rendszer webhook tulajdonságként.)
* Bármikor letilthatja vagy engedélyezése hello riasztás: hello gombok hello panel hello tetején.

*Hello Hozzáadás riasztási gomb nem látható.* 

* Egy szervezeti fiók használatával? Riasztások állíthatja be, ha tulajdonosa vagy közreműködője hozzáférés toothis alkalmazás erőforrás. Vessen egy pillantást hello hozzáférés-vezérlés panelen. [További tudnivalók a hozzáférés-vezérlés][roles].

> [!NOTE]
> Hello riasztások panelen láthatja, hogy már van egy riasztási beállítva: [proaktív diagnosztika](app-insights-proactive-failure-diagnostics.md). hello automatikus riasztás egy adott metrika, hibaarányú figyeli. Kivéve toodisable hello proaktív riasztás mellett dönt, a saját hibaarányú riasztás tooset nincs szükség. 
> 
> 

## <a name="see-your-alerts"></a>A riasztások megtekintéséhez
Ha egy riasztás módosításokat állam e-mailt kap aktív és inaktív között. 

Minden riasztás aktuális állapota hello hello riasztási szabályok panelen látható.

Nincs a hello riasztások legutóbbi tevékenységét legördülő:

![Riasztások legördülő](./media/app-insights-alerts/010-alert-drop.png)

azon állapotváltozások hello előzmények hello tevékenységnapló kell megadni:

![Hello áttekintése paneljén kattintson a beállítások, a naplókat](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>A riasztások működése
* Riasztás három állapota van: "Soha nem aktivált", "Aktív" és "Feloldva." Aktivált azt jelenti, hogy a feltétel igaz értéke esetén volt, amikor a legutóbbi értékelésének megadott hello.
* Értesítés jön létre, amikor egy riasztás állapota. (Ha hello riasztási feltétel hello riasztás létrehozása után már nem igaz, nem kaphat értesítést amíg hello feltétel hamis kerül.)
* Minden értesítési e-mailt állít elő, ha be van jelölve hello e-mailek mezőbe, vagy a megadott e-mail címet. Is megtalálhatja hello értesítések legördülő listából.
* Riasztás minden alkalommal, amikor egy metrika megérkeznek, de más módon nem értékeli.
* hello értékelési hello metrika aggregátumok hello megelőző időszak alatt, és toohello küszöbérték toodetermine hello új állapot összehasonlítja.
* választott időszak hello időköze hello amelyben metrikák összesítése. Nem befolyásolja, hogy milyen gyakran hello riasztás ki lesz értékelve: hello gyakoriság metrikák érkezési függ.
* Nincs adat érkezik egy adott metrika egy kis ideig, ha hello résnek rendelkezik különböző hatások riasztási kiértékelési és a metrika explorer hello diagramokat. A metrika Intézőben adatot nem látható több mint hello diagram mintavételi időköze, ha hello diagram jeleníti meg a 0 érték. De a riasztás hello alapján ugyanazt a metrika nem lehet reevaluated, és hello riasztás változatlan állapotban marad. 
  
    Végül érkező adatokat, amikor hello diagram egyszer hátsó tooa nullától eltérő értékre. hello riasztás kiértékeli a megadott hello időszak hello adatok alapján. Ha új adatpont hello hello csak az egyik hello időtartamon belül, összesített hello alapul, csak az, hogy az adatpontok.
* Riasztást vibrálását gyakran közötti riasztási és kifogástalan állapotok, még akkor is, ha hosszú ideig állít be. Ez akkor fordulhat elő, ha hello Átjárómetrika értékeként körül hello küszöbérték felett áll. Nem kapott van hello küszöb: hello áttűnés tooalert: ugyanaz, mint hello áttűnés toohealthy érték hello történik.

## <a name="what-are-good-alerts-tooset"></a>Mik azok a helyes riasztások tooset?
Ez az alkalmazás függ. toostart, akkor rendelkezik az ajánlott nem tooset túl sok metrikákat. Szánjon némi időt, az alkalmazás futtatása közben a metrika diagramok megnézi tooget viselkedését leíró általában a abba. Ez az eljárás segítségével található módon tooimprove a teljesítményét. Majd állítsa be riasztások tootell, amikor hello metrikák hello normál zóna kívül. 

Népszerű riasztások a következők:

* [Böngésző metrikák][client], különösen a böngésző **lapon a betöltési idők**, webes alkalmazásokhoz megfelelőek. Ha az oldala sok parancsfájlok, kell keresnie az **böngésző kivételek**. A sorrend tooget a metrikák és a riasztások van tooset [weblap figyelési][client].
* **Kiszolgáló válaszideje** hello kiszolgálóoldali webalkalmazások. Riasztások beállítása, valamint nyomon követheti a metrika toosee a Ha aránytalanul sok aránnyal változik: variation utalhat, hogy az alkalmazás fut-e elegendő erőforrással. 
* **Kivételek** -toosee őket, néhány rendelkezik toodo [további telepítési](app-insights-asp-net-exceptions.md).

Ne feledje, hogy [proaktív hiba sebesség diagnosztika](app-insights-proactive-failure-diagnostics.md) automatikusan figyelő hello sebesség, amellyel az alkalmazás hiba kódokkal toorequests válaszol. 

## <a name="automation"></a>Automatizálás
* [Riasztások beállítása PowerShell tooautomate használata](app-insights-powershell-alerts.md)
* [Webhook tooautomate válaszol tooalerts használata](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Lásd még:
* [A webteszt rendelkezésre állása](app-insights-monitor-web-app-availability.md)
* [Riasztások beállítása automatizálásához](app-insights-powershell-alerts.md)
* [Proaktív diagnosztika](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

