---
title: "aaaAzure SQL adatbázis-metrikák & diagnosztikai naplózás |} Microsoft Docs"
description: "További tudnivalók az Azure SQL Database erőforrás toostore erőforrás-használat, a kapcsolat és a lekérdezés végrehajtási statisztika konfigurálása."
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
ms.openlocfilehash: e6f9e24992ca4f84f701e1ef858e98dc7b481e28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Az Azure SQL Database metrikák és diagnosztikai naplózás 
Az Azure SQL-adatbázis el tudná küldeni, metrikákat és egyszerűbb figyelési diagnosztikai naplókat. Az Azure SQL Database toostore erőforrás-használat, a munkavállalók és a munkamenetek és a kapcsolat egy Azure erőforrásainak konfigurálhatja:
- **Azure Storage**: Nagy tömegű telemetriai adat alacsony költségű archiválására
- **Az Azure Event Hubs**: az Azure SQL Database telemetriai integrálása a figyelési megoldást igényelnek egyéni vagy a működés közbeni folyamatok
- **Az Azure Naplóelemzés**: A felügyeleti megoldás reporting, riasztás és képességek kiküszöböléséhez hello kezdő verzióról 

    ![architektúra](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Naplózás engedélyezése

Metrikák és diagnosztikai naplózás alapértelmezés szerint nincs engedélyezve. Engedélyezi, és kezelheti a metrikák és diagnosztikai naplózás hello a következő módszerek egyikével:
- Azure Portal
- PowerShell
- Azure CLI
- REST API 
- Resource Manager-sablon

Metrikák és diagnosztikai naplózás engedélyezéséhez meg kell toospecify hello Azure erőforráscsoport, ahol a kijelölt adatok gyűjtése. Elérhető lehetőségek:
- Log Analytics
- Eseményközpont
- Azure Storage 

Új Azure-erőforrás kiépítése, vagy jelöljön ki egy meglévő erőforrást. Hello tárolási erőforrások kijelölése, után kell toospecify mely adatok toocollect. Elérhető lehetőségek a következők:

- **[1 perces metrikák](sql-database-metrics-diag-logging.md#1-minute-metrics)**  -DTU százaléka, a DTU határt, a Processzor százalékban tartalmaz napló írása fizikai adatot olvasott a következő százalékos aránya, százalékos, sikeres vagy sikertelen/letiltott tűzfalkapcsolatok, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, XTP tárolási százalékos aránya

Az Event Hubs vagy AzureStorage fiók megadása esetén is megadhat egy megőrzési házirend toospecify régebbi, mint egy kiválasztott időszakban törlése adatok. A Naplóelemzési ad meg, ha hello adatmegőrzési hello kijelölt tarifacsomag függ. Tudjon meg többet az [Naplóelemzési árképzési](https://azure.microsoft.com/pricing/details/log-analytics/). 

Azt javasoljuk, hogy olvassa el a mindkét hello [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikkek toogain megismerhesse, nem csak hogyan tooenable naplózást is, de hello metrikák és a napló kategóriák hello által támogatott különböző Azure-szolgáltatásokhoz.

### <a name="azure-portal"></a>Azure Portal

tooenable metrikák és diagnosztikai naplók gyűjtemény hello Azure-portálon lépjen a tooyour Azure SQL adatbázis vagy a rugalmas készlet lap, és kattintson **diagnosztikai beállítások**.

   ![az Azure-portálon hello engedélyezése](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a>PowerShell

tooenable metrikákat és naplózási diagnosztikai PowerShell, a használatával a következő parancsok hello:

- egy Tárfiókot, a diagnosztikai naplók tárolására tooenable használja ezt a parancsot:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Tárfiók azonosítója hello hello tárolási fiók toowhich toosend hello kívánt naplók hello erőforrás-azonosító.

- tooenable streaming diagnosztikai naplók tooan Eseményközpontot, az alábbi parancsot használja:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Diagnosztikai naplók tooa Naplóelemzési munkaterület küldése tooenable használja ezt a parancsot:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
   ```

- Ezt úgy szerezheti be a Naplóelemzési munkaterület használatával a következő parancs hello hello erőforrás-azonosító:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Ezek a paraméterek tooenable kombinálhatja több kimenet beállításai.

### <a name="cli"></a>parancssori felület

tooenable metrikák és diagnosztikai naplózás használatával hello Azure parancssori felület, a következő parancsok használata hello:

- egy Tárfiókot, a diagnosztikai naplók tárolására tooenable használja ezt a parancsot:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Tárfiók azonosítója hello hello tárolási fiók toowhich toosend hello kívánt naplók hello erőforrás-azonosító.

- tooenable streaming diagnosztikai naplók tooan Eseményközpontot, az alábbi parancsot használja:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Diagnosztikai naplók tooa Naplóelemzési munkaterület küldése tooenable használja ezt a parancsot:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
   ```

Ezek a paraméterek tooenable kombinálhatja több kimenet beállításai.

### <a name="rest-api"></a>REST API

Megtudhatja, hogyan túl[hello Azure figyelő REST API használatával diagnosztikai beállítások módosításához](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-sablon

Megtudhatja, hogyan túl[engedélyezze a diagnosztikát a Resource Manager sablonnal erőforrás létrehozásakor](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>A Naplóelemzési adatfolyam 
Az Azure SQL Database metrikák és diagnosztikai naplók továbbítható a Log Analyticshez hello beépített "küldési tooLog Analytics" kapcsoló használatával hello portálon, vagy egy diagnosztikai beállításban Azure PowerShell parancsmagok, Azure CLI vagy a figyelő többi Azure Log Analytics engedélyezésével API-T.

### <a name="installation-overview"></a>Telepítés – áttekintés

Figyelése Azure SQL Database járműflotta a Naplóelemzési egyszerű. Három lépésre szükség:

1.  Log Analytics-erőforrás létrehozása
2.  A Naplóelemzési létrehozott hello adatbázisok toorecord metrikák és diagnosztikai naplók konfigurálása
3.  Telepítés **Azure SQL elemzés** Log Analytics-galériából megoldás

### <a name="create-log-analytics-resource"></a>Log Analytics-erőforrás létrehozása

1. Kattintson a **új** hello bal oldali menüben.
2. Kattintson a **figyelési + kezelése**
3. Kattintson a **Analytics naplózása**
4. Töltse ki hello Naplóelemzési űrlapot hello szükséges további információt: munkaterület nevét, előfizetés, erőforráscsoportot, helyét és IP-címek.

   ![a naplóelemzési](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-toorecord-metrics-and-diagnostic-logs"></a>Adatbázisok toorecord metrikák és diagnosztikai naplók konfigurálása

hello ahol adatbázisok jegyezze fel a metrikák legegyszerűbb módja tooconfigure hello Azure-portálon keresztül történik. A hello Azure-portálon, keresse meg az Azure SQL Database erőforrás tooyour, és kattintson **diagnosztikai beállítások**. 

### <a name="install-hello-azure-sql-analytics-solution-from-gallery"></a>Gyűjteményből hello Azure SQL elemzési megoldás telepítése  

1. Miután hello Naplóelemzési erőforrás jön létre, és az adatok áramlik bele, telepítse az Azure SQL elemzési megoldások. Ezt rábízhatja a hello **megoldások gyűjtemény** , amely a hello OMS kezdőlap és hello oldalsó menüben található. Hello gyűjteményben található, és kattintson a **Azure SQL elemzés** megoldás, és kattintson **Hozzáadás**.

   ![felügyeleti megoldás](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. A kezdőlapon OMS, egy új csempe nevű **Azure SQL elemzés** jelenik meg. Ez a csempe kiválasztásával megnyílik a hello Azure SQL-elemzések irányítópultján.

### <a name="using-azure-sql-analytics-solution"></a>Az Azure SQL elemzés megoldással

Az Azure SQL elemzés, amely lehetővé teszi az Azure SQL Database erőforrások hello hierarchia keresztül toonavigate hierarchikus irányítópult. A funkció lehetővé teszi, hogy Ön toodo magas szintű figyelési azonban azt is lehetővé teszi a tooscope a megfelelő figyelési toojust hello beállítása az erőforrások.
Az irányítópulton kiválasztott hello erőforrás a különböző erőforrások hello listája. Például a kiválasztott előfizetéshez láthatja hello összes kiszolgáló, a rugalmas készletek és a toohello tartozó adatbázis kiválasztott előfizetéshez. Emellett a rugalmas készletek és adatbázisokat, láthatja hello erőforrás-használati metrikák erőforrás. Ez magában foglalja diagramok DTU, Processzor, IO, napló, munkamenetek, dolgozók, kapcsolatok, valamint tárolási GB-ban.

## <a name="stream-into-azure-event-hub"></a>Az Azure Event Hubs adatfolyam

Az Azure SQL Database metrikák és diagnosztikai naplók továbbítható az Event Hubs hello beépített "adatfolyam tooan eseményközpont" kapcsoló használatával hello portálon, vagy a Service Bus szabály azonosítója az Azure PowerShell-parancsmagok, az Azure parancssori felület vagy a Azure figyelő REST diagnosztikai beállítás engedélyezésével API-T. 

### <a name="what-toodo-with-metrics-and-diagnostic-logs-in-event-hub"></a>Milyen toodo metrikák és diagnosztikai naplófájlok az Event Hubs?
Miután kijelölt hello adatokat az Event Hubs továbbítja adatfolyamként, egy lépéssel közelebb tooenabling speciális megfigyelési forgatókönyvekhez áll. Az Event Hubs úgy működik, mint egy eseményfolyamat számára "bejárati ajtón" hello, és amennyiben az eseményközpontnak összegyűjtött adatok átalakíthatók, és bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével tárolják. Az Event Hubs hello éles tartozó események hello felhasználásától, az adatfolyam leválasztja az, hogy az eseményfelhasználók férhetnek hozzá a saját ütemezésüknek hello események. Az Event Hubs további információkért lásd:

- [Mik az Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
- [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Az alábbiakban néhány módokon használhatja a streaming funkció hello:

-   Szolgáltatás állapotának megtekintéséhez folyamatos "forró path" adatok tooPowerBI - Event Hubs használatával, a Stream Analytics és a Power BI, könnyen alakíthatja a metrikák és diagnosztikai adatokat közel valós idejű elemzése az Azure-szolgáltatásoknak. Egy megtudhatja, hogyan tooset fel az Event Hubs feldolgozni az adatokat az Stream Analytics használ, és kimenetként használata a Power bi [Stream Analytics és a Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).
-   Stream naplók toothird féltől naplózása és telemetriai adatfolyamok – használatával az Event Hubs streaming, kérheti le a metrikák és diagnosztikai naplókat a toodifferent külső figyelése és a naplófájlok elemzési megoldásokat. 
-   És hozhat létre egy egyéni telemetria naplózási platform – Ha már rendelkezik egy egyedi telemetriai platform vagy a rendszer csak a gondolat egy kiválóan méretezhető hello közzétételi-feliratkozási épület jellegétől függően az Event Hubs lehetővé teszi a tooflexibly betöltési diagnosztikai naplók. Lásd: [Dan Rosanova útmutató toousing Event Hubs egy globális méretű telemetriai Platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-azure-storage"></a>Az Azure Storage adatfolyam

Az Azure SQL Database metrikák és diagnosztikai naplók tárolhatja az Azure Storage hello beépített "Archiválására tooa storage-fiók" beállítás használatával hello Azure-portálon vagy az Azure Storage engedélyezése az Azure PowerShell-parancsmagok, az Azure parancssori felület vagy a Azure diagnosztikai beállítás A figyelő REST API-t.

### <a name="schema-of-metrics-and-diagnostic-logs-in-hello-storage-account"></a>A metrikák és diagnosztikai naplók hello tárfiókban lévő séma

Ha metrikák és diagnosztikai naplók gyűjtemény áll, tárolót választotta, mikor érhetők el adatok első sora hello hello tárfiók jön létre. a blobok hello szerkezete van:

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

Abban az esetben, ha azt szeretné, hogy a rugalmas készlet hello toorecord hello adatait, blob neve a következő kissé eltérő:

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

- Mindkét hello olvasási [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) toogain nem csak hogyan metrikák hello azonban a naplózás tooenable és kategóriák jelentkezzen cikkek hello által támogatott különböző Azure-szolgáltatásokhoz.
- Olvassa el ezeket az event hubs vonatkozó cikkek toolearn:
   - [Mik az Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
   - [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Lásd: [metrikák és diagnosztikai naplók letöltése Azure Storage-ból](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
