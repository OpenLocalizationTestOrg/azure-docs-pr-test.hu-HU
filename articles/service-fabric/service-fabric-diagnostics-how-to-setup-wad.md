---
title: "Azure Diagnostics használatával naplógyűjtéshez |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan állíthat be Azure Diagnostics egy Azure-ban futó Service Fabric-fürt naplóinak gyűjtése."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 190a8a393f2e7d74a792db4efa81f94a18b02221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="5d92f-103">Naplók gyűjtése az Azure Diagnostics használatával</span><span class="sxs-lookup"><span data-stu-id="5d92f-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5d92f-104">Windows</span><span class="sxs-lookup"><span data-stu-id="5d92f-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="5d92f-105">Linux</span><span class="sxs-lookup"><span data-stu-id="5d92f-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="5d92f-106">Amikor egy Azure Service Fabric-fürtöt használ, célszerű gyűjteni a egy központi helyen található minden csomópontot.</span><span class="sxs-lookup"><span data-stu-id="5d92f-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="5d92f-107">A naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy a futó alkalmazások és szolgáltatások az adott fürtben található a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="5d92f-107">Having the logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in the applications and services running in that cluster.</span></span>

<span data-ttu-id="5d92f-108">Egy feltöltése és naplógyűjtéshez módja az Azure Diagnostics bővítménnyel Azure Storage, Azure Application Insights vagy az Azure Event Hubs naplók feltöltését.</span><span class="sxs-lookup"><span data-stu-id="5d92f-108">One way to upload and collect logs is to use the Azure Diagnostics extension, which uploads logs to Azure Storage, Azure Application Insights, or Azure Event Hubs.</span></span> <span data-ttu-id="5d92f-109">A napló az értékek nem, amely közvetlenül a tároló, vagy az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="5d92f-109">The logs are not that useful directly in storage or in Event Hubs.</span></span> <span data-ttu-id="5d92f-110">De használhat külső folyamat kiolvassa az eseményeket a tárolóból, és helyezze el őket a termék például [Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás.</span><span class="sxs-lookup"><span data-stu-id="5d92f-110">But you can use an external process to read the events from storage and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="5d92f-111">[Az Azure Application Insights](https://azure.microsoft.com/services/application-insights/) egy átfogó naplózási keresési és analytics szolgáltatás beépített tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5d92f-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d92f-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d92f-112">Prerequisites</span></span>
<span data-ttu-id="5d92f-113">Az eszközök segítségével hajtsa végre műveleteket ebben a dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="5d92f-113">You use these tools to perform some of the operations in this document:</span></span>

* <span data-ttu-id="5d92f-114">[Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure Felhőszolgáltatások azonban az helyes útmutatást és példákat tartalmaz)</span><span class="sxs-lookup"><span data-stu-id="5d92f-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related to Azure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="5d92f-115">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5d92f-115">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="5d92f-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d92f-116">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="5d92f-117">Az Azure Resource Manager-ügyfél</span><span class="sxs-lookup"><span data-stu-id="5d92f-117">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="5d92f-118">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="5d92f-118">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a><span data-ttu-id="5d92f-119">Előfordulhat, hogy szeretne gyűjteni napló források</span><span class="sxs-lookup"><span data-stu-id="5d92f-119">Log sources that you might want to collect</span></span>
* <span data-ttu-id="5d92f-120">**A Service Fabric naplók**: kibocsátott platformnak szabványos esemény-nyomkövetés Windows (ETW) és az EventSource csatorna.</span><span class="sxs-lookup"><span data-stu-id="5d92f-120">**Service Fabric logs**: Emitted from the platform to standard Event Tracing for Windows (ETW) and EventSource channels.</span></span> <span data-ttu-id="5d92f-121">Naplók több típusok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="5d92f-121">Logs can be one of several types:</span></span>
  * <span data-ttu-id="5d92f-122">A működési események: műveletek a Service Fabric-platformról végző naplókat.</span><span class="sxs-lookup"><span data-stu-id="5d92f-122">Operational events: Logs for operations that the Service Fabric platform performs.</span></span> <span data-ttu-id="5d92f-123">Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását.</span><span class="sxs-lookup"><span data-stu-id="5d92f-123">Examples include creation of applications and services, node state changes, and upgrade information.</span></span>
  * [<span data-ttu-id="5d92f-124">Reliable Actors programozási modell események</span><span class="sxs-lookup"><span data-stu-id="5d92f-124">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="5d92f-125">Megbízható Services-programozási modell események</span><span class="sxs-lookup"><span data-stu-id="5d92f-125">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)
* <span data-ttu-id="5d92f-126">**Alkalmazásesemények**: a szolgáltatás kódból kibocsátott, és a Visual Studio sablonok találhatók az EventSource segítőosztály írhatók eseményeket.</span><span class="sxs-lookup"><span data-stu-id="5d92f-126">**Application events**: Events emitted from your service's code and written out by using the EventSource helper class provided in the Visual Studio templates.</span></span> <span data-ttu-id="5d92f-127">A naplók írásával az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="5d92f-127">For more information on how to write logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="5d92f-128">A diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="5d92f-128">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="5d92f-129">Az első lépés a naplók gyűjtésére központi telepítése a diagnosztika bővítmény minden egyes a Service Fabric-fürt a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5d92f-129">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="5d92f-130">A diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti a megadott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5d92f-130">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="5d92f-131">A lépések kissé függően hogy használ-e az Azure portálon vagy az Azure Resource Manager változhat.</span><span class="sxs-lookup"><span data-stu-id="5d92f-131">The steps vary a little based on whether you use the Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="5d92f-132">A lépéseket is függően változnak, hogy a központi telepítés része a fürt létrehozása, vagy egy már létező fürt.</span><span class="sxs-lookup"><span data-stu-id="5d92f-132">The steps also vary based on whether the deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="5d92f-133">Vizsgáljuk meg az egyes forgatókönyv lépéseit.</span><span class="sxs-lookup"><span data-stu-id="5d92f-133">Let's look at the steps for each scenario.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a><span data-ttu-id="5d92f-134">A fürt létrehozása a portálon keresztül részeként diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="5d92f-134">Deploy the Diagnostics extension as part of cluster creation through the portal</span></span>
<span data-ttu-id="5d92f-135">A diagnosztika bővítményt telepíteni a virtuális gépeket a fürt fürt létrehozásának részeként, használhatja a diagnosztikai beállítások panelen a következő ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="5d92f-135">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, you use the Diagnostics settings panel shown in the following image.</span></span> <span data-ttu-id="5d92f-136">Engedélyezi a Reliable Actors vagy Reliable Services eseménygyűjtés, győződjön meg arról, hogy diagnosztikai beállításai **a** (az alapértelmezett beállítás).</span><span class="sxs-lookup"><span data-stu-id="5d92f-136">To enable Reliable Actors or Reliable Services event collection, ensure that Diagnostics is set to **On** (the default setting).</span></span> <span data-ttu-id="5d92f-137">A fürt létrehozása után ezeket a beállításokat nem módosíthatja a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5d92f-137">After you create the cluster, you can't change these settings by using the portal.</span></span>

![A fürt létrehozása a portálon az Azure diagnosztikai beállításai](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

<span data-ttu-id="5d92f-139">Az Azure támogatási csapata *szükséges* naplók segíthetnek megoldani az összes Ön által létrehozott támogatási kérelmek támogatásához.</span><span class="sxs-lookup"><span data-stu-id="5d92f-139">The Azure support team *requires* support logs to help resolve any support requests that you create.</span></span> <span data-ttu-id="5d92f-140">Ezek a naplók gyűjtött valós időben, és a storage-fiókok létrehozása az erőforráscsoportban valamelyikében.</span><span class="sxs-lookup"><span data-stu-id="5d92f-140">These logs are collected in real time and are stored in one of the storage accounts created in the resource group.</span></span> <span data-ttu-id="5d92f-141">A diagnosztikai beállítások alkalmazásszintű események beállítása.</span><span class="sxs-lookup"><span data-stu-id="5d92f-141">The Diagnostics settings configure application-level events.</span></span> <span data-ttu-id="5d92f-142">Ezek az események tartalmazzák [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) események, [Reliable Services](service-fabric-reliable-services-diagnostics.md) eseményeket, és egyes rendszerszintű Service Fabric események Azure Storage tárolja.</span><span class="sxs-lookup"><span data-stu-id="5d92f-142">These events include [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) events, [Reliable Services](service-fabric-reliable-services-diagnostics.md) events, and some system-level Service Fabric events to be stored in Azure Storage.</span></span>

<span data-ttu-id="5d92f-143">Például termékek [Elasticsearch](https://www.elastic.co/guide/index.html) vagy a saját folyamatának az események lekérheti a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5d92f-143">Products such as [Elasticsearch](https://www.elastic.co/guide/index.html) or your own process can get the events from the storage account.</span></span> <span data-ttu-id="5d92f-144">Jelenleg nincs mód való vagy készítsen fel a figyelmet, hogy a tábla küldött.</span><span class="sxs-lookup"><span data-stu-id="5d92f-144">There is currently no way to filter or groom the events that are sent to the table.</span></span> <span data-ttu-id="5d92f-145">Egy folyamat leállását események eltávolítja a tábla nem valósítja meg, ha a tábla egyre nő.</span><span class="sxs-lookup"><span data-stu-id="5d92f-145">If you don't implement a process to remove events from the table, the table will continue to grow.</span></span>

<span data-ttu-id="5d92f-146">A fürt létrehozásakor a portál használatával erősen ajánlott, hogy töltse le a sablon **OK gombra kattintás előtt** a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5d92f-146">When you're creating a cluster by using the portal, we highly recommend that you download the template **before you click OK** to create the cluster.</span></span> <span data-ttu-id="5d92f-147">További információkért tekintse meg [Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5d92f-147">For details, refer to [Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="5d92f-148">A módosítások később, a sablon lesz szüksége, mert nem módosítja a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5d92f-148">You'll need the template to make changes later, because you can't make some changes by using the portal.</span></span>

<span data-ttu-id="5d92f-149">Sablonok a következő lépések segítségével exportálhatja a portálról.</span><span class="sxs-lookup"><span data-stu-id="5d92f-149">You can export templates from the portal by using the following steps.</span></span> <span data-ttu-id="5d92f-150">Azonban ezeket a sablonokat is több használata is nehézkes lehet miatt előfordulhat, hogy hiányoznak a szükséges információk null értékeket.</span><span class="sxs-lookup"><span data-stu-id="5d92f-150">However, these templates can be more difficult to use because they might have null values that are missing required information.</span></span>

1. <span data-ttu-id="5d92f-151">Nyissa meg az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5d92f-151">Open your resource group.</span></span>
2. <span data-ttu-id="5d92f-152">Válassza ki **beállítások** a beállítások panel megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d92f-152">Select **Settings** to display the settings panel.</span></span>
3. <span data-ttu-id="5d92f-153">Válassza ki **központi telepítések** a központi telepítés előzményei panel megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d92f-153">Select **Deployments** to display the deployment history panel.</span></span>
4. <span data-ttu-id="5d92f-154">Jelölje ki a telepítést, a központi telepítés részleteinek megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d92f-154">Select a deployment to display the details of the deployment.</span></span>
5. <span data-ttu-id="5d92f-155">Válassza ki **sablon exportálása** a sablon panel megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d92f-155">Select **Export Template** to display the template panel.</span></span>
6. <span data-ttu-id="5d92f-156">Válassza ki **fájlba Mentés** a sablonban, a paraméter és a PowerShell-fájlokat tartalmazó .zip fájl exportálása.</span><span class="sxs-lookup"><span data-stu-id="5d92f-156">Select **Save to file** to export a .zip file that contains the template, parameter, and PowerShell files.</span></span>

<span data-ttu-id="5d92f-157">Miután exportálta a fájlokat, a módosítások kell.</span><span class="sxs-lookup"><span data-stu-id="5d92f-157">After you export the files, you need to make a modification.</span></span> <span data-ttu-id="5d92f-158">A parameters.json fájl szerkesztése és eltávolítása a **adminPassword** elemet.</span><span class="sxs-lookup"><span data-stu-id="5d92f-158">Edit the parameters.json file and remove the **adminPassword** element.</span></span> <span data-ttu-id="5d92f-159">Ennek következtében a a jelszó kérése a telepítési parancsfájl futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="5d92f-159">This will cause a prompt for the password when the deployment script is run.</span></span> <span data-ttu-id="5d92f-160">Jelenik meg a telepítési parancsfájlt, akkor előfordulhat, hogy javítsa ki a null paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="5d92f-160">When you're running the deployment script, you might have to fix null parameter values.</span></span>

<span data-ttu-id="5d92f-161">A letöltött sablonok használata a konfiguráció frissítése:</span><span class="sxs-lookup"><span data-stu-id="5d92f-161">To use the downloaded template to update a configuration:</span></span>

1. <span data-ttu-id="5d92f-162">Bontsa ki a tartalmát a helyi számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="5d92f-162">Extract the contents to a folder on your local computer.</span></span>
2. <span data-ttu-id="5d92f-163">A tartalom tükrözzék az új konfiguráció módosítása.</span><span class="sxs-lookup"><span data-stu-id="5d92f-163">Modify the contents to reflect the new configuration.</span></span>
3. <span data-ttu-id="5d92f-164">Indítsa el a Powershellt, és módosítsa a mappát, amelyikbe kibontotta a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="5d92f-164">Start PowerShell and change to the folder where you extracted the content.</span></span>
4. <span data-ttu-id="5d92f-165">Futtatás **deploy.ps1** , és töltse ki az előfizetés-azonosító, az erőforráscsoport neve (használja ugyanazt a konfiguráció frissítése) és egyedi telepítési nevét.</span><span class="sxs-lookup"><span data-stu-id="5d92f-165">Run **deploy.ps1** and fill in the subscription ID, the resource group name (use the same name to update the configuration), and a unique deployment name.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="5d92f-166">A fürt létrehozásának részeként diagnosztika bővítmény telepítése Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="5d92f-166">Deploy the Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="5d92f-167">Fürt létrehozásához erőforrás-kezelő használatával kell hozzáadnia a diagnosztika konfigurációs JSON a teljes fürt Resource Manager-sablon, a fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="5d92f-167">To create a cluster by using Resource Manager, you need to add the Diagnostics configuration JSON to the full cluster Resource Manager template before you create the cluster.</span></span> <span data-ttu-id="5d92f-168">Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk a Resource Manager sablon minták részeként hozzáadott diagnosztika konfigurációjával.</span><span class="sxs-lookup"><span data-stu-id="5d92f-168">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added to it as part of our Resource Manager template samples.</span></span> <span data-ttu-id="5d92f-169">Ezen a helyen az Azure-minták oldalon láthatja: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span><span class="sxs-lookup"><span data-stu-id="5d92f-169">You can see it at this location in the Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span></span>

<span data-ttu-id="5d92f-170">A diagnosztika beállítást a Resource Manager-sablon megtekintéséhez nyissa meg az azuredeploy.json fájlt, és keresse meg **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="5d92f-170">To see the Diagnostics setting in the Resource Manager template, open the azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="5d92f-171">A fürt létrehozásához a sablon használatával, jelölje ki a **az Azure telepítéséhez** gomb érhető el az előző kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5d92f-171">To create a cluster by using this template, select the **Deploy to Azure** button available at the previous link.</span></span>

<span data-ttu-id="5d92f-172">Azt is megteheti, töltse le az erőforrás-kezelő minta, módosítani és hozzon létre egy fürtöt a módosított sablon használatával a `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="5d92f-172">Alternatively, you can download the Resource Manager sample, make changes to it, and create a cluster with the modified template by using the `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="5d92f-173">Tekintse meg a következő kódot a paraméterek, amelyeket átad a a parancsot.</span><span class="sxs-lookup"><span data-stu-id="5d92f-173">See the following code for the parameters that you pass in to the command.</span></span> <span data-ttu-id="5d92f-174">Az erőforráscsoport PowerShell használatával történő központi telepítéséről részletes információkért lásd: a cikk [központi telepítése egy erőforráscsoportot az Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5d92f-174">For detailed information on how to deploy a resource group by using PowerShell, see the article [Deploy a resource group with the Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a><span data-ttu-id="5d92f-175">A diagnosztika bővítmény telepítése egy meglévő fürthöz</span><span class="sxs-lookup"><span data-stu-id="5d92f-175">Deploy the Diagnostics extension to an existing cluster</span></span>
<span data-ttu-id="5d92f-176">Ha egy meglévő fürtöt, amely nem rendelkezik telepített diagnosztika, vagy ha azt szeretné, hogy egy meglévő konfigurációjának módosítása, adja hozzá, vagy frissítheti azt.</span><span class="sxs-lookup"><span data-stu-id="5d92f-176">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want to modify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="5d92f-177">Módosítsa a Resource Manager-sablon, amellyel a meglévő fürt létrehozása, vagy a sablon letöltéséről a portálon, a fentebb leírt módon.</span><span class="sxs-lookup"><span data-stu-id="5d92f-177">Modify the Resource Manager template that's used to create the existing cluster or download the template from the portal as described earlier.</span></span> <span data-ttu-id="5d92f-178">A template.json fájl módosítása a következő feladatok végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="5d92f-178">Modify the template.json file by performing the following tasks.</span></span>

<span data-ttu-id="5d92f-179">A sablon a források szakaszában ad hozzá egy új tárolási erőforrás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="5d92f-179">Add a new storage resource to the template by adding to the resources section.</span></span>

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

 <span data-ttu-id="5d92f-180">A következő után vesz fel a Paraméterek szakaszban csak a tárolási fiók definíciók között `supportLogStorageAccountName` és `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="5d92f-180">Next, add to the parameters section just after the storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="5d92f-181">Cserélje le a helyőrzőket *ide kerül a tárfiók neve* a tárfiók nevével.</span><span class="sxs-lookup"><span data-stu-id="5d92f-181">Replace the placeholder text *storage account name goes here* with the name of the storage account.</span></span>

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
<span data-ttu-id="5d92f-182">Ezt követően frissítse a `VirtualMachineProfile` a template.json fájl a következő kódrészletet a bővítmények tömbön belüli szakaszában.</span><span class="sxs-lookup"><span data-stu-id="5d92f-182">Then, update the `VirtualMachineProfile` section of the template.json file by adding the following code within the extensions array.</span></span> <span data-ttu-id="5d92f-183">Ne feledje hozzáadni vesszővel elején vagy végén, attól függően, hogy hol csatlakoztatva van-e.</span><span class="sxs-lookup"><span data-stu-id="5d92f-183">Be sure to add a comma at the beginning or the end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="5d92f-184">Lásd a template.json fájl módosítása, után közzé a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="5d92f-184">After you modify the template.json file as described, republish the Resource Manager template.</span></span> <span data-ttu-id="5d92f-185">Ha a sablon exportált, a sablon a deploy.ps1 fájlt futtatja addig.</span><span class="sxs-lookup"><span data-stu-id="5d92f-185">If the template was exported, running the deploy.ps1 file republishes the template.</span></span> <span data-ttu-id="5d92f-186">Miután telepít, győződjön meg arról, hogy **ProvisioningState** van **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="5d92f-186">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="update-diagnostics-to-collect-health-and-load-events"></a><span data-ttu-id="5d92f-187">Frissítse a diagnosztikai állapot gyűjtése és események betöltése</span><span class="sxs-lookup"><span data-stu-id="5d92f-187">Update diagnostics to collect health and load events</span></span>

<span data-ttu-id="5d92f-188">A Service Fabric 5.4 kiadástól kezdve, állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="5d92f-188">Starting with the 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="5d92f-189">Ezek az események tükrözze az állapotfigyelő segítségével a rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d92f-189">These events reflect events generated by the system or your code by using the health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="5d92f-190">Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján.</span><span class="sxs-lookup"><span data-stu-id="5d92f-190">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="5d92f-191">Ezeket az eseményeket a Visual Studio diagnosztikai eseménynapló adja hozzá megtekintése "Microsoft-ServiceFabric:4:0x4000000000000008" ETW-szolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="5d92f-191">To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" to the list of ETW providers.</span></span>

<span data-ttu-id="5d92f-192">Az események összegyűjtésére, tartalmazza a resource manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="5d92f-192">To collect the events, modify the resource manager template to include</span></span>

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

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a><span data-ttu-id="5d92f-193">Diagnosztika összegyűjtésére és a naplók feltöltése az új EventSource csatornák frissítése</span><span class="sxs-lookup"><span data-stu-id="5d92f-193">Update Diagnostics to collect and upload logs from new EventSource channels</span></span>
<span data-ttu-id="5d92f-194">Frissíteni a diagnosztikai naplók összegyűjtésére új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, amely a kívánt központi telepítése, hajtsa végre a lépéseket, mint a a [előző szakaszban](#deploywadarm) diagnosztika egy meglévő fürt beállításához.</span><span class="sxs-lookup"><span data-stu-id="5d92f-194">To update Diagnostics to collect logs from new EventSource channels that represent a new application that you're about to deploy, perform the same steps as in the [previous section](#deploywadarm) for setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="5d92f-195">Frissítse a `EtwEventSourceProviderConfiguration` az template.json fájl bejegyzés hozzáadása előtt, a konfiguráció új EventSource csatornák használatával módosítsa a `New-AzureRmResourceGroupDeployment` PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="5d92f-195">Update the `EtwEventSourceProviderConfiguration` section in the template.json file to add entries for the new EventSource channels before you apply the configuration update by using the `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="5d92f-196">A forrás nevét a kódot, a Visual Studio által létrehozott ServiceEventSource.cs fájl részeként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="5d92f-196">The name of the event source is defined as part of your code in the Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="5d92f-197">Ha a forrás saját Eventsource neve, adja hozzá például a következő kódot a saját Eventsource eseményei helyezzen MyDestinationTableName nevű tábla.</span><span class="sxs-lookup"><span data-stu-id="5d92f-197">For example, if your event source is named My-Eventsource, add the following code to place the events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="5d92f-198">Gyűjti az teljesítményszámlálók és az eseménynaplók, módosítsa a Resource Manager-sablon a megadott példák felhasználásával [figyelése és diagnosztika Windows virtuális gép létrehozása Azure Resource Manager-sablon használatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5d92f-198">To collect performance counters or event logs, modify the Resource Manager template by using the examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="5d92f-199">A Resource Manager-sablon, majd közzé.</span><span class="sxs-lookup"><span data-stu-id="5d92f-199">Then, republish the Resource Manager template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d92f-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d92f-200">Next steps</span></span>
<span data-ttu-id="5d92f-201">Milyen eseményeket kell keresnie az problémák elhárítása során részletes ismertetése: az a kibocsátott diagnosztikai eseményei [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) és [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="5d92f-201">To understand in more detail what events you should look for while troubleshooting issues, see the diagnostic events emitted for [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) and [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="5d92f-202">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="5d92f-202">Related articles</span></span>
* [<span data-ttu-id="5d92f-203">Megtudhatja, hogyan gyűjti a teljesítményszámlálók és a naplókat a diagnosztika bővítmény segítségével</span><span class="sxs-lookup"><span data-stu-id="5d92f-203">Learn how to collect performance counters or logs by using the Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="5d92f-204">A Naplóelemzési Service Fabric-megoldás</span><span class="sxs-lookup"><span data-stu-id="5d92f-204">Service Fabric solution in Log Analytics</span></span>](../log-analytics/log-analytics-service-fabric.md)

