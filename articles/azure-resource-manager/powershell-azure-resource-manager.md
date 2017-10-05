---
title: "Az Azure PowerShell-megoldások kezelése |} Microsoft Docs"
description: "Azure PowerShell és a Resource Manager segítségével kezelheti az erőforrásokat."
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
ms.openlocfilehash: ff42e5cb29005c5f4b97babdae58bef9382071f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a><span data-ttu-id="b7c91-103">Az Azure PowerShell és a Resource Manager-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="b7c91-103">Manage resources with Azure PowerShell and Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7c91-104">Portal</span><span class="sxs-lookup"><span data-stu-id="b7c91-104">Portal</span></span>](resource-group-portal.md)
> * [<span data-ttu-id="b7c91-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b7c91-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="b7c91-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c91-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="b7c91-107">REST API</span><span class="sxs-lookup"><span data-stu-id="b7c91-107">REST API</span></span>](resource-manager-rest-api.md)
>
>

<span data-ttu-id="b7c91-108">Ebből a cikkből megismerheti, hogyan kezelheti a megoldások Azure PowerShell és az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b7c91-108">In this article, you learn how to manage your solutions with Azure PowerShell and Azure Resource Manager.</span></span> <span data-ttu-id="b7c91-109">Ha nem ismeri a Resource Manager, lásd: [Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-109">If you are not familiar with Resource Manager, see [Resource Manager Overview](resource-group-overview.md).</span></span> <span data-ttu-id="b7c91-110">Ez a témakör ismerteti a felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b7c91-110">This topic focuses on management tasks.</span></span> <span data-ttu-id="b7c91-111">Az alábbiakat fogja elvégezni:</span><span class="sxs-lookup"><span data-stu-id="b7c91-111">You will:</span></span>

1. <span data-ttu-id="b7c91-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b7c91-112">Create a resource group</span></span>
2. <span data-ttu-id="b7c91-113">Az erőforráscsoport erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7c91-113">Add a resource to the resource group</span></span>
3. <span data-ttu-id="b7c91-114">Az erőforrás egy címke hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7c91-114">Add a tag to the resource</span></span>
4. <span data-ttu-id="b7c91-115">A nevek vagy címke értékek alapján-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b7c91-115">Query resources based on names or tag values</span></span>
5. <span data-ttu-id="b7c91-116">Alkalmazza, és távolítsa el az erőforrás zárolását</span><span class="sxs-lookup"><span data-stu-id="b7c91-116">Apply and remove a lock on the resource</span></span>
6. <span data-ttu-id="b7c91-117">Egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b7c91-117">Delete a resource group</span></span>

<span data-ttu-id="b7c91-118">Ez a cikk nem szerepelnek a Resource Manager-sablon üzembe helyezése az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b7c91-118">This article does not show how to deploy a Resource Manager template to your subscription.</span></span> <span data-ttu-id="b7c91-119">Ez az információ, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-119">For that information, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>

## <a name="get-started-with-azure-powershell"></a><span data-ttu-id="b7c91-120">Ismerkedés az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7c91-120">Get started with Azure PowerShell</span></span>

<span data-ttu-id="b7c91-121">Ha még nem telepítette az Azure PowerShell, lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b7c91-121">If you have not installed Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="b7c91-122">Ha korábban telepítette az Azure PowerShell, de nem frissítve, legutóbb, javasoljuk, hogy telepítse a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="b7c91-122">If you have installed Azure PowerShell in the past but have not updated it recently, consider installing the latest version.</span></span> <span data-ttu-id="b7c91-123">A verziófrissítés keresztül ugyanezt a módszert akkor telepítéséhez is használt.</span><span class="sxs-lookup"><span data-stu-id="b7c91-123">You can update the version through the same method you used to install it.</span></span> <span data-ttu-id="b7c91-124">Például ha a Webplatform-telepítő, indítsa el újra, és keresse meg frissítés.</span><span class="sxs-lookup"><span data-stu-id="b7c91-124">For example, if you used the Web Platform Installer, launch it again and look for an update.</span></span>

<span data-ttu-id="b7c91-125">Az Azure-erőforrások modul verziójának ellenőrzéséhez használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="b7c91-125">To check your version of the Azure Resources module, use the following cmdlet:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="b7c91-126">Ez a témakör frissítve lett a 3.3.0 verziója.</span><span class="sxs-lookup"><span data-stu-id="b7c91-126">This topic was updated for version 3.3.0.</span></span> <span data-ttu-id="b7c91-127">Ha egy korábbi, a felhasználói élmény nem egyeznek meg ebben a témakörben bemutatott lépések.</span><span class="sxs-lookup"><span data-stu-id="b7c91-127">If you have an earlier version, your experience might not match the steps shown in this topic.</span></span> <span data-ttu-id="b7c91-128">Az ebben a verzióban a parancsmagokkal dokumentációjáért lásd: [AzureRM.Resources modul](/powershell/module/azurerm.resources).</span><span class="sxs-lookup"><span data-stu-id="b7c91-128">For documentation about the cmdlets in this version, see [AzureRM.Resources Module](/powershell/module/azurerm.resources).</span></span>

## <a name="log-in-to-your-azure-account"></a><span data-ttu-id="b7c91-129">Jelentkezzen be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="b7c91-129">Log in to your Azure account</span></span>
<span data-ttu-id="b7c91-130">A megoldás dolgozik, mielőtt be kell jelentkezni fiókját.</span><span class="sxs-lookup"><span data-stu-id="b7c91-130">Before working on your solution, you must log in to your account.</span></span>

<span data-ttu-id="b7c91-131">Jelentkezzen be az Azure-fiókjával, használja a **Login-AzureRmAccount** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b7c91-131">To log in to your Azure account, use the **Login-AzureRmAccount** cmdlet.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="b7c91-132">A parancsmag kéri az Azure-fiók bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b7c91-132">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="b7c91-133">A bejelentkezés után letölti a fiók beállításait, hogy elérhetők legyenek az Azure PowerShell számára.</span><span class="sxs-lookup"><span data-stu-id="b7c91-133">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span>

<span data-ttu-id="b7c91-134">A parancsmag fiókját és a feladatokhoz használhatja, hogy az előfizetés információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b7c91-134">The cmdlet returns information about your account and the subscription to use for the tasks.</span></span>

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

<span data-ttu-id="b7c91-135">Ha egynél több előfizetéssel rendelkezik, átválthat egy másik előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b7c91-135">If you have more than one subscription, you can switch to a different subscription.</span></span> <span data-ttu-id="b7c91-136">Első lépésként Ismerkedjen meg az összes olyan előfizetést, a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="b7c91-136">First, let's see all the subscriptions for your account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="b7c91-137">Engedélyezett és letiltott előfizetésekhez adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b7c91-137">It returns enabled and disabled subscriptions.</span></span>

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

<span data-ttu-id="b7c91-138">Váltson át egy másik előfizetésben található, adja meg az előfizetés nevét a **Set-AzureRmContext** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b7c91-138">To switch to a different subscription, provide the subscription name with the **Set-AzureRmContext** cmdlet.</span></span>

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="b7c91-139">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b7c91-139">Create a resource group</span></span>
<span data-ttu-id="b7c91-140">Az előfizetéshez erőforrásokat telepítés megkezdése előtt létre kell hoznia az erőforrásokat tartalmazó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="b7c91-140">Before deploying any resources to your subscription, you must create a resource group that will contain the resources.</span></span>

<span data-ttu-id="b7c91-141">Erőforráscsoport létrehozásához használja a **New-AzureRmResourceGroup** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b7c91-141">To create a resource group, use the **New-AzureRmResourceGroup** cmdlet.</span></span> <span data-ttu-id="b7c91-142">A parancs a **neve** paraméter segítségével adjon meg egy nevet az erőforráscsoporthoz tartozó és a **hely** paramétert adja meg a helyet.</span><span class="sxs-lookup"><span data-stu-id="b7c91-142">The command uses the **Name** parameter to specify a name for the resource group and the **Location** parameter to specify its location.</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

<span data-ttu-id="b7c91-143">A kimenet a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="b7c91-143">The output is in the following format:</span></span>

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

<span data-ttu-id="b7c91-144">Ha az erőforráscsoport beolvasni később van szüksége, használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="b7c91-144">If you need to retrieve the resource group later, use the following cmdlet:</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

<span data-ttu-id="b7c91-145">Ahhoz, hogy az erőforráscsoportok az előfizetéshez, ne adjon meg egy nevet:</span><span class="sxs-lookup"><span data-stu-id="b7c91-145">To get all the resource groups in your subscription, do not specify a name:</span></span>

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a><span data-ttu-id="b7c91-146">Erőforrások hozzáadása egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b7c91-146">Add resources to a resource group</span></span>
<span data-ttu-id="b7c91-147">Erőforrás hozzáadása az erőforráscsoporthoz, használhatja a **New-AzureRmResource** létrehozásakor parancsmag vagy az erőforrás típusára vonatkozó parancsmag (például **New-AzureRmStorageAccount**).</span><span class="sxs-lookup"><span data-stu-id="b7c91-147">To add a resource to the resource group, you can use the **New-AzureRmResource** cmdlet or a cmdlet that is specific to the type of resource you are creating (like **New-AzureRmStorageAccount**).</span></span> <span data-ttu-id="b7c91-148">Előfordulhat, hogy ez egyszerűbbé teszi, hogy csak az erőforrástípus, mert azokat a tulajdonságokat, amelyeket az új erőforrás szükséges paramétereket tartalmazza a parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="b7c91-148">You might find it easier to use a cmdlet that is specific to a resource type because it includes parameters for the properties that are needed for the new resource.</span></span> <span data-ttu-id="b7c91-149">Használandó **New-AzureRmResource**, ismernie kell az összes tulajdonság beállítása nélkül meg kell adni őket.</span><span class="sxs-lookup"><span data-stu-id="b7c91-149">To use **New-AzureRmResource**, you must know all the properties to set without being prompted for them.</span></span>

<span data-ttu-id="b7c91-150">Azonban parancsmagokon keresztül erőforrás hozzáadása okozhat jövőbeli zavart, mert az új erőforrás nem létezik a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="b7c91-150">However, adding a resource through cmdlets might cause future confusion because the new resource does not exist in a Resource Manager template.</span></span> <span data-ttu-id="b7c91-151">A Microsoft azt javasolja, adjon meg az infrastruktúra az Azure-megoldás a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="b7c91-151">Microsoft recommends defining the infrastructure for your Azure solution in a Resource Manager template.</span></span> <span data-ttu-id="b7c91-152">Sablonok lehetővé teszik megbízható és ismételten a megoldás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7c91-152">Templates enable you to reliably and repeatedly deploy your solution.</span></span> <span data-ttu-id="b7c91-153">Ez a témakör egy PowerShell-parancsmaggal hozzon létre egy tárfiókot, de később az erőforráscsoport a sablon létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b7c91-153">For this topic, you create a storage account with a PowerShell cmdlet, but later you generate a template from your resource group.</span></span>

<span data-ttu-id="b7c91-154">A következő parancsmag létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b7c91-154">The following cmdlet creates a storage account.</span></span> <span data-ttu-id="b7c91-155">A példában látható módon a neve helyett adja meg egy egyedi nevet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b7c91-155">Instead of using the name shown in the example, provide a unique name for the storage account.</span></span> <span data-ttu-id="b7c91-156">A névnek kell 3 és 24 karakter hosszúságúnak kell, és csak számokat és kisbetűket tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="b7c91-156">The name must be between 3 and 24 characters in length, and use only numbers and lower-case letters.</span></span> <span data-ttu-id="b7c91-157">A példában látható módon a nevet használja, ha egy hibaüzenetet kapja, mert a név már használatban van.</span><span class="sxs-lookup"><span data-stu-id="b7c91-157">If you use the name shown in the example, you receive an error because that name is already in use.</span></span>

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

<span data-ttu-id="b7c91-158">Ha ezt az erőforrást beolvasni később van szüksége, használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="b7c91-158">If you need to retrieve this resource later, use the following cmdlet:</span></span>

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a><span data-ttu-id="b7c91-159">Címke hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7c91-159">Add a tag</span></span>

<span data-ttu-id="b7c91-160">Címkék lehetővé teszik az erőforrások más tulajdonságokkal szerint rendezheti.</span><span class="sxs-lookup"><span data-stu-id="b7c91-160">Tags enable you to organize your resources according to different properties.</span></span> <span data-ttu-id="b7c91-161">Például előfordulhat, hogy több erőforrás részleghez tartozó különböző erőforrás csoportokban.</span><span class="sxs-lookup"><span data-stu-id="b7c91-161">For example, you may have several resources in different resource groups that belong to the same department.</span></span> <span data-ttu-id="b7c91-162">Ezeket az erőforrásokat megjelölése azonos kategóriába tartozó alkalmazhat egy részleg címke és értéket.</span><span class="sxs-lookup"><span data-stu-id="b7c91-162">You can apply a department tag and value to those resources to mark them as belonging to the same category.</span></span> <span data-ttu-id="b7c91-163">Vagy jelölheti meg használja-e egy üzemi és tesztkörnyezetben környezetben egy erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b7c91-163">Or, you can mark whether a resource is used in a production or test environment.</span></span> <span data-ttu-id="b7c91-164">Ebben a témakörben címkékkel csak egy erőforráshoz, de a környezetben valószínűleg érdemes címkék azokra az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b7c91-164">In this topic, you apply tags to only one resource, but in your environment it most likely makes sense to apply tags to all your resources.</span></span>

<span data-ttu-id="b7c91-165">A következő parancsmagot a tárfiók két címkék vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="b7c91-165">The following cmdlet applies two tags to your storage account:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

<span data-ttu-id="b7c91-166">Címkék egyetlen objektumként frissülnek.</span><span class="sxs-lookup"><span data-stu-id="b7c91-166">Tags are updated as a single object.</span></span> <span data-ttu-id="b7c91-167">Egy címke hozzáadása egy erőforrást, amely már tartalmazza a címkék, először kérjen le a meglévő címkéket.</span><span class="sxs-lookup"><span data-stu-id="b7c91-167">To add a tag to a resource that already includes tags, first retrieve the existing tags.</span></span> <span data-ttu-id="b7c91-168">Az új címke hozzáadása a meglévő címkék tartalmazó objektumot, és alkalmazza újra a címkék az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="b7c91-168">Add the new tag to the object that contains the existing tags, and reapply all the tags to the resource.</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a><span data-ttu-id="b7c91-169">Erőforrások keresése</span><span class="sxs-lookup"><span data-stu-id="b7c91-169">Search for resources</span></span>

<span data-ttu-id="b7c91-170">Használja a **keresés-AzureRmResource** parancsmag használatával kérhet le az erőforrások más keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="b7c91-170">Use the **Find-AzureRmResource** cmdlet to retrieve resources for different search conditions.</span></span>

* <span data-ttu-id="b7c91-171">Ahhoz, hogy az erőforrás neve, adja meg a **ResourceNameContains** paraméter:</span><span class="sxs-lookup"><span data-stu-id="b7c91-171">To get a resource by name, provide the **ResourceNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* <span data-ttu-id="b7c91-172">Ahhoz, hogy az összes erőforrást erőforráscsoportban, adja meg a **ResourceGroupNameContains** paraméter:</span><span class="sxs-lookup"><span data-stu-id="b7c91-172">To get all the resources in a resource group, provide the **ResourceGroupNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* <span data-ttu-id="b7c91-173">Ahhoz, hogy a címke nevét és értékét az erőforrások, adja meg a **TagName** és **TagValue** paraméterek:</span><span class="sxs-lookup"><span data-stu-id="b7c91-173">To get all the resources with a tag name and value, provide the **TagName** and **TagValue** parameters:</span></span>

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* <span data-ttu-id="b7c91-174">Az összes erőforrást az adott erőforrástípus, adjon meg a **ResourceType** paraméter:</span><span class="sxs-lookup"><span data-stu-id="b7c91-174">To all the resources with a particular resource type, provide the **ResourceType** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a><span data-ttu-id="b7c91-175">Egy erőforrás zárolása</span><span class="sxs-lookup"><span data-stu-id="b7c91-175">Lock a resource</span></span>

<span data-ttu-id="b7c91-176">Győződjön meg arról, hogy a kritikus fontosságú erőforrás nem véletlenül törölték vagy módosították van szüksége, alkalmazza a zárolási az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="b7c91-176">When you need to make sure a critical resource is not accidentally deleted or modified, apply a lock to the resource.</span></span> <span data-ttu-id="b7c91-177">Vagy megadhat egy **CanNotDelete** vagy **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="b7c91-177">You can specify either a **CanNotDelete** or **ReadOnly**.</span></span>

<span data-ttu-id="b7c91-178">Hozzon létre, vagy törölje a felügyeleti zárolás, hozzáféréssel kell rendelkeznie `Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek.</span><span class="sxs-lookup"><span data-stu-id="b7c91-178">To create or delete management locks, you must have access to `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="b7c91-179">A beépített szerepkörök csak a tulajdonos és a felhasználói hozzáférés adminisztrátora kapnak ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="b7c91-179">Of the built-in roles, only Owner and User Access Administrator are granted those actions.</span></span>

<span data-ttu-id="b7c91-180">A zárolási segítségével a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="b7c91-180">To apply a lock, use the following cmdlet:</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="b7c91-181">Az előző példában a zárolt erőforrás nem törölhető, amíg a rendszer eltávolítja a zárolást.</span><span class="sxs-lookup"><span data-stu-id="b7c91-181">The locked resource in the preceding example cannot be deleted until the lock is removed.</span></span> <span data-ttu-id="b7c91-182">A zárolási eltávolításához használja:</span><span class="sxs-lookup"><span data-stu-id="b7c91-182">To remove a lock, use:</span></span>

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="b7c91-183">A beállítás zárolások kapcsolatos további információkért lásd: [erőforrások az Azure Resource Manager zárolása](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-183">For more information about setting locks, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

## <a name="remove-resources-or-resource-group"></a><span data-ttu-id="b7c91-184">Távolítsa el az erőforrás vagy erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="b7c91-184">Remove resources or resource group</span></span>
<span data-ttu-id="b7c91-185">Egy erőforrás vagy egy erőforráscsoportot is eltávolíthat.</span><span class="sxs-lookup"><span data-stu-id="b7c91-185">You can remove a resource or resource group.</span></span> <span data-ttu-id="b7c91-186">Egy erőforráscsoport, akkor is távolítsa el erőforrás csoportba tartozó összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b7c91-186">When you remove a resource group, you also remove all the resources within that resource group.</span></span>

* <span data-ttu-id="b7c91-187">Az erőforráscsoport egy erőforrás törléséhez használja a **Remove-AzureRmResource** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b7c91-187">To delete a resource from the resource group, use the **Remove-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="b7c91-188">Ez a parancsmag törli az erőforrást, de nem törli az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b7c91-188">This cmdlet deletes the resource, but does not delete the resource group.</span></span>

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* <span data-ttu-id="b7c91-189">Az erőforráscsoport és az ahhoz tartozó összes erőforrást törölheti a **Remove-AzureRmResourceGroup** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b7c91-189">To delete a resource group and all its resources, use the **Remove-AzureRmResourceGroup** cmdlet.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

<span data-ttu-id="b7c91-190">Mindkét parancsmagokkal a rendszer felkéri győződjön meg arról, hogy szeretne-e az erőforrás vagy erőforráscsoport eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="b7c91-190">For both cmdlets, you are asked to confirm that you wish to remove the resource or resource group.</span></span> <span data-ttu-id="b7c91-191">Ha a művelet sikeresen törli az erőforrás vagy az erőforráscsoportot, visszatérési **igaz**.</span><span class="sxs-lookup"><span data-stu-id="b7c91-191">If the operation successfully deletes the resource or resource group, it returns **True**.</span></span>

## <a name="run-resource-manager-scripts-with-azure-automation"></a><span data-ttu-id="b7c91-192">Az Azure Automation szolgáltatásban, erőforrás-kezelő parancsfájlok futtatása</span><span class="sxs-lookup"><span data-stu-id="b7c91-192">Run Resource Manager scripts with Azure Automation</span></span>

<span data-ttu-id="b7c91-193">Ez a témakör bemutatja, hogyan az erőforrások az Azure PowerShell alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="b7c91-193">This topic shows you how to perform basic operations on your resources with Azure PowerShell.</span></span> <span data-ttu-id="b7c91-194">Speciális kezelési forgatókönyvek esetén általában kívánt hozzon létre egy parancsfájlt, és újra felhasználhatja a parancsfájlhoz, igény szerint vagy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b7c91-194">For more advanced management scenarios, you typically want to create a script, and reuse that script as needed or on a schedule.</span></span> <span data-ttu-id="b7c91-195">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) biztosít, amelynek automatizálhatja a gyakran használt parancsfájlok, amelyek az Azure megoldások kezelése.</span><span class="sxs-lookup"><span data-stu-id="b7c91-195">[Azure Automation](../automation/automation-intro.md) provides a way for you to automate frequently used scripts that manage your Azure solutions.</span></span>

<span data-ttu-id="b7c91-196">A következő témakörökben láthatja, hogyan használható az Azure Automation, az erőforrás-kezelő és a PowerShell felügyeleti feladatok hatékony elvégzésére:</span><span class="sxs-lookup"><span data-stu-id="b7c91-196">The following topics show you how to use Azure Automation, Resource Manager, and PowerShell to effectively perform management tasks:</span></span>

- <span data-ttu-id="b7c91-197">Egy runbook létrehozásával kapcsolatos további információkért lásd: [az első PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-197">For information about creating a runbook, see [My first PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span></span>
- <span data-ttu-id="b7c91-198">További információ a gyűjtemények, parancsfájlok,: [az Azure Automation forgatókönyv- és gyűjtemények](../automation/automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-198">For information about working with galleries of scripts, see [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md).</span></span>
- <span data-ttu-id="b7c91-199">A runbookok, amelyek indítása és leállítása a virtuális gépek, lásd: [Azure Automation-forgatókönyv: címkék használatával JSON-formátumú az Azure virtuális gép indítási és leállítási ütemezés létrehozása](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-199">For runbooks that start and stop virtual machines, see [Azure Automation scenario: Using JSON-formatted tags to create a schedule for Azure VM startup and shutdown](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span></span>
- <span data-ttu-id="b7c91-200">A runbookok, amelyek indítása és leállítása a virtuális gépek munkaidőn kívüli, lásd: [indítása/leállítása virtuális gépek munkaidőn kívüli megoldás az automatizálás során](../automation/automation-solution-vm-management.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-200">For runbooks that start and stop virtual machines off-hours, see [Start/Stop VMs during off-hours solution in Automation](../automation/automation-solution-vm-management.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7c91-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7c91-201">Next steps</span></span>
* <span data-ttu-id="b7c91-202">Resource Manager-sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-202">To learn about creating Resource Manager templates, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b7c91-203">Sablonok telepítésével kapcsolatos további tudnivalókért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-203">To learn about deploying templates, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="b7c91-204">Meglévő erőforrásokat áthelyezheti egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b7c91-204">You can move existing resources to a new resource group.</span></span> <span data-ttu-id="b7c91-205">Tekintse meg a [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b7c91-205">For examples, see [Move Resources to New Resource Group or Subscription](resource-group-move-resources.md).</span></span>
* <span data-ttu-id="b7c91-206">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="b7c91-206">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

