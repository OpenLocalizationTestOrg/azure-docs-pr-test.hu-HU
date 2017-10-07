---
title: "aaaMonitor el a naplókat, a Teljesítménynaplók, a háttér-állapot és a metrikák Alkalmazásátjáró |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable és kezelheti a belépési naplók és a Teljesítménynaplók Alkalmazásátjáró"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 36ebf15c28f776158350ef8e73d617ef68e09266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Háttér-állapot, a diagnosztikai naplók és a metrikák az Alkalmazásátjáró

Azure Application Gateway használatával figyelheti a következő módokon hello erőforrások:

* [Háttér-állapotfigyelő](#back-end-health): Application Gateway biztosít hello funkció toomonitor hello hello kiszolgálók állapotának hello a háttér-készletek hello Azure-portálon keresztül, és a PowerShell segítségével. Hello háttér-készletek keresztül hello diagnosztikai Teljesítménynaplók hello állapotát is tájékozódhat.

* [Naplók](#diagnostic-logs): naplóihoz a teljesítmény, a hozzáférést, és egyéb adatok toobe mentésekor, illetve egy erőforráshoz, ellenőrzési célból használni.

* [Metrikák](#metrics): Application Gateway még egy metrika van. Ez a metrika hello átviteli sebessége bájt / mp a hello Alkalmazásátjáró méri.

## <a name="back-end-health"></a>Háttér-állapota

Alkalmazásátjáró hello funkció toomonitor hello állapotát az egyes tagok hello háttér-készletek hello portálhoz, a PowerShell és a hello parancssori felület (CLI) használatával biztosít. Is megkeresheti az összesített állapotát összesítő a háttér-címkészletek hello diagnosztikai Teljesítménynaplók keresztül. 

hello háttér-állapotjelentés hello kimeneti hello Alkalmazásátjáró állapotfigyelő mintavételi toohello háttér-példányok által adott jelentéseket tükrözik. Ha az ellenőrzés sikeres, és vissza hello end forgalom fogadására, akkor tekinthető kifogástalan. Ellenkező esetben azt nem megfelelő állapotúnak számít.

> [!IMPORTANT]
> Ha egy alkalmazás átjáró-alhálózatot a hálózati biztonsági csoport (NSG), nyissa meg a bejövő forgalom hello Alkalmazásátjáró alhálózaton 65503-65534 porttartományok. Ezeket a portokat hello háttér-állapotfigyelő API toowork szükségesek.


### <a name="view-back-end-health-through-hello-portal"></a>Háttér-állapotának megtekintése hello portálon keresztül

Hello portálon háttér-állapotfigyelő valósul meg automatikusan. Válassza ki egy meglévő Alkalmazásátjáró **figyelés** > **háttér állapotfigyelő**. 

Minden tag hello háttér-készlet szerepel ezen a lapon (hogy-e a hálózati adapter, IP vagy FQDN Formátumban). Háttér-alkalmazáskészlet neve, a port, a háttér HTTP beállítások neve és az állapot látható. Érvényes állapot értékei **kifogástalan**, **nem kifogástalan állapotú**, és **ismeretlen**.

> [!NOTE]
> Ha megjelenik egy háttér-állapotát **ismeretlen**, győződjön meg arról, hogy hozzáférési toohello háttér ne blokkolja az NSG-szabályok, a felhasználó által megadott útvonal (UDR) vagy egy egyéni DNS hello virtuális hálózatban.

![Háttér-állapota][10]

### <a name="view-back-end-health-through-powershell"></a>Háttér-állapotának megtekintése a PowerShell segítségével

a következő PowerShell-kódjába hello bemutatja, hogyan tooview háttér-állapotfigyelő segítségével hello `Get-AzureRmApplicationGatewayBackendHealth` parancsmagot:

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a>Azure CLI 2.0 keresztül háttér-állapotának megtekintése

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Results (Eredmények)

a következő kódrészletet hello hello válasz példáját mutatja be:

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Diagnosztikai naplók

A naplók különböző típusait használják az Azure toomanage, és hibáinak elhárítása alkalmazásátjárót. Ezek a naplók némelyike hello portálon keresztül érheti el. Összes napló kinyert Azure Blob Storage tárolóban, és a különböző eszközök, például a megtekintett [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md), az Excel és a Power bi-ban. További hello különböző típusú naplók a következő lista hello:

* **Tevékenységnapló**: használható [Azure tevékenységi naplóit](../monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén műveleti naplókat, és a vizsgálati naplók ismert) tooview összes művelet benyújtott tooyour Azure-előfizetéssel, és azok állapotát. Tevékenység naplóbejegyzések alapértelmezett által gyűjtött, és megtekintheti azokat a hello Azure-portálon.
* **Hozzáférési napló**: fontos információkat, a többek között a következőket hello hívó IP, a kért URL-cím, a válasz késés, a visszatérési kód és a bájtok bejövő és kimenő adatforgalma elemzése és a napló tooview Alkalmazásátjáró hozzáférési minták használhatja. A hozzáférési napló gyűjtése 300 másodpercenként. Ez a napló Application Gateway-példányonként egy bejegyzést tartalmaz. hello alkalmazás átjárópéldány azonosítható hello instanceId tulajdonság.
* **Teljesítménynapló**: használhatja a napló tooview hogyan Alkalmazásátjáró példányok hajtja végre. Ez a napló minden egyes előfordulás esetén kérelmek teljes száma a kiszolgált, beleértve az átviteli sebesség bájtban teljesítmény információit rögzíti, kiszolgált kérelmek teljes száma, sikertelen kérelmek száma, és a megfelelő és nem megfelelő a háttér-példányok száma. A Teljesítménynapló 60 másodpercenként gyűjti.
* **Tűzfal napló**: a napló tooview hello kérések, amelyeket a rendszer az Alkalmazásátjáró hello webalkalmazási tűzfal a konfigurált észlelési vagy megelőzési módban is használhatja.

> [!NOTE]
> Kizárólag a hello Azure Resource Manager üzembe helyezési modellben telepített erőforrások érhetők el naplók. Naplók erőforrások hello klasszikus üzembe helyezési modellben nem használhat. Szeretné jobban megismerni a két hello modellek, lásd: hello [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md) cikk.

A naplók tárolásához három lehetőség közül választhat:

* **A tárfiók**: tárfiókok használata leginkább a naplók Ha naplók hosszabb ideig tárolja, és szükség esetén tekintse át.
* **Az Event hubs**: az Event hubs pedig integrálása az egyéb biztonsági adatokat, és eseményt (SEIM) kezelőeszközök, az erőforrások tooget ki riasztást.
* **Naplófájl Analytics**: Naplóelemzési használható általános valós idejű figyelés az alkalmazás, vagy megtekint trendeket.

### <a name="enable-logging-through-powershell"></a>A PowerShell segítségével naplózásának engedélyezése

Minden erőforrás-kezelő erőforrás naplózása automatikusan engedélyezve van. Engedélyeznie kell a hozzáférést és a teljesítmény naplózási toostart hello adatok elérhetők lesznek a naplók gyűjtésére. tooenable naplózás használata hello a következő lépéseket:

1. Vegye figyelembe a tárfiók erőforrás-azonosító, hello napló adatok tárolására. Hello űrlap értéke: /subscriptions/\<subscriptionId\>/resourceGroups/\<erőforráscsoport-név\>/providers/Microsoft.Storage/storageAccounts/\<tárfióknév\>. Használhatja a tárfiók az előfizetésben. Az Azure portál toofind hello használhatja ezeket az információkat.

    ![Portál: tárfiók erőforrás-azonosító](./media/application-gateway-diagnostics/diagnostics1.png)

2. Vegye figyelembe az Alkalmazásátjáró erőforrás-azonosító, amelynek a naplózás engedélyezve van. Az értéket nem hello űrlap: /subscriptions/\<subscriptionId\>/resourceGroups/\<erőforráscsoport-név\>/providers/Microsoft.Network/applicationGateways/\<Alkalmazásátjáró név\>. Hello portál toofind használhatja ezeket az információkat.

    ![Portál: az Alkalmazásátjáró erőforrás-azonosító](./media/application-gateway-diagnostics/diagnostics2.png)

3. Diagnosztikai naplózás engedélyezése hello a következő PowerShell-parancsmag segítségével:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Tevékenység naplók nem igényelnek külön tárfiókot. a hozzáférés- és teljesítménynaplózás hello tárolás használatát szolgáltatás terhel.

### <a name="enable-logging-through-hello-azure-portal"></a>Azure-portálon keresztül hello naplózásának engedélyezése

1. A hello Azure-portálon, az erőforrás található, és kattintson a **diagnosztikai naplók**.

   Az Alkalmazásátjáró három érhetők el naplók:

   * Hozzáférési napló
   * Teljesítmény-napló
   * Tűzfal-napló

2. toostart végzett adatgyűjtés, kattintson a **a diagnosztika bekapcsolásához**.

   ![Diagnosztika bekapcsolása][1]

3. Hello **diagnosztikai beállítások** panel hello hello beállításokat biztosít diagnosztikai naplókat. Ebben a példában a Naplóelemzési hello naplók tárolja. Kattintson a **konfigurálása** alatt **Naplóelemzési** tooconfigure a munkaterületen. Az event hubs és a tárolási fiók toosave hello diagnosztikai naplók is használható.

   ![Hello konfigurációs folyamat elindítása][2]

4. Válassza ki egy meglévő Operations Management Suite (OMS) munkaterületet, vagy hozzon létre egy újat. A példa egy meglévőt.

   ![Beállítások OMS-munkaterület][3]

5. Ellenőrizze hello beállításokat, majd kattintson a **mentése**.

   ![Diagnosztikai beállítások panel választása esetén][4]

### <a name="activity-log"></a>Tevékenységnapló

Alapértelmezés szerint az Azure létrehoz hello tevékenységnapló. hello naplók hello Azure eseménynaplók tárolójában 90 napig megőrződnek. További információ a naplók hello olvasásával [események és tevékenységnapló](../monitoring-and-diagnostics/insights-debugging-with-events.md) cikk.

### <a name="access-log"></a>Hozzáférési napló

hello hozzáférési napló jön létre, csak akkor, ha engedélyezte az összes Application Gateway-példányt, ahogy az az előző lépésekben hello. Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatokat tárolja. Minden egyes hozzáférés az Application Gateway JSON formátumban van bejelentkezve, ahogy az alábbi példa hello:


|Érték  |Leírás  |
|---------|---------|
|instanceId     | Alkalmazásátjáró példány szolgálatban hello kérésre.        |
|Ügyfélip     | IP-címekről hello kérelem.        |
|clientPort     | Származó hello kérelem port.       |
|HttpMethod     | Hello a kérelemhez használt HTTP-metódus.       |
|requestUri     | Hello URI-kérelmet kapott.        |
|RequestQuery     | **Kiszolgáló irányított**: háttér-készlet példány hello kérelmet küldött. </br> **X-AzureApplicationGateway-napló-ID**: hello kéréshez használt korrelációs azonosítója. Lehet, hogy a használt tootroubleshoot forgalom problémák hello háttér-kiszolgálókon. </br>**KISZOLGÁLÓ-állapota**: Application Gateway kapott hello háttér HTTP válaszkódot.       |
|Felhasználói ügynök     | Felhasználói ügynök a hello HTTP-kérelem fejléce.        |
|httpStatus     | HTTP-állapotkód toohello ügyfél Alkalmazásátjáró adott vissza.       |
|httpVersion     | Hello kérelem HTTP-verzió.        |
|ReceivedBytes     | Csomag érkezett, bájtban kifejezett mérete.        |
|SentBytes| Küldött bájtok a csomag mérete.|
|TimeTaken| (Ezredmásodpercben), hogy mennyi ideig tart egy kérelem toobe feldolgozása és a válasz toobe küldött. Ez akkor a program hello intervallum kezdete hello idő, amikor megkapja az Application Gateway a hello első bájtig eltelt egy HTTP-kérelem toohello idő amikor hello válasz küldése művelet befejeződik. Fontos, amely általában a Time-Taken mező hello toonote tartalmaz hello idő hello kérés- és csomagok utazás hello hálózaton keresztül. |
|sslEnabled| E kommunikációs toohello háttér-készletek használt SSL. Érvényes értékei a be- és kikapcsolható.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a>Teljesítmény-napló

hello teljesítmény napló jön létre, csak akkor, ha engedélyezte az összes Application Gateway-példányt, ahogy az az előző lépésekben hello. Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatokat tárolja. a Teljesítménynapló-adatok hello 1 perces időközönként jön létre. a következő adatok hello jegyzi fel:


|Érték  |Leírás  |
|---------|---------|
|instanceId     |  Átjáró alkalmazáspéldányt amelyek esetében az adatok létrehozása. Az egy több-példány Alkalmazásátjáró soronként egy példány van.        |
|healthyHostCount     | Hello háttér-készlet megfelelő gazdagépek száma.        |
|unHealthyHostCount     | Nem megfelelő állapotú gazdagépek hello háttér-készlet száma.        |
|RequestCount     | Kiszolgált kérelmek száma.        |
|Késés | Késése (ezredmásodpercben) hello példány toohello kérelmek háttér hello kérelmek szolgál. |
|failedRequestCount| Sikertelen kérelmek száma.|
|Átviteli sebesség| Átlagos átviteli sebességgel hello utolsó napló másodpercenként bájtban mért óta.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> Késés hello időpontot, amikor hello HTTP-kérelem első bájtját hello hello HTTP-válasz utolsó bájtját hello rendszer mikor küldjön kapott toohello idő kiszámítása. Az hello összege hello feldolgozási ideje az Alkalmazásátjáró, valamint hello vissza a hálózati költségeket toohello végén, és a hello időt, amely hello háttér vesz tooprocess hello kérelem.

### <a name="firewall-log"></a>Tűzfal-napló

hello tűzfal napló jön létre, csak akkor, ha engedélyezte az egyes Alkalmazásátjáró, ahogy az az előző lépésekben hello. Ez a napló is szükséges, hogy hello webalkalmazási tűzfal Alkalmazásátjáró konfigurálva van. Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatokat tárolja. a következő adatok hello jegyzi fel:


|Érték  |Leírás  |
|---------|---------|
|instanceId     | Átjáró alkalmazáspéldányt mely tűzfal adatok létrehozása folyamatban van. Az egy több-példány Alkalmazásátjáró soronként egy példány van.         |
|Ügyfélip     |   IP-címekről hello kérelem.      |
|clientPort     |  Származó hello kérelem port.       |
|requestUri     | Hello URL-kérelmet kapott.       |
|ruleSetType     | A szabály típusának beállítása. rendelkezésre álló hello értéke-OWASP.        |
|ruleSetVersion     | A szabálykészlet használt verzió. Lehetséges értékek a következők: program 2.2.9-es és 3.0-s.     |
|ruleId     | A szabály azonosítója hello kiváltó eseményt.        |
|Üzenet     | Az eseményt kiváltó hello saját üzenetet. További részletek hello részletes adatait tartalmazó részben szerepelnek.        |
|A művelet     |  Hello kérésre végrehajtott műveletet. Lehetséges értékek a következők: letiltott és engedélyezett.      |
|Helykiszolgáló     | A hely számára melyik hello napló jött létre. Jelenleg csak globális mert szabályok globális szerepel.|
|Részletek     | Hello kiváltó esemény részleteit.        |
|details.Message     | Hello szabály leírása.        |
|details.Data     | Kérelem adott egyező hello szabály található adatokat.         |
|details.File     | Hello szabály található konfigurációs fájl.        |
|details.line     | A sor hello eseményt kiváltó hello konfigurációs fájlban.       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-hello-activity-log"></a>Megtekintése és elemzése hello műveletnapló

Megtekintheti és tevékenység napló adatelemzés hello a következő módszerek egyikével sem:

* **Azure-eszközök**: adatok lekérését tevékenységnapló hello Azure PowerShell hello hello Azure REST API-t, az Azure parancssori felület használatával vagy hello Azure-portálon. Részletes útmutatás az egyes módszerek részletes leírást talál hello [tevékenység műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md) cikk.
* **A Power BI**: Ha még nem rendelkezik egy [Power BI](https://powerbi.microsoft.com/pricing) fiók, próbálkozzon az ingyenes. Hello segítségével [tevékenységi naplóit Azure-csomag a Power BI tartalmat](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), elemezheti az adatokat, előre konfigurált irányítópult közül választhat, vagy testre szabhatja.

### <a name="view-and-analyze-hello-access-performance-and-firewall-logs"></a>Megtekintése és elemzése hello hozzáférést, a teljesítmény és a tűzfalon naplók

Azure [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md) hello számláló és az Eseménynapló fájlokat is gyűjthet a Blob storage-fiók. Tartalmazza a képi megjelenítések és hatékony keresési képességek tooanalyze a naplókat.

Is tooyour tárfiók csatlakozhat, és hello JSON naplóbejegyzések a hozzáférés és a teljesítmény-naplók beolvasása. Hello JSON-fájlok letöltését követően tooCSV alakíthatja át őket, és megtekinthetők az Excel-, Power bi-ban, vagy bármely más adatábrázolási eszközt.

> [!TIP]
> Ha ismeri a Visual Studio és állandók és a C# változók értékeinek módosítása alapvető fogalmait, használhatja a hello [konverter eszközök jelentkezzen](https://github.com/Azure-Samples/networking-dotnet-log-converter) elérhető a Githubon.
> 
> 

## <a name="metrics"></a>Mérőszámok

Metrikák egyik újdonsága az egyes Azure-erőforrások tekintheti meg többek teljesítményszámlálók hello portálon. Az Alkalmazásátjáró még egy metrika már elérhető. Ez a metrika átviteli, és megtekintheti a hello portálon. Keresse meg az Alkalmazásátjáró tooan, és kattintson a **metrikák**. tooview hello értékek, a hello válassza átviteli **elérhető** szakasz. A kép a következő hello hello szűrőkkel használható toodisplay hello adatokat a különböző időtartományhoz példa látható.

![A szűrők metrika megtekintése][5]

toosee metrika, aktuális listáját lásd: [támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

### <a name="alert-rules"></a>A riasztási szabályok

Megkezdheti a riasztási szabályok alapján egy erőforrás metrikáit. Például egy riasztás hívható meg olyan webhook vagy a rendszergazda e-mail, ha hello átviteli sebességgel hello Alkalmazásátjáró fent, alatt vagy küszöbértékkel van a megadott idő.

hello a következő példa bemutatja, hogyan által küldött e-mailek tooan rendszergazda után átviteli megszegése küszöbérték a riasztási szabály létrehozása:

1. Kattintson a **metrika riasztás hozzáadása** tooopen hello **Hozzáadás szabály** panelen. Ezen a panelen hello metrikák paneljéről is elérhető.

   !["Metrika riasztás felvétele" gombra][6]

2. A hello **Hozzáadás szabály** panelen hello nevét, az állapot, és értesíti a szakaszok, majd kattintson **OK**.

   * A hello **feltétel** választó, válasszon ki egy hello négy értékek: **nagyobb, mint**, **nagyobb vagy egyenlő**, **kisebb, mint**, vagy  **Kisebb vagy egyenlő, mint**.

   * A hello **időszak** választó, jelölje be a időszak, 5 perc too6 óra.

   * Ha **E-mail-tulajdonosok, közreműködőknek és olvasóknak**, hello e-mail dinamikus lehet hello, akik rendelkeznek hozzáféréssel toothat erőforrás alapján. Ellenkező esetben megadhatja vesszővel tagolt azoknak a felhasználóknak a hello **további rendszergazda email(s)** mezőbe.

   ![A szabály panel hozzáadása][7]

Ha hello küszöbérték áldozata ilyen illetéktelen behatolásnak, egy e-mailt, amely hasonló toohello egyet a következő kép hello érkezik:

![E-mailt a megszegéshez tartozó küszöbérték][8]

A riasztások listája egy metrika riasztás létrehozása után jelenik meg. Minden hello riasztási szabályok áttekintést biztosít.

![Riasztások és a szabályok listája][9]

További információ a riasztási értesítések toolearn lásd [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

További információ a webhookok, és hogyan használhatja ezeket az értesítések, toounderstand látogasson el [olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="next-steps"></a>Következő lépések

* A számláló és az eseménynaplók színtartományok használatával [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md).
* [Megjelenítheti a Power bi Azure tevékenységnapló](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzést.
* [Megtekintése és elemzése a Power bi-ban és több Azure tevékenységi naplóit](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
