---
title: "Azure Diagnostics használatával aaaCollect naplók |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan naplózza az Azure Diagnostics toocollect be tooset az Azure-ban futó Service Fabric-fürt."
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
ms.openlocfilehash: afbcfbe972b1847ef33bf0539b4398794b1bd56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="9aeb6-103">Naplók gyűjtése az Azure Diagnostics használatával</span><span class="sxs-lookup"><span data-stu-id="9aeb6-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9aeb6-104">Windows</span><span class="sxs-lookup"><span data-stu-id="9aeb6-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="9aeb6-105">Linux</span><span class="sxs-lookup"><span data-stu-id="9aeb6-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="9aeb6-106">Az Azure Service Fabric-fürtöt használ, esetén a jó ötlet toocollect hello jelentkezik minden hello csomópontról egy központi helyen.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-106">When you're running an Azure Service Fabric cluster, it's a good idea toocollect hello logs from all hello nodes in a central location.</span></span> <span data-ttu-id="9aeb6-107">Hello naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy az adott fürtben található futó hello alkalmazások és szolgáltatások a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-107">Having hello logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in hello applications and services running in that cluster.</span></span>

<span data-ttu-id="9aeb6-108">Egyirányú tooupload és a gyűjtés naplók toouse hello Azure Diagnostics bővítményt, mely feltöltések tooAzure tároló-, Azure Application Insights, és az Azure Event Hubs naplózza.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-108">One way tooupload and collect logs is toouse hello Azure Diagnostics extension, which uploads logs tooAzure Storage, Azure Application Insights, or Azure Event Hubs.</span></span> <span data-ttu-id="9aeb6-109">hello naplók hasznosak nem, amely közvetlenül a tároló, vagy az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-109">hello logs are not that useful directly in storage or in Event Hubs.</span></span> <span data-ttu-id="9aeb6-110">Azonban egy külső folyamat tooread hello események tárolóból, és helyezze el őket a termék például [Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-110">But you can use an external process tooread hello events from storage and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="9aeb6-111">[Az Azure Application Insights](https://azure.microsoft.com/services/application-insights/) egy átfogó naplózási keresési és analytics szolgáltatás beépített tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9aeb6-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9aeb6-112">Prerequisites</span></span>
<span data-ttu-id="9aeb6-113">Ezen eszközök tooperform használja a jelen dokumentum hello műveleteket:</span><span class="sxs-lookup"><span data-stu-id="9aeb6-113">You use these tools tooperform some of hello operations in this document:</span></span>

* <span data-ttu-id="9aeb6-114">[Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (tooAzure Felhőszolgáltatások azonban rendelkezik jó útmutatást és példákat kapcsolatos)</span><span class="sxs-lookup"><span data-stu-id="9aeb6-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related tooAzure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="9aeb6-115">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9aeb6-115">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="9aeb6-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9aeb6-116">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="9aeb6-117">Az Azure Resource Manager-ügyfél</span><span class="sxs-lookup"><span data-stu-id="9aeb6-117">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="9aeb6-118">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9aeb6-118">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-toocollect"></a><span data-ttu-id="9aeb6-119">Napló adatforrások, hogy érdemes-e toocollect</span><span class="sxs-lookup"><span data-stu-id="9aeb6-119">Log sources that you might want toocollect</span></span>
* <span data-ttu-id="9aeb6-120">**A Service Fabric naplók**: hello platform toostandard esemény Windows (nyomkövetés) és az EventSource csatornák által kibocsátott.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-120">**Service Fabric logs**: Emitted from hello platform toostandard Event Tracing for Windows (ETW) and EventSource channels.</span></span> <span data-ttu-id="9aeb6-121">Naplók több típusok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="9aeb6-121">Logs can be one of several types:</span></span>
  * <span data-ttu-id="9aeb6-122">A működési események: hello platform hajtja végre a Service Fabric-műveletek naplókat.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-122">Operational events: Logs for operations that hello Service Fabric platform performs.</span></span> <span data-ttu-id="9aeb6-123">Például alkalmazások és a szolgáltatások, a csomópont állapotváltozások és a frissítési információkat létrehozását.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-123">Examples include creation of applications and services, node state changes, and upgrade information.</span></span>
  * [<span data-ttu-id="9aeb6-124">Reliable Actors programozási modell események</span><span class="sxs-lookup"><span data-stu-id="9aeb6-124">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="9aeb6-125">Megbízható Services-programozási modell események</span><span class="sxs-lookup"><span data-stu-id="9aeb6-125">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)
* <span data-ttu-id="9aeb6-126">**Alkalmazásesemények**: a szolgáltatás kódból kibocsátott, és a hello EventSource segítőosztály megadott hello Visual Studio sablonok használatával írt események.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-126">**Application events**: Events emitted from your service's code and written out by using hello EventSource helper class provided in hello Visual Studio templates.</span></span> <span data-ttu-id="9aeb6-127">Hogyan toowrite naplózza az alkalmazásról további információkért lásd: [figyelő és diagnosztizálhatja a helyi számítógép fejlesztési telepítő szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-127">For more information on how toowrite logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-hello-diagnostics-extension"></a><span data-ttu-id="9aeb6-128">Hello diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="9aeb6-128">Deploy hello Diagnostics extension</span></span>
<span data-ttu-id="9aeb6-129">hello első lépése a naplók gyűjtésére toodeploy hello diagnosztika bővítmény minden egyes hello Service Fabric-fürt hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-129">hello first step in collecting logs is toodeploy hello Diagnostics extension on each of hello VMs in hello Service Fabric cluster.</span></span> <span data-ttu-id="9aeb6-130">hello diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti megadott toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-130">hello Diagnostics extension collects logs on each VM and uploads them toohello storage account that you specify.</span></span> <span data-ttu-id="9aeb6-131">hogy használ-e hello Azure-portálon vagy az Azure Resource Manager kissé függően változhat a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-131">hello steps vary a little based on whether you use hello Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="9aeb6-132">hello lépéseket is e hello telepítés része a fürt létrehozása, vagy egy már létező fürt függően változhat.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-132">hello steps also vary based on whether hello deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="9aeb6-133">Nézzük hello lépéseket az egyes forgatókönyvek esetében.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-133">Let's look at hello steps for each scenario.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-hello-portal"></a><span data-ttu-id="9aeb6-134">Hello portálon keresztül fürt létrehozásának részeként hello diagnosztika bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="9aeb6-134">Deploy hello Diagnostics extension as part of cluster creation through hello portal</span></span>
<span data-ttu-id="9aeb6-135">toodeploy hello diagnosztika bővítmény toohello virtuális gépek hello fürt fürt létrehozásának részeként, hello diagnosztikai beállítások panelen látható a következő kép hello használja.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-135">toodeploy hello Diagnostics extension toohello VMs in hello cluster as part of cluster creation, you use hello Diagnostics settings panel shown in hello following image.</span></span> <span data-ttu-id="9aeb6-136">tooenable Reliable Actors vagy Reliable Services eseménygyűjtés, győződjön meg arról, hogy túl van-e állítva a diagnosztika**a** (hello az alapértelmezett beállítás).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-136">tooenable Reliable Actors or Reliable Services event collection, ensure that Diagnostics is set too**On** (hello default setting).</span></span> <span data-ttu-id="9aeb6-137">Hello fürt létrehozása után ezeket a beállításokat nem módosíthatja hello portál használatával.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-137">After you create hello cluster, you can't change these settings by using hello portal.</span></span>

![A fürt létrehozásához hello portálon az Azure diagnosztikai beállításai](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

<span data-ttu-id="9aeb6-139">az Azure támogatási csapata hello *szükséges* támogatási naplózza toohelp resolve bármely Ön által létrehozott támogatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-139">hello Azure support team *requires* support logs toohelp resolve any support requests that you create.</span></span> <span data-ttu-id="9aeb6-140">Ezek a naplók valós időben gyűjti, és vannak tárolva egy hello storage-fiókok létrehozása hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-140">These logs are collected in real time and are stored in one of hello storage accounts created in hello resource group.</span></span> <span data-ttu-id="9aeb6-141">hello diagnosztikai beállítások alkalmazásszintű események beállítása.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-141">hello Diagnostics settings configure application-level events.</span></span> <span data-ttu-id="9aeb6-142">Ezek az események tartalmazzák [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) események, [Reliable Services](service-fabric-reliable-services-diagnostics.md) eseményeket, és néhány rendszerszintű Service Fabric események toobe Azure Storage szolgáltatásban tárolja.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-142">These events include [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) events, [Reliable Services](service-fabric-reliable-services-diagnostics.md) events, and some system-level Service Fabric events toobe stored in Azure Storage.</span></span>

<span data-ttu-id="9aeb6-143">Például termékek [Elasticsearch](https://www.elastic.co/guide/index.html) vagy a saját folyamatának hello események lekérheti hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-143">Products such as [Elasticsearch](https://www.elastic.co/guide/index.html) or your own process can get hello events from hello storage account.</span></span> <span data-ttu-id="9aeb6-144">Jelenleg nincs módja toofilter vagy karcsúsítása hello küldött eseményeket toohello tábla.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-144">There is currently no way toofilter or groom hello events that are sent toohello table.</span></span> <span data-ttu-id="9aeb6-145">Ha nem valósítja meg a folyamat tooremove események hello táblából, hello tábla toogrow továbbra is.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-145">If you don't implement a process tooremove events from hello table, hello table will continue toogrow.</span></span>

<span data-ttu-id="9aeb6-146">A fürt létrehozásakor hello portál használatával erősen ajánlott, hogy töltse le a hello sablon **OK gombra kattintás előtt** toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-146">When you're creating a cluster by using hello portal, we highly recommend that you download hello template **before you click OK** toocreate hello cluster.</span></span> <span data-ttu-id="9aeb6-147">Részletekért lásd a túl[Azure Resource Manager-sablonok használatával a Service Fabric-fürt beállított](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-147">For details, refer too[Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="9aeb6-148">Szüksége lesz hello sablon toomake módosítások később, mert néhány módosítást hello portál használatával nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-148">You'll need hello template toomake changes later, because you can't make some changes by using hello portal.</span></span>

<span data-ttu-id="9aeb6-149">Exportálhatja sablonok hello portálról hello lépések segítségével.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-149">You can export templates from hello portal by using hello following steps.</span></span> <span data-ttu-id="9aeb6-150">Ezeket a sablonokat lehet azonban nehezebb toouse, mert előfordulhat, hogy hiányoznak a szükséges információk null értékeket.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-150">However, these templates can be more difficult toouse because they might have null values that are missing required information.</span></span>

1. <span data-ttu-id="9aeb6-151">Nyissa meg az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-151">Open your resource group.</span></span>
2. <span data-ttu-id="9aeb6-152">Válassza ki **beállítások** toodisplay hello-beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-152">Select **Settings** toodisplay hello settings panel.</span></span>
3. <span data-ttu-id="9aeb6-153">Válassza ki **központi telepítések** toodisplay hello telepítési előzmények panelen.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-153">Select **Deployments** toodisplay hello deployment history panel.</span></span>
4. <span data-ttu-id="9aeb6-154">Válassza ki a hello központi telepítés központi telepítési toodisplay hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-154">Select a deployment toodisplay hello details of hello deployment.</span></span>
5. <span data-ttu-id="9aeb6-155">Válassza ki **sablon exportálása** toodisplay hello sablon panel.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-155">Select **Export Template** toodisplay hello template panel.</span></span>
6. <span data-ttu-id="9aeb6-156">Válassza ki **toofile mentése** tooexport hello sablon, a paraméter és a PowerShell-fájlokat tartalmazó .zip fájl.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-156">Select **Save toofile** tooexport a .zip file that contains hello template, parameter, and PowerShell files.</span></span>

<span data-ttu-id="9aeb6-157">Hello fájlok exportálását követően kell toomake módosítását.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-157">After you export hello files, you need toomake a modification.</span></span> <span data-ttu-id="9aeb6-158">Hello parameters.json fájl szerkesztése és eltávolítása hello **adminPassword** elemet.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-158">Edit hello parameters.json file and remove hello **adminPassword** element.</span></span> <span data-ttu-id="9aeb6-159">Ennek hatására a hello jelszó kérése, hello telepítési parancsfájl futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-159">This will cause a prompt for hello password when hello deployment script is run.</span></span> <span data-ttu-id="9aeb6-160">Amikor hello telepítési parancsfájlt futtatja, lehetséges, hogy toofix null paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-160">When you're running hello deployment script, you might have toofix null parameter values.</span></span>

<span data-ttu-id="9aeb6-161">toouse hello letöltött sablon tooupdate egy konfigurációt:</span><span class="sxs-lookup"><span data-stu-id="9aeb6-161">toouse hello downloaded template tooupdate a configuration:</span></span>

1. <span data-ttu-id="9aeb6-162">Bontsa ki a hello tartalma tooa mappát a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-162">Extract hello contents tooa folder on your local computer.</span></span>
2. <span data-ttu-id="9aeb6-163">Módosítsa a hello tartalma tooreflect hello új konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-163">Modify hello contents tooreflect hello new configuration.</span></span>
3. <span data-ttu-id="9aeb6-164">Indítsa el a Powershellt, és módosítsa a toohello mappát, amelyikbe kibontotta hello tartalmat.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-164">Start PowerShell and change toohello folder where you extracted hello content.</span></span>
4. <span data-ttu-id="9aeb6-165">Futtatás **deploy.ps1** , és töltse ki hello előfizetés-azonosító, hello erőforráscsoport-név (használata hello azonos tooupdate hello konfigurációja), és egyedi telepítési nevét.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-165">Run **deploy.ps1** and fill in hello subscription ID, hello resource group name (use hello same name tooupdate hello configuration), and a unique deployment name.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="9aeb6-166">Fürt létrehozásának részeként hello diagnosztika bővítmény telepítése Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="9aeb6-166">Deploy hello Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="9aeb6-167">egy fürt erőforrás-kezelő használatával toocreate, kell tooadd hello diagnosztika konfigurációs JSON toohello teljes fürt Resource Manager sablon hello fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-167">toocreate a cluster by using Resource Manager, you need tooadd hello Diagnostics configuration JSON toohello full cluster Resource Manager template before you create hello cluster.</span></span> <span data-ttu-id="9aeb6-168">Egy minta öt-Virtuálisgép-fürt Resource Manager sablon nyújtunk hozzá diagnosztikai konfiguráció tooit a Resource Manager sablon minták részeként.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-168">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added tooit as part of our Resource Manager template samples.</span></span> <span data-ttu-id="9aeb6-169">Ezen a helyen hello Azure-minták katalógusban látható: [öt csomópontból álló fürt diagnosztika Resource Manager sablon példával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-169">You can see it at this location in hello Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span></span>

<span data-ttu-id="9aeb6-170">toosee hello diagnosztika beállítás hello Resource Manager-sablon, nyitott hello azuredeploy.json fájlt, és keresse meg a **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-170">toosee hello Diagnostics setting in hello Resource Manager template, open hello azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="9aeb6-171">a fürt ezt a sablont, jelölje be hello segítségével toocreate **tooAzure telepítése** gomb hello előző hivatkozás érhető el.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-171">toocreate a cluster by using this template, select hello **Deploy tooAzure** button available at hello previous link.</span></span>

<span data-ttu-id="9aeb6-172">Azt is megteheti, hello erőforrás-kezelő minta letöltése, ellenőrizze a módosítások tooit és hello segítségével hozzon létre egy fürtöt hello módosított sablon `New-AzureRmResourceGroupDeployment` parancsot egy Azure PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-172">Alternatively, you can download hello Resource Manager sample, make changes tooit, and create a cluster with hello modified template by using hello `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="9aeb6-173">Tekintse meg a következő kód hello paraméter jelzi, hogy átadta toohello parancsban hello.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-173">See hello following code for hello parameters that you pass in toohello command.</span></span> <span data-ttu-id="9aeb6-174">Hogyan toodeploy erőforrás szerint kell csoportosítani a PowerShell használatával további információkért lásd: hello cikk [hello Azure Resource Manager sablonnal erőforráscsoport telepítése](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-174">For detailed information on how toodeploy a resource group by using PowerShell, see hello article [Deploy a resource group with hello Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a><span data-ttu-id="9aeb6-175">Hello diagnosztika bővítmény tooan meglévő fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="9aeb6-175">Deploy hello Diagnostics extension tooan existing cluster</span></span>
<span data-ttu-id="9aeb6-176">Ha egy meglévő fürtöt, amely nem rendelkezik telepített diagnosztika, vagy ha azt szeretné, hogy egy meglévő konfigurációt toomodify, adja hozzá, vagy frissítheti azt.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-176">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want toomodify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="9aeb6-177">Hello Resource Manager-sablon által használt toocreate hello meglévő fürt módosítása vagy hello sablon letöltése hello portálról korábban leírt módon.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-177">Modify hello Resource Manager template that's used toocreate hello existing cluster or download hello template from hello portal as described earlier.</span></span> <span data-ttu-id="9aeb6-178">Módosítsa a hello template.json fájlt hello a következő feladatok végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-178">Modify hello template.json file by performing hello following tasks.</span></span>

<span data-ttu-id="9aeb6-179">Vegyen fel egy új tároló-erőforrás toohello sablont toohello források szakaszában hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-179">Add a new storage resource toohello template by adding toohello resources section.</span></span>

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

 <span data-ttu-id="9aeb6-180">Ezután adja hozzá a toohello paraméterek között szakasz után hello tárolási fiók definíciók `supportLogStorageAccountName` és `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-180">Next, add toohello parameters section just after hello storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="9aeb6-181">Cserélje le a helyőrzőket hello *ide kerül a tárfiók neve* hello nevű hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-181">Replace hello placeholder text *storage account name goes here* with hello name of hello storage account.</span></span>

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
<span data-ttu-id="9aeb6-182">Frissítse a hello `VirtualMachineProfile` hello template.json fájl a következő kód hello bővítmények tömbön belüli hello hozzáadásával szakaszában.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-182">Then, update hello `VirtualMachineProfile` section of hello template.json file by adding hello following code within hello extensions array.</span></span> <span data-ttu-id="9aeb6-183">Lehet, hogy tooadd hello elején vagy hello végén, attól függően, hogy hol csatlakoztatva van egy vesszővel.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-183">Be sure tooadd a comma at hello beginning or hello end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="9aeb6-184">Hello template.json fájl leírtak módosítása után közzé hello Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-184">After you modify hello template.json file as described, republish hello Resource Manager template.</span></span> <span data-ttu-id="9aeb6-185">Hello sablon exportált, ha a fájl futtatása hello deploy.ps1 addig hello sablont.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-185">If hello template was exported, running hello deploy.ps1 file republishes hello template.</span></span> <span data-ttu-id="9aeb6-186">Miután telepít, győződjön meg arról, hogy **ProvisioningState** van **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-186">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="update-diagnostics-toocollect-health-and-load-events"></a><span data-ttu-id="9aeb6-187">Diagnosztika toocollect állapotát és a betöltési események frissítése</span><span class="sxs-lookup"><span data-stu-id="9aeb6-187">Update diagnostics toocollect health and load events</span></span>

<span data-ttu-id="9aeb6-188">Hello 5.4-es verziójában a Service Fabric-től kezdődően állapotát és a terheléselosztási metrika események állnak rendelkezésre a gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-188">Starting with hello 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="9aeb6-189">Ezek az események tükrözze hello állapotfigyelő segítségével hello rendszer vagy a kód által előállított eseményeket vagy nem tölthető be, mint jelentéskészítési API-k [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) vagy [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-189">These events reflect events generated by hello system or your code by using hello health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="9aeb6-190">Ez lehetővé teszi, hogy összesítésére és idővel állapotának megtekintése és a riasztás állapotát vagy a betöltési események alapján.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-190">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="9aeb6-191">Ezek az események a Visual Studio diagnosztikai eseménynapló hozzáadása tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello ETW-hitelesítésszolgáltatók listáját.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-191">tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" toohello list of ETW providers.</span></span>

<span data-ttu-id="9aeb6-192">toocollect hello események, hello resource manager sablon tooinclude módosítása</span><span class="sxs-lookup"><span data-stu-id="9aeb6-192">toocollect hello events, modify hello resource manager template tooinclude</span></span>

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

## <a name="update-diagnostics-toocollect-and-upload-logs-from-new-eventsource-channels"></a><span data-ttu-id="9aeb6-193">Diagnosztika toocollect frissítése és a naplók feltöltése az új EventSource csatornák</span><span class="sxs-lookup"><span data-stu-id="9aeb6-193">Update Diagnostics toocollect and upload logs from new EventSource channels</span></span>
<span data-ttu-id="9aeb6-194">tooupdate diagnosztika toocollect naplókat az új EventSource csatornák, amelyek megfelelnek egy új alkalmazást, amely körülbelül toodeploy, hajtsa végre a ugyanaz, mint hello lépések hello [előző szakaszban](#deploywadarm) ad egy meglévő diagnosztikai telepítéshez fürt.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-194">tooupdate Diagnostics toocollect logs from new EventSource channels that represent a new application that you're about toodeploy, perform hello same steps as in hello [previous section](#deploywadarm) for setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="9aeb6-195">Hello frissítése `EtwEventSourceProviderConfiguration` hello új EventSource csatornák hello konfiguráció alkalmazása előtt módosítsa hello segítségével hello template.json tooadd bejegyzések szakasz `New-AzureRmResourceGroupDeployment` PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-195">Update hello `EtwEventSourceProviderConfiguration` section in hello template.json file tooadd entries for hello new EventSource channels before you apply hello configuration update by using hello `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="9aeb6-196">hello eseményforrás hello nevét a kód hello ServiceEventSource.cs Visual Studio által létrehozott fájl részeként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-196">hello name of hello event source is defined as part of your code in hello Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="9aeb6-197">Ha a forrás saját Eventsource neve, például saját Eventsource kód tooplace hello események követően egy táblába MyDestinationTableName nevű hello hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-197">For example, if your event source is named My-Eventsource, add hello following code tooplace hello events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="9aeb6-198">toocollect teljesítményszámlálók vagy eseménynaplók, hello Resource Manager sablon módosításához megadott hello példák felhasználásával [Windows virtuális gép létrehozása a figyelési és diagnosztikaAzureResourceManager-sablonhasználatával](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-198">toocollect performance counters or event logs, modify hello Resource Manager template by using hello examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="9aeb6-199">Újbóli hello Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="9aeb6-199">Then, republish hello Resource Manager template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9aeb6-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9aeb6-200">Next steps</span></span>
<span data-ttu-id="9aeb6-201">részletesebben toounderstand milyen eseményeket kell keresnie az közben hibaelhárításával, lásd: a kibocsátott diagnosztikai eseményei hello [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) és [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="9aeb6-201">toounderstand in more detail what events you should look for while troubleshooting issues, see hello diagnostic events emitted for [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) and [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="9aeb6-202">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="9aeb6-202">Related articles</span></span>
* [<span data-ttu-id="9aeb6-203">Ismerje meg, hogyan toocollect teljesítményszámlálókkal és a naplók segítségével hello diagnosztika bővítmény</span><span class="sxs-lookup"><span data-stu-id="9aeb6-203">Learn how toocollect performance counters or logs by using hello Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="9aeb6-204">A Naplóelemzési Service Fabric-megoldás</span><span class="sxs-lookup"><span data-stu-id="9aeb6-204">Service Fabric solution in Log Analytics</span></span>](../log-analytics/log-analytics-service-fabric.md)

