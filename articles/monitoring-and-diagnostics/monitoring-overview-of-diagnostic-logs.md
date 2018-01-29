---
title: "Az Azure diagnosztikai naplók áttekintése |} Microsoft Docs"
description: "Ismerje meg az Azure diagnosztikai naplók és hogyan használhatja őket egy Azure-erőforrás belül bekövetkező események megértéséhez."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem; magoedte
ms.openlocfilehash: d59abde29fc7b73a799e5bf3659b02f824b693de
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Gyűjtése és felhasználása az Azure-erőforrások naplóadatait

## <a name="what-are-azure-resource-diagnostic-logs"></a>Mik az Azure-erőforrás diagnosztikai naplók
**Az Azure erőforrás-szintű diagnosztikai naplók** erőforrás a művelet részletes, gyakori adatait adja meg az erőforrás által kibocsátott naplók. Ezek a naplók tartalmának erőforrástípusok szerint változik. Hálózati biztonsági csoport szabály számlálók és a Key Vault-naplók például erőforrás naplók két kategóriájuk.

Erőforrás-szintű diagnosztikai naplók eltérnek a [tevékenységnapló](monitoring-overview-activity-logs.md). A műveletnapló erőforrást az előfizetésében Resource Manager használatával, például egy virtuális gép létrehozása vagy törlése a logikai alkalmazás a végrehajtott műveletek betekintést nyújt. A műveletnapló egy előfizetési szintű napló. Erőforrás-szintű diagnosztikai naplók Észreveheti az olyan műveletek végrehajtott belül az adott erőforrás, például a titkos kulcs lekérése a kulcstároló.

Erőforrás-szintű diagnosztikai naplók is eltérnek a vendég operációs rendszer szintű diagnosztikai naplókat. Vendég operációs rendszer diagnosztikai naplók, ezeket a virtuális gépek belül futó ügynök által gyűjtött vagy egyéb támogatott erőforrástípus. Erőforrás-szintű diagnosztikai naplók nem rögzítése és ügynök erőforrás-specifikus adatait az Azure platformon, igényelnek, amíg a vendég operációs rendszer szintű diagnosztikai naplók rögzítése az operációs rendszer és a virtuális gépen futó alkalmazások adatait.

Nem minden erőforrások támogatja az erőforrás diagnosztikai naplók az itt ismertetett új típusú. A cikkben egy szakaszt, amely felsorolja, milyen támogatja az új erőforrás szintű diagnosztikai naplók.

![Erőforrás diagnosztikai naplók és a naplók más típusú ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Teendők, az erőforrás-szintű diagnosztikai naplók
Az alábbiakban néhány erőforrás diagnosztikai naplók teheti:

![Az erőforrás diagnosztikai naplók logikai elhelyezése](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* Mentse azokat egy [ **Tárfiók** ](monitoring-archive-diagnostic-logs.md) naplózási vagy manuális ellenőrzést. A megőrzési ideje (nap) használatával is megadhat **erőforrás diagnosztikai beállításainak**.
* [Adatfolyam-őket **Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) egy külső szolgáltatás vagy az egyéni elemzési megoldások, például a Power bi szempontjából.
* Elemezheti őket a [OMS szolgáltatáshoz](../log-analytics/log-analytics-azure-storage.md)

Egy tárfiókhoz vagy az Event Hubs névtér, amely megegyezik a naplók kibocsátó ugyanahhoz az előfizetéshez nincs használható. A beállítás konfiguráló felhasználónak mindkét előfizetéshez megfelelő RBAC hozzáféréssel kell rendelkeznie.

## <a name="resource-diagnostic-settings"></a>Erőforrás diagnosztikai beállítások
Erőforrás diagnosztikai naplókat a további nem-számítási erőforrások erőforrás diagnosztikai beállításainak használatával vannak konfigurálva. **Erőforrás diagnosztikai beállításainak** egy erőforrás-vezérlő:

* Ha erőforrás diagnosztikai naplók és a metrikák küldése (Storage-fiók, az Event Hubs, és/vagy az OMS szolgáltatáshoz).
* Napló kategóriák kerülnek, és hogy metrikaadatokat is lett van küldve.
* Mennyi ideig napló kategóriákhoz rendszer meddig őrizze meg a storage-fiók
    - Egy nulla napos megőrzési azt jelenti, hogy a naplók végtelen tartanak. Ellenkező esetben a értéke lehet bármely 1 és 2147483647 között eltelt napok számát.
    - Ha a megőrzési házirend-beállításokat, de naplók tárolása a Storage-fiók le van tiltva, (például, ha csak az Event Hubs vagy OMS beállítások vannak jelölve), az adatmegőrzési hatástalan.
    - Adatmegőrzési alkalmazott napi,, így napi (UTC) szerint naplókat, amelyik most már a megőrzési túl napjától végén házirend törlődnek. Például ha egy nap adatmegőrzési, mai nap kezdetén a napló, a nap előtt tegnap törlése akkor történik meg.

Ezek a beállítások könnyen vannak konfigurálva, az Azure portálon erőforrás diagnosztikai beállításait, Azure PowerShell és a parancssori felület parancsait, vagy keresztül a [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!WARNING]
> Diagnosztikai naplók és a vendég operációs rendszer rétegből számítási erőforrások (például a virtuális gépek vagy a Service Fabric) által használt metrikáját [konfigurációs és kimenetek kiválasztása külön mechanizmusát](../azure-diagnostics.md).
>
>

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a>Az erőforrás diagnosztikai naplók gyűjtésének engedélyezése
Az erőforrás diagnosztikai naplók gyűjtésének engedélyezhető [erőforrás létrehozása a Resource Manager-sablon részeként](./monitoring-enable-diagnostic-logs-using-template.md) vagy a portál az adott erőforrás oldalról erőforrás létrehozása után. Azure PowerShell vagy a CLI-parancsok használatával, vagy a Azure REST API használatával bármikor gyűjtemény is engedélyezheti.

> [!TIP]
> Ezek az utasítások nem feltétlenül vonatkozik közvetlenül minden erőforráshoz. Tekintse meg a séma hivatkozásokat, különleges lépések, amelyek érvényesek az egyes erőforrástípusok megérteni a lap alján.
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a>Az erőforrás diagnosztikai naplókat a portálon gyűjtésének engedélyezése
Engedélyezheti az erőforrás diagnosztikai naplók az Azure portálon gyűjtemény egy erőforrás vagy egy adott erőforrás címen, vagy navigáljon az Azure-figyelő létrehozása után. Ennek engedélyezéséhez Azure figyelő keresztül:

1. Az a [Azure-portálon](http://portal.azure.com), navigáljon az Azure-figyelő, és kattintson a **diagnosztikai beállítások**

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. Opcionálisan erőforráscsoport és erőforrások típus szerint a lista szűréséhez, majd kattintson az erőforráson, amelynek szeretné beállítani a diagnosztikai.

3. Ha a beállítások nem található az erőforrás a választott, kéri beállítás létrehozása. Kattintson a "Diagnosztika bekapcsolásához."

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Ha az erőforrás-meglévő beállítások, látni fogja már ehhez az erőforráshoz konfigurált beállítások listája. A "Hozzáadás diagnosztikai beállításának."

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Adjon a beállítás nevét, jelölje be minden célhelyéhez, amelyet szeretne adatokat küldeni, és konfigurálja, hogy melyik erőforrást minden egyes célhelyhez használt. Beállíthatja a napokban ezek a naplók segítségével számos a **megőrzés (nap)** csúszkák (fiók célhelyet csak érvényes). Egy nulla napos megőrzési határozatlan ideig tárolja a naplókat.
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Kattintson a **Save** (Mentés) gombra.

Néhány másodpercen belül az új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplókat a megadott célhelyre küldött, amint létrejön az új esemény-adatokat.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Az erőforrás diagnosztikai naplók PowerShell gyűjtésének engedélyezése
Az erőforrás diagnosztikai naplók Azure PowerShell gyűjtésének engedélyezéséhez használja a következő parancsokat:

Ahhoz, hogy a storage-fiókok a diagnosztikai naplók tárolására, az alábbi parancsot használja:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

A tárfiók azonosítója az erőforrás-azonosítója, amelyhez hozzá szeretné küldeni a naplókat a tárfiók.

Adatfolyamként való küldése a diagnosztikai naplók eseményközpontokba való engedélyezéséhez az alábbi parancsot használja:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

A service bus Szabályazonosító: karakterlánc a következő formátumban: `{Service Bus resource ID}/authorizationrules/{key name}`.

A diagnosztikai naplókat a Naplóelemzési munkaterület küldésének engedélyezéséhez az alábbi parancsot használja:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Az erőforrás-azonosítója a Naplóelemzési munkaterület a következő paranccsal szerezheti be:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Ezek a paraméterek ahhoz, hogy több kimenet beállításai kombinálhatja.

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a>Az erőforrás diagnosztikai naplók parancssori felületen keresztül gyűjtésének engedélyezése
Ahhoz, hogy az erőforrás diagnosztikai naplók az Azure parancssori felület használatával gyűjteménye, a következő parancsokkal:

Ahhoz, hogy a Storage-fiókok a diagnosztikai naplók tárolására, az alábbi parancsot használja:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

A tárfiók azonosítója az erőforrás-azonosítója, amelyhez hozzá szeretné küldeni a naplókat a tárfiók.

Adatfolyamként való küldése a diagnosztikai naplók eseményközpontokba való engedélyezéséhez az alábbi parancsot használja:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

A service bus Szabályazonosító: karakterlánc a következő formátumban: `{Service Bus resource ID}/authorizationrules/{key name}`.

A diagnosztikai naplókat a Naplóelemzési munkaterület küldésének engedélyezéséhez az alábbi parancsot használja:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
```

Ezek a paraméterek ahhoz, hogy több kimenet beállításai kombinálhatja.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Az erőforrás diagnosztikai naplók REST API-n keresztül gyűjtésének engedélyezése
A Azure REST API használatával diagnosztikai beállítások módosításához lásd [Ez a dokumentum](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>A portál erőforrás diagnosztikai beállítások kezelése
Győződjön meg arról, hogy az erőforrások vannak beállítva a diagnosztikai beállítások. Navigáljon a **figyelő** a portálon, és nyissa meg a **diagnosztikai beállítások**.

![Diagnosztikai naplók panelen a portálon](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Lehet, hogy kattintson a "További szolgáltatás" a figyelő szakaszban találja.

Itt megtekintheti és összes erőforrást, amely támogatja a diagnosztikai beállításokat, hogy azok rendelkeznek-e engedélyezve diagnosztikai szűréséhez. Lásd: is lebontva, ha több beállítások erőforrás van beállítva, és ellenőrizze, melyik tárfiók, az Event Hubs névtér és/vagy adatok halad a Naplóelemzési munkaterület.

![Diagnosztikai naplók eredmények a portál](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Megjeleníti egy diagnosztikai beállítás hozzáadása a diagnosztikai beállítások nézet, ahol engedélyezheti, letiltása, vagy módosíthatja a kiválasztott erőforrás diagnosztikai beállításait.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Támogatott szolgáltatások, a kategóriák és a sémák erőforrás diagnosztikai naplók
[Ebben a cikkben találhat](monitoring-diagnostic-logs-schema.md) támogatott szolgáltatások és a napló kategóriák és a szolgáltatások által használt sémák teljes listáját.

## <a name="next-steps"></a>Következő lépések

* [Adatfolyam-erőforrás diagnosztikai naplók **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [A Azure REST API használatával erőforrás diagnosztikai beállításainak módosítása](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [A Naplóelemzési az Azure storage naplóinak elemzése](../log-analytics/log-analytics-azure-storage.md)
