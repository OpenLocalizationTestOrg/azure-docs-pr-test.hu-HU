---
title: "Figyelő CLI 1.0 aaaAzure gyors üzembe helyezési minta. | Microsoft Docs"
description: "Parancssori felület 1.0 Példaparancsok Azure figyelő szolgáltatások. A figyelő az Azure a Microsoft Azure szolgáltatást, amely lehetővé teszi a toosend riasztási értesítéseket, a hívás webes URL-címek konfigurált telemetriai adatokat, és az automatikus skálázás Cloud Services, a virtuális gépek és a Web Apps értékek alapján."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6cd9cd62b3a1977276563f5e43f5384ccca66247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a>Azure-figyelő platformfüggetlen parancssori felület 1.0-ás gyors üzembe helyezési minták
A parancssori felület (CLI) parancsok toohelp figyelő Azure-szolgáltatások elérésének mintavételhez. a cikk tartalmazza. Az Azure a figyelő lehetővé teszi tooAutoScale Felhőszolgáltatásokat, a virtuális gépek és a Web Apps és a toosend riasztási értesítések és hívás webes URL-címek konfigurált telemetriai adatok értékek alapján.

> [!NOTE]
> Az Azure a figyelő az hello új neve "Azure Insights" nevezett csak 2016. Szeptembertől 25. Azonban hello névterek, ezért az alábbi parancsok hello továbbra is tartalmazhat hello "insights".
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ha még nem telepítette az Azure parancssori felület hello, lásd: [telepítés hello Azure CLI](../cli-install-nodejs.md). Ha ismeri az Azure parancssori felület, további információ a [használata hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](../xplat-cli-azure-resource-manager.md).

A Windows, telepítse a hello npm [Node.js webhely](https://nodejs.org/). Hello telepítés befejezése után Futtatás rendszergazdai jogosultságokkal rendelkező CMD.exe használatával hajtható végre hello következő hello mappájából, amelyen telepítve van-e az npm:

```console
npm install azure-cli --global
```

Ezután keresse meg a tooany/mappába szeretné, és írja be a parancssori hello:

```console
azure help
```

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure
hello első lépéseként toologin tooyour Azure-fiókra.

```console
azure login
```

Ez a parancs futtatása után vannak toosign keresztül üdvözlő képernyőt hello utasításokat. A későbbiekben láthatja a fiókhoz, a TenantId és az alapértelmezett előfizetés-azonosító. Minden parancs az alapértelmezett előfizetés hello környezetben működik.

a jelenlegi előfizetéséhez, a következő parancs használata hello toolist hello részleteit.

```console
azure account show
```

toochange működő környezetben tooa másik előfizetést, a következő parancs használata hello.

```console
azure account set "subscription ID or subscription name"
```

az Azure Resource Manager toouse és Azure figyelő parancsok, toobe az Azure Resource Manager módra van szüksége.

```console
azure config mode arm
```

az összes támogatott Azure-figyelő parancsok listáját tooview hello következőket hajthatja végre.

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a>Az előfizetéshez tevékenység napló megtekintése
tevékenység alkalmazásnapló-események listája tooview hello következőket hajthatja végre.

```console
azure insights logs list [options]
```

Próbálja a következő tooview hello összes rendelkezésre álló lehetőségeket.

```console
azure insights logs list -help
```

Íme egy példa toolist naplók által a resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Példa toolist naplók hívó

```console
azure insights logs list --caller "myname@company.com"
```

Példa toolist naplók hívó egy erőforrás típuson belül a kezdő és záró dátum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>A riasztások kezelése
A riasztások hello szakasz toowork hello információt is használhatja.

### <a name="get-alert-rules-in-a-resource-group"></a>A riasztási szabályok lekérése egy erőforráscsoportban található
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Hozzon létre egy metrika riasztási szabályt
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a>Hozzon létre riasztási szabályt webtesztben.
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Riasztási szabály törlése
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Napló-profilok
Ez a szakasz toowork napló profilokkal hello információkat használja.

### <a name="get-a-log-profile"></a>A napló profil beolvasása
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Megőrzési nélkül napló profil hozzáadása
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Napló-profil eltávolítása
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Az adatmegőrzési naplót profil hozzáadása
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>A megőrzési és EventHub napló profil hozzáadása
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnosztika
Ez a szakasz toowork a diagnosztikai beállítások hello információkat használja.

### <a name="get-a-diagnostic-setting"></a>A diagnosztikai beállításának beolvasása
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>A diagnosztikai beállítás letiltása
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Egy diagnosztikai beállítás nélkül megőrzési engedélyezése
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatikus méretezés
Ez a szakasz toowork automatikus skálázás beállításokkal hello információkat használja. Ezek a példák kell toomodify.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Erőforráscsoport automatikus skálázás beállításainak beolvasása
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Automatikus skálázás beállításainak beolvasása nevű erőforráscsoportban
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Auotoscale beállításainak megadása
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
