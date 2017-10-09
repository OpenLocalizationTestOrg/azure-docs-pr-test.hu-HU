---
title: "az Azure diagnosztikai naplók aaaOverview |} Microsoft Docs"
description: "Ismerje meg az Azure diagnosztikai naplók és hogyan használhatja őket egy Azure-erőforrás belül előforduló toounderstand események."
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
ms.openlocfilehash: e38991c540626b4bb5b5b9a995276881ee58f368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Gyűjtése és felhasználása az Azure-erőforrások naplóadatait

## <a name="what-are-azure-resource-diagnostic-logs"></a>Mik az Azure-erőforrás diagnosztikai naplók
**Az Azure erőforrás-szintű diagnosztikai naplók** nyújtanak részletes, gyakori adatokat erőforrás hello műveletet az erőforrás által kibocsátott naplók. Ezek a naplók tartalmának hello erőforrástípusok szerint változik. Hálózati biztonsági csoport szabály számlálók és a Key Vault-naplók például erőforrás naplók két kategóriájuk.

Erőforrás-szintű diagnosztikai naplók eltér a hello [tevékenységnapló](monitoring-overview-activity-logs.md). hello tevékenységnapló erőforrást az előfizetésében Resource Manager használatával, például egy virtuális gép létrehozása vagy törlése a logikai alkalmazás a végrehajtott műveletek hello betekintést nyújt. hello tevékenységnapló egy előfizetési szintű naplót. Erőforrás-szintű diagnosztikai naplók Észreveheti az olyan műveletek végrehajtott belül az adott erőforrás, például a titkos kulcs lekérése a kulcstároló.

Erőforrás-szintű diagnosztikai naplók is eltérnek a vendég operációs rendszer szintű diagnosztikai naplókat. Vendég operációs rendszer diagnosztikai naplók, ezeket a virtuális gépek belül futó ügynök által gyűjtött vagy egyéb támogatott erőforrástípus. Erőforrás-szintű diagnosztikai naplók szükséges nincs rögzítése és ügynök erőforrás-specifikus adatok hello Azure platformon, a, amíg a vendég operációs rendszer szintű diagnosztikai naplók hello operációs rendszer és a virtuális gépen futó alkalmazások adatait rögzíti.

Nem minden erőforrások támogatja hello új az itt leírt erőforrás diagnosztikai naplókat. A cikkben melyik erőforrástípusok hello új erőforrás szintű diagnosztikai naplók szakasz lista tartalmazza.

![Erőforrás diagnosztikai naplók és a naplók más típusú ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Teendők, az erőforrás-szintű diagnosztikai naplók
Az alábbiakban néhány hello lehetőségek az erőforrás diagnosztikai naplók:

![Az erőforrás diagnosztikai naplók logikai elhelyezése](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* Mentse őket tooa [ **Tárfiók** ](monitoring-archive-diagnostic-logs.md) naplózási vagy manuális ellenőrzést. Hello megőrzési ideje (nap) használatával is megadhat **erőforrás diagnosztikai beállításainak**.
* [Túl adatfolyam formájában**Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) egy külső szolgáltatás vagy az egyéni elemzési megoldások, például a Power bi szempontjából.
* Elemezheti őket a [OMS szolgáltatáshoz](../log-analytics/log-analytics-azure-storage.md)

Használhatja a storage-fiók, vagy az Event Hubs névtér, amely nem része hello ugyanahhoz az előfizetéshez, hello egy kibocsátó naplókat. hello beállítás konfiguráló hello felhasználónak rendelkeznie kell hello megfelelő Szerepalapú hozzáférés tooboth előfizetések.

## <a name="resource-diagnostic-settings"></a>Erőforrás diagnosztikai beállítások
Erőforrás diagnosztikai naplókat a további nem-számítási erőforrások erőforrás diagnosztikai beállításainak használatával vannak konfigurálva. **Erőforrás diagnosztikai beállításainak** egy erőforrás-vezérlő:

* Ha erőforrás diagnosztikai naplók és a metrikák küldése (Storage-fiók, az Event Hubs, és/vagy az OMS szolgáltatáshoz).
* Napló kategóriák kerülnek, és hogy metrikaadatokat is lett van küldve.
* Mennyi ideig napló kategóriákhoz rendszer meddig őrizze meg a storage-fiók
    - Egy nulla napos megőrzési azt jelenti, hogy a naplók végtelen tartanak. Ellenkező esetben a hello értéke lehet bármely 1 és 2147483647 között eltelt napok számát.
    - Ha a megőrzési házirend-beállításokat, de naplók tárolása a Storage-fiók le van tiltva, (például, ha csak az Event Hubs vagy OMS beállítások vannak jelölve), hello adatmegőrzési hatástalan.
    - Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét, amelyik most már túl hello adatmegőrzési hello nap naplózza a rendszer törli. Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók törlése akkor történik meg.

Ezek a beállítások könnyen vannak konfigurálva, hello diagnosztikai beállítások hello Azure-portálon az erőforráshoz tartozó keresztül, Azure PowerShell és a parancssori felület parancsai keresztül vagy keresztül hello [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!WARNING]
> Diagnosztikai naplók és rétegből hello vendég operációs rendszer számítási erőforrások (például a virtuális gépek vagy a Service Fabric) által használt metrikáját [konfigurációs és kimenetek kiválasztása külön mechanizmusát](../azure-diagnostics.md).
>
>

## <a name="how-tooenable-collection-of-resource-diagnostic-logs"></a>Hogyan erőforrás diagnosztikai naplók tooenable gyűjteménye
Az erőforrás diagnosztikai naplók gyűjtésének engedélyezhető [erőforrás létrehozása a Resource Manager-sablon részeként](./monitoring-enable-diagnostic-logs-using-template.md) vagy az adott erőforrás oldalról hello portálon erőforrás létrehozása után. Azure PowerShell vagy a CLI-parancsok használatával, vagy hello Azure figyelő REST API használatával bármikor gyűjtemény is engedélyezheti.

> [!TIP]
> Ezek az utasítások nem feltétlenül vonatkozik közvetlenül tooevery erőforrás. Tekintse meg a hello séma hivatkozásokat a lap toounderstand különleges lépések toocertain erőforrástípusok alkalmazandó hello alján.
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-hello-portal"></a>Az erőforrás diagnosztikai naplók hello portálon gyűjtésének engedélyezése
Engedélyezheti a hello Azure-erőforrás diagnosztikai naplók gyűjteménye portal erőforrás tooa adott erőforrás lesz vagy máshová tooAzure figyelő létrehozása után. tooenable Azure Monitor segítségével:

1. A hello [Azure-portálon](http://portal.azure.com), keresse meg a figyelő tooAzure, és kattintson a **diagnosztikai beállítások**

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. Opcionálisan hello szűrőlistával erőforráscsoport és erőforrások típus szerint, majd a hello erőforrás diagnosztikai beállításának tooset milyen, amelynek.

3. Ha a beállítások nem található a kiválasztott hello erőforrás, felszólító toocreate beállítás áll. Kattintson a "Diagnosztika bekapcsolásához."

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Ha meglévő beállítások hello erőforráson, látni fogja a már ehhez az erőforráshoz konfigurált beállítások listáját. A "Hozzáadás diagnosztikai beállításának."

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Adjon a beállítás a neve, hello jelölőnégyzeteket minden cél toowhich kívánja toosend adatok, majd konfigurálja az erőforrás-minden célhelyéhez használja a rendszer. Beállíthatja a nap tooretain számú ezek a naplók hello segítségével **megőrzés (nap)** csúszkák (csak megfelelő toohello fiók célhelyet). Egy nulla napos megőrzési hello naplók határozatlan ideig tárolja.
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Kattintson a **Save** (Mentés) gombra.

Néhány másodpercen belül hello új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók küldése toohello, amint létrejön az új eseményadatok megadott célhelyekre.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Az erőforrás diagnosztikai naplók PowerShell gyűjtésének engedélyezése
az erőforrás diagnosztikai naplók Azure PowerShell, a következő parancsok használata hello tooenable gyűjtemény:

egy tárfiókot, a diagnosztikai naplók tárolására tooenable használja ezt a parancsot:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

hello tárolási adatbázisfiók azonosítója hello erőforrás-azonosító, a hello tárolási fiók toowhich toosend hello kívánt naplózza.

diagnosztikai naplók tooan az event hubs streaming tooenable használja ezt a parancsot:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

hello service bus Szabályazonosító egy ilyen formátumú karakterláncot: `{Service Bus resource ID}/authorizationrules/{key name}`.

diagnosztikai naplók tooa Naplóelemzési munkaterület küldése tooenable használja ezt a parancsot:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
```

Ezt úgy szerezheti be a Naplóelemzési munkaterület használatával a következő parancs hello hello erőforrás-azonosító:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Ezek a paraméterek tooenable kombinálhatja több kimenet beállításai.

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a>Az erőforrás diagnosztikai naplók parancssori felületen keresztül gyűjtésének engedélyezése
erőforrás diagnosztikai naplók hello Azure parancssori felületen keresztül tooenable gyűjteménye a következő parancsok hello használata:

egy Tárfiókot, a diagnosztikai naplók tárolására tooenable használja ezt a parancsot:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

hello tárolási adatbázisfiók azonosítója hello erőforrás-azonosító, a hello tárolási fiók toowhich toosend hello kívánt naplózza.

diagnosztikai naplók tooan az event hubs streaming tooenable használja ezt a parancsot:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

hello service bus Szabályazonosító egy ilyen formátumú karakterláncot: `{Service Bus resource ID}/authorizationrules/{key name}`.

diagnosztikai naplók tooa Naplóelemzési munkaterület küldése tooenable használja ezt a parancsot:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
```

Ezek a paraméterek tooenable kombinálhatja több kimenet beállításai.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Az erőforrás diagnosztikai naplók REST API-n keresztül gyűjtésének engedélyezése
toochange diagnosztikai beállítások hello Azure figyelő REST API használatával, lásd: [Ez a dokumentum](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-hello-portal"></a>Hello portál erőforrás diagnosztikai beállítások kezelése
Győződjön meg arról, hogy az erőforrások vannak beállítva a diagnosztikai beállítások. Keresse meg a túl**figyelő** hello portál, és nyissa meg a **diagnosztikai beállítások**.

![Diagnosztikai naplók panel hello portálon](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Előfordulhat, hogy tooclick "További szolgáltatások" toofind hello figyelése című szakaszában.

Itt megtekintheti és összes erőforrást, amely támogatja a diagnosztikai beállítások toosee, ha rendelkeznek engedélyezve diagnosztikai szűréséhez. Részletekbe menően tárhatják toosee Ha erőforráson több beállítást is, és ellenőrizze, melyik tárfiók, az Event Hubs névtér és/vagy adatok halad a Naplóelemzési munkaterület.

![Diagnosztikai naplók eredmények a portál](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Erőforrás hozzáadása a diagnosztikai beállítások megtekintése, ahol engedélyezése, letiltása vagy hello diagnosztikai beállításainak módosításához hello megjelenik egy diagnosztikai beállítást választani.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Támogatott szolgáltatások, a kategóriák és a sémák erőforrás diagnosztikai naplók
[Ebben a cikkben találhat](monitoring-diagnostic-logs-schema.md) támogatott szolgáltatások és a hello napló kategóriák és a szolgáltatások által használt sémák teljes listáját.

## <a name="next-steps"></a>Következő lépések

* [Adatfolyam-erőforrás diagnosztikai naplók túl**Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Hello Azure figyelő REST API használatával erőforrás diagnosztikai beállításainak módosítása](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [A Naplóelemzési az Azure storage naplóinak elemzése](../log-analytics/log-analytics-azure-storage.md)
