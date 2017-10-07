---
title: "aaaAzure tároló beállításjegyzék adattárak |} Microsoft Docs"
description: "Hogyan toouse Azure tároló beállításjegyzék tárolóhelyekkel Docker lemezképek"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.openlocfilehash: 448fb812f537c9502041ce5fb372b0681a9dac4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-powershell"></a><span data-ttu-id="19660-103">Hozza létre a titkos Docker tároló beállításkulcs hello Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="19660-103">Create a private Docker container registry using hello Azure PowerShell</span></span>
<span data-ttu-id="19660-104">Parancsok használata [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate egy tároló beállításjegyzék és a beállítások kezelése a Windows rendszerű számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19660-104">Use commands in [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate a container registry and manage its settings from your Windows computer.</span></span> <span data-ttu-id="19660-105">Is hozhat létre, és hello segítségével kezeléséhez tároló [Azure-portálon](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), vagy programozott módon hello tároló beállításjegyzék [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="19660-105">You can also create and manage container registries using hello [Azure portal](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="19660-106">Háttér és fogalmak: [hello áttekintése](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="19660-106">For background and concepts, see [hello overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="19660-107">Támogatott parancsmagok teljes listáját lásd: [Azure tároló beállításjegyzék parancsmagokat](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span><span class="sxs-lookup"><span data-stu-id="19660-107">For a full list of supported cmdlets, see [Azure Container Registry Management Cmdlets](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="19660-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="19660-108">Prerequisites</span></span>
* <span data-ttu-id="19660-109">**Az Azure PowerShell**: tooinstall és Ismerkedés az Azure PowerShell, lásd: hello [telepítési utasításokat](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="19660-109">**Azure PowerShell**: tooinstall and get started with Azure PowerShell, see hello [installation instructions](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> <span data-ttu-id="19660-110">Jelentkezzen be Azure-előfizetés tooyour futtatásával `Login-AzureRMAccount`.</span><span class="sxs-lookup"><span data-stu-id="19660-110">Log in tooyour Azure subscription by running `Login-AzureRMAccount`.</span></span> <span data-ttu-id="19660-111">További információkért lásd: [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span><span class="sxs-lookup"><span data-stu-id="19660-111">For more information, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span></span>
* <span data-ttu-id="19660-112">**Erőforráscsoport**: A tárolójegyzék létrehozása előtt hozzon létre egy [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md#resource-groups), vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="19660-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="19660-113">Ellenőrizze, hogy hello erőforráscsoport van egy helyet, ahol hello tároló beállításszolgáltatás [elérhető](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="19660-113">Make sure hello resource group is in a location where hello Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="19660-114">toocreate egy erőforráscsoportot az Azure PowerShell, lásd: [PowerShell-referencia hello](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span><span class="sxs-lookup"><span data-stu-id="19660-114">toocreate a resource group using Azure PowerShell, see [hello PowerShell reference](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span></span>
* <span data-ttu-id="19660-115">**A tárfiók** (nem kötelező): hozzon létre egy szabványos Azure [tárfiók](../storage/common/storage-introduction.md) tooback hello tároló hello rendszerleíró adatbázis ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="19660-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) tooback hello container registry in hello same location.</span></span> <span data-ttu-id="19660-116">Ha nem adja meg a storage-fiók létrehozásakor a beállításjegyzéket `New-AzureRMContainerRegistry`, hello parancs létrehoz egyet.</span><span class="sxs-lookup"><span data-stu-id="19660-116">If you don't specify a storage account when creating a registry with `New-AzureRMContainerRegistry`, hello command creates one for you.</span></span> <span data-ttu-id="19660-117">toocreate tárolási fiók PowerShell segítségével, lásd: [PowerShell-referencia hello](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span><span class="sxs-lookup"><span data-stu-id="19660-117">toocreate a storage account using PowerShell, see [hello PowerShell reference](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span></span> <span data-ttu-id="19660-118">A Premium Storage jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="19660-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="19660-119">**Szolgáltatás egyszerű** (nem kötelező): a beállításjegyzék a PowerShell használatával való létrehozásakor alapértelmezés szerint azt nincs beállítva a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="19660-119">**Service principal** (optional): When you create a registry with PowerShell, by default it is not set up for access.</span></span> <span data-ttu-id="19660-120">Igényeitől függően rendelhet egy meglévő Azure Active Directory szolgáltatás egyszerű tooa beállításjegyzék vagy létrehozása és hozzárendelése egy újat.</span><span class="sxs-lookup"><span data-stu-id="19660-120">Depending on your needs, you can assign an existing Azure Active Directory service principal tooa registry or create and assign a new one.</span></span> <span data-ttu-id="19660-121">Másik lehetőségként engedélyezheti a hello beállításjegyzék rendszergazda felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="19660-121">Alternatively, you can enable hello registry's admin user account.</span></span> <span data-ttu-id="19660-122">A cikk későbbi részében hello szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="19660-122">See hello sections later in this article.</span></span> <span data-ttu-id="19660-123">Beállításjegyzék-hozzáféréssel kapcsolatos további információkért lásd: [hitelesítés hello tároló rendszerleíró](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="19660-123">For more information about registry access, see [Authenticate with hello container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="19660-124">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="19660-124">Create a container registry</span></span>
<span data-ttu-id="19660-125">Futtassa a hello `New-AzureRMContainerRegistry` parancs toocreate tároló beállításjegyzékbeli.</span><span class="sxs-lookup"><span data-stu-id="19660-125">Run hello `New-AzureRMContainerRegistry` command toocreate a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="19660-126">A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="19660-126">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="19660-127">hello beállításjegyzék neve hello példákban `MyRegistry`, de helyettesítse a saját, egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="19660-127">hello registry name in hello examples is `MyRegistry`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="19660-128">a következő parancsot használja hello minimális paraméterek toocreate tároló beállításjegyzék hello `MyRegistry` hello erőforráscsoportban `MyResourceGroup` hello déli középső Régiójában helyen található:</span><span class="sxs-lookup"><span data-stu-id="19660-128">hello following command uses hello minimal parameters toocreate container registry `MyRegistry` in hello resource group `MyResourceGroup` in hello South Central US location:</span></span>

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* <span data-ttu-id="19660-129">A(z) `-StorageAccountName` nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="19660-129">`-StorageAccountName` is optional.</span></span> <span data-ttu-id="19660-130">Ha nem megadott, a storage-fiók létrejön a neve, amely hello beállításjegyzék neve és hello időbélyegzőnek megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="19660-130">If not specified, a storage account is created with a name consisting of hello registry name and a timestamp in hello specified resource group.</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="19660-131">Egyszerű szolgáltatás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="19660-131">Assign a service principal</span></span>
<span data-ttu-id="19660-132">Használjon PowerShell-parancsok tooassign egy Azure Active Directory [egyszerű](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="19660-132">Use PowerShell commands tooassign an Azure Active Directory [service principal](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registry.</span></span> <span data-ttu-id="19660-133">hello szolgáltatás egyszerű ezekben a példákban szerepét hello tulajdonos, de rendelhet [más szerepkörök](../active-directory/role-based-access-control-configure.md) irányíthatók.</span><span class="sxs-lookup"><span data-stu-id="19660-133">hello service principal in these examples is assigned hello Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal"></a><span data-ttu-id="19660-134">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="19660-134">Create a service principal</span></span>
<span data-ttu-id="19660-135">A következő parancs hello egy új szolgáltatás egyszerű jön létre.</span><span class="sxs-lookup"><span data-stu-id="19660-135">In hello following command, a new service principal is created.</span></span> <span data-ttu-id="19660-136">Adjon meg egy erős jelszót a hello `-Password` paraméter.</span><span class="sxs-lookup"><span data-stu-id="19660-136">Specify a strong password with hello `-Password` parameter.</span></span>

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a><span data-ttu-id="19660-137">Rendelje hozzá egy új vagy meglévő egyszerű</span><span class="sxs-lookup"><span data-stu-id="19660-137">Assign a new or existing service principal</span></span>
<span data-ttu-id="19660-138">Egy új vagy egy meglévő szolgáltatás egyszerű tooa beállításjegyzék is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="19660-138">You can assign a new or an existing service principal tooa registry.</span></span> <span data-ttu-id="19660-139">tooassign azt tulajdonos szerepkör hozzáférés toohello beállításjegyzék, futtassa a következő példa egy parancshoz hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="19660-139">tooassign it Owner role access toohello registry, run a command similar toohello following example:</span></span>

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-toohello-registry-with-hello-service-principal"></a><span data-ttu-id="19660-140">Jelentkezzen be toohello beállításjegyzéket hello egyszerű szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="19660-140">Sign in toohello registry with hello service principal</span></span>
<span data-ttu-id="19660-141">Miután hello szolgáltatás egyszerű toohello beállításjegyzék, bármikor beléphet hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="19660-141">After assigning hello service principal toohello registry, you can sign in using hello following command:</span></span>

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a><span data-ttu-id="19660-142">Rendszergazdai hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="19660-142">Manage admin credentials</span></span>
<span data-ttu-id="19660-143">Minden tároló-beállításjegyzékhez automatikusan létrejön egy rendszergazdai fiók, amely alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="19660-143">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="19660-144">hello következő példák azt szemléltetik PowerShell-parancsokat a tároló rendszerleíró toomanage hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="19660-144">hello following examples show PowerShell commands toomanage hello admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="19660-145">Rendszergazdai hitelesítő adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="19660-145">Obtain admin user credentials</span></span>
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="19660-146">Rendszergazdai felhasználó engedélyezése meglévő beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="19660-146">Enable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="19660-147">Rendszergazdai felhasználó letiltása meglévő beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="19660-147">Disable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a><span data-ttu-id="19660-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19660-148">Next steps</span></span>
* [<span data-ttu-id="19660-149">Leküldéses az első kép hello Docker parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="19660-149">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
