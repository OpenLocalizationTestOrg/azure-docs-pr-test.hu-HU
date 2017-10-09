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
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a><span data-ttu-id="8cfa8-105">Azure-figyelő platformfüggetlen parancssori felület 1.0-ás gyors üzembe helyezési minták</span><span class="sxs-lookup"><span data-stu-id="8cfa8-105">Azure Monitor  Cross-platform CLI 1.0 quick start samples</span></span>
<span data-ttu-id="8cfa8-106">A parancssori felület (CLI) parancsok toohelp figyelő Azure-szolgáltatások elérésének mintavételhez. a cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-106">This article shows you sample command-line interface (CLI) commands toohelp you access Azure Monitor features.</span></span> <span data-ttu-id="8cfa8-107">Az Azure a figyelő lehetővé teszi tooAutoScale Felhőszolgáltatásokat, a virtuális gépek és a Web Apps és a toosend riasztási értesítések és hívás webes URL-címek konfigurált telemetriai adatok értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-107">Azure Monitor allows you tooAutoScale Cloud Services, Virtual Machines, and Web Apps and toosend alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="8cfa8-108">Az Azure a figyelő az hello új neve "Azure Insights" nevezett csak 2016. Szeptembertől 25.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-108">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="8cfa8-109">Azonban hello névterek, ezért az alábbi parancsok hello továbbra is tartalmazhat hello "insights".</span><span class="sxs-lookup"><span data-stu-id="8cfa8-109">However, hello namespaces and thus hello commands below still contain hello "insights".</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="8cfa8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8cfa8-110">Prerequisites</span></span>
<span data-ttu-id="8cfa8-111">Ha még nem telepítette az Azure parancssori felület hello, lásd: [telepítés hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8cfa8-111">If you haven't already installed hello Azure CLI, see [Install hello Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="8cfa8-112">Ha ismeri az Azure parancssori felület, további információ a [használata hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8cfa8-112">If you're unfamiliar with Azure CLI, you can read more about it at [Use hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../xplat-cli-azure-resource-manager.md).</span></span>

<span data-ttu-id="8cfa8-113">A Windows, telepítse a hello npm [Node.js webhely](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="8cfa8-113">In Windows, install npm from hello [Node.js website](https://nodejs.org/).</span></span> <span data-ttu-id="8cfa8-114">Hello telepítés befejezése után Futtatás rendszergazdai jogosultságokkal rendelkező CMD.exe használatával hajtható végre hello következő hello mappájából, amelyen telepítve van-e az npm:</span><span class="sxs-lookup"><span data-stu-id="8cfa8-114">After you complete hello installation, using CMD.exe with Run As Administrator privileges, execute hello following from hello folder where npm is installed:</span></span>

```console
npm install azure-cli --global
```

<span data-ttu-id="8cfa8-115">Ezután keresse meg a tooany/mappába szeretné, és írja be a parancssori hello:</span><span class="sxs-lookup"><span data-stu-id="8cfa8-115">Next, navigate tooany folder/location you want and type at hello command-line:</span></span>

```console
azure help
```

## <a name="log-in-tooazure"></a><span data-ttu-id="8cfa8-116">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="8cfa8-116">Log in tooAzure</span></span>
<span data-ttu-id="8cfa8-117">hello első lépéseként toologin tooyour Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-117">hello first step is toologin tooyour Azure account.</span></span>

```console
azure login
```

<span data-ttu-id="8cfa8-118">Ez a parancs futtatása után vannak toosign keresztül üdvözlő képernyőt hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-118">After running this command, you have toosign in via hello instructions on hello screen.</span></span> <span data-ttu-id="8cfa8-119">A későbbiekben láthatja a fiókhoz, a TenantId és az alapértelmezett előfizetés-azonosító. Minden parancs az alapértelmezett előfizetés hello környezetben működik.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-119">Afterward, you see your Account, TenantId, and default Subscription Id. All commands work in hello context of your default subscription.</span></span>

<span data-ttu-id="8cfa8-120">a jelenlegi előfizetéséhez, a következő parancs használata hello toolist hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-120">toolist hello details of your current subscription, use hello following command.</span></span>

```console
azure account show
```

<span data-ttu-id="8cfa8-121">toochange működő környezetben tooa másik előfizetést, a következő parancs használata hello.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-121">toochange working context tooa different subscription, use hello following command.</span></span>

```console
azure account set "subscription ID or subscription name"
```

<span data-ttu-id="8cfa8-122">az Azure Resource Manager toouse és Azure figyelő parancsok, toobe az Azure Resource Manager módra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-122">toouse Azure Resource Manager and Azure Monitor commands, you need toobe in Azure Resource Manager mode.</span></span>

```console
azure config mode arm
```

<span data-ttu-id="8cfa8-123">az összes támogatott Azure-figyelő parancsok listáját tooview hello következőket hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-123">tooview a list of all supported Azure Monitor commands, perform hello following.</span></span>

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a><span data-ttu-id="8cfa8-124">Az előfizetéshez tevékenység napló megtekintése</span><span class="sxs-lookup"><span data-stu-id="8cfa8-124">View activity log for a subscription</span></span>
<span data-ttu-id="8cfa8-125">tevékenység alkalmazásnapló-események listája tooview hello következőket hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-125">tooview a list of activity log events, perform hello following.</span></span>

```console
azure insights logs list [options]
```

<span data-ttu-id="8cfa8-126">Próbálja a következő tooview hello összes rendelkezésre álló lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-126">Try hello following tooview all available options.</span></span>

```console
azure insights logs list -help
```

<span data-ttu-id="8cfa8-127">Íme egy példa toolist naplók által a resourceGroup</span><span class="sxs-lookup"><span data-stu-id="8cfa8-127">Here is an example toolist logs by a resourceGroup</span></span>

```console
azure insights logs list --resourceGroup "myrg1"
```

<span data-ttu-id="8cfa8-128">Példa toolist naplók hívó</span><span class="sxs-lookup"><span data-stu-id="8cfa8-128">Example toolist logs by caller</span></span>

```console
azure insights logs list --caller "myname@company.com"
```

<span data-ttu-id="8cfa8-129">Példa toolist naplók hívó egy erőforrás típuson belül a kezdő és záró dátum</span><span class="sxs-lookup"><span data-stu-id="8cfa8-129">Example toolist logs by caller on a resource type, within a start and end date</span></span>

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a><span data-ttu-id="8cfa8-130">A riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="8cfa8-130">Work with alerts</span></span>
<span data-ttu-id="8cfa8-131">A riasztások hello szakasz toowork hello információt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-131">You can use hello information in hello section toowork with alerts.</span></span>

### <a name="get-alert-rules-in-a-resource-group"></a><span data-ttu-id="8cfa8-132">A riasztási szabályok lekérése egy erőforráscsoportban található</span><span class="sxs-lookup"><span data-stu-id="8cfa8-132">Get alert rules in a resource group</span></span>
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a><span data-ttu-id="8cfa8-133">Hozzon létre egy metrika riasztási szabályt</span><span class="sxs-lookup"><span data-stu-id="8cfa8-133">Create a metric alert rule</span></span>
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a><span data-ttu-id="8cfa8-134">Hozzon létre riasztási szabályt webtesztben.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-134">Create webtest alert rule</span></span>
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a><span data-ttu-id="8cfa8-135">Riasztási szabály törlése</span><span class="sxs-lookup"><span data-stu-id="8cfa8-135">Delete an alert rule</span></span>
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a><span data-ttu-id="8cfa8-136">Napló-profilok</span><span class="sxs-lookup"><span data-stu-id="8cfa8-136">Log profiles</span></span>
<span data-ttu-id="8cfa8-137">Ez a szakasz toowork napló profilokkal hello információkat használja.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-137">Use hello information in this section toowork with log profiles.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="8cfa8-138">A napló profil beolvasása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-138">Get a log profile</span></span>
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a><span data-ttu-id="8cfa8-139">Megőrzési nélkül napló profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-139">Add a log profile without retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="8cfa8-140">Napló-profil eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-140">Remove a log profile</span></span>
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a><span data-ttu-id="8cfa8-141">Az adatmegőrzési naplót profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-141">Add a log profile with retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="8cfa8-142">A megőrzési és EventHub napló profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-142">Add a log profile with retention and EventHub</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a><span data-ttu-id="8cfa8-143">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="8cfa8-143">Diagnostics</span></span>
<span data-ttu-id="8cfa8-144">Ez a szakasz toowork a diagnosztikai beállítások hello információkat használja.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-144">Use hello information in this section toowork with diagnostic settings.</span></span>

### <a name="get-a-diagnostic-setting"></a><span data-ttu-id="8cfa8-145">A diagnosztikai beállításának beolvasása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-145">Get a diagnostic setting</span></span>
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a><span data-ttu-id="8cfa8-146">A diagnosztikai beállítás letiltása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-146">Disable a diagnostic setting</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a><span data-ttu-id="8cfa8-147">Egy diagnosztikai beállítás nélkül megőrzési engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8cfa8-147">Enable a diagnostic setting without retention</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a><span data-ttu-id="8cfa8-148">Automatikus méretezés</span><span class="sxs-lookup"><span data-stu-id="8cfa8-148">Autoscale</span></span>
<span data-ttu-id="8cfa8-149">Ez a szakasz toowork automatikus skálázás beállításokkal hello információkat használja.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-149">Use hello information in this section toowork with autoscale settings.</span></span> <span data-ttu-id="8cfa8-150">Ezek a példák kell toomodify.</span><span class="sxs-lookup"><span data-stu-id="8cfa8-150">You need toomodify these examples.</span></span>

### <a name="get-autoscale-settings-for-a-resource-group"></a><span data-ttu-id="8cfa8-151">Erőforráscsoport automatikus skálázás beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-151">Get autoscale settings for a resource group</span></span>
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a><span data-ttu-id="8cfa8-152">Automatikus skálázás beállításainak beolvasása nevű erőforráscsoportban</span><span class="sxs-lookup"><span data-stu-id="8cfa8-152">Get autoscale settings by name in a resource group</span></span>
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a><span data-ttu-id="8cfa8-153">Auotoscale beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="8cfa8-153">Set auotoscale settings</span></span>
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
