---
title: "az Azure PowerShell használatába aaaGet |} Microsoft Docs"
description: "Gyors bevezetés toohello toomanage kötegelt erőforrások használható Azure PowerShell-parancsmagokat."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e4d12e9c1e52a5b2db2dd44346edda93b7ef92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a><span data-ttu-id="dda53-103">Batch-erőforrások kezelése PowerShell-parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="dda53-103">Manage Batch resources with PowerShell cmdlets</span></span>

<span data-ttu-id="dda53-104">Az Azure Batch PowerShell-parancsmagok hello, végrehajtása és a parancsfájl-hello számos ugyanazokhoz a feladatokhoz a kötegelt API-k, hello hajthat végre hello Azure-portálon, és hello Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="dda53-104">With hello Azure Batch PowerShell cmdlets, you can perform and script many of hello same tasks you carry out with hello Batch APIs, hello Azure portal, and hello Azure Command-Line Interface (CLI).</span></span> <span data-ttu-id="dda53-105">Ez az egy gyors bevezetés toohello parancsmagokat használhatja toomanage a Batch-fiókok és a a kötegelt erőforrásokat, például a készletek, a feladatok és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="dda53-105">This is a quick introduction toohello cmdlets you can use toomanage your Batch accounts and work with your Batch resources such as pools, jobs, and tasks.</span></span>

<span data-ttu-id="dda53-106">Kötegelt parancsmagok és részletes parancsmag szintaxisát teljes listáját lásd: hello [Azure Batch parancsmag-referencia](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="dda53-106">For a complete list of Batch cmdlets and detailed cmdlet syntax, see hello [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>

<span data-ttu-id="dda53-107">Ez a cikk az Azure PowerShell 3.0.0-s verziójának parancsmagjain alapul.</span><span class="sxs-lookup"><span data-stu-id="dda53-107">This article is based on cmdlets in Azure PowerShell version 3.0.0.</span></span> <span data-ttu-id="dda53-108">Azt javasoljuk, hogy a frissíteni az Azure PowerShell gyakran tootake előnyeit szolgáltatás frissítéseket és fejlesztéseket.</span><span class="sxs-lookup"><span data-stu-id="dda53-108">We recommend that you update your Azure PowerShell frequently tootake advantage of service updates and enhancements.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dda53-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dda53-109">Prerequisites</span></span>
<span data-ttu-id="dda53-110">Hajtsa végre a következő műveletek toouse Azure PowerShell toomanage hello a kötegelt erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="dda53-110">Perform hello following operations toouse Azure PowerShell toomanage your Batch resources.</span></span>

* [<span data-ttu-id="dda53-111">Telepítse és konfigurálja az Azure PowerShellt</span><span class="sxs-lookup"><span data-stu-id="dda53-111">Install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* <span data-ttu-id="dda53-112">Futtassa a hello **Login-AzureRmAccount** parancsmag tooconnect tooyour előfizetés (hello Azure Batch parancsmagok szállítási hello Azure Resource Manager modul):</span><span class="sxs-lookup"><span data-stu-id="dda53-112">Run hello **Login-AzureRmAccount** cmdlet tooconnect tooyour subscription (hello Azure Batch cmdlets ship in hello Azure Resource Manager module):</span></span>
  
    `Login-AzureRmAccount`
* <span data-ttu-id="dda53-113">**Hello kötegelt szolgáltatójának névtere regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="dda53-113">**Register with hello Batch provider namespace**.</span></span> <span data-ttu-id="dda53-114">Ez a művelet csak kell végrehajtani toobe **előfizetésenként egyszer**.</span><span class="sxs-lookup"><span data-stu-id="dda53-114">This operation only needs toobe performed **once per subscription**.</span></span>
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a><span data-ttu-id="dda53-115">Batch-fiókok és kulcsok felügyelete</span><span class="sxs-lookup"><span data-stu-id="dda53-115">Manage Batch accounts and keys</span></span>
### <a name="create-a-batch-account"></a><span data-ttu-id="dda53-116">Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="dda53-116">Create a Batch account</span></span>
<span data-ttu-id="dda53-117">A **New-AzureRmBatchAccount** parancs egy Batch-fiókot hoz létre a meghatározott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="dda53-117">**New-AzureRmBatchAccount** creates a Batch account in a specified resource group.</span></span> <span data-ttu-id="dda53-118">Ha még nem rendelkezik egy erőforráscsoport, hozzon létre egyet hello futtatásával [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="dda53-118">If you don't already have a resource group, create one by running hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span> <span data-ttu-id="dda53-119">Adjon meg egy hello Azure régiói hello **hely** paraméter, például az "USA középső RÉGIÓJA".</span><span class="sxs-lookup"><span data-stu-id="dda53-119">Specify one of hello Azure regions in hello **Location** parameter, such as "Central US".</span></span> <span data-ttu-id="dda53-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="dda53-120">For example:</span></span>

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

<span data-ttu-id="dda53-121">Ezt követően hello erőforráscsoportban hello fióknak a nevét adja meg a Batch-fiók létrehozása <*fióknév*> és hello helyét, és az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="dda53-121">Then, create a Batch account in hello resource group, specifying a name for hello account in <*account_name*> and hello location and name of your resource group.</span></span> <span data-ttu-id="dda53-122">Hello Batch-fiók létrehozása eltarthat néhány alkalommal toocomplete.</span><span class="sxs-lookup"><span data-stu-id="dda53-122">Creating hello Batch account can take some time toocomplete.</span></span> <span data-ttu-id="dda53-123">Példa:</span><span class="sxs-lookup"><span data-stu-id="dda53-123">For example:</span></span>

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> <span data-ttu-id="dda53-124">hello Batch-fiók nevének kell lennie az Azure-régió hello erőforráscsoport, egyedi toohello csak 3 és 24 karakter közötti lehet, és kisbetűket és számokat tartalmazhat csak.</span><span class="sxs-lookup"><span data-stu-id="dda53-124">hello Batch account name must be unique toohello Azure region for hello resource group, contain between 3 and 24 characters, and use lowercase letters and numbers only.</span></span>
> 
> 

### <a name="get-account-access-keys"></a><span data-ttu-id="dda53-125">Fiók hívóbetűinek beszerzése</span><span class="sxs-lookup"><span data-stu-id="dda53-125">Get account access keys</span></span>
<span data-ttu-id="dda53-126">**Get-AzureRmBatchAccountKeys** Azure Batch-fiók társított hello hívóbetűk jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="dda53-126">**Get-AzureRmBatchAccountKeys** shows hello access keys associated with an Azure Batch account.</span></span> <span data-ttu-id="dda53-127">Például futtassa a következő tooget hello elsődleges és másodlagos létrehozott hello fiókjának kulcsok hello.</span><span class="sxs-lookup"><span data-stu-id="dda53-127">For example, run hello following tooget hello primary and secondary keys of hello account you created.</span></span>

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a><span data-ttu-id="dda53-128">Új hívóbetű létrehozása</span><span class="sxs-lookup"><span data-stu-id="dda53-128">Generate a new access key</span></span>
<span data-ttu-id="dda53-129">A **New-AzureRmBatchAccountKey** új elsődleges vagy másodlagos hívóbetűt hoz létre az Azure Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="dda53-129">**New-AzureRmBatchAccountKey** generates a new primary or secondary account key for an Azure Batch account.</span></span> <span data-ttu-id="dda53-130">Például toogenerate egy új elsődleges kulcsot a Batch-fiókhoz, típus:</span><span class="sxs-lookup"><span data-stu-id="dda53-130">For example, toogenerate a new primary key for your Batch account, type:</span></span>

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> <span data-ttu-id="dda53-131">egy új másodlagos kulcs toogenerate "Másodlagos" megadása hello **KeyType** paraméter.</span><span class="sxs-lookup"><span data-stu-id="dda53-131">toogenerate a new secondary key, specify "Secondary" for hello **KeyType** parameter.</span></span> <span data-ttu-id="dda53-132">Külön-külön kell tooregenerate hello elsődleges és másodlagos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="dda53-132">You have tooregenerate hello primary and secondary keys separately.</span></span>
> 
> 

### <a name="delete-a-batch-account"></a><span data-ttu-id="dda53-133">Batch-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="dda53-133">Delete a Batch account</span></span>
<span data-ttu-id="dda53-134">A **Remove-AzureRmBatchAccount** törli a Batch-fiókot.</span><span class="sxs-lookup"><span data-stu-id="dda53-134">**Remove-AzureRmBatchAccount** deletes a Batch account.</span></span> <span data-ttu-id="dda53-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="dda53-135">For example:</span></span>

    Remove-AzureRmBatchAccount -AccountName <account_name>

<span data-ttu-id="dda53-136">Amikor a rendszer kéri, erősítse meg tooremove hello fiók.</span><span class="sxs-lookup"><span data-stu-id="dda53-136">When prompted, confirm you want tooremove hello account.</span></span> <span data-ttu-id="dda53-137">Fiók eltávolításának is igénybe vehet néhány alkalommal toocomplete.</span><span class="sxs-lookup"><span data-stu-id="dda53-137">Account removal can take some time toocomplete.</span></span>

## <a name="create-a-batchaccountcontext-object"></a><span data-ttu-id="dda53-138">BatchAccountContext objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="dda53-138">Create a BatchAccountContext object</span></span>
<span data-ttu-id="dda53-139">Kötegelt PowerShell-parancsmagok használatával tooauthenticate hello, létrehozása és kezelése a Batch-készletek, feladatok, feladatok, és más erőforrások, először létre kell hoznia egy BatchAccountContext objektum toostore a fiók nevét és a kulcsok:</span><span class="sxs-lookup"><span data-stu-id="dda53-139">tooauthenticate using hello Batch PowerShell cmdlets when you create and manage Batch pools, jobs, tasks, and other resources, first create a BatchAccountContext object toostore your account name and keys:</span></span>

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

<span data-ttu-id="dda53-140">Át hello BatchAccountContext objektum parancsmagok be, hogy használjon hello **BatchContext** paraméter.</span><span class="sxs-lookup"><span data-stu-id="dda53-140">You pass hello BatchAccountContext object into cmdlets that use hello **BatchContext** parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="dda53-141">Alapértelmezés szerint hello fiók elsődleges kulcsot a hitelesítéshez használja, de hello kulcs toouse explicit módon kiválaszthatja a BatchAccountContext objektum módosításával **KeyInUse** tulajdonság: `$context.KeyInUse = "Secondary"`.</span><span class="sxs-lookup"><span data-stu-id="dda53-141">By default, hello account's primary key is used for authentication, but you can explicitly select hello key toouse by changing your BatchAccountContext object’s **KeyInUse** property: `$context.KeyInUse = "Secondary"`.</span></span>
> 
> 

## <a name="create-and-modify-batch-resources"></a><span data-ttu-id="dda53-142">Batch-erőforrások létrehozása és módosítása</span><span class="sxs-lookup"><span data-stu-id="dda53-142">Create and modify Batch resources</span></span>
<span data-ttu-id="dda53-143">Parancsmagokat használja, mint **New-AzureBatchPool**, **New-AzureBatchJob**, és **New-AzureBatchTask** toocreate erőforrások a Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="dda53-143">Use cmdlets such as **New-AzureBatchPool**, **New-AzureBatchJob**, and **New-AzureBatchTask** toocreate resources under a Batch account.</span></span> <span data-ttu-id="dda53-144">Megfelelő **Get -** és **Set -** parancsmagok tooupdate hello tulajdonságainak meglévő erőforrásokat, és **Remove -** parancsmagok tooremove erőforrások a Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="dda53-144">There are corresponding **Get-** and **Set-** cmdlets tooupdate hello properties of existing resources, and  **Remove-** cmdlets tooremove resources under a Batch account.</span></span>

<span data-ttu-id="dda53-145">Ha ezek a parancsmagok számos a hozzáadása toopassing egy BatchContext objektum, meg kell toocreate, vagy adja át az objektumokat, amelyek részletes erőforrás-beállítások, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="dda53-145">When using many of these cmdlets, in addition toopassing a BatchContext object, you need toocreate or pass objects that contain detailed resource settings, as shown in hello following example.</span></span> <span data-ttu-id="dda53-146">Lásd: hello részletes minden további példákat a parancsmag súgóját.</span><span class="sxs-lookup"><span data-stu-id="dda53-146">See hello detailed help for each cmdlet for additional examples.</span></span>

### <a name="create-a-batch-pool"></a><span data-ttu-id="dda53-147">Batch-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="dda53-147">Create a Batch pool</span></span>
<span data-ttu-id="dda53-148">Létrehozásakor vagy frissítésekor. a Batch-készlet, akkor válasszon ki hello felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció hello hello operációs rendszer hello a számítási csomópontok (lásd: [Batch funkcióinak áttekintése](batch-api-basics.md#pool)).</span><span class="sxs-lookup"><span data-stu-id="dda53-148">When creating or updating a Batch pool, you select either hello cloud service configuration or hello virtual machine configuration for hello operating system on hello compute nodes (see [Batch feature overview](batch-api-basics.md#pool)).</span></span> <span data-ttu-id="dda53-149">Hello felhőalapú szolgáltatás konfigurációja adja meg, ha a számítási csomópontok telepíti egy hello [Azure vendég operációs rendszer feloldja a](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span><span class="sxs-lookup"><span data-stu-id="dda53-149">If you specify hello cloud service configuration, your compute nodes are imaged with one of hello [Azure Guest OS releases](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span></span> <span data-ttu-id="dda53-150">Ha hello virtuálisgép-konfiguráció ad meg, vagy megadhatja hello egyik támogatott Linux vagy a Windows virtuális gép lemezképek szerepel az hello [Azure virtuális gépek piactér][vm_marketplace], vagy adjon meg egy egyéni előkészítette a kép.</span><span class="sxs-lookup"><span data-stu-id="dda53-150">If you specify hello virtual machine configuration, you can either specify one of hello supported Linux or Windows VM images listed in hello [Azure Virtual Machines Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span>

<span data-ttu-id="dda53-151">Amikor futtatja **New-AzureBatchPool**, adjon át egy PSCloudServiceConfiguration vagy PSVirtualMachineConfiguration objektum hello operációsrendszer-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="dda53-151">When you run **New-AzureBatchPool**, pass hello operating system settings in a PSCloudServiceConfiguration or PSVirtualMachineConfiguration object.</span></span> <span data-ttu-id="dda53-152">Például hello következő parancsmag hoz létre egy új Batch-készlet mérete kis számítási csomópontjai hello felhőalapú szolgáltatás konfigurációja, telepíti a legújabb operációs rendszer verziójával hello termékcsalád 3 (Windows Server 2012).</span><span class="sxs-lookup"><span data-stu-id="dda53-152">For example, hello following cmdlet creates a new Batch pool with size Small compute nodes in hello cloud service configuration, imaged with hello latest operating system version of family 3 (Windows Server 2012).</span></span> <span data-ttu-id="dda53-153">Itt hello **CloudServiceConfiguration** paraméterrel hello *$configuration* változó hello PSCloudServiceConfiguration objektumként.</span><span class="sxs-lookup"><span data-stu-id="dda53-153">Here, hello **CloudServiceConfiguration** parameter specifies hello *$configuration* variable as hello PSCloudServiceConfiguration object.</span></span> <span data-ttu-id="dda53-154">Hello **BatchContext** paraméter adja meg egy korábban definiált változó *$context* hello BatchAccountContext objektumként.</span><span class="sxs-lookup"><span data-stu-id="dda53-154">hello **BatchContext** parameter specifies a previously defined variable *$context* as hello BatchAccountContext object.</span></span>

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

<span data-ttu-id="dda53-155">az automatikus skálázás képlet hello új készlet számítási csomópontjai hello cél számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="dda53-155">hello target number of compute nodes in hello new pool is determined by an autoscaling formula.</span></span> <span data-ttu-id="dda53-156">Ebben az esetben hello képlete egyszerűen **$TargetDedicated = 4**, érték 4, legfeljebb hello hello készlet számítási csomópontjainak számát.</span><span class="sxs-lookup"><span data-stu-id="dda53-156">In this case, hello formula is simply **$TargetDedicated=4**, indicating hello number of compute nodes in hello pool is 4 at most.</span></span>

## <a name="query-for-pools-jobs-tasks-and-other-details"></a><span data-ttu-id="dda53-157">Készletek, feladatok, tevékenységek és egyéb részletek lekérdezése</span><span class="sxs-lookup"><span data-stu-id="dda53-157">Query for pools, jobs, tasks, and other details</span></span>
<span data-ttu-id="dda53-158">Használjon például parancsmagok **Get-AzureBatchPool**, **Get-AzureBatchJob**, és **Get-AzureBatchTask** tooquery entitások Batch-fiók alatt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="dda53-158">Use cmdlets such as **Get-AzureBatchPool**, **Get-AzureBatchJob**, and **Get-AzureBatchTask** tooquery for entities created under a Batch account.</span></span>

### <a name="query-for-data"></a><span data-ttu-id="dda53-159">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="dda53-159">Query for data</span></span>
<span data-ttu-id="dda53-160">Tegyük fel, használjon **Get-AzureBatchPools** toofind az készletek.</span><span class="sxs-lookup"><span data-stu-id="dda53-160">As an example, use **Get-AzureBatchPools** toofind your pools.</span></span> <span data-ttu-id="dda53-161">Ez a fiók alatt készletekben lekérdezi alapértelmezés szerint tárolja hello BatchAccountContext objektum már feltéve, hogy *$context*:</span><span class="sxs-lookup"><span data-stu-id="dda53-161">By default this queries for all pools under your account, assuming you already stored hello BatchAccountContext object in *$context*:</span></span>

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a><span data-ttu-id="dda53-162">OData-szűrő használata</span><span class="sxs-lookup"><span data-stu-id="dda53-162">Use an OData filter</span></span>
<span data-ttu-id="dda53-163">Megadhat egy OData-szűrő használatával hello **szűrő** paraméter toofind csak hello kíváncsiak vagyunk objektumok.</span><span class="sxs-lookup"><span data-stu-id="dda53-163">You can supply an OData filter using hello **Filter** parameter toofind only hello objects you’re interested in.</span></span> <span data-ttu-id="dda53-164">Megkeresheti például az összes olyan készletet, amelynek az azonosítója a „myPool” karakterlánccal kezdődik:</span><span class="sxs-lookup"><span data-stu-id="dda53-164">For example, you can find all pools with ids starting with “myPool”:</span></span>

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

<span data-ttu-id="dda53-165">Ez a módszer nem olyan rugalmas, mint a „Where-Object” használata a helyi folyamatokban.</span><span class="sxs-lookup"><span data-stu-id="dda53-165">This method is not as flexible as using “Where-Object” in a local pipeline.</span></span> <span data-ttu-id="dda53-166">Azonban hello lekérdezés küldi el toohello Batch szolgáltatás közvetlenül, hogy az összes szűrése történik, az internetes sávszélességet mentése hello kiszolgálóoldali.</span><span class="sxs-lookup"><span data-stu-id="dda53-166">However, hello query gets sent toohello Batch service directly so that all filtering happens on hello server side, saving Internet bandwidth.</span></span>

### <a name="use-hello-id-parameter"></a><span data-ttu-id="dda53-167">Hello Id paraméter használata</span><span class="sxs-lookup"><span data-stu-id="dda53-167">Use hello Id parameter</span></span>
<span data-ttu-id="dda53-168">Egy alternatív tooan OData szűrő toouse hello **azonosító** paraméter.</span><span class="sxs-lookup"><span data-stu-id="dda53-168">An alternative tooan OData filter is toouse hello **Id** parameter.</span></span> <span data-ttu-id="dda53-169">az azonosító "myPool" adott készlet tooquery:</span><span class="sxs-lookup"><span data-stu-id="dda53-169">tooquery for a specific pool with id "myPool":</span></span>

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

<span data-ttu-id="dda53-170">Hello **azonosító** paraméter csak teljes-id keresési, nem a helyettesítő karakterekkel vagy OData-stílusú szűrők támogatja.</span><span class="sxs-lookup"><span data-stu-id="dda53-170">hello **Id** parameter supports only full-id search, not wildcards or OData-style filters.</span></span>

### <a name="use-hello-maxcount-parameter"></a><span data-ttu-id="dda53-171">Hello MaxCount paraméter használata</span><span class="sxs-lookup"><span data-stu-id="dda53-171">Use hello MaxCount parameter</span></span>
<span data-ttu-id="dda53-172">Alapértelmezés szerint mindegyik parancsmag maximum 1000 objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="dda53-172">By default, each cmdlet returns a maximum of 1000 objects.</span></span> <span data-ttu-id="dda53-173">Ha eléri ezt a határt, pontosítsa a szűrő toobring vissza kevesebb objektumot, vagy beállítással, hello használatával legfeljebb **MaxCount** paraméter.</span><span class="sxs-lookup"><span data-stu-id="dda53-173">If you reach this limit, either refine your filter toobring back fewer objects, or explicitly set a maximum using hello **MaxCount** parameter.</span></span> <span data-ttu-id="dda53-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="dda53-174">For example:</span></span>

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

<span data-ttu-id="dda53-175">tooremove hello felső határa, állítsa be **MaxCount** too0 vagy kisebb.</span><span class="sxs-lookup"><span data-stu-id="dda53-175">tooremove hello upper bound, set **MaxCount** too0 or less.</span></span>

### <a name="use-hello-powershell-pipeline"></a><span data-ttu-id="dda53-176">Hello PowerShell kimenetátirányításának használata</span><span class="sxs-lookup"><span data-stu-id="dda53-176">Use hello PowerShell pipeline</span></span>
<span data-ttu-id="dda53-177">Kötegelt parancsmagok kihasználhatják a hello PowerShell adatcsatorna toosend adatok parancsmag között.</span><span class="sxs-lookup"><span data-stu-id="dda53-177">Batch cmdlets can leverage hello PowerShell pipeline toosend data between cmdlets.</span></span> <span data-ttu-id="dda53-178">Ugyanaz, mintha egy paraméter, de használata több entitás egyszerűbbé teszi érvényesíti hello azt.</span><span class="sxs-lookup"><span data-stu-id="dda53-178">This has hello same effect as specifying a parameter, but makes working with multiple entities easier.</span></span>

<span data-ttu-id="dda53-179">Például keresse meg és jelenítse meg fiókjában az összes feladatot:</span><span class="sxs-lookup"><span data-stu-id="dda53-179">For example, find and display all tasks under your account:</span></span>

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

<span data-ttu-id="dda53-180">Indítsa újra a készlet összes számítási csomópontját:</span><span class="sxs-lookup"><span data-stu-id="dda53-180">Restart (reboot) every compute node in a pool:</span></span>

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a><span data-ttu-id="dda53-181">Alkalmazáscsomagok kezelése</span><span class="sxs-lookup"><span data-stu-id="dda53-181">Application package management</span></span>
<span data-ttu-id="dda53-182">Alkalmazáscsomagok lehetőséget nyújtanak olyan egyszerűsített toodeploy alkalmazások toohello számítási csomópontjain a készletek.</span><span class="sxs-lookup"><span data-stu-id="dda53-182">Application packages provide a simplified way toodeploy applications toohello compute nodes in your pools.</span></span> <span data-ttu-id="dda53-183">A kötegelt PowerShell-parancsmagok hello feltöltése és alkalmazáscsomagok kezelése a Batch-fiókhoz, és telepítse csomag verziók toocompute csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="dda53-183">With hello Batch PowerShell cmdlets, you can upload and manage application packages in your Batch account, and deploy package versions toocompute nodes.</span></span>

<span data-ttu-id="dda53-184">Alkalmazás **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="dda53-184">**Create** an application:</span></span>

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

<span data-ttu-id="dda53-185">Alkalmazáscsomag **hozzáadása**:</span><span class="sxs-lookup"><span data-stu-id="dda53-185">**Add** an application package:</span></span>

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

<span data-ttu-id="dda53-186">Set hello **alapértelmezett verzió** hello alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="dda53-186">Set hello **default version** for hello application:</span></span>

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

<span data-ttu-id="dda53-187">Alkalmazás csomagjainak **listázása**</span><span class="sxs-lookup"><span data-stu-id="dda53-187">**List** an application's packages</span></span>

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

<span data-ttu-id="dda53-188">Alkalmazáscsomag **törlése**</span><span class="sxs-lookup"><span data-stu-id="dda53-188">**Delete** an application package</span></span>

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

<span data-ttu-id="dda53-189">Alkalmazás **törlése**</span><span class="sxs-lookup"><span data-stu-id="dda53-189">**Delete** an application</span></span>

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> <span data-ttu-id="dda53-190">Előtt törölnie kell minden olyan alkalmazás alkalmazáscsomag-verziók hello alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="dda53-190">You must delete all of an application's application package versions before you delete hello application.</span></span> <span data-ttu-id="dda53-191">Egy "Ütközés" hibaüzenetet fog kapni, ha egy alkalmazás jelenleg alkalmazáscsomagok toodelete kísérli meg.</span><span class="sxs-lookup"><span data-stu-id="dda53-191">You will receive a 'Conflict' error if you try toodelete an application that currently has application packages.</span></span>
> 
> 

### <a name="deploy-an-application-package"></a><span data-ttu-id="dda53-192">Alkalmazáscsomag üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="dda53-192">Deploy an application package</span></span>
<span data-ttu-id="dda53-193">Készlet létrehozásakor megadhat egy vagy több alkalmazáscsomagot az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="dda53-193">You can specify one or more application packages for deployment when you create a pool.</span></span> <span data-ttu-id="dda53-194">Olyan csomagot adjon meg a készlet létrehozás időpontjában, esetén telepített tooeach csomópont hello csomópont illesztések készlet szerint.</span><span class="sxs-lookup"><span data-stu-id="dda53-194">When you specify a package at pool creation time, it is deployed tooeach node as hello node joins pool.</span></span> <span data-ttu-id="dda53-195">A csomagok akkor is üzembe lesznek helyezve, ha a csomópontot újraindítják vagy alaphelyzetbe állítják.</span><span class="sxs-lookup"><span data-stu-id="dda53-195">Packages are also deployed when a node is rebooted or reimaged.</span></span>

<span data-ttu-id="dda53-196">Adja meg a hello `-ApplicationPackageReference` létrehozásakor egy alkalmazáskészlet toodeploy egy csomag toohello alkalmazáskészlet csomópontok, azok csatlakoztatását hello készlet lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="dda53-196">Specify hello `-ApplicationPackageReference` option when creating a pool toodeploy an application package toohello pool's nodes as they join hello pool.</span></span> <span data-ttu-id="dda53-197">Először hozzon létre egy **PSApplicationPackageReference** objektumot, és állítsa be úgy a hello csomagazonosító és csomagnév Alkalmazásverzió kívánt toodeploy toohello készlet számítási csomópontok:</span><span class="sxs-lookup"><span data-stu-id="dda53-197">First, create a **PSApplicationPackageReference** object, and configure it with hello application Id and package version you want toodeploy toohello pool's compute nodes:</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

<span data-ttu-id="dda53-198">Most hello készlet létrehozásához, és adja meg hello csomag referenciaobjektum, mivel argumentum toohello hello `ApplicationPackageReferences` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="dda53-198">Now create hello pool, and specify hello package reference object as hello argument toohello `ApplicationPackageReferences` option:</span></span>

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

<span data-ttu-id="dda53-199">További információt az alkalmazáscsomagok a [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="dda53-199">You can find more information on application packages in [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dda53-200">Meg kell [egy Azure Storage-fiók csatolása](#linked-storage-account-autostorage) tooyour Batch-fiók toouse alkalmazáscsomagok.</span><span class="sxs-lookup"><span data-stu-id="dda53-200">You must [link an Azure Storage account](#linked-storage-account-autostorage) tooyour Batch account toouse application packages.</span></span>
> 
> 

### <a name="update-a-pools-application-packages"></a><span data-ttu-id="dda53-201">Készlet alkalmazáscsomagjainak frissítése</span><span class="sxs-lookup"><span data-stu-id="dda53-201">Update a pool's application packages</span></span>
<span data-ttu-id="dda53-202">tooupdate hello alkalmazások kijelölt tooan meglévő készlet, először létre kell hoznia egy PSApplicationPackageReference objektum szükséges hello tulajdonságokkal (csomagazonosító és csomagnév Alkalmazásverzió):</span><span class="sxs-lookup"><span data-stu-id="dda53-202">tooupdate hello applications assigned tooan existing pool, first create a PSApplicationPackageReference object with hello desired properties (application Id and package version):</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

<span data-ttu-id="dda53-203">A következő hello készlet beolvasása kötegben, törölje ki a meglévő csomagokat, az új csomag hivatkozás hozzáadása és hello Batch szolgáltatás hello új alkalmazáskészlet beállításainak frissítése:</span><span class="sxs-lookup"><span data-stu-id="dda53-203">Next, get hello pool from Batch, clear out any existing packages, add our new package reference, and update hello Batch service with hello new pool settings:</span></span>

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

<span data-ttu-id="dda53-204">Most már frissítette hello készlet tulajdonságok hello Batch szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="dda53-204">You've now updated hello pool's properties in hello Batch service.</span></span> <span data-ttu-id="dda53-205">tooactually hello új alkalmazás csomag toocompute csomópontok hello készletben, a rendszer azonban kell indítani, vagy újból lemezképet létrehozni azokat a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="dda53-205">tooactually deploy hello new application package toocompute nodes in hello pool, however, you must restart or reimage those nodes.</span></span> <span data-ttu-id="dda53-206">Az alábbi paranccsal újraindíthatja a készlet összes számítási csomópontját:</span><span class="sxs-lookup"><span data-stu-id="dda53-206">You can restart every node in a pool with this command:</span></span>

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> <span data-ttu-id="dda53-207">Telepíthet több alkalmazás csomagok toohello számítási csomópont a készletben.</span><span class="sxs-lookup"><span data-stu-id="dda53-207">You can deploy multiple application packages toohello compute nodes in a pool.</span></span> <span data-ttu-id="dda53-208">Ha azt szeretné, túl*hozzáadása* jelenleg telepített hello csomagok felülírása helyett egy alkalmazáscsomagot, hagyja el hello `$pool.ApplicationPackageReferences.Clear()` fenti sor.</span><span class="sxs-lookup"><span data-stu-id="dda53-208">If you'd like too*add* an application package instead of replacing hello currently deployed packages, omit hello `$pool.ApplicationPackageReferences.Clear()` line above.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="dda53-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dda53-209">Next steps</span></span>
* <span data-ttu-id="dda53-210">A parancsmag részletes szintaxisáért és példákért lásd: [Azure Batch-parancsmagok referenciája](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="dda53-210">For detailed cmdlet syntax and examples, see [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>
* <span data-ttu-id="dda53-211">Alkalmazások és az alkalmazáscsomagok kötegben kapcsolatos további információkért lásd: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="dda53-211">For more information about applications and application packages in Batch, see [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/