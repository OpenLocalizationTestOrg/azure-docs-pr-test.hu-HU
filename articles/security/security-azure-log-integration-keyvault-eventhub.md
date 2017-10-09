---
title: "az Event Hubs használatával az Azure Key Vault aaaIntegrate naplók |} Microsoft Docs"
description: "Az oktatóanyag hello szükséges lépéseket toomake Key Vault biztosító elérhető tooa SIEM naplózza az Azure napló integrálása révén"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: ada2fc846cc6bf09e12cc2c016815b27afef0d50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Azure napló integrációs Útmutató: az Event Hubs használatával folyamat az Azure Key Vault-események

Azure napló integrációs tooretrieve naplózott eseményeket használ, és tegye őket elérhetővé tooyour biztonsági információkat és az esemény (SIEM) rendszer. Ez az oktatóanyag azt szemlélteti, hogyan Azure napló integrációs használt tooprocess naplók az Azure Event Hubs beszerzett lehet.
 
Az oktatóanyag tooget szerezhessenek hogyan együtt által a következő Azure napló integráció és az Event Hubs munkahelyi hello példák azt ismertetik, és hogyan támogatja az egyes lépések a hello megoldás használata. Akkor is igénybe vehet néhány hogy megismerte itt toocreate saját lépéseket toosupport a vállalat egyedi követelményeinek.

>[!WARNING]
hello lépésekről és parancsokról ebben az oktatóanyagban nincsenek tervezett toobe másolni és beilleszteni. Példák csak fontosságúak. Ne használja az "adott állapotban" hello PowerShell-parancsok élő környezetében. Kell testre szabnia a saját egyedi környezete alapján.


Ez az oktatóanyag bemutatja, hogyan hello folyamat véve az Azure Key Vault tevékenység naplózott tooan eseményközpont, és így JSON fájlok tooyour SIEM-rendszerben érhető el. Konfigurálhatja a SIEM rendszer tooprocess hello JSON-fájlokat.

>[!NOTE]
>Ebben az oktatóanyagban hello lépéseket a legtöbb tartalmaz, amely kulcstárolót, storage-fiókok és az event hubs konfigurálása. hello adott Azure napló integrációs lépésekre hello Ez az oktatóanyag végén. Éles környezetben ne hajtsa végre ezeket a lépéseket. Csak egy tesztkörnyezetet szolgálnak. Az éles használat előtt testre kell szabnia hello lépéseket.

Megadva mentén hello módon segítséget nyújt az egyes lépések hello okaival tisztában. Hivatkozások tooother cikkek nyújtanak további információkhoz juthat az egyes témakörök.

Ez az oktatóanyag említi hello szolgáltatások kapcsolatos további információkért lásd: 

- [Azure Key Vault](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure Naplóelemzés integráció](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Kezdeti telepítés

A cikkben ismertetett hello befejezése előtt hello következőkre lesz szüksége:

1. Azure-előfizetések és az adott előfizetéshez, rendszergazdai jogosultságokkal rendelkező fiókot. Ha nem rendelkezik előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).
 
2. A rendszer, amely hozzáférési toohello internet hello Azure napló integrációs telepítéséhez szükséges követelményeknek. hello rendszer lehet egy felhőalapú szolgáltatás, vagy helyben tárolt.

3. [Azure napló-integráció](https://www.microsoft.com/download/details.aspx?id=53324) telepítve. tooinstall azt:

   a. A távoli asztal tooconnect toohello rendszer a 2. lépésben említett használja.   
   b. Hello Azure napló integrációs telepítő toohello rendszer másolja. Is [hello telepítési fájlok letöltési](https://www.microsoft.com/download/details.aspx?id=53324).   
   c. Hello a telepítőprogram indítása, és fogadja el a hello Microsoft szoftverlicenc-szerződést.   
   d. Ha telemetriai tájékoztatást fogunk adni, hagyja bejelölve hello jelölőnégyzetet. Ha nem ahelyett, hogy elküldése használati információk tooMicrosoft, törölje a hello jelölőnégyzetet.
   
   További információ az Azure napló integrációs és hogyan tooinstall, lásd: [Azure napló integráció az Azure diagnosztikai naplózás és a Windows-Eseménytovábbítást](security-azure-log-integration-get-started.md).

4. hello PowerShell letöltéséhez.
 
   Ha a Windows Server 2016 telepítve van, akkor legalább rendelkezik PowerShell 5.0. A Windows Server bármely verziója használata, lehetséges, hogy egy korábbi telepített PowerShell. Hello verzió beírásával ellenőrizheti ```get-host``` egy PowerShell-ablakban. Ha nincs PowerShell 5.0 verziója, akkor [le is](https://www.microsoft.com/download/details.aspx?id=50395).

   Miután legalább PowerShell 5.0 tooinstall hello legújabb verzióra a Folytatás:
   
   a. A PowerShell-ablakot, írja be a hello ```Install-Module Azure``` parancsot. Hello telepítés végrehajtásához.    
   b. Adja meg a hello ```Install-Module AzureRM``` parancsot. Hello telepítés végrehajtásához.

   További információkért lásd: [Azure PowerShell telepítése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## <a name="create-supporting-infrastructure-elements"></a>Támogató infrastruktúra elemeinek létrehozása

1. Nyisson meg egy emelt szintű PowerShell ablakot, és nyissa meg túl**C:\Program Files\Microsoft Azure napló integrációs**.
2. Importálja a hello AzLog parancsmagok LoadAzLogModule.ps1 hello parancsfájl futtatásával. Adja meg a hello `.\LoadAzLogModule.ps1` parancsot. (Értesítés hello ". \" parancsnak a.) Ennek nagyjából a következőképpen kell kinéznie:</br>

   ![A betöltött modulok listáján](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Adja meg a hello `Login-AzureRmAccount` parancsot. Hello bejelentkezési ablakban adja meg, amelyekkel a jelen oktatóanyag hello előfizetés hello hitelesítő adatokat.

   >[!NOTE]
   >Ha hello először, akkor még a bejelentkezés tooAzure erről a gépről, akkor engedélyezése a Microsoft toocollect PowerShell használati adatok egy üzenet jelenik meg. Azt javasoljuk, hogy engedélyezze az adatgyűjtés, mert használt tooimprove Azure PowerShell lesz.

4. Sikeres hitelesítést követően jelentkezett be, és a következő képernyőkép hello hello adatok láthatók. Jegyezze fel a hello előfizetési azonosító és az előfizetés nevét, mert lesz szüksége toocomplete később lépések.

   ![PowerShell-ablakot](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Változók létrehozása toostore értékeket, amelyeket a rendszer később. Adja meg az alábbi PowerShell hello mindegyikének. Szükség lehet tooadjust hello értékek toomatch a környezetben.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Az előfizetés neve eltérő lehet. Megnézheti hello kimeneti hello előző parancs részeként.)
    - ```$location = 'West US'```(Ez a változó lehet használt toopass hello helyre, ahol erőforrásokat kell létrehozni. A változó toobe bármely helyét módosíthatja a.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random```(hello neve bármi lehet, de ez csak kisbetűket és számokat kell tartalmaznia.)
    - ``` $storageName = $name```(Ezt a változót használja a tárfiók nevének hello.)
    - ```$rgname = $name ```(Ez a változó használandó hello erőforráscsoport-név.)
    - ``` $eventHubNameSpaceName = $name```(Ez a hello hello event hub névtér nevét.)
6. Adja meg, hogy a működik hello előfizetés:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Hozzon létre egy erőforráscsoportot:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   Ha beírja `$rg` Ekkor megtekintheti az kimeneti hasonló toothis képernyőkép:

   ![Kimeneti erőforráscsoport létrehozása után](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Hozzon létre egy tárfiókot, amely állapotadatokat használt tookeep nyomon lesz:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Hello event hub névtér létrehozása. Ez azért szükség toocreate egy eseményközpontba.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. Töltse le a hello insights szolgáltatónál használandó hello Szabályazonosító:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Az összes lehetséges az Azure-helyeinek lekéréséhez, és adja hozzá a hello nevek tooa változó, amely használható egy későbbi lépésben:
    
    a. ```$locationObjects = Get-AzureRMLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Ha beírja `$locations` ekkor megjelenik az hello helynevek hello Get-AzureRmLocation által visszaadott további adatok nélkül.
12. Azure Resource Manager napló profil létrehozása: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Hello Azure naplóelemzés profil kapcsolatos további információkért lásd: [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

> [!NOTE]
> Egy hibaüzenetet kaphat toocreate napló profil meg. Majd a Get-AzureRmLogProfile és eltávolítása-AzureRmLogProfile hello dokumentációjában tekintheti meg. Ha a Get-AzureRmLogProfile futtatja, lásd: hello napló profillal kapcsolatos információk. Hello meglévő napló profil törlése hello megadásával ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` parancsot.
>
>![Erőforrás-kezelő profil hiba](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Kulcstartó létrehozása

1. Hello kulcstároló létrehozása:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Hello kulcstároló naplózásának konfigurálása:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Napló tevékenység létrehozása

Kérelmek tooKey tároló toogenerate naplózása küldött toobe kell. Műveletek, például Kulcslétrehozási, titkok, tárolása, vagy titkos kulcsok beolvasása a Key Vault naplóbejegyzések hoz létre.

1. Hello aktuális tárolási kulcsok megjelenítése:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Egy új készítése **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. Jelenjen meg többé hello kulcsok és, hogy látható **key2** egy másik értéket tartalmazza:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Állítsa be, és olvassa el a titkos toogenerate további naplóbejegyzések:
    
   a. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Visszaadott titkos](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Azure Naplóelemzés-integráció konfigurálása

Most, hogy az összes hello belőlük bizonyos megkövetelt elemek toohave Key Vault naplózásának tooan eseményközpont van beállítva, tooconfigure Azure napló integrációs szüksége:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Futtassa az egyes eseményközpontban hello AzLog parancsot:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Egy perc vagy így hello utolsó két parancsok futtatására megtekintheti az JSON-fájlok létrehozása folyamatban. Hello directory figyelésével ellenőrizheti, hogy **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Következő lépések

- [Azure Naplóelemzés integrációs – gyakori kérdések](security-azure-log-integration-faq.md)
- [Ismerkedés az Azure napló integráció](security-azure-log-integration-get-started.md)
- [Az Azure-erőforrások naplók integrálja a SIEM-rendszerekről](security-azure-log-integration-overview.md)
