---
title: "Key Vault megoldás Naplóelemzési aaaAzure |} Microsoft Docs"
description: "Az Azure Key Vault-naplók Naplóelemzési tooreview hello Azure Key Vault megoldás is használhatja."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 1c6eae26ded7ad55b0159a3be09cdc9901596298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a>Log Analytics az Azure Key Vault Analytics megoldás

![Key Vault szimbólum](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

A Naplóelemzési tooreview naplózza az Azure Key Vault AuditEvent hello Azure Key Vault megoldás is használhatja.

toouse hello megoldás, az Azure Key Vault diagnosztika és a közvetlen hello diagnosztika tooa Naplóelemzési munkaterület tooenable naplózása szüksége. Már nem szükséges toowrite hello naplók tooAzure Blob-tároló.

> [!NOTE]
> 2017. január hello támogatott módszer a naplók küldésével Key Vault tooLog Analytics megváltozott. Ha hello Key Vault megoldás használatával jeleníti meg a *(elavult)* hello cím alatt túl[hello régi Key Vault megoldás áttelepítés](#migrating-from-the-old-key-vault-solution) lépéseket toofollow kell.
>
>

## <a name="install-and-configure-hello-solution"></a>Telepítse és konfigurálja a hello megoldás
A következő utasításokat tooinstall hello használja, és hello Azure Key Vault megoldás konfigurálása:

1. Hello Azure Key Vault megoldást engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Diagnosztikai naplózás a Key Vault-erőforrások toomonitor hello, vagy hello segítségével engedélyezése [portal](#enable-key-vault-diagnostics-in-the-portal) vagy [PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-hello-portal"></a>Key Vault diagnosztika hello portálon engedélyezése

1. Hello Azure-portálon lépjen a toohello Key Vault erőforrás toomonitor
2. Válassza ki *diagnosztikai naplók* tooopen hello a következő lap

   ![az Azure Key Vault csempe képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. Kattintson a *a diagnosztika bekapcsolásához* tooopen hello a következő lap

   ![az Azure Key Vault csempe képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. a diagnosztikát, tooturn kattintson *a* alatt *állapota*
5. Kattintson a hello jelölőnégyzetét *tooLog Analytics küldése*
6. Válasszon ki egy meglévő Naplóelemzési munkaterület, vagy egy munkaterület létrehozása
7. tooenable *AuditEvent* naplókat, kattintson a hello jelölőnégyzetet a napló
8. Kattintson a *mentése* diagnosztika tooLog Analytics tooenable hello naplózása

### <a name="enable-key-vault-diagnostics-using-powershell"></a>Engedélyezze a Key Vault diagnosztika PowerShell használatával
a következő PowerShell-parancsfájl hello azt szemlélteti, hogyan toouse `Set-AzureRmDiagnosticSetting` tooenable diagnosztikai Key Vault naplózása:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Tekintse át az Azure Key Vault az gyűjtemény adatait
Az Azure Key Vault megoldás diagnosztikai naplók közvetlenül a Key Vault hello gyűjti.
Már nem szükséges toowrite hello naplók tooAzure Blob-tároló, és nincs ügynök szükséges adatok gyűjtését.

hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan Azure Key Vault részleteit.

| Platform | Közvetlen ügynök | Systems Center Operations Manager-ügynök | Azure | Az Operations Manager szükséges? | Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött | A gyűjtés gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | érkezésükkor |

## <a name="use-azure-key-vault"></a>Az Azure Key Vault használata
Miután [hello megoldás telepítése](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), hello kulcstároló adatainak megtekintéséhez kattintson a hello **Azure Key Vault** hello a csempe **áttekintése** Naplóelemzési oldalán.

![az Azure Key Vault csempe képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Miután rákattintott hello **áttekintése** csempe, megtekintheti a naplókat, valamint majd részletezési összegzéseinek toodetails a következő kategóriák hello:

* Az összes kulcstároló művelet időbeli kötet
* Nem sikerült a művelet kötetek adott idő alatt
* Átlagos műveleti várakozási művelet
* Szolgáltatásminőség műveletekhez hello száma, amelyek több mint 1000 ms műveletek és a műveleteket, amelyek több mint 1000 ms listája

![az Azure Key Vault irányítópult képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![az Azure Key Vault irányítópult képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="tooview-details-for-any-operation"></a>bármely művelet tooview részletei
1. A hello **áttekintése** hello kattintson **Azure Key Vault** csempére.
2. A hello **Azure Key Vault** az irányítópult hello összefoglaló információkat valamelyik hello paneleken ellenőrizni, és kattintson egy tooview részletes információkat, akkor a hello napló keresése oldal.

    Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók. Értékkorlátozás toonarrow hello eredmények is végezhet.

## <a name="log-analytics-records"></a>Log Analytics-rekordok
az Azure Key Vault megoldás hello elemzi azt jelzi, hogy rendelkezik olyan típusú **KeyVaults** , hogy a rendszer begyűjti az [AuditEvent naplók](../key-vault/key-vault-logging.md) az Azure Diagnostics.  Ezeket a rekordokat tulajdonságainak vannak a következő táblázat hello:  

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| CallerIpAddress |Hello kérelmet leadó hello ügyfél IP-címe |
| Kategória | *AuditEvent* |
| CorrelationId |Egy nem kötelező GUID, amely az ügyfél hello teljen toocorrelate ügyféloldali naplók és a Szolgáltatásoldali (Key Vault) naplók. |
| DurationMs |Idő, ezredmásodpercben megadva tooservice hello REST API-kérelem. Most nem tartalmazza a hálózati késés, így hello időt, amely mérni a hello ügyféloldalon nem egyeznek meg a megadott idő. |
| httpStatusCode_d |Hello kérelem által visszaadott HTTP-állapotkódot (például *200*) |
| id_s |Hello kérelem egyedi azonosítója |
| identity_claim_appid_g | Hello alkalmazásazonosító GUID azonosítója |
| OperationName |Hello művelet, ahogy neve [Azure Key Vault naplózása](../key-vault/key-vault-logging.md) |
| OperationVersion |Hello ügyfél által kért REST API-verzió (például *2015-06-01*) |
| requestUri_s |Hello kérelem URI-val |
| Erőforrás |Hello kulcstároló neve |
| ResourceGroup |Hello kulcstároló erőforráscsoport |
| ResourceId |Az Azure Resource Manager szerinti erőforrás-azonosító. Key Vault naplóinak ez pedig hello Key Vault erőforrás-azonosító. |
| ResourceProvider |*MICROSOFT. KEYVAULT* |
| ResourceType | *TÁROLÓK* |
| ResultSignature |HTTP-állapot (például *OK*) |
| ResultType |REST API-kérelem eredménye (például *sikeres*) |
| SubscriptionId |Azure-előfizetése Azonosítóját tartalmazó hello Key Vault hello előfizetés |

## <a name="migrating-from-hello-old-key-vault-solution"></a>Hello régi Key Vault megoldás áttelepítése
2017. január hello támogatott módszer a naplók küldésével Key Vault tooLog Analytics megváltozott. Ezek a változások hello a következő előnyöket biztosítja:
+ Közvetlen tooLog Analytics hello nélkül kell toouse tárfiók írja a naplókat
+ Kisebb késést hello időpontot, amikor naplók a generált legyenek elérhetők a Naplóelemzési toothem
+ Kevesebb konfigurációs lépések
+ Az összes Azure diagnostics közös formátum

toouse hello megoldás frissítése:

1. [Diagnosztika toobe küldött Key Vault közvetlenül tooLog Analytics konfigurálása](#enable-key-vault-diagnostics-in-the-portal)  
2. Engedélyezze a hello Azure Key Vault megoldás hello ismertetett folyamatot [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjteménye](log-analytics-add-solutions.md)
3. A lekérdezések, irányítópultok vagy riasztások toouse hello új adattípusra frissítése
  + Típus: módosítást: KeyVaults tooAzureDiagnostics. Hello ResourceType toofilter tooKey Vault naplóinak is használhatja.
  - Ahelyett, hogy: `Type=KeyVaults`, használata`Type=AzureDiagnostics ResourceType=VAULTS`
  + Mezők: (mezőnevek számítanak a kis-és nagybetűket)
  - Bármely mezőhöz, amely rendelkezik egy utótagja \_s, \_d, vagy \_g a módosítás hello első karakter toolower esetben hello neve
  - Bármely mezőhöz, amely rendelkezik egy utótagja \_a név hello adatok o oszlik, egyedi mezők beágyazott hello mezőnevek alapján. Például egy mező hello hello hívó UPN tárolja`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   - Módosított mező CallerIpAddress tooCallerIPAddress
   - A mező RemoteIPCountry már nincs jelen
4. Távolítsa el a hello *Key Vault Analytics (elavult)* megoldás. Ha a PowerShell használata esetén`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Adatgyűjtés előtt hello módosítása nem látható a hello új megoldás. A régi típusú és mezőnevek ezen adatok segítségével hello tooquery folytathatja.

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes adatok az Azure Key Vault.
