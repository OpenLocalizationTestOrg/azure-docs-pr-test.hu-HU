---
title: "aaaManage Azure PowerShell-megoldások |} Microsoft Docs"
description: "Azure PowerShell és az erőforrás-kezelő toomanage az erőforrások használatára."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 47a91af8d7eb59585bcfd43571ce76b90a0d7971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a><span data-ttu-id="ecd31-103">Az Azure PowerShell és a Resource Manager-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="ecd31-103">Manage resources with Azure PowerShell and Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ecd31-104">Portál</span><span class="sxs-lookup"><span data-stu-id="ecd31-104">Portal</span></span>](resource-group-portal.md)
> * [<span data-ttu-id="ecd31-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ecd31-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="ecd31-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecd31-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="ecd31-107">REST API</span><span class="sxs-lookup"><span data-stu-id="ecd31-107">REST API</span></span>](resource-manager-rest-api.md)
>
>

<span data-ttu-id="ecd31-108">Ebből a cikkből megismerheti, hogyan toomanage a megoldások Azure PowerShell és az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ecd31-108">In this article, you learn how toomanage your solutions with Azure PowerShell and Azure Resource Manager.</span></span> <span data-ttu-id="ecd31-109">Ha nem ismeri a Resource Manager, lásd: [Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-109">If you are not familiar with Resource Manager, see [Resource Manager Overview](resource-group-overview.md).</span></span> <span data-ttu-id="ecd31-110">Ez a témakör ismerteti a felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="ecd31-110">This topic focuses on management tasks.</span></span> <span data-ttu-id="ecd31-111">Az alábbiakat fogja elvégezni:</span><span class="sxs-lookup"><span data-stu-id="ecd31-111">You will:</span></span>

1. <span data-ttu-id="ecd31-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ecd31-112">Create a resource group</span></span>
2. <span data-ttu-id="ecd31-113">Erőforráscsoport toohello erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ecd31-113">Add a resource toohello resource group</span></span>
3. <span data-ttu-id="ecd31-114">Címke toohello erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ecd31-114">Add a tag toohello resource</span></span>
4. <span data-ttu-id="ecd31-115">A nevek vagy címke értékek alapján-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ecd31-115">Query resources based on names or tag values</span></span>
5. <span data-ttu-id="ecd31-116">Alkalmazza, és távolítsa el a hello erőforrás zárolását</span><span class="sxs-lookup"><span data-stu-id="ecd31-116">Apply and remove a lock on hello resource</span></span>
6. <span data-ttu-id="ecd31-117">Egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ecd31-117">Delete a resource group</span></span>

<span data-ttu-id="ecd31-118">Ne jelenjen meg ez a cikk hogyan toodeploy Resource Manager sablon tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ecd31-118">This article does not show how toodeploy a Resource Manager template tooyour subscription.</span></span> <span data-ttu-id="ecd31-119">Ez az információ, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-119">For that information, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>

## <a name="get-started-with-azure-powershell"></a><span data-ttu-id="ecd31-120">Ismerkedés az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecd31-120">Get started with Azure PowerShell</span></span>

<span data-ttu-id="ecd31-121">Ha még nem telepítette az Azure PowerShell, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ecd31-121">If you have not installed Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="ecd31-122">Ha Azure PowerShell-lel telepítették az elmúlt hello, de nem frissítve, legutóbb, javasoljuk, hogy hello legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="ecd31-122">If you have installed Azure PowerShell in hello past but have not updated it recently, consider installing hello latest version.</span></span> <span data-ttu-id="ecd31-123">Frissítheti az hello hello verzióból ugyanezt a módszert tooinstall használta azt.</span><span class="sxs-lookup"><span data-stu-id="ecd31-123">You can update hello version through hello same method you used tooinstall it.</span></span> <span data-ttu-id="ecd31-124">Például ha hello Webplatform-telepítővel, indítsa el újra, és keresse meg egy frissítés.</span><span class="sxs-lookup"><span data-stu-id="ecd31-124">For example, if you used hello Web Platform Installer, launch it again and look for an update.</span></span>

<span data-ttu-id="ecd31-125">toocheck hello Azure-erőforrások modul verziójának használatára hello a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="ecd31-125">toocheck your version of hello Azure Resources module, use hello following cmdlet:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="ecd31-126">Ez a témakör frissítve lett a 3.3.0 verziója.</span><span class="sxs-lookup"><span data-stu-id="ecd31-126">This topic was updated for version 3.3.0.</span></span> <span data-ttu-id="ecd31-127">Ha egy korábbi, a felhasználói élmény nem egyeznek meg ebben a témakörben leírt hello lépés.</span><span class="sxs-lookup"><span data-stu-id="ecd31-127">If you have an earlier version, your experience might not match hello steps shown in this topic.</span></span> <span data-ttu-id="ecd31-128">Az ebben a verzióban a hello parancsmagokról dokumentációjáért lásd: [AzureRM.Resources modul](/powershell/module/azurerm.resources).</span><span class="sxs-lookup"><span data-stu-id="ecd31-128">For documentation about hello cmdlets in this version, see [AzureRM.Resources Module](/powershell/module/azurerm.resources).</span></span>

## <a name="log-in-tooyour-azure-account"></a><span data-ttu-id="ecd31-129">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="ecd31-129">Log in tooyour Azure account</span></span>
<span data-ttu-id="ecd31-130">A megoldás dolgozik, előtt be kell jelentkeznie tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="ecd31-130">Before working on your solution, you must log in tooyour account.</span></span>

<span data-ttu-id="ecd31-131">az Azure-fiókra, tooyour toolog hello használata **Login-AzureRmAccount** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ecd31-131">toolog in tooyour Azure account, use hello **Login-AzureRmAccount** cmdlet.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="ecd31-132">hello parancsmag kéri hello bejelentkezési hitelesítő adatait az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="ecd31-132">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="ecd31-133">A bejelentkezés után az tölti le a fiók beállításait,-e elérhető tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ecd31-133">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span>

<span data-ttu-id="ecd31-134">hello parancsmag a fiók és hello előfizetés toouse hello feladatok információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ecd31-134">hello cmdlet returns information about your account and hello subscription toouse for hello tasks.</span></span>

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

<span data-ttu-id="ecd31-135">Ha egynél több előfizetéssel rendelkezik, megváltoztathatja az tooa másik előfizetést.</span><span class="sxs-lookup"><span data-stu-id="ecd31-135">If you have more than one subscription, you can switch tooa different subscription.</span></span> <span data-ttu-id="ecd31-136">Első lépésként Ismerkedjen meg a fiókhoz az összes hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="ecd31-136">First, let's see all hello subscriptions for your account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="ecd31-137">Engedélyezett és letiltott előfizetésekhez adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ecd31-137">It returns enabled and disabled subscriptions.</span></span>

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

<span data-ttu-id="ecd31-138">tooswitch tooa másik előfizetést, adja meg a hello előfizetés nevét a hello **Set-AzureRmContext** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ecd31-138">tooswitch tooa different subscription, provide hello subscription name with hello **Set-AzureRmContext** cmdlet.</span></span>

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="ecd31-139">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ecd31-139">Create a resource group</span></span>
<span data-ttu-id="ecd31-140">Bármely erőforrások tooyour előfizetés telepítés megkezdése előtt létre kell hoznia a hello erőforrásokat tartalmazó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="ecd31-140">Before deploying any resources tooyour subscription, you must create a resource group that will contain hello resources.</span></span>

<span data-ttu-id="ecd31-141">egy erőforráscsoport, toocreate hello használata **New-AzureRmResourceGroup** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ecd31-141">toocreate a resource group, use hello **New-AzureRmResourceGroup** cmdlet.</span></span> <span data-ttu-id="ecd31-142">hello parancs hello **neve** paraméter toospecify nevét hello erőforráscsoport és hello **hely** paraméter toospecify helyére.</span><span class="sxs-lookup"><span data-stu-id="ecd31-142">hello command uses hello **Name** parameter toospecify a name for hello resource group and hello **Location** parameter toospecify its location.</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

<span data-ttu-id="ecd31-143">hello kimeneti hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="ecd31-143">hello output is in hello following format:</span></span>

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

<span data-ttu-id="ecd31-144">Ha később kell tooretrieve hello erőforráscsoport, használja a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="ecd31-144">If you need tooretrieve hello resource group later, use hello following cmdlet:</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

<span data-ttu-id="ecd31-145">tooget összes hello az előfizetés az erőforráscsoportokat, ne adjon meg egy nevet:</span><span class="sxs-lookup"><span data-stu-id="ecd31-145">tooget all hello resource groups in your subscription, do not specify a name:</span></span>

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-tooa-resource-group"></a><span data-ttu-id="ecd31-146">Erőforrások tooa erőforráscsoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ecd31-146">Add resources tooa resource group</span></span>
<span data-ttu-id="ecd31-147">tooadd erőforrás toohello erőforráscsoport, hello használható **New-AzureRmResource** létrehozásakor parancsmag vagy egy parancsmagot, amely adott toohello típusú erőforrás (például **New-AzureRmStorageAccount**).</span><span class="sxs-lookup"><span data-stu-id="ecd31-147">tooadd a resource toohello resource group, you can use hello **New-AzureRmResource** cmdlet or a cmdlet that is specific toohello type of resource you are creating (like **New-AzureRmStorageAccount**).</span></span> <span data-ttu-id="ecd31-148">Bizonyára hasznosnak találja az egyszerűbb toouse parancsmag, amely adott tooa erőforrástípus, mert azokat a paramétereket a hello új erőforrás szükséges hello tulajdonságok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ecd31-148">You might find it easier toouse a cmdlet that is specific tooa resource type because it includes parameters for hello properties that are needed for hello new resource.</span></span> <span data-ttu-id="ecd31-149">toouse **New-AzureRmResource**, minden hello tulajdonságok tooset ismernie kell őket megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="ecd31-149">toouse **New-AzureRmResource**, you must know all hello properties tooset without being prompted for them.</span></span>

<span data-ttu-id="ecd31-150">Azonban parancsmagokon keresztül erőforrás hozzáadása okozhat jövőbeli zavart mert hello új erőforrás nem létezik a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="ecd31-150">However, adding a resource through cmdlets might cause future confusion because hello new resource does not exist in a Resource Manager template.</span></span> <span data-ttu-id="ecd31-151">A Microsoft azt javasolja, adjon meg hello infrastruktúra az Azure-megoldás a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="ecd31-151">Microsoft recommends defining hello infrastructure for your Azure solution in a Resource Manager template.</span></span> <span data-ttu-id="ecd31-152">Sablonok lehetővé teszik a tooreliably, és a megoldás ismételt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="ecd31-152">Templates enable you tooreliably and repeatedly deploy your solution.</span></span> <span data-ttu-id="ecd31-153">Ez a témakör egy PowerShell-parancsmaggal hozzon létre egy tárfiókot, de később az erőforráscsoport a sablon létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ecd31-153">For this topic, you create a storage account with a PowerShell cmdlet, but later you generate a template from your resource group.</span></span>

<span data-ttu-id="ecd31-154">a következő parancsmag hello hoz létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="ecd31-154">hello following cmdlet creates a storage account.</span></span> <span data-ttu-id="ecd31-155">Hello példában látható módon hello neve helyett adjon egyedi nevet a hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="ecd31-155">Instead of using hello name shown in hello example, provide a unique name for hello storage account.</span></span> <span data-ttu-id="ecd31-156">hello neve 3 – 24 karakter hosszúságúnak kell, és csak számokat és kisbetűket tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="ecd31-156">hello name must be between 3 and 24 characters in length, and use only numbers and lower-case letters.</span></span> <span data-ttu-id="ecd31-157">Ha hello példában látható módon hello nevet használja, hibaüzenet, mert a név már használatban van.</span><span class="sxs-lookup"><span data-stu-id="ecd31-157">If you use hello name shown in hello example, you receive an error because that name is already in use.</span></span>

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

<span data-ttu-id="ecd31-158">Ha szükség tooretrieve ehhez az erőforráshoz később, akkor a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="ecd31-158">If you need tooretrieve this resource later, use hello following cmdlet:</span></span>

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a><span data-ttu-id="ecd31-159">Címke hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ecd31-159">Add a tag</span></span>

<span data-ttu-id="ecd31-160">Címkék lehetővé teszik a tooorganize az erőforrások toodifferent tulajdonságok alapján történik.</span><span class="sxs-lookup"><span data-stu-id="ecd31-160">Tags enable you tooorganize your resources according toodifferent properties.</span></span> <span data-ttu-id="ecd31-161">Például előfordulhat, hogy több erőforrás toohello tartozó különböző erőforrás-csoportok a részleghez.</span><span class="sxs-lookup"><span data-stu-id="ecd31-161">For example, you may have several resources in different resource groups that belong toohello same department.</span></span> <span data-ttu-id="ecd31-162">A részleg címke és érték toothose erőforrások toomark alkalmazhatja őket tartozó toohello, ugyanilyen kategóriájú.</span><span class="sxs-lookup"><span data-stu-id="ecd31-162">You can apply a department tag and value toothose resources toomark them as belonging toohello same category.</span></span> <span data-ttu-id="ecd31-163">Vagy jelölheti meg használja-e egy üzemi és tesztkörnyezetben környezetben egy erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ecd31-163">Or, you can mark whether a resource is used in a production or test environment.</span></span> <span data-ttu-id="ecd31-164">Ebben a témakörben telepítését címkék tooonly egy erőforrást, de a környezetben valószínűleg érdemes tooapply címkéket tooall az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ecd31-164">In this topic, you apply tags tooonly one resource, but in your environment it most likely makes sense tooapply tags tooall your resources.</span></span>

<span data-ttu-id="ecd31-165">a következő parancsmag hello két címkék tooyour tárfiók vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="ecd31-165">hello following cmdlet applies two tags tooyour storage account:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

<span data-ttu-id="ecd31-166">Címkék egyetlen objektumként frissülnek.</span><span class="sxs-lookup"><span data-stu-id="ecd31-166">Tags are updated as a single object.</span></span> <span data-ttu-id="ecd31-167">tooadd egy címke tooa erőforrást, amely már tartalmazza a címkék, először kérjen le a meglévő hello címkék.</span><span class="sxs-lookup"><span data-stu-id="ecd31-167">tooadd a tag tooa resource that already includes tags, first retrieve hello existing tags.</span></span> <span data-ttu-id="ecd31-168">Hello új címke toohello tartalmazó objektum hello meglévő címkék hozzáadása, és alkalmazza újra az összes hello címkék toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ecd31-168">Add hello new tag toohello object that contains hello existing tags, and reapply all hello tags toohello resource.</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a><span data-ttu-id="ecd31-169">Erőforrások keresése</span><span class="sxs-lookup"><span data-stu-id="ecd31-169">Search for resources</span></span>

<span data-ttu-id="ecd31-170">Használjon hello **keresés-AzureRmResource** parancsmag tooretrieve erőforrások különböző keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="ecd31-170">Use hello **Find-AzureRmResource** cmdlet tooretrieve resources for different search conditions.</span></span>

* <span data-ttu-id="ecd31-171">tooget erőforrás nevét, adja meg a hello **ResourceNameContains** paraméter:</span><span class="sxs-lookup"><span data-stu-id="ecd31-171">tooget a resource by name, provide hello **ResourceNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* <span data-ttu-id="ecd31-172">tooget összes hello erőforráscsoportban, forrásokban hello **ResourceGroupNameContains** paraméter:</span><span class="sxs-lookup"><span data-stu-id="ecd31-172">tooget all hello resources in a resource group, provide hello **ResourceGroupNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* <span data-ttu-id="ecd31-173">tooget összes hello egy olyan címke nevét és értékét, forrásokban hello **TagName** és **TagValue** paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ecd31-173">tooget all hello resources with a tag name and value, provide hello **TagName** and **TagValue** parameters:</span></span>

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* <span data-ttu-id="ecd31-174">tooall hello erőforrásokhoz egy adott erőforrástípusra, adja meg a hello **ResourceType** paraméter:</span><span class="sxs-lookup"><span data-stu-id="ecd31-174">tooall hello resources with a particular resource type, provide hello **ResourceType** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a><span data-ttu-id="ecd31-175">Egy erőforrás zárolása</span><span class="sxs-lookup"><span data-stu-id="ecd31-175">Lock a resource</span></span>

<span data-ttu-id="ecd31-176">Toomake meg arról, hogy a kritikus fontosságú erőforrás nem véletlenül törölték vagy módosították van szüksége, a zárolás toohello erőforrás vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="ecd31-176">When you need toomake sure a critical resource is not accidentally deleted or modified, apply a lock toohello resource.</span></span> <span data-ttu-id="ecd31-177">Vagy megadhat egy **CanNotDelete** vagy **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="ecd31-177">You can specify either a **CanNotDelete** or **ReadOnly**.</span></span>

<span data-ttu-id="ecd31-178">toocreate vagy delete felügyeleti zárolás, hozzá kell férnie túl`Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek.</span><span class="sxs-lookup"><span data-stu-id="ecd31-178">toocreate or delete management locks, you must have access too`Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="ecd31-179">Hello beépített szerepkörök csak a tulajdonos és a felhasználói hozzáférés adminisztrátora kapnak ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="ecd31-179">Of hello built-in roles, only Owner and User Access Administrator are granted those actions.</span></span>

<span data-ttu-id="ecd31-180">a zárolási tooapply hello parancsmag a következő használja:</span><span class="sxs-lookup"><span data-stu-id="ecd31-180">tooapply a lock, use hello following cmdlet:</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="ecd31-181">hello zárolt erőforrás a fenti példa hello nem törölhető hello zárolási eltávolításáig.</span><span class="sxs-lookup"><span data-stu-id="ecd31-181">hello locked resource in hello preceding example cannot be deleted until hello lock is removed.</span></span> <span data-ttu-id="ecd31-182">a zárolási tooremove használja:</span><span class="sxs-lookup"><span data-stu-id="ecd31-182">tooremove a lock, use:</span></span>

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="ecd31-183">A beállítás zárolások kapcsolatos további információkért lásd: [erőforrások az Azure Resource Manager zárolása](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-183">For more information about setting locks, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

## <a name="remove-resources-or-resource-group"></a><span data-ttu-id="ecd31-184">Távolítsa el az erőforrás vagy erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="ecd31-184">Remove resources or resource group</span></span>
<span data-ttu-id="ecd31-185">Egy erőforrás vagy egy erőforráscsoportot is eltávolíthat.</span><span class="sxs-lookup"><span data-stu-id="ecd31-185">You can remove a resource or resource group.</span></span> <span data-ttu-id="ecd31-186">Egy erőforráscsoport, akkor is távolítsa el, amelyeket belül minden hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ecd31-186">When you remove a resource group, you also remove all hello resources within that resource group.</span></span>

* <span data-ttu-id="ecd31-187">hello erőforráscsoport, használjon hello erőforrás toodelete **Remove-AzureRmResource** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ecd31-187">toodelete a resource from hello resource group, use hello **Remove-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="ecd31-188">Ez a parancsmag hello erőforrás törli, de nem törli a hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ecd31-188">This cmdlet deletes hello resource, but does not delete hello resource group.</span></span>

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* <span data-ttu-id="ecd31-189">toodelete az erőforráscsoportot és a hozzá tartozó összes erőforrásnak, használja a hello **Remove-AzureRmResourceGroup** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ecd31-189">toodelete a resource group and all its resources, use hello **Remove-AzureRmResourceGroup** cmdlet.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

<span data-ttu-id="ecd31-190">Mindkét parancsmag, a rendszer kéri tooconfirm tooremove hello erőforrás vagy erőforráscsoport kívánja.</span><span class="sxs-lookup"><span data-stu-id="ecd31-190">For both cmdlets, you are asked tooconfirm that you wish tooremove hello resource or resource group.</span></span> <span data-ttu-id="ecd31-191">Ha hello sikeresen töröl hello erőforrás vagy erőforráscsoportban található, visszaadja **igaz**.</span><span class="sxs-lookup"><span data-stu-id="ecd31-191">If hello operation successfully deletes hello resource or resource group, it returns **True**.</span></span>

## <a name="run-resource-manager-scripts-with-azure-automation"></a><span data-ttu-id="ecd31-192">Az Azure Automation szolgáltatásban, erőforrás-kezelő parancsfájlok futtatása</span><span class="sxs-lookup"><span data-stu-id="ecd31-192">Run Resource Manager scripts with Azure Automation</span></span>

<span data-ttu-id="ecd31-193">Ez a témakör bemutatja, hogyan tooperform az erőforrások az Azure PowerShell alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="ecd31-193">This topic shows you how tooperform basic operations on your resources with Azure PowerShell.</span></span> <span data-ttu-id="ecd31-194">Speciális felügyeleti forgatókönyvekhez akkor általában szeretné, hogy egy parancsfájl toocreate, és újra felhasználhatja, hogy a parancsfájl igény szerint vagy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ecd31-194">For more advanced management scenarios, you typically want toocreate a script, and reuse that script as needed or on a schedule.</span></span> <span data-ttu-id="ecd31-195">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) lehetőséget biztosít az Ön gyakran használt tooautomate olyan parancsfájlok, amelyek az Azure megoldások kezelése.</span><span class="sxs-lookup"><span data-stu-id="ecd31-195">[Azure Automation](../automation/automation-intro.md) provides a way for you tooautomate frequently used scripts that manage your Azure solutions.</span></span>

<span data-ttu-id="ecd31-196">hello következő témakörök bemutatják, hogyan toouse Azure Automation, az erőforrás-kezelő és a PowerShell tooeffectively felügyeleti feladatok elvégzésére:</span><span class="sxs-lookup"><span data-stu-id="ecd31-196">hello following topics show you how toouse Azure Automation, Resource Manager, and PowerShell tooeffectively perform management tasks:</span></span>

- <span data-ttu-id="ecd31-197">Egy runbook létrehozásával kapcsolatos további információkért lásd: [az első PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-197">For information about creating a runbook, see [My first PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span></span>
- <span data-ttu-id="ecd31-198">További információ a gyűjtemények, parancsfájlok,: [az Azure Automation forgatókönyv- és gyűjtemények](../automation/automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-198">For information about working with galleries of scripts, see [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md).</span></span>
- <span data-ttu-id="ecd31-199">A runbookok, amelyek indítása és leállítása a virtuális gépek, lásd: [Azure Automation-forgatókönyv: egy ütemezést az Azure virtuális gép indítási és leállítási címkék használatával JSON-formátumú toocreate](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-199">For runbooks that start and stop virtual machines, see [Azure Automation scenario: Using JSON-formatted tags toocreate a schedule for Azure VM startup and shutdown](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span></span>
- <span data-ttu-id="ecd31-200">A runbookok, amelyek indítása és leállítása a virtuális gépek munkaidőn kívüli, lásd: [indítása/leállítása virtuális gépek munkaidőn kívüli megoldás az automatizálás során](../automation/automation-solution-vm-management.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-200">For runbooks that start and stop virtual machines off-hours, see [Start/Stop VMs during off-hours solution in Automation](../automation/automation-solution-vm-management.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecd31-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecd31-201">Next steps</span></span>
* <span data-ttu-id="ecd31-202">toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-202">toolearn about creating Resource Manager templates, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="ecd31-203">toolearn sablonok telepítésével kapcsolatban lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-203">toolearn about deploying templates, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="ecd31-204">Áthelyezheti a meglévő erőforrásokat tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ecd31-204">You can move existing resources tooa new resource group.</span></span> <span data-ttu-id="ecd31-205">Tekintse meg a [áthelyezési tooNew erőforráscsoport vagy előfizetés](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-205">For examples, see [Move Resources tooNew Resource Group or Subscription](resource-group-move-resources.md).</span></span>
* <span data-ttu-id="ecd31-206">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="ecd31-206">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

