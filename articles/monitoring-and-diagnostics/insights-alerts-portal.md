---
title: "Azure-szolgáltatások - aaaCreate értesítések az Azure portálon |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a webhely URL-címek (webhookok), vagy az automation megadott hello feltételek teljesülése esetén hívható."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 78d862d25255cda9fdfe347329e908a471c39846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - Azure-portálon
> [!div class="op_single_selector"]
> * [Portál](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Parancssori felület](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan használja az Azure metrika figyelmeztetéseket tooset hello Azure-portálon.   

A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.

* **Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ. Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.    
* **Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be. További információk a napló tevékenységriasztásokat toolearn [kattintson ide](monitoring-activity-log-alerts.md)

A metrika riasztási toodo hello követően amikor elindítja a konfigurálhatja:

* e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése
* e-mail küldése a megadott tooadditional e-maileket.
* A webhook hívása
* egy Azure-runbook (csak az Azure-portálon hello) végrehajtásának elindítása

Konfigurálhatja és metrika riasztási szabályok adatainak beolvasása

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [parancssori felület (CLI)](insights-alerts-command-line-interface.md)
* [Az Azure figyelő REST API-n](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a>Riasztási szabályt létrehozni egy metrika hello Azure-portálon
1. A hello [portal](https://portal.azure.com/), keresse meg a hello erőforrás figyelési érdekli, és válassza ki azt.

2. Válassza ki **riasztások** vagy **riasztási szabályok** hello figyelés szakasz alatt. hello szöveg és ikon eltérő lehet attól függően némileg különböző erőforrások.  

    ![Figyelés](./media/insights-alerts-portal/AlertRulesButton.png)

3. Jelölje be hello **riasztás hozzáadása** parancsot, majd hello mezők kitöltésével.

    ![Riasztások hozzáadása](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Név** a riasztás szabályt, majd válassza ki a **leírása**, amely értesítési e-mailt is mutatja.

5. Jelölje be hello **metrika** toomonitor szeretné, majd kattintson egy **feltétel** és **küszöbérték** hello metrika értékét. Hello is választott **időszak** metrika hello idő szabály kell teljesíteni hello riasztási eseményindítók előtt. Így például hello időszak "PT5M" használ, és 80 % fölötti CPU keresi a riasztás, ha hello riasztás váltja ki, ha hello CPU már következetesen fenti 80 % 5 perc. Amennyiben az első eseményindító hello következik be, azt újra váltja ki, ha 5 percig 80 % alatt marad hello CPU. hello CPU mérési 1 percenként történik.   

6. Ellenőrizze **E-mail-tulajdonosok...**  Ha azt szeretné, hogy a rendszergazdák és a társadminisztrátorok toobe e-mailben amikor hello riasztási következik be.

7. Ha azt szeretné, hogy további e-mailek tooreceive egy értesítést, ha hello riasztási következik be, vegye fel a hello **további rendszergazda email(s)** mező. Több e-mailek külön és pontosvesszővel kell elválasztani -  *email@contoso.com;email2@contoso.com*

8. Be egy érvényes URI-azonosító található hello **Webhook** mező Ha azt szeretné, az úgynevezett amikor hello riasztási következik be.

9. Azure Automation használatakor választhatja a Runbook toobe hello riasztás aktiválódásakor futtassa.

10. Válassza ki **OK** Amikor kész toocreate hello riasztás.   

Néhány percen belül hello riasztás aktív, és elindítja a leírt módon.

## <a name="managing-your-alerts"></a>A riasztások kezelése
Miután létrehozott egy riasztást, kijelölheti azt és:

* Egy grafikonon hello metrika küszöbérték és a tényleges értékek hello hello előző nap megtekintése.
* Szerkesztheti és törölheti azt.
* **Tiltsa le a** vagy **engedélyezése** azt, ha azt szeretné tootemporarily stop, vagy folytassa a riasztás-mailjeire.

## <a name="next-steps"></a>Következő lépések
* [Az Azure Figyelés áttekintése](monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.
* További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).
* További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).
* További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).
* Első egy [diagnosztikai naplók áttekintése](monitoring-overview-of-diagnostic-logs.md) és begyűjtése részletes nagyon gyakori a szolgáltatásban.
* Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
