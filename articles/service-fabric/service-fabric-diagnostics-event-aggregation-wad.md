---
title: "Az Azure Service Fabric esemény összesítési a Windows Azure diagnosztikai |} Microsoft Docs"
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
ms.openlocfilehash: 5773361fdec4cb8ee54fa2856f6aa969d5dac4e9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a><span data-ttu-id="acc45-103">Esemény összesítésére és az adatgyűjtést, a Windows Azure diagnosztikai</span><span class="sxs-lookup"><span data-stu-id="acc45-103">Event aggregation and collection using Windows Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="acc45-104">Windows</span><span class="sxs-lookup"><span data-stu-id="acc45-104">Windows</span></span>](service-fabric-diagnostics-event-aggregation-wad.md)
> * [<span data-ttu-id="acc45-105">Linux</span><span class="sxs-lookup"><span data-stu-id="acc45-105">Linux</span></span>](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

<span data-ttu-id="acc45-106">Amikor egy Azure Service Fabric-fürtöt használ, célszerű gyűjteni a egy központi helyen található minden csomópontot.</span><span class="sxs-lookup"><span data-stu-id="acc45-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="acc45-107">A naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy a futó alkalmazások és szolgáltatások az adott fürtben található a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="acc45-107">Having the logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in the applications and services running in that cluster.</span></span>

<span data-ttu-id="acc45-108">Egy feltöltése és naplógyűjtéshez módja a Windows Azure diagnosztikai (ÜVEGVATTA) bővítménnyel Azure Storage naplók feltöltését, és naplókat küld Azure Application Insights vagy az Event Hubs lehetősége is van.</span><span class="sxs-lookup"><span data-stu-id="acc45-108">One way to upload and collect logs is to use the Windows Azure Diagnostics (WAD) extension, which uploads logs to Azure Storage, and also has the option to send logs to Azure Application Insights or Event Hubs.</span></span> <span data-ttu-id="acc45-109">Kiolvassa az eseményeket a tárolóból, és helyezze el őket egy analysis platform termék, például a külső folyamat is használhatja [OMS Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás.</span><span class="sxs-lookup"><span data-stu-id="acc45-109">You can also use an external process to read the events from storage and place them in an analysis platform product, such as [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acc45-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="acc45-110">Prerequisites</span></span>
<span data-ttu-id="acc45-111">Ezek az eszközök hajthatók végre műveleteket ebben a dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="acc45-111">These tools are used to perform some of the operations in this document:</span></span>

* <span data-ttu-id="acc45-112">[Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure Felhőszolgáltatások azonban az helyes útmutatást és példákat tartalmaz)</span><span class="sxs-lookup"><span data-stu-id="acc45-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related to Azure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="acc45-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="acc45-113">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="acc45-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="acc45-114">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="acc45-115">Az Azure Resource Manager-ügyfél</span><span class="sxs-lookup"><span data-stu-id="acc45-115">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="acc45-116">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="acc45-116">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a><span data-ttu-id="acc45-117">Naplófájl és esemény források</span><span class="sxs-lookup"><span data-stu-id="acc45-117">Log and event sources</span></span>

### <a name="service-fabric-platform-events"></a><span data-ttu-id="acc45-118">A Service Fabric platform események</span><span class="sxs-lookup"><span data-stu-id="acc45-118">Service Fabric platform events</span></span>
<span data-ttu-id="acc45-119">A bemutatott [Ez a cikk](service-fabric-diagnostics-event-generation-infra.md), a Service Fabric beállítja, néhány out-of-az-box naplózási csatornák, amelyek a következő csatornák vannak könnyen konfigurálva ÜVEGVATTA küldeni a figyelési és diagnosztikai adatokat tároló táblára vagy máshol:</span><span class="sxs-lookup"><span data-stu-id="acc45-119">As discussed in [this article](service-fabric-diagnostics-event-generation-infra.md), Service Fabric sets you up with a few out-of-the-box logging channels, of which the following channels are easily configured with WAD to send monitoring and diagnostics data to a storage table or elsewhere:</span></span>
  * <span data-ttu-id="acc45-120">A működési események: magasabb szintű műveletek a Service Fabric-platformról hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="acc45-120">Operational events: higher-level operations that the Service Fabric platform performs.</span></span> <span data-ttu-id="acc45-121">Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását.</span><span class="sxs-lookup"><span data-stu-id="acc45-121">Examples include creation of applications and services, node state changes, and upgrade information.</span></span> <span data-ttu-id="acc45-122">Ezek kibocsátott mint esemény Windows (nyomkövetés) naplók</span><span class="sxs-lookup"><span data-stu-id="acc45-122">These are emitted as Event Tracing for Windows (ETW) logs</span></span>
  * [<span data-ttu-id="acc45-123">Reliable Actors programozási modell események</span><span class="sxs-lookup"><span data-stu-id="acc45-123">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="acc45-124">Megbízható Services-programozási modell események</span><span class="sxs-lookup"><span data-stu-id="acc45-124">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a><span data-ttu-id="acc45-125">Alkalmazás-események</span><span class="sxs-lookup"><span data-stu-id="acc45-125">Application events</span></span>
 <span data-ttu-id="acc45-126">Az alkalmazások és szolgáltatások kódból kibocsátott, és a Visual Studio sablonok találhatók az EventSource segítőosztály írhatók eseményeket.</span><span class="sxs-lookup"><span data-stu-id="acc45-126">Events emitted from your applications' and services' code and written out by using the EventSource helper class provided in the Visual Studio templates.</span></span> <span data-ttu-id="acc45-127">Az EventSource naplók írásával az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="acc45-127">For more information on how to write EventSource logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="acc45-128">A diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="acc45-128">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="acc45-129">Az első lépés a naplók gyűjtésére központi telepítése a diagnosztika bővítmény minden egyes a Service Fabric-fürt a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="acc45-129">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="acc45-130">A diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti a megadott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="acc45-130">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="acc45-131">A lépések kissé függően hogy használ-e az Azure portálon vagy az Azure Resource Manager változhat.</span><span class="sxs-lookup"><span data-stu-id="acc45-131">The steps vary a little based on whether you use the Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="acc45-132">A lépéseket is függően változnak, hogy a központi telepítés része a fürt létrehozása, vagy egy már létező fürt.</span><span class="sxs-lookup"><span data-stu-id="acc45-132">The steps also vary based on whether the deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="acc45-133">Vizsgáljuk meg az egyes forgatókönyv lépéseit.</span><span class="sxs-lookup"><span data-stu-id="acc45-133">Let's look at the steps for each scenario.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a><span data-ttu-id="acc45-134">A fürt létrehozása az Azure portálon keresztül részeként diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="acc45-134">Deploy the Diagnostics extension as part of cluster creation through Azure portal</span></span>
<span data-ttu-id="acc45-135">A diagnosztikai beállítások panel segítségével központilag telepíteni a diagnosztika bővítményt a virtuális gépeket a fürt fürt létrehozásának részeként, az alábbi ábrának - győződjön meg arról, hogy diagnosztikai beállításai **a** (az alapértelmezett beállítás).</span><span class="sxs-lookup"><span data-stu-id="acc45-135">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, you use the Diagnostics settings panel shown in the following image - ensure that Diagnostics is set to **On** (the default setting).</span></span> <span data-ttu-id="acc45-136">A fürt létrehozása után ezeket a beállításokat nem módosíthatja a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="acc45-136">After you create the cluster, you can't change these settings by using the portal.</span></span>

![A fürt létrehozása a portálon az Azure diagnosztikai beállításai](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

<span data-ttu-id="acc45-138">A fürt létrehozásakor a portál használatával erősen ajánlott, hogy töltse le a sablon **OK gombra kattintás előtt** a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="acc45-138">When you're creating a cluster by using the portal, we highly recommend that you download the template **before you click OK** to create the cluster.</span></span> <span data-ttu-id="acc45-139">További információkért tekintse meg [Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="acc45-139">For details, refer to [Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="acc45-140">A módosítások később, a sablon lesz szüksége, mert nem módosítja a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="acc45-140">You'll need the template to make changes later, because you can't make some changes by using the portal.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="acc45-141">A fürt létrehozásának részeként diagnosztika bővítmény telepítése Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="acc45-141">Deploy the Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="acc45-142">Fürt létrehozásához erőforrás-kezelő használatával kell hozzáadnia a diagnosztika konfigurációs JSON a teljes fürt Resource Manager-sablon, a fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="acc45-142">To create a cluster by using Resource Manager, you need to add the Diagnostics configuration JSON to the full cluster Resource Manager template before you create the cluster.</span></span> <span data-ttu-id="acc45-143">Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk a Resource Manager sablon minták részeként hozzáadott diagnosztika konfigurációjával.</span><span class="sxs-lookup"><span data-stu-id="acc45-143">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added to it as part of our Resource Manager template samples.</span></span> <span data-ttu-id="acc45-144">Ezen a helyen az Azure-minták oldalon láthatja: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span><span class="sxs-lookup"><span data-stu-id="acc45-144">You can see it at this location in the Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span></span>

<span data-ttu-id="acc45-145">A diagnosztika beállítást a Resource Manager-sablon megtekintéséhez nyissa meg az azuredeploy.json fájlt, és keresse meg **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="acc45-145">To see the Diagnostics setting in the Resource Manager template, open the azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="acc45-146">A fürt létrehozásához a sablon használatával, jelölje ki a **az Azure telepítéséhez** gomb érhető el az előző kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="acc45-146">To create a cluster by using this template, select the **Deploy to Azure** button available at the previous link.</span></span>

<span data-ttu-id="acc45-147">Azt is megteheti, töltse le az erőforrás-kezelő minta, módosítani és hozzon létre egy fürtöt a módosított sablon használatával a `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="acc45-147">Alternatively, you can download the Resource Manager sample, make changes to it, and create a cluster with the modified template by using the `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="acc45-148">Tekintse meg a következő kódot a paraméterek, amelyeket átad a a parancsot.</span><span class="sxs-lookup"><span data-stu-id="acc45-148">See the following code for the parameters that you pass in to the command.</span></span> <span data-ttu-id="acc45-149">Az erőforráscsoport PowerShell használatával történő központi telepítéséről részletes információkért lásd: a cikk [központi telepítése egy erőforráscsoportot az Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="acc45-149">For detailed information on how to deploy a resource group by using PowerShell, see the article [Deploy a resource group with the Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a><span data-ttu-id="acc45-150">A diagnosztika bővítmény telepítése egy meglévő fürthöz</span><span class="sxs-lookup"><span data-stu-id="acc45-150">Deploy the Diagnostics extension to an existing cluster</span></span>
<span data-ttu-id="acc45-151">Ha egy meglévő fürtöt, amely nem rendelkezik telepített diagnosztika, vagy ha azt szeretné, hogy egy meglévő konfigurációjának módosítása, adja hozzá, vagy frissítheti azt.</span><span class="sxs-lookup"><span data-stu-id="acc45-151">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want to modify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="acc45-152">Módosítsa a Resource Manager-sablon, amellyel a meglévő fürt létrehozása, vagy a sablon letöltéséről a portálon, a fentebb leírt módon.</span><span class="sxs-lookup"><span data-stu-id="acc45-152">Modify the Resource Manager template that's used to create the existing cluster or download the template from the portal as described earlier.</span></span> <span data-ttu-id="acc45-153">A template.json fájl módosítása a következő feladatok végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="acc45-153">Modify the template.json file by performing the following tasks.</span></span>

<span data-ttu-id="acc45-154">A sablon a források szakaszában ad hozzá egy új tárolási erőforrás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="acc45-154">Add a new storage resource to the template by adding to the resources section.</span></span>

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

 <span data-ttu-id="acc45-155">A következő után vesz fel a Paraméterek szakaszban csak a tárolási fiók definíciók között `supportLogStorageAccountName` és `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="acc45-155">Next, add to the parameters section just after the storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="acc45-156">Cserélje le a helyőrzőket *ide kerül a tárfiók neve* a tárfiók nevével.</span><span class="sxs-lookup"><span data-stu-id="acc45-156">Replace the placeholder text *storage account name goes here* with the name of the storage account.</span></span>

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
<span data-ttu-id="acc45-157">Ezt követően frissítse a `VirtualMachineProfile` a template.json fájl a következő kódrészletet a bővítmények tömbön belüli szakaszában.</span><span class="sxs-lookup"><span data-stu-id="acc45-157">Then, update the `VirtualMachineProfile` section of the template.json file by adding the following code within the extensions array.</span></span> <span data-ttu-id="acc45-158">Ne feledje hozzáadni vesszővel elején vagy végén, attól függően, hogy hol csatlakoztatva van-e.</span><span class="sxs-lookup"><span data-stu-id="acc45-158">Be sure to add a comma at the beginning or the end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="acc45-159">Lásd a template.json fájl módosítása, után közzé a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="acc45-159">After you modify the template.json file as described, republish the Resource Manager template.</span></span> <span data-ttu-id="acc45-160">Ha a sablon exportált, a sablon a deploy.ps1 fájlt futtatja addig.</span><span class="sxs-lookup"><span data-stu-id="acc45-160">If the template was exported, running the deploy.ps1 file republishes the template.</span></span> <span data-ttu-id="acc45-161">Miután telepít, győződjön meg arról, hogy **ProvisioningState** van **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="acc45-161">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="collect-health-and-load-events"></a><span data-ttu-id="acc45-162">Állapot gyűjtése és események betöltése</span><span class="sxs-lookup"><span data-stu-id="acc45-162">Collect health and load events</span></span>

<span data-ttu-id="acc45-163">A Service Fabric 5.4 kiadástól kezdve, állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="acc45-163">Starting with the 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="acc45-164">Ezek az események tükrözze az állapotfigyelő segítségével a rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="acc45-164">These events reflect events generated by the system or your code by using the health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="acc45-165">Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján.</span><span class="sxs-lookup"><span data-stu-id="acc45-165">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="acc45-166">Ezeket az eseményeket a Visual Studio diagnosztikai eseménynapló adja hozzá megtekintése "Microsoft-ServiceFabric:4:0x4000000000000008" ETW-szolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="acc45-166">To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" to the list of ETW providers.</span></span>

<span data-ttu-id="acc45-167">Az események összegyűjtésére, tartalmazza a Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="acc45-167">To collect the events, modify the Resource Manager template to include</span></span>

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

## <a name="collect-reverse-proxy-events"></a><span data-ttu-id="acc45-168">Fordított proxy eseményeinek gyűjtése</span><span class="sxs-lookup"><span data-stu-id="acc45-168">Collect reverse proxy events</span></span>

<span data-ttu-id="acc45-169">A Service Fabric 5.7 kiadástól kezdve [fordított proxy](service-fabric-reverseproxy.md) események állnak rendelkezésre a következő gyűjtemény számára.</span><span class="sxs-lookup"><span data-stu-id="acc45-169">Starting with the 5.7 release of Service Fabric, [reverse proxy](service-fabric-reverseproxy.md) events are available for collection.</span></span>
<span data-ttu-id="acc45-170">Fordított proxy események két csatornákon, egy kérelem feldolgozása sikertelen, a másik a kérelmekkel kapcsolatos részletes eseményeket tartalmazó tükröző tartalmazó hibaesemények feldolgozni a fordított proxy bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="acc45-170">Reverse proxy emits events into two channels, one containing error events reflecting request processing failures and the other one containing verbose events about all the requests processed at the reverse proxy.</span></span> 

1. <span data-ttu-id="acc45-171">Hiba eseményeinek gyűjtése: megtekintéséhez ezeket az eseményeket a Visual Studio diagnosztikai eseménynapló hozzáadása "Microsoft-ServiceFabric:4:0x4000000000000010" ETW-szolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="acc45-171">Collect error events: To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000010" to the list of ETW providers.</span></span>
<span data-ttu-id="acc45-172">Az események összegyűjtésére Azure fürtök, tartalmazza a Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="acc45-172">To collect the events from Azure clusters, modify the Resource Manager template to include</span></span>

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

2. <span data-ttu-id="acc45-173">Összegyűjteni az összes kérelem események feldolgozását: A Visual Studio diagnosztikai eseménynapló, frissítés a Microsoft-ServiceFabric bejegyzés ETW szolgáltató listában "Microsoft-ServiceFabric:4:0x4000000000000020".</span><span class="sxs-lookup"><span data-stu-id="acc45-173">Collect all request processing events: In Visual Studio's Diagnostic Event Viewer, update the Microsoft-ServiceFabric entry in the ETW provider list to "Microsoft-ServiceFabric:4:0x4000000000000020".</span></span>
<span data-ttu-id="acc45-174">Az Azure Service Fabric-fürtök esetén tartalmazza a resource manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="acc45-174">For Azure Service Fabric clusters, modify the resource manager template to include</span></span>

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
> <span data-ttu-id="acc45-175">Javasoljuk, hogy körültekintően a csatornán gyűjtését események ez gyűjti az összes forgalom a fordított proxyn keresztül és engedélyezése gyorsan felhasználhat a tárolási kapacitást.</span><span class="sxs-lookup"><span data-stu-id="acc45-175">It is recommended to judiciously enable collecting events from this channel as this collects all traffic through the reverse proxy and can quickly consume storage capacity.</span></span>

<span data-ttu-id="acc45-176">Az Azure Service Fabric-fürtök esetén a csomópontok eseményei gyűjtött és a SystemEventTable összesíteni.</span><span class="sxs-lookup"><span data-stu-id="acc45-176">For Azure Service Fabric clusters, the events from all the nodes are collected and aggregated in the SystemEventTable.</span></span>
<span data-ttu-id="acc45-177">Részletes fordított proxy eseményeket, tekintse meg a [fordított proxy diagnosztika útmutató](service-fabric-reverse-proxy-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="acc45-177">For detailed troubleshooting of the reverse proxy events, refer the [reverse proxy diagnostics guide](service-fabric-reverse-proxy-diagnostics.md).</span></span>

## <a name="collect-from-new-eventsource-channels"></a><span data-ttu-id="acc45-178">Új EventSource csatornák gyűjtése</span><span class="sxs-lookup"><span data-stu-id="acc45-178">Collect from new EventSource channels</span></span>

<span data-ttu-id="acc45-179">Frissíteni a diagnosztikai naplók összegyűjtésére új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, hogy Ön éppen kapcsolatos telepítéséhez hajtsa végre azonos fürt a meglévő diagnosztikai beállításának korábban ismertetett lépéseket.</span><span class="sxs-lookup"><span data-stu-id="acc45-179">To update Diagnostics to collect logs from new EventSource channels that represent a new application that you're about to deploy, perform the same steps as previously described for the setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="acc45-180">Frissítse a `EtwEventSourceProviderConfiguration` az template.json fájl bejegyzés hozzáadása előtt, a konfiguráció új EventSource csatornák használatával módosítsa a `New-AzureRmResourceGroupDeployment` PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="acc45-180">Update the `EtwEventSourceProviderConfiguration` section in the template.json file to add entries for the new EventSource channels before you apply the configuration update by using the `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="acc45-181">A forrás nevét a kódot, a Visual Studio által létrehozott ServiceEventSource.cs fájl részeként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="acc45-181">The name of the event source is defined as part of your code in the Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="acc45-182">Ha a forrás saját Eventsource neve, adja hozzá például a következő kódot a saját Eventsource eseményei helyezzen MyDestinationTableName nevű tábla.</span><span class="sxs-lookup"><span data-stu-id="acc45-182">For example, if your event source is named My-Eventsource, add the following code to place the events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="acc45-183">Gyűjti az teljesítményszámlálók és az eseménynaplók, módosítsa a Resource Manager-sablon a megadott példák felhasználásával [figyelése és diagnosztika Windows virtuális gép létrehozása Azure Resource Manager-sablon használatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="acc45-183">To collect performance counters or event logs, modify the Resource Manager template by using the examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="acc45-184">A Resource Manager-sablon, majd közzé.</span><span class="sxs-lookup"><span data-stu-id="acc45-184">Then, republish the Resource Manager template.</span></span>

## <a name="collect-performance-counters"></a><span data-ttu-id="acc45-185">A teljesítményszámlálók adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="acc45-185">Collect Performance Counters</span></span>

<span data-ttu-id="acc45-186">Teljesítményadatok gyűjtéséhez a fürtről, a teljesítményszámlálók hozzáadása a "WadCfg > DiagnosticMonitorConfiguration" a Resource Manager sablon a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="acc45-186">To collect performance metrics from your cluster, add the performance counters to your "WadCfg > DiagnosticMonitorConfiguration" in the Resource Manager template for your cluster.</span></span> <span data-ttu-id="acc45-187">Lásd: [Service Fabric teljesítményszámlálók](service-fabric-diagnostics-event-generation-perf.md) teljesítményszámlálóinak, javasoljuk a összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="acc45-187">See [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) for performance counters that we recommend collecting.</span></span>

<span data-ttu-id="acc45-188">Például itt hivatott egy teljesítményszámláló mintát 15 másodpercenként (ez módosítható, és a formátuma a következő "PT\<idő >\<egység >", például a PT3M három perces időközönként mintát lenne), és továbbítja a megfelelő tárolási tábla egy percenként.</span><span class="sxs-lookup"><span data-stu-id="acc45-188">For example, here we set one performance counter, sampled every 15 seconds (this can be changed and follows the format of "PT\<time>\<unit>", for example, PT3M would sample at three minute intervals), and transferred to the appropriate storage table every one minute.</span></span>

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
  
<span data-ttu-id="acc45-189">Ha Ön egy Application Insights fogadó használja a következő szakaszban leírtak szerint, és ezeket a metrikákat az Application Insights megjelennek, majd győződjön meg arról, a fogadó név hozzáadásához, ahogy fent látható "mosdók" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="acc45-189">If you are using an Application Insights sink, as described in the section below, and want these metrics to show up in Application Insights, then make sure to add the sink name in the "sinks" section as shown above.</span></span> <span data-ttu-id="acc45-190">Ezenkívül célszerű küldeni a teljesítményszámlálókat, különálló táblában, azok utasok nagyobb nem csoportjának kimenő az adatforrásból származó az egyéb naplózási csatornák engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="acc45-190">Additionally, consider creating a separate table to send your Performance Counters to, so they don't crowd out the data coming from the other logging channels you have enabled.</span></span>


## <a name="send-logs-to-application-insights"></a><span data-ttu-id="acc45-191">Az Application Insights elküldeni a naplókat</span><span class="sxs-lookup"><span data-stu-id="acc45-191">Send logs to Application Insights</span></span>

<span data-ttu-id="acc45-192">Figyelés és diagnosztikai adatok küldése az Application Insights (AI) teheti a ÜVEGVATTA konfiguráció részeként.</span><span class="sxs-lookup"><span data-stu-id="acc45-192">Sending monitoring and diagnostics data to Application Insights (AI) can be done as part of the WAD configuration.</span></span> <span data-ttu-id="acc45-193">Ha úgy dönt, hogy az esemény elemzése és a képi megjelenítés AI használni, olvassa el a [esemény elemzése és az Application insights szolgáltatással a képi megjelenítés](service-fabric-diagnostics-event-analysis-appinsights.md) egy AI fogadó beállítása a "WadCfg" részeként.</span><span class="sxs-lookup"><span data-stu-id="acc45-193">If you decide to use AI for event analysis and visualization, read [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) to set up an AI Sink as part of your "WadCfg".</span></span>

## <a name="next-steps"></a><span data-ttu-id="acc45-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="acc45-194">Next steps</span></span>

<span data-ttu-id="acc45-195">Miután konfigurálta az Azure diagnostics megfelelően, adatait jeleníti meg a tárolási táblákba a ETW és EventSource naplókból.</span><span class="sxs-lookup"><span data-stu-id="acc45-195">Once you have correctly configured Azure diagnostics, you will see data in your Storage tables from the ETW and EventSource logs.</span></span> <span data-ttu-id="acc45-196">Ha OMS, Kibana vagy bármely más elemzés és a képi megjelenítés adatplatform, amely közvetlenül nincs konfigurálva a Resource Manager-sablon használatát választja, győződjön meg arról, hogy az Ön által választott, olvassa el a tárolási táblák adatait a platform beállításához.</span><span class="sxs-lookup"><span data-stu-id="acc45-196">If you choose to use OMS, Kibana, or any other data analytics and visualization platform that is not directly configured in the Resource Manager template, make sure to set up the platform of your choice to read in the data from these storage tables.</span></span> <span data-ttu-id="acc45-197">Az OMS nagyon viszonylag egyszerű, és az ismertetése [esemény és a naplófájlok elemzése OMS keresztül](service-fabric-diagnostics-event-analysis-oms.md).</span><span class="sxs-lookup"><span data-stu-id="acc45-197">Doing this for OMS is relatively trivial, and is explained in [Event and log analysis through OMS](service-fabric-diagnostics-event-analysis-oms.md).</span></span> <span data-ttu-id="acc45-198">Az Application Insights egy bit egy különleges esetben abban az értelemben, a diagnosztika bővítménykonfiguráció részeként konfigurálhatók, mivel így tekintse meg a [megfelelő cikk](service-fabric-diagnostics-event-analysis-appinsights.md) Ha úgy dönt, hogy AI használja.</span><span class="sxs-lookup"><span data-stu-id="acc45-198">Application Insights is a bit of a special case in this sense, since it can be configured as part of the Diagnostics extension configuration, so refer to the [appropriate article](service-fabric-diagnostics-event-analysis-appinsights.md) if you choose to use AI.</span></span>

>[!NOTE]
><span data-ttu-id="acc45-199">Jelenleg nincs mód való vagy készítsen fel a figyelmet, hogy a tábla küldött.</span><span class="sxs-lookup"><span data-stu-id="acc45-199">There is currently no way to filter or groom the events that are sent to the table.</span></span> <span data-ttu-id="acc45-200">Egy folyamat leállását események eltávolítja a tábla nem valósítja meg, ha a tábla egyre nő.</span><span class="sxs-lookup"><span data-stu-id="acc45-200">If you don't implement a process to remove events from the table, the table will continue to grow.</span></span> <span data-ttu-id="acc45-201">Jelenleg egy futó karcsúsítási szolgáltatás például a [figyelő minta](https://github.com/Azure-Samples/service-fabric-watchdog-service), és javasolt, hogy lehet írni egy a saját kezűleg is, kivéve ha egy jó 30 vagy 90 nap határidőn túli naplók tárolásához.</span><span class="sxs-lookup"><span data-stu-id="acc45-201">Currently, there is an example of a data grooming service running in the [Watchdog sample](https://github.com/Azure-Samples/service-fabric-watchdog-service), and it is recommended that you write one for yourself as well, unless there is a good reason for you to store logs beyond a 30 or 90 day timeframe.</span></span>

* [<span data-ttu-id="acc45-202">Megtudhatja, hogyan gyűjti a teljesítményszámlálók és a naplókat a diagnosztika bővítmény segítségével</span><span class="sxs-lookup"><span data-stu-id="acc45-202">Learn how to collect performance counters or logs by using the Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="acc45-203">Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="acc45-203">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="acc45-204">Esemény elemzése és az OMS képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="acc45-204">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)