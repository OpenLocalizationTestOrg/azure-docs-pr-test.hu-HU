---
title: "aaaEnable figyelése és a Microsoft Azure diagnostics |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset mentése az Azure-erőforrások diagnosztika."
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: af1947a9-c211-4aa1-8924-880a86240be4
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: 865d010c5846fff6d871e20eca2bc4ac35028354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-monitoring-and-diagnostics"></a>Figyelési és diagnosztikai funkciók engedélyezése
A hello [Azure Portal](https://portal.azure.com), az erőforrásokra vonatkozó gazdag, gyakori, figyelés és diagnosztikai adatokat is beállíthat. Is használhatja a hello [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) vagy [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooconfigure diagnosztika programozott módon.

A kiválasztott tárfiók mentéskor diagnosztika, figyelés és metrika adatok Azure-ban. Ez lehetővé teszi, hogy toouse bármilyen tooling eszköz meg szeretné, hogy tooread hello adatait, a Tártallózó tooPower BI toothird féltől tooling.

## <a name="when-you-create-a-resource"></a>Amikor egy erőforrást hoz létre
A legtöbb a szolgáltatások lehetővé teszik tooenable diagnosztika létrehozásakor először őket hello [Azure Portal](https://portal.azure.com).

1. Nyissa meg túl**új** és érdekli hello erőforrás kiválasztása.
2. Válassza ki **opcionális konfigurációs**.
    ![Diagnosztika panel](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)
3. Válassza ki **diagnosztika**, és kattintson a **a**. Szüksége lesz toochoose hello Storage-fiók, amelyet az diagnosztika toobe mentve. Ön számítjuk fel a tároló és a tranzakciókért a megszokott adatforgalmi díjakat diagnosztikai tooa tárfiók küldésekor.
4. Kattintson a **OK** és hello erőforrás létrehozása.

## <a name="change-settings-for-an-existing-resource"></a>Meglévő erőforrás-beállításainak módosítása
Ha már létrehozott egy erőforrást, és azt szeretné, hogy toochange hello diagnosztikai beállítások (adatgyűjtés, például toochange hello szintjén), hogy közvetlenül hello Azure portálon teheti meg.

1. Nyissa meg toohello erőforrás és hello **beállítások** parancsot.
2. Válassza ki **diagnosztika**.
3. Hello **diagnosztika** panel rendelkezik az összes hello lehetséges diagnosztika és a gyűjtemény adatait az adott erőforrás figyelését. Az egyes erőforrások is beállíthatja egy **megőrzési** hello adatok esetén tooclean házirend importáljon a tárfiókba azt be.
    ![Tárolási diagnosztika](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)
4. Ha a beállítások választotta, kattintson a hello **mentése** parancsot. Szükség lehet egy kicsit közben az adatok tooshow figyelési fel, ha engedélyezi az hello az első alkalommal a.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Az adatgyűjtés a virtuális gépek kategóriák
A virtuális gépek metrikák és a naplók lesznek rögzítve egy perces időközönként, tehát mindig hello legtöbb naprakész információt a géphez tudnivalók.

* **Alapvető metrikák** : állapotfigyelő metrikák kapcsolatos a virtuális gép, például a processzor és memória
* **Hálózati és webes metrikák** : a hálózati kapcsolatok és webszolgáltatások metrikák
* **.NET-metrikák** : a virtuális gépen futó hello .NET és az ASP.NET alkalmazások kapcsolatos metrikákat
* **SQL-metrikák** : Ha a Microsoft SQL-szolgáltatás, a metrikák futtatja
* **Windows alkalmazás-eseménynaplói** : toohello alkalmazás csatorna küldött Windows-események
* **A Windows rendszer-eseménynaplói** : toohello rendszer csatorna küldött Windows-eseményeket. Ez is magában foglalja az összes esemény [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
* **A Windows biztonsági eseménynaplói** : toohello biztonsági csatorna küldött Windows-események
* **Diagnosztika infrastruktúra naplók** : hello diagnosztika gyűjtemény infrastruktúrával kapcsolatos naplózás
* **IIS-napló** : az IIS-kiszolgáló kapcsolatos információkat naplózza

Ne feledje, hogy jelenleg nem támogatottak az egyes Linux terjesztéseket, hello virtuális gép hello Vendégügynököt kell telepíteni.

## <a name="next-steps"></a>Következő lépések
* [Riasztási értesítéseket kaphat](insights-receive-alert-notifications.md), ha működési események történnek vagy a mérőszámok átlépnek egy küszöbértéket.
* [Szolgáltatás metrikát](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
* [Automatikus méretezése a példányok száma](insights-how-to-scale.md) toomake meg arról, hogy a szolgáltatás méretezési igény szerint.
* [Az alkalmazások teljesítményének figyelésére](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, toounderstand pontosan hogyan a kód hajt végre hello felhőben.
* [Események és tevékenységnapló](insights-debugging-with-events.md) toolearn minden, ami következett be a szolgáltatásban.
* [Nyomon követheti a szolgáltatás állapotát](insights-service-health.md) toofind, amikor Azure észlelt a teljesítmény romlását vagy a szolgáltatás megszakítás mellett folytathatja.

