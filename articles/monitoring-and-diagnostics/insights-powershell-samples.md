---
title: "aaaAzure figyelő PowerShell gyors üzembe helyezési minta. | Microsoft Docs"
description: "Használjon PowerShell tooaccess Azure figyelő szolgáltatásokkal, például az automatikus skálázás, riasztások, webhookok és tevékenységi naplóit keresése."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6eece0b0227e0bbf08225bd330d0601169911f55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>A figyelő PowerShell Azure gyors üzembe helyezési-minták
A PowerShell-parancsok toohelp figyelő Azure-szolgáltatások elérésének mintavételhez. a cikk tartalmazza. Az Azure a figyelő lehetővé teszi tooAutoScale Felhőszolgáltatásokat, a virtuális gépek és a Web Apps és a toosend riasztási értesítések és hívás webes URL-címek konfigurált telemetriai adatok értékek alapján.

> [!NOTE]
> Az Azure a figyelő az hello új neve "Azure Insights" nevezett csak 2016. Szeptembertől 25. Hello névterek, és így a következő parancsok továbbra is hello tartalmazhat hello "insights".
> 
> 

## <a name="set-up-powershell"></a>PowerShell beállítása
Ha még nem tette meg, állítsa be a számítógépre a PowerShell toorun. További információkért lásd: [hogyan tooInstall és PowerShell konfigurálása](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>A cikkben szereplő példák
hello cikkben hello példák bemutatják, hogyan használhatja az Azure-figyelő parancsmagok. Emellett áttekintheti, Azure figyelő PowerShell parancsmagok teljes listája hello [Azure Monitor (Insights) parancsmagok](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).

## <a name="sign-in-and-use-subscriptions"></a>Jelentkezzen be, és előfizetések használata
Első lépésként jelentkezzen be Azure-előfizetés tooyour.

```PowerShell
Login-AzureRmAccount
```

Ehhez a toosign. Ha így tesz, a fiókjához, megjelenik a TenantID és alapértelmezett előfizetés-azonosító. Az összes hello Azure-parancsmagokkal munkahelyi alapértelmezett előfizetése hello környezetében. előfizetések listáját tooview hello rendelkezik hozzáféréssel, a következő parancs hello használata.

```PowerShell
Get-AzureRmSubscription
```

toochange működő környezetben tooa különböző előfizetését, a következő parancs használata hello.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Az előfizetéshez tevékenységnapló beolvasása
Használjon hello `Get-AzureRmLog` parancsmag.  hello az alábbiakban néhány gyakori példán.

Napló bejegyzéseinek lekérése a időpontot vagy dátumot toopresent:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Egy időpontot vagy dátumot tartományba eső naplóbejegyzések beolvasása:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Naplóbejegyzéseket beszerzése egy adott erőforráscsoporthoz:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Egy adott erőforrás-szolgáltató egy időpontot vagy dátumot tartományba eső beolvasása a naplóbejegyzések:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Minden naplóbejegyzések adott hívó az beszerzése:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

a következő parancs beolvassa hello hello hello tevékenységnapló utolsó 1000 események:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`sok más paramétereket támogatja. Lásd: hello `Get-AzureRmLog` további információt.

> [!NOTE]
> `Get-AzureRmLog`csak a előzmények számított 15 nyújt. Hello segítségével **- MaxEvents** paraméter lehetővé teszi tooquery hello utolsó N események, 15 napon túl. régebbi, mint 15 nappal tooaccess események hello REST API vagy (C#-mintákkal hello SDK) SDK használatára. Ha nem adja meg **StartTime**, majd hello alapértelmezett értéke **EndTime** mínusz 1 óra. Ha nem adja meg **EndTime**, majd hello alapértelmezett érték az aktuális idő. Minden alkalommal UTC szerepelnek.
> 
> 

## <a name="retrieve-alerts-history"></a>Riasztások listájáról beolvasása
minden riasztási események, lekérheti tooview hello Azure Resource Manager-naplók a következő példák hello segítségével.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

tooview hello előzmények adott riasztásra vonatkozó szabály, használhatja a hello `Get-AzureRmAlertHistory` parancsmag benyújtása hello riasztási szabály hello erőforrás-azonosító.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Hello `Get-AzureRmAlertHistory` parancsmag különböző paramétereket támogatja. További információkért lásd: [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>A riasztási szabályokhoz adatbeolvasás
A következő parancsok hello mindegyikét intézkedjen "montest" nevű erőforráscsoport.

Hello riasztási szabály az összes hello tulajdonságainak megtekintése:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Az erőforráscsoport összes riasztás beolvasása:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

A tároló-erőforrások esetében minden riasztási szabályok beolvasása. Például a virtuális gép beállított összes riasztási szabályok.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`más paramétereket támogatja. Lásd: [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) további információt.

## <a name="create-metric-alerts"></a>Riasztások metrika létrehozása
Használhatja a hello `Add-AlertRule` parancsmag toocreate frissítése, vagy tiltsa le a riasztási szabályt.

E-mailek és a webhook tulajdonságok használatával hozhat létre `New-AzureRmAlertRuleEmail` és `New-AzureRmAlertRuleWebhook`, illetve. Rendelje hozzá hello riasztási szabály parancsmag, ezek műveletek toohello **műveletek** hello riasztási szabály tulajdonsága.

a következő táblázat hello hello paramétereket ismerteti, és értékek használt toocreate metrika használatával riasztást.

| A paraméter | érték |
| --- | --- |
| Név |simpletestdiskwrite |
| A riasztási szabály helye |USA keleti régiója |
| ResourceGroup |montest |
| Targetresourceid azonosítója |/Subscriptions/S1/resourceGroups/montest/Providers/Microsoft.COMPUTE/virtualMachines/testconfig |
| MetricName létrehozott hello riasztás |\PhysicalDisk (_Total) \Disk/mp. Lásd: hello `Get-MetricDefinitions` parancsmag kapcsolatos hogyan tooretrieve hello pontos metrika neve |
| Operátor |GreaterThan |
| A küszöbérték (száma másodpercenként a Ez a mérőszám a) |1 |
| Ablakméret (ÓÓ: pp: formátum) |00:05:00 |
| a gyűjtő (statisztika hello mérőszám, amely ebben az esetben használja az átlagos száma) |Átlagos |
| egyéni e-mailek (karakterlánc-tömbben) |'foo@example.com','bar@example.com' |
| küldjön e-mailek tooowners, közreműködő szerepkörrel rendelkező személyek és olvasók |-SendToServiceOwners |

E-mailek művelet létrehozása

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

A Webhook művelet létrehozása

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Hello riasztási szabályt létrehozni hello CPU % metrika egy klasszikus virtuális gépen

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Riasztási szabály hello beolvasása

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

hello Hozzáadás riasztási parancsmag frissíti hello szabályt is, ha a riasztási szabály már létezik a megadott tulajdonságok hello. riasztási szabály, toodisable tartalmazza hello paraméter **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>A riasztások elérhető listájának lekérdezése
Használhatja a hello `Get-AzureRmMetricDefinition` parancsmag tooview hello listája egy adott erőforráshoz tartozó összes metrikát.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

hello alábbi példa állít elő, az egység hello és táblázat hello metrikájú nevét.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Az elérhető lehetőségek teljes listáját `Get-AzureRmMetricDefinition` érhető el [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-autoscale-settings"></a>Automatikus skálázási beállítások létrehozása és kezelése
Egy erőforrás, például egy webalkalmazást, a virtuális gép, a felhőalapú szolgáltatás vagy a virtuálisgép-méretezési csoport rendelkezhet beállított csak egy automatikus skálázási beállítás.
Van azonban, az minden automatikus skálázási beállítás profiljainak. Például egy méretezési teljesítmény-alapú profil és egy másikat a ütemezésalapú profilra. Az egyes profilok rendelkezhet több szabály konfigurálva. Automatikus méretezéssel kapcsolatos további információkért lásd: [hogyan tooAutoscale alkalmazás](../cloud-services/cloud-services-how-to-scale.md).

Az alábbiakban hello lépéseket fogjuk használni:

1. Szabályok létrehozása.
2. Profil létrehozása leképezési hello szabályokat, hogy korábban létrehozott toohello profilok.
3. Választható lehetőség: Az automatikus skálázás értesítések létrehozásához webhook és e-mailek tulajdonságainak konfigurálása.
4. Az automatikus skálázási beállítás hello cél erőforráson nevű létrehozása leképezi a hello-profilok és hello előző lépésekben létrehozott értesítéseket.

hello következő példák azt szemléltetik, hogyan hozhat létre az automatikus skálázási beállítás egy virtuálisgép-méretezési csoportban hello CPU-kihasználtság metrika használatával alapú Windows operációs rendszerhez.

Először hozzon létre egy szabályt tooscale kibővített, a példány számának növelését.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Ezután létrehoz egy szabályt tooscale a számára egy példány száma csökken.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Ezután hozzon létre egy profilt hello szabályok.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Hozzon létre egy webhook tulajdonságot.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Hozzon létre hello értesítési tulajdonság hello automatikus skálázási beállításhoz, beleértve az e-mailt, és korábban létrehozott webhook hello.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Végezetül hello automatikus skálázási beállítás tooadd hello-profil létrehozása, amely a fenti létrehozott.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Automatikus skálázási beállítások kezelésével kapcsolatos további információkért lásd: [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Automatikus skálázás előzmények
hello alábbi példa bemutatja, hogyan megtekintheti a legutóbbi automatikus skálázás és riasztási események. Hello napló keresése tooview hello automatikus skálázás tevékenységelőzményeihez használja.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Használhatja a hello `Get-AzureRmAutoScaleHistory` parancsmag tooretrieve automatikus skálázás előzményeit.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

További információkért lásd: [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Az automatikus skálázási beállítás részleteinek megtekintése
Használhatja a hello `Get-Autoscalesetting` parancsmag tooretrieve hello automatikus skálázási beállítás további információt.

hello alábbi példa részleteit jeleníti meg minden automatikus skálázási beállítás a hello erőforrás csoport "myrg1".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

hello alábbi példa a hello erőforrás csoport "myrg1" minden automatikus skálázási beállítás részleteit jeleníti meg, és kifejezetten hello "MyScaleVMSSSetting" nevű automatikus skálázási beállítás.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Távolítsa el az automatikus skálázási beállítás
Használhatja a hello `Remove-Autoscalesetting` parancsmag toodelete az automatikus skálázási beállítás.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>A műveletnapló napló profilok kezelése
Létrehozhat egy *profilt naplózni* és az adatok exportálása a tevékenység napló tooa tárfiók, és konfigurálhatja az adatmegőrzés azt. Szükség esetén is képes adatfolyam hello adatok tooyour Eseményközpontot. Vegye figyelembe, hogy ez a funkció jelenleg előzetes állapotban van, akkor csak egy naplófájl profil előfizetésenként hozhat létre. A következő parancsmagot a jelenlegi előfizetés toocreate hello használja, és a napló profilok kezeléséhez. Egy adott előfizetést is beállíthatja. Bár PowerShell alapértelmezés szerint a jelenlegi előfizetés toohello, bármikor módosíthatja, hogy használatával `Set-AzureRmContext`. Tevékenység napló tooroute adatok tooany tárfiók vagy az Eseményközpont kiválasztásával konfigurálhatja, hogy az előfizetésen belül. Adatok blob fájlok JSON formátumban van megírva.

### <a name="get-a-log-profile"></a>A napló profil beolvasása
toofetch a meglévő napló-profilok használatára hello `Get-AzureRmLogProfile` parancsmag.

### <a name="add-a-log-profile-without-data-retention"></a>Az adatok megőrzése nélkül napló profil hozzáadása
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Napló-profil eltávolítása
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Az adatok megőrzése napló profil hozzáadása
Megadhatja a hello **- RetentionInDays** tulajdonság hello napok számát, amelyek, mint egy pozitív egész számot, ahol hello adatok őrződnek meg.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>A megőrzési és EventHub napló profil hozzáadása
Továbbá toorouting az adatok toostorage fiókját, akkor is is adatfolyamként tooan Eseményközpontot. Fontos megjegyezni, hogy az előzetes verzióban kiadás és hello a tárolási konfiguráció használata kötelező az Event Hubs konfigurálása nem kötelező.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Diagnosztikai naplók konfigurálása
Számos Azure-szolgáltatásokat nyújtanak további naplók és a telemetriai adatokból, hogy a konfigurált toosave adatokat az Azure Storage-fiók is lehet küldeni tooEvent hubok, és/vagy tooan OMS Naplóelemzési munkaterület küldött. Ez a művelet csak egy erőforrás szinten hajtható végre, és hello tárolási fiók vagy az event hub megtalálható hello és ugyanabban a régióban hello célerőforrása ahol hello diagnosztika beállítás.

### <a name="get-diagnostic-setting"></a>Diagnosztikai beállításának beolvasása
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnosztikai beállítás letiltása

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Diagnosztikai beállítás nélkül megőrzési engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Diagnosztikai beállításának megőrzést engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Egy adott napló kategória megőrzést diagnosztikai beállítás engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Az Event Hubs diagnosztikai beállításának engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Diagnosztikai beállításának OMS engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
