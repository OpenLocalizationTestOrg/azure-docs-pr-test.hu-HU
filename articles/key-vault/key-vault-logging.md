---
title: "aaaAzure Key Vault naplózása |} Microsoft Docs"
description: "Használja az Azure Key Vault használatának első lépéseit az oktatóanyag toohelp naplózása."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 38a173297948748bef45e3d857c06b50b3e21e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-logging"></a>Az Azure Key Vault naplózása
Az Azure Key Vault a legtöbb régióban elérhető. További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Bevezetés
Miután létrehozott egy vagy több kulcstároló naplófájljainak, érdemes toomonitor használt, és ki hogyan és mikor történjen a kulcs-tárolók. Ehhez engedélyezze a Key Vault naplózását, amely egy Ön által megadott Azure-tárfiókba menti az adatokat. A megadott tárfiókhoz automatikusan létrehozunk egy **insights-logs-auditevent** nevű tárolót, amelyet több kulcstároló naplófájljainak tárolására is használhat.

Elérheti a naplóinformációkat legfeljebb, hello kulcs követő 10 percen kulcstároló műveletei. A legtöbb esetben azonban ez nem fog ennyi ideig tartani.  Hogy működik-e tooyou toomanage a tárfiók naplófájljait:

* A naplók az Azure szabványos hozzáférés-vezérlési módszerek toosecure használatára korlátozza, hogy ki férhet hozzá.
* Törölje a megjeleníteni nem kívánt tookeep a tárfiókban lévő naplókat.

Az Azure Key Vault naplózása, toocreate használatának első lépéseit az oktatóanyag toohelp használja a tárfiók naplózását, és hello összegyűjtött naplóinformációk értelmezésével.  

> [!NOTE]
> Ez az oktatóanyag nem tartalmazza a hogyan toocreate tárolók, a kulcsok vagy titkos kulcs. Ezekről a [Get started with Azure Key Vault](key-vault-get-started.md) (Bevezetés az Azure Key Vault használatába) című cikkben találhat információt. A platformfüggetlen parancssori felületre vonatkozó utasításokat megtekintheti [ebben a megfelelő oktatóanyagban](key-vault-manage-with-cli2.md).
>
> Jelenleg nem konfigurálhatja az Azure Key Vault hello Azure-portálon. Ehelyett kövesse ezeket az Azure PowerShell-utasításokat.
>
>

Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az az Azure Key Vault?) című cikkben találhat.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:

* Egy meglévő kulcstároló.  
* Az Azure PowerShell **legalább 1.0.1-es verziója**. Azure PowerShell tooinstall és rendelje hozzá azt az Azure-előfizetéssel, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Ha már telepítette az Azure PowerShell, és nem tudja hello verzió hello Azure PowerShell-konzolon, írja be a `(Get-Module azure -ListAvailable).Version`.  
* A Key Vault naplóihoz elegendő tárhely az Azure-ban.

## <a id="connect"></a>Csatlakozás tooyour előfizetések
Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancsot:  

    Login-AzureRmAccount

Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát. Az Azure PowerShell beolvassa hello előfizetéseket, amelyek társítva ezzel a fiókkal, és alapértelmezés szerint, használja az elsőt hello.

Ha több előfizetéssel rendelkezik, lehetséges, hogy toospecify, de a használt toocreate melyiket az Azure Key Vault. Írja be a fiókhoz toosee hello előfizetések a következő hello:

    Get-AzureRmSubscription

Ezt követően toospecify hello előfizetés, amely a kulcstartót akkor lesz naplózás, típusa van társítva:

    Set-AzureRmContext -SubscriptionId <subscription ID>

> [!NOTE]
> Ez egy nagyon fontos lépés, és különösen hasznosnak bizonyulhat, ha több előfizetés tartozik a fiókjához. Egy hiba tooregister Microsoft.Insights is megjelenhet, ha ez a lépés kimarad.
>   
>

Azure PowerShell konfigurálásával kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a id="storage"></a>Új tárfiók létrehozása a naplóknak
Bár a naplók használhat meglévő tárfiókot, mi létrehozunk egy új tárfiókot, amely dedikált tooKey Vault naplóinak lesz. Kényelmi kell toospecify ezt később, azt fogja tárolni hello részletek egy változóba nevű **sa**.

Az egyszerű, a is használjuk hello ugyanabban az erőforráscsoportban, mint a kulcstároló tartalmazó hello. A hello [használatába bevezető oktatóanyagot](key-vault-get-started.md), ez az erőforráscsoport neve **ContosoResourceGroup** és toouse hello Kelet-Ázsia helyét is. Az alábbi értékeket helyettesítse a sajátjainak megfelelőkkel:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> Ha úgy dönt, hogy a meglévő tárfiók toouse, azt kell használnia hello ugyanahhoz az előfizetéshez, mint a kulcstartót és hello Resource Manager üzembe helyezési modellben, nem pedig hello klasszikus telepítési modellt kell használnia.
>
>

## <a id="identify"></a>Hello a naplók kulcstárolójának azonosítása
Az oktatóanyagban a kulcstároló neve lett **ContosoKeyVault**, így az is, amelyek neve és a tárolás a hello részletek nevű változó toouse **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Naplózás engedélyezése
tooenable Key Vault naplózását, hello Set-AzureRmDiagnosticSetting parancsmaggal fogjuk használni, az új tárfiók és a key vault létrehozott hello változók együtt. Hello is be **-engedélyezve** túl jelzőt**$true** hello kategória tooAuditEvent (hello egyetlen kategóriája Key Vault naplózása), és:

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

hello kimenet tartalmazza:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Ez megerősíti, hogy naplózás engedélyezve van a kulcstartót információk tooyour storage-fiók mentése.

Opcionálisan beállíthat egy megtartási házirendet a naplóihoz, így a régebbi naplófájlok automatikusan törlődni fognak. Például a megőrzési házirend használatával be **- RetentionEnabled** túl jelzőt**$true** és **- RetentionInDays** paraméter túl**90** , hogy a naplófájlok 90 napnál régebbi automatikusan törölve lesznek.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Mi kerül naplózásra?

* Minden hitelesített REST API-kérés naplózásra kerül, beleértve a hozzáférési engedélyekből, rendszerhibákból vagy hibás kérésekből adótó sikertelen kérelmeket is.
* Műveletek hello kulcsot tároló magát, beleértve a létrehozás, törlés, beállítás kulcstároló hozzáférési szabályzatainak, és címkéket, mint a kulcstároló tulajdonságainak frissítése.
* Műveletek a kulcsok és titkos hello a key vaultban, amely tartalmazza a létrehozása, módosítása vagy törlése a kulcsok vagy titkos; Műveletek, például a bejelentkezési, győződjön meg arról, titkosítás, visszafejtése, burkolja és kulcsok kicsomagolásához, titkos kulcsok, listában kulcsok és titkos kulcsok és a verzióját.
* A 401-es választ eredményező, nem hitelesített kérelmek. Ilyenek például azok a kérelmek, amelyek nem rendelkeznek tulajdonosi jogkivonattal, helytelen formátumúak vagy lejártak, vagy érvénytelen a jogkivonatuk.  

## <a id="access"></a>A naplók elérése
Kulcstároló naplófájljainak tárolása a hello **insights-logs-auditevent** hello storage-fiók a megadott tárolóhoz. toolist valamennyi hello BLOB a tárolóban található írja be:

Először hozzon létre egy változót hello tároló neve. Ez fogja használni a teljes hello lépésein végighaladva hello többi részétől.

    $container = 'insights-logs-auditevent'

toolist valamennyi hello BLOB a tárolóban található írja be:

    Get-AzureStorageBlob -Container $container -Context $sa.Context
hello kimeneti valami hasonló toothis fog kinézni:

**Tároló URI-ja: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**

**Name (Név)**

- - -
**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****

Mivel a kimenetből látható, hello blobok egy elnevezési konvenciója: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/fájlnév.JSON**

hello dátum- és időértékek az UTC használata.

Mivel a hello ugyanazt a tárfiókot több erőforrást használt toocollect naplókat, hello hello blob nevének teljes erőforrás-azonosító nem tooaccess nagyon hasznos vagy szükséges letöltési csak hello blobokat. De előtte még a nem, azt először érintünk hogyan toodownload összes hello blobokat.

Először hozzon létre egy mappát toodownload hello blobokat. Példa:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Majd kérje le az összes blob listáját:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Ebben a listában, a "Get-AzureStorageBlobContent" toodownload hello blobok átadhatja a célként megadott mappába:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

A második parancs futtatásakor hello  **/**  hello blob nevének határolója hozzon létre egy teljes mapparendszert hello célmappát, és ez a struktúra lesz használt toodownload tároló hello blobok és -fájlok formájában.

tooselectively blobok letöltéséhez használjon helyettesítő karaktereket. Példa:

* Ha több kulcstárolóval rendelkezik, és toodownload naplók csak egy kulcstartót, csak a contosokeyvault3 nevűhöz szeretne:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* Ha több erőforráscsoportban, és szeretné toodownload naplók csak egy erőforráscsoport, `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* Programmal toodownload összes hello napló 2016. január hello hónap, `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Most, hogy most már készen áll a hello megnézi toostart naplózza. Mielőtt ezt a két paramétert a Get-azurermdiagnosticsetting parancshoz, hogy szükség lehet a tooknow azonban:

* a kulcstároló erőforrásához tartozó diagnosztikai beállítások tooquery hello állapota:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`
* a kulcstároló erőforrásához toodisable naplózás:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`

## <a id="interpret"></a>A Key Vault naplóinak értelmezése
Az egyes blobok JSON-blobként, szöveges formában vannak tárolva. Íme egy példa a `Get-AzureRmKeyVault -VaultName 'contosokeyvault'` parancsot futtató naplóbejegyzésre:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


hello következő táblázatban hello mezők neveit és leírásait.

| Mező neve | Leírás |
| --- | --- |
| time |Dátum és idő (UTC). |
| resourceId |Az Azure Resource Manager szerinti erőforrás-azonosító. A Key Vault-naplók esetében mindig ez hello Key Vault erőforrás-azonosító. |
| operationName |Hello művelet, a következő tábla hello ismertetett neve. |
| operationVersion |Ez a hello hello ügyfél által kért REST API-verzió. |
| category |A Key Vault-naplók AuditEvent érték hello egyetlen, érhető el. |
| resultType |A REST API-kérelem eredménye. |
| resultSignature |A HTTP-állapot. |
| resultDescription |Hello eredmény, ha elérhető további leírása. |
| durationMs |Idő, ezredmásodpercben megadva tooservice hello REST API-kérelem. Hello hálózati késés, ez nem vonatkozik, ezért mérni a hello ügyféloldalon hello idő nem egyeznek meg a megadott idő. |
| callerIpAddress |Hello kérelmet leadó hello ügyfél IP-címe. |
| correlationId |Egy nem kötelező GUID, amely az ügyfél hello teljen toocorrelate ügyféloldali naplók és a Szolgáltatásoldali (Key Vault) naplók. |
| identity |Hello tokenben hello REST API-kérelem meghozásakor szereplő identitás. Ez általában egy „felhasználó”, „egyszerű szolgáltatásnév” vagy „felhasználó + alkalmazás-azonosító”, az Azure PowerShell-parancsmagok által eredményezett kérelmekhez hasonlóan. |
| properties |Ez a mező alapján hello művelettől (operationName) kapcsolatos különféle információk fogja tartalmazni. A legtöbb esetben ügyféladatokat (hello sztringjét hello ügyfél), tartalmaz pontos REST API-kérelem URI-azonosítója és a HTTP-állapotkód: hello. Továbbá amikor egy kérelem (például a KeyCreate vagy a VaultGet) eredményeként ad vissza egy objektumot is tartalmaz hello kulcs URI-JÁT ("id"), a tároló URI vagy a titkos kulcs URI. |

Hello **operationName** mező értékei ObjectVerb formátumúak. Példa:

* Minden kulcstárolón elvégzett művelet hello "tárolóban`<action>`" formátumú, például `VaultGet` és `VaultCreate`.
* Minden kulcson elvégzett művelet hello "kulcs`<action>`" formátumú, például `KeySign` és `KeyList`.
* Minden titkos kulcson elvégzett művelet hello "Secret`<action>`" formátumú, például `SecretGet` és `SecretListVersions`.

hello a következő táblázat felsorolja a hello operationname műveleteket és a megfelelő REST API-parancsot.

| operationName | REST API-parancs |
| --- | --- |
| Authentication |Az Azure Active Directory végpontján keresztül |
| VaultGet |[Kulcstároló adatainak lekérése](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| VaultPut |[Kulcstároló létrehozása vagy frissítése](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| VaultDelete |[Kulcstároló törlése](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| VaultPatch |[Kulcstároló frissítése](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultList |[Az erőforráscsoport összes kulcstárolójának listázása](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| KeyCreate |[Kulcs létrehozása](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| KeyGet |[Kulcs adatainak lekérése](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| KeyImport |[Kulcs importálása egy tárolóba](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| KeyBackup |[Kulcs biztonsági mentése](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx). |
| KeyDelete |[Kulcs törlése](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| KeyRestore |[Kulcs helyreállítása](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| KeySign |[Aláírás kulccsal](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| KeyVerify |[Ellenőrzés kulccsal](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| KeyWrap |[Kulcs becsomagolása](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| KeyUnwrap |[Kulcs kicsomagolása](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| KeyEncrypt |[Titkosítás kulccsal](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| KeyDecrypt |[Visszafejtés kulccsal](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| KeyUpdate |[Kulcs frissítése](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| KeyList |[Egy tároló kulcsainak hello listája](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| KeyListVersions |[A kulcs verzióinak listázása hello](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| SecretSet |[Titkos kulcs létrehozása](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| SecretGet |[Titkos kulcs lekérése](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| SecretUpdate |[Titkos kulcs frissítése](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| SecretDelete |[Titkos kulcs törlése](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| SecretList |[Egy tároló titkos kulcsainak listázása](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| SecretListVersions |[Titkos kulcs verzióinak listázása](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>A Log Analytics használata

A Naplóelemzési tooreview naplózza az Azure Key Vault AuditEvent hello Azure Key Vault megoldás is használhatja. További információt, beleértve a hogyan tooset, ez: [Naplóelemzési megoldás az Azure Key Vault](../log-analytics/log-analytics-azure-key-vault.md). A cikk utasításokat is tartalmaz, ha toomigrate hello régi Key Vault-megoldásból által kínált hello Naplóelemzési előzetes, amelyen először átirányítva a naplók tooan Azure Storage-fiók, és ott Naplóelemzési tooread konfigurálva kell.

## <a id="next"></a>Következő lépések
Az Azure Key Vault webalkalmazásban való használatáról a [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md) (Az Azure Key Vault webalkalmazással való használata) című témakörben találhat útmutatást.

Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).

Az Azure Key Vaultra vonatkozó Azure PowerShell 1.0-parancsmagok listáját az [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault) (Az Azure Key Vault parancsmagjai) című témakörben találja.

Kulcs rotációjával és az Azure Key Vault naplózása napló oktatóanyag, lásd: [hogyan end tooend a Key Vault toosetup kulcs elforgatás és naplózási](key-vault-key-rotation-log-monitoring.md).
