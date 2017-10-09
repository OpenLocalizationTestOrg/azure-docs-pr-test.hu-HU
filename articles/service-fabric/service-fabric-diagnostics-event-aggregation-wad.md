---
title: "Service Fabric esemény összesítési a Windows Azure diagnosztikai aaaAzure |} Microsoft Docs"
description: "További tudnivalók összesítésére és események gyűjtése ÜVEGVATTA figyelési és diagnosztika Azure Service Fabric-fürt segítségével."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 4827ce66620e61c5b4a8682db55952333113188a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a><span data-ttu-id="9a0d0-103">Esemény összesítésére és az adatgyűjtést, a Windows Azure diagnosztikai</span><span class="sxs-lookup"><span data-stu-id="9a0d0-103">Event aggregation and collection using Windows Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9a0d0-104">Windows</span><span class="sxs-lookup"><span data-stu-id="9a0d0-104">Windows</span></span>](service-fabric-diagnostics-event-aggregation-wad.md)
> * [<span data-ttu-id="9a0d0-105">Linux</span><span class="sxs-lookup"><span data-stu-id="9a0d0-105">Linux</span></span>](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

<span data-ttu-id="9a0d0-106">Az Azure Service Fabric-fürtöt használ, esetén a jó ötlet toocollect hello jelentkezik minden hello csomópontról egy központi helyen.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-106">When you're running an Azure Service Fabric cluster, it's a good idea toocollect hello logs from all hello nodes in a central location.</span></span> <span data-ttu-id="9a0d0-107">Hello naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy az adott fürtben található futó hello alkalmazások és szolgáltatások a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-107">Having hello logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in hello applications and services running in that cluster.</span></span>

<span data-ttu-id="9a0d0-108">Egyirányú tooupload és a gyűjtés naplók toouse hello Windows Azure diagnosztikai (ÜVEGVATTA) bővítmény a naplók tooAzure tárolási feltölti, és hello beállítás toosend naplók tooAzure Application Insights vagy az Event Hubs is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-108">One way tooupload and collect logs is toouse hello Windows Azure Diagnostics (WAD) extension, which uploads logs tooAzure Storage, and also has hello option toosend logs tooAzure Application Insights or Event Hubs.</span></span> <span data-ttu-id="9a0d0-109">Is használja egy külső folyamat tooread hello eseményeket tárolóból, és helyezze el őket egy analysis platform termék, például a [OMS Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-109">You can also use an external process tooread hello events from storage and place them in an analysis platform product, such as [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a0d0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9a0d0-110">Prerequisites</span></span>
<span data-ttu-id="9a0d0-111">Ezek az eszközök a használt tooperform hello műveleteket ebben a dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="9a0d0-111">These tools are used tooperform some of hello operations in this document:</span></span>

* <span data-ttu-id="9a0d0-112">[Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (tooAzure Felhőszolgáltatások azonban rendelkezik jó útmutatást és példákat kapcsolatos)</span><span class="sxs-lookup"><span data-stu-id="9a0d0-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related tooAzure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="9a0d0-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9a0d0-113">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="9a0d0-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9a0d0-114">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="9a0d0-115">Az Azure Resource Manager-ügyfél</span><span class="sxs-lookup"><span data-stu-id="9a0d0-115">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="9a0d0-116">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9a0d0-116">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a><span data-ttu-id="9a0d0-117">Naplófájl és esemény források</span><span class="sxs-lookup"><span data-stu-id="9a0d0-117">Log and event sources</span></span>

### <a name="service-fabric-platform-events"></a><span data-ttu-id="9a0d0-118">A Service Fabric platform események</span><span class="sxs-lookup"><span data-stu-id="9a0d0-118">Service Fabric platform events</span></span>
<span data-ttu-id="9a0d0-119">A bemutatott [Ez a cikk](service-fabric-diagnostics-event-generation-infra.md), a Service Fabric beállítja, néhány out-of-az-box naplózási csatornák, amely hello a következő csatornák könnyen konfigurált ÜVEGVATTA toosend figyelése és diagnosztikai adatok tooa tárolási tábla vagy máshol:</span><span class="sxs-lookup"><span data-stu-id="9a0d0-119">As discussed in [this article](service-fabric-diagnostics-event-generation-infra.md), Service Fabric sets you up with a few out-of-the-box logging channels, of which hello following channels are easily configured with WAD toosend monitoring and diagnostics data tooa storage table or elsewhere:</span></span>
  * <span data-ttu-id="9a0d0-120">A működési események: hello platform hajtja végre a Service Fabric-magasabb szintű műveletek.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-120">Operational events: higher-level operations that hello Service Fabric platform performs.</span></span> <span data-ttu-id="9a0d0-121">Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-121">Examples include creation of applications and services, node state changes, and upgrade information.</span></span> <span data-ttu-id="9a0d0-122">Ezek kibocsátott mint esemény Windows (nyomkövetés) naplók</span><span class="sxs-lookup"><span data-stu-id="9a0d0-122">These are emitted as Event Tracing for Windows (ETW) logs</span></span>
  * [<span data-ttu-id="9a0d0-123">Reliable Actors programozási modell események</span><span class="sxs-lookup"><span data-stu-id="9a0d0-123">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="9a0d0-124">Megbízható Services-programozási modell események</span><span class="sxs-lookup"><span data-stu-id="9a0d0-124">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a><span data-ttu-id="9a0d0-125">Alkalmazás-események</span><span class="sxs-lookup"><span data-stu-id="9a0d0-125">Application events</span></span>
 <span data-ttu-id="9a0d0-126">Hello Visual Studio sablonok találhatók az alkalmazások és szolgáltatások kódból kibocsátott, és a hello EventSource segítőosztály használatával írt események.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-126">Events emitted from your applications' and services' code and written out by using hello EventSource helper class provided in hello Visual Studio templates.</span></span> <span data-ttu-id="9a0d0-127">Hogyan naplózza az toowrite EventSource az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-127">For more information on how toowrite EventSource logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-hello-diagnostics-extension"></a><span data-ttu-id="9a0d0-128">Hello diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-128">Deploy hello Diagnostics extension</span></span>
<span data-ttu-id="9a0d0-129">hello első lépése a naplók gyűjtésére toodeploy hello diagnosztika bővítmény minden egyes hello Service Fabric-fürt hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-129">hello first step in collecting logs is toodeploy hello Diagnostics extension on each of hello VMs in hello Service Fabric cluster.</span></span> <span data-ttu-id="9a0d0-130">hello diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti megadott toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-130">hello Diagnostics extension collects logs on each VM and uploads them toohello storage account that you specify.</span></span> <span data-ttu-id="9a0d0-131">hogy használ-e hello Azure-portálon vagy az Azure Resource Manager kissé függően változhat a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-131">hello steps vary a little based on whether you use hello Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="9a0d0-132">hello lépéseket is e hello telepítés része a fürt létrehozása, vagy egy már létező fürt függően változhat.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-132">hello steps also vary based on whether hello deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="9a0d0-133">Nézzük hello lépéseket az egyes forgatókönyvek esetében.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-133">Let's look at hello steps for each scenario.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a><span data-ttu-id="9a0d0-134">A fürt létrehozása az Azure portálon keresztül részeként hello diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-134">Deploy hello Diagnostics extension as part of cluster creation through Azure portal</span></span>
<span data-ttu-id="9a0d0-135">toodeploy hello diagnosztika bővítmény toohello virtuális gépek hello fürt fürt létrehozásának részeként, használhat hello diagnosztikai beállítások panel hello következő látható kép – győződjön meg arról, hogy túl van-e állítva a diagnosztika**a** (hello az alapértelmezett beállítás) .</span><span class="sxs-lookup"><span data-stu-id="9a0d0-135">toodeploy hello Diagnostics extension toohello VMs in hello cluster as part of cluster creation, you use hello Diagnostics settings panel shown in hello following image - ensure that Diagnostics is set too**On** (hello default setting).</span></span> <span data-ttu-id="9a0d0-136">Hello fürt létrehozása után ezeket a beállításokat nem módosíthatja hello portál használatával.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-136">After you create hello cluster, you can't change these settings by using hello portal.</span></span>

![A fürt létrehozásához hello portálon az Azure diagnosztikai beállításai](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

<span data-ttu-id="9a0d0-138">A fürt létrehozásakor hello portál használatával erősen ajánlott, hogy töltse le a hello sablon **OK gombra kattintás előtt** toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-138">When you're creating a cluster by using hello portal, we highly recommend that you download hello template **before you click OK** toocreate hello cluster.</span></span> <span data-ttu-id="9a0d0-139">Részletekért lásd a túl[Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-139">For details, refer too[Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="9a0d0-140">Szüksége lesz hello sablon toomake módosítások később, mert néhány módosítást hello portál használatával nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-140">You'll need hello template toomake changes later, because you can't make some changes by using hello portal.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="9a0d0-141">Fürt létrehozásának részeként hello diagnosztika bővítmény telepítése Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="9a0d0-141">Deploy hello Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="9a0d0-142">egy fürt erőforrás-kezelő használatával toocreate, kell tooadd hello diagnosztika konfigurációs JSON toohello teljes fürt Resource Manager sablon hello fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-142">toocreate a cluster by using Resource Manager, you need tooadd hello Diagnostics configuration JSON toohello full cluster Resource Manager template before you create hello cluster.</span></span> <span data-ttu-id="9a0d0-143">Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk hozzá diagnosztikai konfiguráció tooit a Resource Manager sablon minták részeként.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-143">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added tooit as part of our Resource Manager template samples.</span></span> <span data-ttu-id="9a0d0-144">Ezen a helyen hello Azure-minták katalógusban látható: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-144">You can see it at this location in hello Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span></span>

<span data-ttu-id="9a0d0-145">toosee hello diagnosztika beállítás hello Resource Manager-sablon, nyitott hello azuredeploy.json fájlt, és keresse meg a **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-145">toosee hello Diagnostics setting in hello Resource Manager template, open hello azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="9a0d0-146">a fürt ezt a sablont, jelölje be hello segítségével toocreate **tooAzure telepítése** gomb hello előző hivatkozás érhető el.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-146">toocreate a cluster by using this template, select hello **Deploy tooAzure** button available at hello previous link.</span></span>

<span data-ttu-id="9a0d0-147">Azt is megteheti, hello erőforrás-kezelő minta letöltése, ellenőrizze a módosítások tooit és hello segítségével hozzon létre egy fürtöt hello módosított sablon `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-147">Alternatively, you can download hello Resource Manager sample, make changes tooit, and create a cluster with hello modified template by using hello `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="9a0d0-148">Tekintse meg a következő kód hello paraméter jelzi, hogy átadta toohello parancsban hello.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-148">See hello following code for hello parameters that you pass in toohello command.</span></span> <span data-ttu-id="9a0d0-149">Hogyan toodeploy erőforrás szerint kell csoportosítani a PowerShell használatával további információkért lásd: hello cikk [hello Azure Resource Manager sablonnal erőforráscsoport telepítése](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-149">For detailed information on how toodeploy a resource group by using PowerShell, see hello article [Deploy a resource group with hello Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a><span data-ttu-id="9a0d0-150">Hello diagnosztika bővítmény tooan meglévő fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-150">Deploy hello Diagnostics extension tooan existing cluster</span></span>
<span data-ttu-id="9a0d0-151">Ha egy meglévő fürtöt, amely nem rendelkezik telepített diagnosztika, vagy ha azt szeretné, hogy egy meglévő konfigurációt toomodify, adja hozzá, vagy frissítheti azt.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-151">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want toomodify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="9a0d0-152">Hello Resource Manager-sablon által használt toocreate hello meglévő fürt módosítása vagy hello sablon letöltése hello portálról korábban leírt módon.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-152">Modify hello Resource Manager template that's used toocreate hello existing cluster or download hello template from hello portal as described earlier.</span></span> <span data-ttu-id="9a0d0-153">Módosítsa a hello template.json fájlt hello a következő feladatok végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-153">Modify hello template.json file by performing hello following tasks.</span></span>

<span data-ttu-id="9a0d0-154">Vegyen fel egy új tároló-erőforrás toohello sablont toohello források szakaszában hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-154">Add a new storage resource toohello template by adding toohello resources section.</span></span>

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 <span data-ttu-id="9a0d0-155">Ezután adja hozzá a toohello paraméterek között szakasz után hello tárolási fiók definíciók `supportLogStorageAccountName` és `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-155">Next, add toohello parameters section just after hello storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="9a0d0-156">Cserélje le a helyőrzőket hello *ide kerül a tárfiók neve* hello nevű hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-156">Replace hello placeholder text *storage account name goes here* with hello name of hello storage account.</span></span>

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for hello application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for hello storage account that contains application diagnostics data from hello cluster"
      }
    },
```
<span data-ttu-id="9a0d0-157">Frissítse a hello `VirtualMachineProfile` hello template.json fájl a következő kód hello bővítmények tömbön belüli hello hozzáadásával szakaszában.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-157">Then, update hello `VirtualMachineProfile` section of hello template.json file by adding hello following code within hello extensions array.</span></span> <span data-ttu-id="9a0d0-158">Lehet, hogy tooadd hello elején vagy hello végén, attól függően, hogy hol csatlakoztatva van egy vesszővel.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-158">Be sure tooadd a comma at hello beginning or hello end, depending on where it's inserted.</span></span>

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

<span data-ttu-id="9a0d0-159">Hello template.json fájl leírtak módosítása után közzé hello Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-159">After you modify hello template.json file as described, republish hello Resource Manager template.</span></span> <span data-ttu-id="9a0d0-160">Hello sablon exportált, ha a fájl futtatása hello deploy.ps1 addig hello sablont.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-160">If hello template was exported, running hello deploy.ps1 file republishes hello template.</span></span> <span data-ttu-id="9a0d0-161">Miután telepít, győződjön meg arról, hogy **ProvisioningState** van **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-161">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="collect-health-and-load-events"></a><span data-ttu-id="9a0d0-162">Állapot gyűjtése és események betöltése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-162">Collect health and load events</span></span>

<span data-ttu-id="9a0d0-163">Hello 5.4-es verziójában a Service Fabric-től kezdődően állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-163">Starting with hello 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="9a0d0-164">Ezek az események tükrözze hello állapotfigyelő segítségével hello rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-164">These events reflect events generated by hello system or your code by using hello health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="9a0d0-165">Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-165">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="9a0d0-166">Ezek az események a Visual Studio diagnosztikai eseménynapló hozzáadása tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello ETW-hitelesítésszolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-166">tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" toohello list of ETW providers.</span></span>

<span data-ttu-id="9a0d0-167">toocollect hello események, hello Resource Manager sablon tooinclude módosítása</span><span class="sxs-lookup"><span data-stu-id="9a0d0-167">toocollect hello events, modify hello Resource Manager template tooinclude</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-reverse-proxy-events"></a><span data-ttu-id="9a0d0-168">Fordított proxy eseményeinek gyűjtése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-168">Collect reverse proxy events</span></span>

<span data-ttu-id="9a0d0-169">A Service Fabric hello 5.7 kiadástól kezdve [fordított proxy](service-fabric-reverseproxy.md) események állnak rendelkezésre a következő gyűjtemény számára.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-169">Starting with hello 5.7 release of Service Fabric, [reverse proxy](service-fabric-reverseproxy.md) events are available for collection.</span></span>
<span data-ttu-id="9a0d0-170">Fordított proxy az események két csatornákon, egy tartalmazó hiba bocsát ki tükröző események kérelmek feldolgozási hibák, és egy másikra hello fordított proxy feldolgozás összes hello kérelmek kapcsolatos részletes eseményeket tartalmazó hello.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-170">Reverse proxy emits events into two channels, one containing error events reflecting request processing failures and hello other one containing verbose events about all hello requests processed at hello reverse proxy.</span></span> 

1. <span data-ttu-id="9a0d0-171">Hiba eseményeinek gyűjtése: ezek az események a Visual Studio diagnosztikai eseménynapló hozzáadása tooview "Microsoft-ServiceFabric:4:0x4000000000000010" toohello ETW-hitelesítésszolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-171">Collect error events: tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000010" toohello list of ETW providers.</span></span>
<span data-ttu-id="9a0d0-172">toocollect hello események Azure fürtök hello Resource Manager sablon tooinclude módosítása</span><span class="sxs-lookup"><span data-stu-id="9a0d0-172">toocollect hello events from Azure clusters, modify hello Resource Manager template tooinclude</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. <span data-ttu-id="9a0d0-173">Összegyűjteni az összes kérelem események feldolgozását: A Visual Studio diagnosztikai eseménynapló, frissítés a Microsoft-ServiceFabric hello bejegyzés túl hello ETW-szolgáltató listában "Microsoft-ServiceFabric:4:0x4000000000000020".</span><span class="sxs-lookup"><span data-stu-id="9a0d0-173">Collect all request processing events: In Visual Studio's Diagnostic Event Viewer, update hello Microsoft-ServiceFabric entry in hello ETW provider list too"Microsoft-ServiceFabric:4:0x4000000000000020".</span></span>
<span data-ttu-id="9a0d0-174">Az Azure Service Fabric-fürtök esetén hello resource manager sablon tooinclude módosítása</span><span class="sxs-lookup"><span data-stu-id="9a0d0-174">For Azure Service Fabric clusters, modify hello resource manager template tooinclude</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> <span data-ttu-id="9a0d0-175">Ez hello fordított proxyn keresztül történő összes forgalom gyűjt, és gyorsan felhasználhat a tárolási kapacitás ajánlott toojudiciously engedélyezése események gyűjtését a csatornán.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-175">It is recommended toojudiciously enable collecting events from this channel as this collects all traffic through hello reverse proxy and can quickly consume storage capacity.</span></span>

<span data-ttu-id="9a0d0-176">Az Azure Service Fabric-fürtök esetén minden hello csomópontról hello események gyűjtése és hello SystemEventTable összesíteni.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-176">For Azure Service Fabric clusters, hello events from all hello nodes are collected and aggregated in hello SystemEventTable.</span></span>
<span data-ttu-id="9a0d0-177">Részletes hello fordított proxy események, tekintse meg a hello [fordított proxy diagnosztika útmutató](service-fabric-reverse-proxy-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-177">For detailed troubleshooting of hello reverse proxy events, refer hello [reverse proxy diagnostics guide](service-fabric-reverse-proxy-diagnostics.md).</span></span>

## <a name="collect-from-new-eventsource-channels"></a><span data-ttu-id="9a0d0-178">Új EventSource csatornák gyűjtése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-178">Collect from new EventSource channels</span></span>

<span data-ttu-id="9a0d0-179">tooupdate diagnosztika toocollect naplókat az új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, amely körülbelül toodeploy, végezze el a hello korábban leírt lépéseket egy meglévő fürt diagnosztika hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-179">tooupdate Diagnostics toocollect logs from new EventSource channels that represent a new application that you're about toodeploy, perform hello same steps as previously described for hello setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="9a0d0-180">Hello frissítése `EtwEventSourceProviderConfiguration` hello új EventSource csatornák hello konfiguráció alkalmazása előtt módosítsa hello segítségével hello template.json tooadd bejegyzések szakasz `New-AzureRmResourceGroupDeployment` PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-180">Update hello `EtwEventSourceProviderConfiguration` section in hello template.json file tooadd entries for hello new EventSource channels before you apply hello configuration update by using hello `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="9a0d0-181">hello eseményforrás hello nevét a kód hello ServiceEventSource.cs Visual Studio által létrehozott fájl részeként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-181">hello name of hello event source is defined as part of your code in hello Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="9a0d0-182">Ha a forrás saját Eventsource neve, például saját Eventsource kód tooplace hello események követően egy táblába MyDestinationTableName nevű hello hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-182">For example, if your event source is named My-Eventsource, add hello following code tooplace hello events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="9a0d0-183">toocollect teljesítményszámlálók vagy eseménynaplók, hello Resource Manager sablon módosításához megadott hello példák felhasználásával [Windows virtuális gép létrehozása a figyelési és diagnosztikaAzureResourceManager-sablonhasználatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-183">toocollect performance counters or event logs, modify hello Resource Manager template by using hello examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="9a0d0-184">Újbóli hello Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-184">Then, republish hello Resource Manager template.</span></span>

## <a name="collect-performance-counters"></a><span data-ttu-id="9a0d0-185">A teljesítményszámlálók adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="9a0d0-185">Collect Performance Counters</span></span>

<span data-ttu-id="9a0d0-186">a fürtről toocollect teljesítménymutatók hello teljesítmény számlálók tooyour "WadCfg > DiagnosticMonitorConfiguration" hello Resource Manager-sablon a fürt adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-186">toocollect performance metrics from your cluster, add hello performance counters tooyour "WadCfg > DiagnosticMonitorConfiguration" in hello Resource Manager template for your cluster.</span></span> <span data-ttu-id="9a0d0-187">Lásd: [Service Fabric teljesítményszámlálók](service-fabric-diagnostics-event-generation-perf.md) teljesítményszámlálóinak, javasoljuk a összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-187">See [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) for performance counters that we recommend collecting.</span></span>

<span data-ttu-id="9a0d0-188">Például itt hivatott egy teljesítményszámláló mintát 15 másodpercenként (ezt módosíthatja, és a következőképpen hello formátuma "PT\<idő >\<egység >", például a PT3M három perces időközönként mintát lenne), és toohello át tárolási tábla egy percenként.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-188">For example, here we set one performance counter, sampled every 15 seconds (this can be changed and follows hello format of "PT\<time>\<unit>", for example, PT3M would sample at three minute intervals), and transferred toohello appropriate storage table every one minute.</span></span>

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
<span data-ttu-id="9a0d0-189">Ha Ön az Application Insights fogadó hello szakaszban leírtak szerint használja, és ezek metrikák tooshow fel az Application Insightsban, végezze el, hogy tooadd hello fogadó neve hello "mosdók" szakaszban, ahogy fent látható.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-189">If you are using an Application Insights sink, as described in hello section below, and want these metrics tooshow up in Application Insights, then make sure tooadd hello sink name in hello "sinks" section as shown above.</span></span> <span data-ttu-id="9a0d0-190">Emellett érdemes lehet létrehozni egy külön táblázatban toosend a teljesítményszámlálókat, így azok utasok nagyobb nem csoportjának kimenő hello adatforrásból származó adatok hello más engedélyezte a naplózási csatornák.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-190">Additionally, consider creating a separate table toosend your Performance Counters to, so they don't crowd out hello data coming from hello other logging channels you have enabled.</span></span>


## <a name="send-logs-tooapplication-insights"></a><span data-ttu-id="9a0d0-191">Küldési tooApplication Insights naplói</span><span class="sxs-lookup"><span data-stu-id="9a0d0-191">Send logs tooApplication Insights</span></span>

<span data-ttu-id="9a0d0-192">Figyelés és diagnosztikai adatok tooApplication Insights (AI) küldése hello ÜVEGVATTA konfiguráció részeként végezhető.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-192">Sending monitoring and diagnostics data tooApplication Insights (AI) can be done as part of hello WAD configuration.</span></span> <span data-ttu-id="9a0d0-193">Ha úgy dönt, hogy az esemény elemzése és a képi megjelenítés AI toouse, olvassa el a [esemény elemzése és az Application insights szolgáltatással a képi megjelenítés](service-fabric-diagnostics-event-analysis-appinsights.md) tooset fel egy AI fogadó a "WadCfg" részeként.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-193">If you decide toouse AI for event analysis and visualization, read [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset up an AI Sink as part of your "WadCfg".</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a0d0-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a0d0-194">Next steps</span></span>

<span data-ttu-id="9a0d0-195">Miután konfigurálta az Azure diagnostics megfelelően, adatait jeleníti meg a tárolási táblákba hello ETW és EventSource naplókat.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-195">Once you have correctly configured Azure diagnostics, you will see data in your Storage tables from hello ETW and EventSource logs.</span></span> <span data-ttu-id="9a0d0-196">Ha toouse OMS, Kibana, vagy bármely más elemzés és a képi megjelenítés adatplatform, amely közvetlenül nincs beállítva a Resource Manager-sablon hello, győződjön meg arról, hogy tooset be a választott tooread hello platformja a következő tárolási táblákból hello adatok.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-196">If you choose toouse OMS, Kibana, or any other data analytics and visualization platform that is not directly configured in hello Resource Manager template, make sure tooset up hello platform of your choice tooread in hello data from these storage tables.</span></span> <span data-ttu-id="9a0d0-197">Az OMS nagyon viszonylag egyszerű, és az ismertetése [esemény és a naplófájlok elemzése OMS keresztül](service-fabric-diagnostics-event-analysis-oms.md).</span><span class="sxs-lookup"><span data-stu-id="9a0d0-197">Doing this for OMS is relatively trivial, and is explained in [Event and log analysis through OMS](service-fabric-diagnostics-event-analysis-oms.md).</span></span> <span data-ttu-id="9a0d0-198">Az Application Insights egy bit egy különleges esetben abban az értelemben, hello diagnosztika bővítmény konfigurációjának részeként konfigurálhatók, mivel így ügyet toohello [megfelelő cikk](service-fabric-diagnostics-event-analysis-appinsights.md) Ha úgy dönt, hogy toouse AI.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-198">Application Insights is a bit of a special case in this sense, since it can be configured as part of hello Diagnostics extension configuration, so refer toohello [appropriate article](service-fabric-diagnostics-event-analysis-appinsights.md) if you choose toouse AI.</span></span>

>[!NOTE]
><span data-ttu-id="9a0d0-199">Jelenleg nincs módja toofilter vagy karcsúsítása hello küldött eseményeket toohello tábla.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-199">There is currently no way toofilter or groom hello events that are sent toohello table.</span></span> <span data-ttu-id="9a0d0-200">Ha nem valósítja meg a folyamat tooremove események hello táblából, hello tábla toogrow továbbra is.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-200">If you don't implement a process tooremove events from hello table, hello table will continue toogrow.</span></span> <span data-ttu-id="9a0d0-201">Jelenleg egy futó hello karcsúsítási szolgáltatás például [figyelő minta](https://github.com/Azure-Samples/service-fabric-watchdog-service), javasoljuk, hogy lehet írni egy a saját kezűleg is, kivéve ha egy jó 30 vagy 90 nap időkereteket toostore naplókat, és.</span><span class="sxs-lookup"><span data-stu-id="9a0d0-201">Currently, there is an example of a data grooming service running in hello [Watchdog sample](https://github.com/Azure-Samples/service-fabric-watchdog-service), and it is recommended that you write one for yourself as well, unless there is a good reason for you toostore logs beyond a 30 or 90 day timeframe.</span></span>

* [<span data-ttu-id="9a0d0-202">Ismerje meg, hogyan toocollect teljesítményszámlálókkal és a naplók segítségével hello diagnosztika bővítmény</span><span class="sxs-lookup"><span data-stu-id="9a0d0-202">Learn how toocollect performance counters or logs by using hello Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="9a0d0-203">Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="9a0d0-203">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="9a0d0-204">Esemény elemzése és az OMS képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="9a0d0-204">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)