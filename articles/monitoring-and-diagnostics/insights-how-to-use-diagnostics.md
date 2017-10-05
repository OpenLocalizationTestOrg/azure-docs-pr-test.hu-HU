---
title: "Figyelés engedélyezésekor és diagnosztika a Microsoft Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan állíthat be az Azure-erőforrások diagnosztika."
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
ms.openlocfilehash: b82bb1ab419831e803689edb2a2a7fe256dde5a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-monitoring-and-diagnostics"></a>Figyelési és diagnosztikai funkciók engedélyezése
Az a [Azure Portal](https://portal.azure.com), az erőforrásokra vonatkozó gazdag, gyakori, figyelés és diagnosztikai adatokat is beállíthat. Használhatja a [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) vagy [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) diagnosztika programozott úton történő konfigurálását.

A kiválasztott tárfiók mentéskor diagnosztika, figyelés és metrika adatok Azure-ban. Ez lehetővé teszi, hogy bármilyen tooling szeretné beolvasni az adatokat, a Tártallózó külső eszközt használunk erre a Power bi-bA használja.

## <a name="when-you-create-a-resource"></a>Amikor egy erőforrást hoz létre
A legtöbb szolgáltatások lehetővé teszik diagnosztika engedélyezése, amikor Ön először létre kell hoznia azokat a [Azure Portal](https://portal.azure.com).

1. Ugrás a **új** , és válassza ki a kívánt erőforrást.
2. Válassza ki **opcionális konfigurációs**.
    ![Diagnosztika panel](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)
3. Válassza ki **diagnosztika**, és kattintson a **a**. Szüksége lesz a diagnosztika a menteni kívánt tárfiókot. Ön számítjuk fel a tároló és a tranzakciókért a megszokott adatforgalmi díjakat diagnosztikai tárfiók küldésekor.
4. Kattintson a **OK** és az erőforrás létrehozása.

## <a name="change-settings-for-an-existing-resource"></a>Meglévő erőforrás-beállításainak módosítása
Ha már létrehozott egy erőforrást, és módosítja a diagnosztikai beállításokat (például módosíthatja az adatgyűjtés szintjét), azt, hogy közvetlenül az Azure portálon teheti meg.

1. Nyissa meg az erőforrást, majd kattintson a **beállítások** parancsot.
2. Válassza ki **diagnosztika**.
3. A **diagnosztika** panel rendelkezik az összes lehetséges diagnosztika és az adott erőforrás figyelési gyűjtemény adatait. Az egyes erőforrások is beállíthatja egy **megőrzési** az adatok tisztítása a storage-fiókhoz tartozó házirend.
    ![Tárolási diagnosztika](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)
4. Ha a beállítások választotta, kattintson a **mentése** parancsot. Szükség lehet egy kicsit miközben figyelemmel kísérésére jelenik meg, ha engedélyezi az első alkalommal adatokat.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Az adatgyűjtés a virtuális gépek kategóriák
A virtuális gépek metrikák és a naplók lesznek rögzítve egy perces időközönként, így mindig a legfrissebb információk a géppel kapcsolatos.

* **Alapvető metrikák** : állapotfigyelő metrikák kapcsolatos a virtuális gép, például a processzor és memória
* **Hálózati és webes metrikák** : a hálózati kapcsolatok és webszolgáltatások metrikák
* **.NET-metrikák** : a .NET- és az ASP.NET alkalmazásokat, a virtuális gépen futó kapcsolatos metrikákat
* **SQL-metrikák** : Ha a Microsoft SQL-szolgáltatás, a metrikák futtatja
* **Windows alkalmazás-eseménynaplói** : az alkalmazás csatorna küldött Windows-események
* **A Windows rendszer-eseménynaplói** : a rendszer csatorna küldött Windows-eseményeket. Ez is magában foglalja az összes esemény [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
* **A Windows biztonsági eseménynaplói** : a biztonsági csatorna küldött Windows-események
* **Diagnosztika infrastruktúra naplók** : a diagnosztika gyűjtemény infrastruktúrával kapcsolatos naplózás
* **IIS-napló** : az IIS-kiszolgáló kapcsolatos információkat naplózza

Vegye figyelembe, hogy jelenleg nem támogatottak bizonyos Linux terjesztéseket, és a Vendég ügynököt telepíteni kell a virtuális gépen.

## <a name="next-steps"></a>Következő lépések
* [Riasztási értesítéseket kaphat](insights-receive-alert-notifications.md), ha működési események történnek vagy a mérőszámok átlépnek egy küszöbértéket.
* [Szolgáltatás metrikát](insights-how-to-customize-monitoring.md) ellenőrizze, hogy a szolgáltatás elérhető, és a gyors.
* [Automatikus méretezése a példányok száma](insights-how-to-scale.md) való győződjön meg arról, hogy a szolgáltatás méretezési igény szerint.
* [Az alkalmazások teljesítményének figyelésére](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné tudni, pontosan hogyan a kód működik-e a felhőben.
* [Események és tevékenységnapló](insights-debugging-with-events.md) további minden, ami következett be a szolgáltatásban.
* [Nyomon követheti a szolgáltatás állapotát](insights-service-health.md) szeretné megtudni, mikor Azure észlelt a teljesítmény romlását vagy a szolgáltatás megszakítás mellett folytathatja.

