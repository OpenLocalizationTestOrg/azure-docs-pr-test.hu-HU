---
title: "Hálózati elemzési megoldások a Naplóelemzési aaaAzure |} Microsoft Docs"
description: "Hello Azure Networking elemzési megoldások Naplóelemzési tooreview Azure hálózati biztonsági csoport naplókat és az Azure Application Gateway-naplókat is használhatja."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 3674189786bacccc82e6708e78f14c92178e6676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Figyelési megoldásoknak a Naplóelemzési Azure hálózatkezelés

A Naplóelemzési megoldások a hálózatok figyelése az alábbi hello kínálja:
* Hálózati teljesítmény figyelő (NPM)
 * Hogy figyelemmel kísérje a hálózat hello állapotát
* Az Azure Application Gateway analytics tooreview
 * Az Azure alkalmazás-átjáró naplói
 * Az Azure Application Gateway metrikák
* Azure hálózati biztonsági csoport analytics tooreview
 * Azure hálózati biztonsági csoport naplófájlok

## <a name="network-performance-monitor-npm"></a>Hálózati teljesítmény figyelése (NPM)

Hello [hálózati Teljesítményfigyelő](log-analytics-network-performance-monitor.md) felügyeleti megoldás egy olyan hálózati felügyeleti megoldás, amely figyeli a hello állapot, a rendelkezésre állás és a hálózatok elérhetőség.  Használt toomonitor közötti kapcsolat:

* Nyilvános felhő és a helyszíni
* Adatközpontok és a felhasználó helye (fiókirodákban)
* Egy többrétegű alkalmazást a különböző rétegeket tartalmazó alhálózat.

További információkért lásd: [hálózati Teljesítményfigyelő](log-analytics-network-performance-monitor.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Az Azure Application Gateway és a hálózati biztonsági csoport elemzés
toouse hello megoldások:
1. Adja hozzá a hello felügyeleti megoldás tooLog elemzés, és
2. Diagnosztika toodirect hello diagnosztika tooa Naplóelemzési munkaterület engedélyezése. Már nem szükséges toowrite hello naplók tooAzure Blob-tároló.

Az egyik vagy mindkét Application Gateway és a hálózati biztonsági csoportok diagnosztika és hello megfelelő megoldás is engedélyezheti.

Ha nem engedélyezi a diagnosztikai naplózást egy adott erőforrástípusra, de hello megoldás telepítése, hello irányítópult paneleket az adott erőforrás üres, és egy hibaüzenetet jeleníti meg.

> [!NOTE]
> 2017. január hello támogatott módszer a naplók küldésével Alkalmazásátjárót és a hálózati biztonsági csoportok tooLog Analytics megváltozott. Ha megjelenik a hello **(elavult az) Azure hálózatkezelési elemzés** megoldás, tekintse meg a túl[hello régi hálózati elemzési megoldások áttelepítése](#migrating-from-the-old-networking-analytics-solution) lépéseket toofollow kell.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Tekintse át az Azure-hálózat az gyűjtemény adatait
hello Azure Application Gateway elemzés és hálózati biztonsági csoport elemzési megoldásokat hello gyűjteni a diagnosztika közvetlenül Azure Alkalmazásátjárót és a hálózati biztonsági csoportok. Már nem szükséges toowrite hello naplók tooAzure Blob-tároló, és nincs ügynök szükséges adatok gyűjtését.

hello következő táblázatban adatgyűjtési módszerek és egyéb hogyan adatgyűjtés Azure Application Gateway elemzés és hálózati biztonsági csoport analytics hello részleteit.

| Platform | Közvetlen ügynök | Systems Center Operations Manager-ügynök | Azure | Az Operations Manager szükséges? | Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött | A gyűjtés gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |Amikor bejelentkezik |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>A Naplóelemzési Azure Application Gateway elemzési megoldások

![Az Azure alkalmazás átjáró Analytics szimbólum](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

a következő naplók hello Alkalmazásátjárót támogatja:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

a következő metrikák hello Alkalmazásátjárót támogatja:

* 5 perc átviteli sebesség

### <a name="install-and-configure-hello-solution"></a>Telepítse és konfigurálja a hello megoldás
A következő utasításokat tooinstall hello használja, és konfigurálja a hello Azure Application Gateway elemzési megoldások:

1. Hello Azure Application Gateway elemzési megoldások a engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Hello diagnosztikai naplózás engedélyezése [Alkalmazásátjárót](../application-gateway/application-gateway-diagnostics.md) toomonitor szeretné.

#### <a name="enable-azure-application-gateway-diagnostics-in-hello-portal"></a>A portál hello Azure Application Gateway diagnosztika engedélyezése

1. Hello Azure-portálon lépjen a toohello Alkalmazásátjáró erőforrás toomonitor
2. Válassza ki *diagnosztikai naplók* tooopen hello a következő lap

   ![Azure Application Gateway erőforrás képe](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. Kattintson a *a diagnosztika bekapcsolásához* tooopen hello a következő lap

   ![Azure Application Gateway erőforrás képe](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. a diagnosztikát, tooturn kattintson *a* alatt *állapota*
5. Kattintson a hello jelölőnégyzetét *tooLog Analytics küldése*
6. Válasszon ki egy meglévő Naplóelemzési munkaterület, vagy egy munkaterület létrehozása
7. Hello jelölőnégyzet alatti **napló** az egyes hello napló típusok toocollect
8. Kattintson a *mentése* diagnosztika tooLog Analytics tooenable hello naplózása

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Engedélyezze a PowerShell használatával az Azure hálózati diagnosztika

a következő PowerShell-parancsfájl hello azt szemlélteti, hogyan tooenable diagnosztikai alkalmazásátjárót naplózása.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Azure Application Gateway analytics használata
![Azure Application Gateway analytics csempe képe](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

Miután rákattintott a hello **Azure Application Gateway analytics** csempére a hello áttekintése, a naplók összegzéseinek megtekintése és majd elemezze a következő kategóriák hello toodetails a:

* Alkalmazás átjáró naplói
  * Alkalmazásátjáró belépési naplók az ügyfél és kiszolgáló hibák
  * Az egyes Alkalmazásátjáró óránként kérelmek
  * Sikertelen kérelmek óránként az egyes Alkalmazásátjáró
  * Felhasználói ügynök alkalmazás átjárók hibák
* Alkalmazásteljesítmény-átjáró
  * Az Alkalmazásátjáró állomás állapota
  * Az Alkalmazásátjáró maximális és 95. percentilis sikertelen kérelmek

![Azure Application Gateway elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Azure Application Gateway elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

A hello **Azure Application Gateway analytics** az irányítópult hello összefoglaló információkat valamelyik hello paneleken ellenőrizni, és kattintson egy tooview részletes információk hello napló keresése oldal.

Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók. Értékkorlátozás toonarrow hello eredmények is végezhet.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>A Naplóelemzési Azure hálózati biztonsági csoport elemzési megoldások

![Azure hálózati biztonsági csoport Analytics szimbólum](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

a következő naplók hello támogatott hálózati biztonsági csoportok:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-hello-solution"></a>Telepítse és konfigurálja a hello megoldás
A következő utasításokat tooinstall hello használja, és hello Azure hálózatkezelési elemzés megoldás konfigurálása:

1. Hello Azure hálózati biztonsági csoport elemzési megoldások a engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Hello diagnosztikai naplózás engedélyezése [hálózati biztonsági csoport](../virtual-network/virtual-network-nsg-manage-log.md) toomonitor kívánt erőforrásokat.

### <a name="enable-azure-network-security-group-diagnostics-in-hello-portal"></a>Az Azure hálózati biztonsági csoport diagnosztikai hello portálon engedélyezése

1. Hello Azure-portálon lépjen a toohello hálózati biztonsági csoport erőforrás toomonitor
2. Válassza ki *diagnosztikai naplók* tooopen hello a következő lap

   ![Azure hálózati biztonsági csoport erőforrás képe](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. Kattintson a *a diagnosztika bekapcsolásához* tooopen hello a következő lap

   ![Azure hálózati biztonsági csoport erőforrás képe](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. a diagnosztikát, tooturn kattintson *a* alatt *állapota*
5. Kattintson a hello jelölőnégyzetét *tooLog Analytics küldése*
6. Válasszon ki egy meglévő Naplóelemzési munkaterület, vagy egy munkaterület létrehozása
7. Hello jelölőnégyzet alatti **napló** az egyes hello napló típusok toocollect
8. Kattintson a *mentése* diagnosztika tooLog Analytics tooenable hello naplózása

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Engedélyezze a PowerShell használatával az Azure hálózati diagnosztika

a következő PowerShell-parancsfájl hello azt szemlélteti, hogyan tooenable diagnosztikai naplózás a hálózati biztonsági csoportok
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Hálózati biztonsági csoport használata Azure elemzés
Miután rákattintott hello **Azure hálózati biztonsági csoport analytics** csempére a hello áttekintése, a naplók összegzéseinek megtekintése és majd elemezze a következő kategóriák hello toodetails a:

* Hálózati biztonsági csoport blokkolva adatfolyamok
  * Hálózati biztonsági csoportszabályok letiltott folyamatokkal
  * Letiltott adatfolyamok MAC-címek
* Hálózati biztonsági csoport engedélyezett adatfolyamok
  * Hálózati biztonsági csoportszabályok engedélyezett folyamatokkal
  * MAC-címek megengedett adatfolyamok

![Azure hálózati biztonsági csoport elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Azure hálózati biztonsági csoport elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

A hello **Azure hálózati biztonsági csoport analytics** az irányítópult hello összefoglaló információkat valamelyik hello paneleken ellenőrizni, és kattintson egy tooview részletes információk hello napló keresése oldal.

Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók. Értékkorlátozás toonarrow hello eredmények is végezhet.

## <a name="migrating-from-hello-old-networking-analytics-solution"></a>Hello régi hálózati elemzési megoldások áttelepítése
2017. január hello támogatott módon történő naplók küldése az Azure alkalmazás átjárók és az Azure hálózati biztonsági csoportok tooLog Analytics megváltozott. Ezek a változások hello a következő előnyöket biztosítja:
+ Közvetlen tooLog Analytics hello nélkül kell toouse tárfiók írja a naplókat
+ Kisebb késést hello időpontot, amikor naplók a generált legyenek elérhetők a Naplóelemzési toothem
+ Kevesebb konfigurációs lépések
+ Az összes Azure diagnostics közös formátum

toouse frissítése hello megoldások:

1. [Diagnosztika toobe küldött Azure Alkalmazásátjárót közvetlenül tooLog Analytics konfigurálása](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Diagnosztika toobe közvetlenül tooLog Analytics küldött Azure hálózati biztonsági csoportok konfigurálása](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Hello engedélyezése *Azure Application Gateway Analytics* és hello *Azure hálózati biztonsági csoport Analytics* hello folyamat használatával megoldás ismertetett [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjteménye](log-analytics-add-solutions.md)
3. A lekérdezések, irányítópultok vagy riasztások toouse hello új adattípusra frissítése
  + A típus tooAzureDiagnostics. Hello ResourceType toofilter tooAzure hálózati naplófájlokat is használhatja.

    | ahelyett, hogy: | Használata: |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + Bármely mezőhöz, amely rendelkezik egy utótagja \_s, \_d, vagy \_g a módosítás hello első karakter toolower esetben hello neve
   + Bármely mezőhöz, amely rendelkezik egy utótagja \_a név hello adatok o oszlik, egyedi mezők beágyazott hello mezőnevek alapján.
4. Távolítsa el a hello *Azure hálózatkezelési elemzés (elavult)* megoldás.
  + Ha a PowerShell használata esetén`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Adatgyűjtés előtt hello módosítása nem látható a hello új megoldás. A régi típusú és mezőnevek ezen adatok segítségével hello tooquery folytathatja.

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes Azure diagnosztikai adatokat.
