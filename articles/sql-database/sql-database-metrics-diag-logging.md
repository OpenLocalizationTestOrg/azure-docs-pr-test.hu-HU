---
title: "Az Azure SQL adatbázis metrikák & diagnosztikai naplózás |} Microsoft Docs"
description: "Ismerje meg az erőforrás-használat, a kapcsolat és a lekérdezés végrehajtási statisztika tárolására az Azure SQL Database-erőforrás konfigurálása."
services: sql-database
documentationcenter: 
author: vvasic
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: vvasic
ms.openlocfilehash: bf41aa530c68ea0e94a09d1dab63237c6f42bce7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Az Azure SQL Database metrikák és diagnosztikai naplózás 
Az Azure SQL-adatbázis el tudná küldeni, metrikákat és egyszerűbb figyelési diagnosztikai naplókat. Konfigurálhatja az Azure SQL-adatbázis tárolásához, erőforrás-használat, a dolgozók és a munkamenetek és a kapcsolat valamelyikébe ezek Azure-erőforrások:
- **Azure Storage**: Nagy tömegű telemetriai adat alacsony költségű archiválására
- **Az Azure Event Hubs**: az Azure SQL Database telemetriai integrálása a figyelési megoldást igényelnek egyéni vagy a működés közbeni folyamatok
- **Az Azure Naplóelemzés**: az a felügyeleti megoldás reporting, riasztás és képességek kiküszöböléséhez kezdő verzióról 

    ![architektúra](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Naplózás engedélyezése

Metrikák és diagnosztikai naplózás alapértelmezés szerint nincs engedélyezve. Engedélyezi, és kezelheti a metrikák és a naplózás a következő módszerek egyikét használva diagnosztika:
- Azure Portal
- PowerShell
- Azure CLI
- REST API 
- Resource Manager-sablon

Metrikák és diagnosztikai naplózás engedélyezése esetén meg kell adnia a Azure erőforráscsoport, ahol a kijelölt adatok gyűjtése. Elérhető lehetőségek:
- Log Analytics
- Eseményközpont
- Azure Storage 

Új Azure-erőforrás kiépítése, vagy jelöljön ki egy meglévő erőforrást. Miután kiválasztotta a tárolási erőforrások, meg kell adnia az összegyűjtendő adatok. Elérhető lehetőségek a következők:

- **[1 perces metrikák](sql-database-metrics-diag-logging.md#1-minute-metrics)**  -DTU százaléka, a DTU határt, a Processzor százalékban tartalmaz napló írása fizikai adatot olvasott a következő százalékos aránya, százalékos, sikeres vagy sikertelen/letiltott tűzfalkapcsolatok, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, XTP tárolási százalékos aránya

Eseményközpont vagy egy AzureStorage fiókot ad meg, ha megadhat egy megőrzési házirend régebbi, mint a kijelölt időszakot törlődik adatok megadásához. A Naplóelemzési ad meg, ha az adatmegőrzési függ a kijelölt tarifacsomag. Tudjon meg többet az [Naplóelemzési árképzési](https://azure.microsoft.com/pricing/details/log-analytics/). 

Azt javasoljuk, hogy olvassa el is a [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) felmérheti, nem csak hogyan engedélyezze a naplózást, de a metrikák és a napló kategóriák különböző Azure-szolgáltatás által támogatott cikkeket.

### <a name="azure-portal"></a>Azure Portal

Metrikák és az Azure portálon diagnosztikai naplók gyűjtemény engedélyezéséhez navigáljon az Azure SQL adatbázis vagy a rugalmas készlet lap, és kattintson **diagnosztikai beállítások**.

   ![az Azure portálon engedélyezése](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a>PowerShell

Metrikák és a PowerShell használatával diagnosztikai naplózás engedélyezéséhez használja a következő parancsokat:

- Ahhoz, hogy a Storage-fiókok a diagnosztikai naplók tárolására, az alábbi parancsot használja:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   A Tárfiók azonosítója az erőforrás-azonosítója, amelyhez hozzá szeretné küldeni a naplókat a tárfiók.

- Adatfolyamként való küldése a diagnosztikai naplók Eseményközpontokba való engedélyezéséhez az alábbi parancsot használja:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   A Service Bus Szabályazonosító egy ilyen formátumú karakterláncot:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- A Naplóelemzési munkaterület elküldését a diagnosztikai naplók engedélyezéséhez az alábbi parancsot használja:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Az erőforrás-azonosítója a Naplóelemzési munkaterület a következő paranccsal szerezheti be:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Ezek a paraméterek ahhoz, hogy több kimenet beállításai kombinálhatja.

### <a name="cli"></a>parancssori felület

Metrikák és az Azure parancssori felület használatával diagnosztikai naplózás engedélyezéséhez használja a következő parancsokat:

- Ahhoz, hogy a Storage-fiókok a diagnosztikai naplók tárolására, az alábbi parancsot használja:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   A Tárfiók azonosítója az erőforrás-azonosítója, amelyhez hozzá szeretné küldeni a naplókat a tárfiók.

- Adatfolyamként való küldése a diagnosztikai naplók Eseményközpontokba való engedélyezéséhez az alábbi parancsot használja:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   A Service Bus Szabályazonosító egy ilyen formátumú karakterláncot:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- A Naplóelemzési munkaterület elküldését a diagnosztikai naplók engedélyezéséhez az alábbi parancsot használja:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Ezek a paraméterek ahhoz, hogy több kimenet beállításai kombinálhatja.

### <a name="rest-api"></a>REST API

Olvassa el, hogyan [a Azure REST API használatával diagnosztikai beállítások módosításához](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-sablon

Olvassa el, hogyan [engedélyezze a diagnosztikát a Resource Manager sablonnal erőforrás létrehozásakor](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>A Naplóelemzési adatfolyam 
Az Azure SQL Database metrikák és diagnosztikai naplók továbbítható a beépített "Küldése a Log Analyticshez" beállítás használatával, a portálon, vagy az Azure PowerShell parancsmagok, az Azure parancssori felület vagy Azure figyelő REST API-n keresztül diagnosztikai beállítás Naplóelemzési engedélyezésével Naplóelemzési be.

### <a name="installation-overview"></a>Telepítés – áttekintés

Figyelése Azure SQL Database járműflotta a Naplóelemzési egyszerű. Három lépésre szükség:

1.  Log Analytics-erőforrás létrehozása
2.  A metrikák és diagnosztikai naplók rekordot a létrehozott Log Analytics-adatbázisok konfigurálása
3.  Telepítés **Azure SQL elemzés** Log Analytics-galériából megoldás

### <a name="create-log-analytics-resource"></a>Log Analytics-erőforrás létrehozása

1. Kattintson a **új** a bal oldali menüben.
2. Kattintson a **figyelési + kezelése**
3. Kattintson a **Analytics naplózása**
4. Töltse ki a Naplóelemzési űrlapot szükséges további információ: munkaterület nevét, előfizetés, erőforráscsoportot, helyét és IP-címek.

   ![a naplóelemzési](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostic-logs"></a>Rekord metrikák és diagnosztikai naplók-adatbázisok konfigurálása

A legegyszerűbben úgy konfigurálja, ahol adatbázisok jegyezze fel a metrikák van az Azure portálon keresztül. Az Azure-portálon az Azure SQL Database erőforrás keresse meg és kattintson a **diagnosztikai beállítások**. 

### <a name="install-the-azure-sql-analytics-solution-from-gallery"></a>Telepítse az Azure SQL elemzési megoldások gyűjteményből  

1. Miután a Naplóelemzési erőforrás jön létre, és az adatok áramlik bele, telepítse az Azure SQL elemzési megoldások. Ez végezhető el a **megoldások gyűjtemény** , amely a OMS kezdőlapon és az ügyféloldali menüjében található. Az oldalon található, és kattintson a **Azure SQL elemzés** megoldás, és kattintson **Hozzáadás**.

   ![felügyeleti megoldás](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. A kezdőlapon OMS, egy új csempe nevű **Azure SQL elemzés** jelenik meg. Ez a csempe kiválasztásával megnyílik az Azure SQL-elemzések irányítópultján.

### <a name="using-azure-sql-analytics-solution"></a>Az Azure SQL elemzés megoldással

Az Azure SQL elemzés, amely lehetővé teszi a hierarchiában, az Azure SQL Database erőforrások közötti navigáláshoz hierarchikus irányítópult. Ez a funkció lehetővé teszi magas szintű figyelését, de emellett lehetővé teszi a hatókör csak a megfelelő készletéhez erőforrások a figyelést.
Az irányítópulton a kiválasztott erőforrás a különböző erőforrások listáját. Például a kiválasztott előfizetéshez megtekintheti az összes kiszolgálók, rugalmas készletek és adatbázisok, amelyek a kijelölt előfizetéshez tartozik. Emellett a rugalmas készletek és adatbázisokat, megtekintheti az erőforrás-használati metrikák erőforrás. Ez magában foglalja diagramok DTU, Processzor, IO, napló, munkamenetek, dolgozók, kapcsolatok, valamint tárolási GB-ban.

## <a name="stream-into-azure-event-hub"></a>Az Azure Event Hubs adatfolyam

Az Azure SQL Database metrikák és diagnosztikai naplók továbbítható a beépített "Stream az eseményközpontba" beállítás használatával, a portálon, vagy a Service Bus szabály azonosítója az Azure PowerShell-parancsmagok, az Azure parancssori felület vagy a Azure figyelő REST API-t egy diagnosztikai beállítás engedélyezésével az Event Hubs be. 

### <a name="what-to-do-with-metrics-and-diagnostic-logs-in-event-hub"></a>Mit kell tenni a metrikák és diagnosztikai naplófájlok az Event Hubs?
Ha a kijelölt adatokat az Event Hubs továbbítja adatfolyamként, áll egy lépéssel közelebb kerülnek engedélyezése speciális figyelési forgatókönyveket. Az Event Hubs a „bejárati ajtó” funkcióját látja el egy eseményfolyamat számára, az adatoknak egy eseményközpontba való összegyűjtését követően az adatok bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével átalakíthatók és tárolhatók. Az Event Hubs elválasztja az eseménystreamek létrehozását azok felhasználásától, így az események felhasználói a saját ütemezésüknek megfelelően férhetnek hozzá az eseményekhez. Az Event Hubs további információkért lásd:

- [Mik az Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
- [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Az alábbiakban néhány módokon használhatja a adatfolyam-továbbítási funkció:

-   Szolgáltatás állapotának megtekintéséhez folyamatos "forró path" adatokat a PowerBI - Event Hubs használatával, a Stream Analytics és a Power BI, könnyen alakíthatja a metrikák és diagnosztikai adatokat közel valós idejű elemzése az Azure-szolgáltatásoknak. Megtudhatja, hogyan állíthat be az Event Hubs adatfeldolgozásra Streamelemzéssel és kimenetként Power bi használatát [Stream Analytics és a Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).
-   Stream naplók a külső naplózása és telemetriai adatfolyamok – használja az Event Hubs streaming meg kérheti le a metrikákat, és diagnosztikai jelentkezik be a különböző külső figyelése és a naplófájlok elemzési megoldásokat. 
-   Egy egyéni telemetriai adatok és a naplózás platform összeállítása – Ha már rendelkezik egy egyedi telemetriai platform vagy vannak csak gondolat egy kiválóan méretezhető épület jellegétől függően az Event Hubs közzétételi-feliratkozási lehetővé teszi rugalmasan betöltési a diagnosztikai naplókat. Lásd: [Dan Rosanova útmutató az Event Hubs egy globális méretű telemetriai platform használatával](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-azure-storage"></a>Az Azure Storage adatfolyam

Az Azure SQL Database metrikák és diagnosztikai naplók az Azure Storage a beépített "Archív tárfiókba" beállítás használatával, az Azure portálon, vagy Azure Storage Azure PowerShell-parancsmagok, az Azure parancssori felület vagy a Azure figyelő REST API-t egy diagnosztikai beállítás engedélyezésével is tárolhatók.

### <a name="schema-of-metrics-and-diagnostic-logs-in-the-storage-account"></a>A metrikák és diagnosztikai naplókat a tárfiókban lévő séma

Ha metrikák és diagnosztikai naplók gyűjtemény áll, tárolót választotta, amikor az első adatsor sem érhető el a storage-fiók jön létre. A blobok szerkezete van:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Vagy egyszerűen több:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

A blob nevének 1 perces metrikáihoz lehet, például:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Abban az esetben, ha szeretné rögzíteni az adatokat a rugalmas készletből, blob neve a következő kissé eltérő:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-azure-storage"></a>Az Azure storage metrikák és a naplók letöltése

Lásd: [metrikák és diagnosztikai naplók letöltése Azure Storage-ból](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)

## <a name="1-minute-metrics"></a>1 perces metrikák

| |  |
|---|---|
|**Erőforrás**|**Metrikák**|
|Adatbázis|DTU százalékos DTU használt, DTU határt, a processzor, a fizikai adatáttelepítések olvasási százalékos, napló írása százalékos, sikeres vagy sikertelen/letiltott tűzfalkapcsolatok, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, XTP tárolási százalékos, holtpont |
|A rugalmas készlet|eDTU százalékos eDTU használt, eDTU határt, a processzor, a fizikai adatáttelepítések olvasási százalékos, napló írása százalékos, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, tárolási kapacitás, XTP tárolási százalékos aránya |
|||

## <a name="next-steps"></a>Következő lépések

- Mindkét olvassa el a [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikkek felmérheti, nem csak hogyan engedélyezze a naplózást, de a metrikák és a napló kategóriák különböző Azure-szolgáltatás által támogatott.
- Ezek a cikkek az event hubs megismeréséhez olvassa el:
   - [Mik az Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
   - [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Lásd: [metrikák és diagnosztikai naplók letöltése Azure Storage-ból](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
