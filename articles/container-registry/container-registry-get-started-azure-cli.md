---
title: "Privát Docker-tárolójegyzék létrehozása – Azure CLI | Microsoft Docs"
description: "Bevezetés a privát Docker-tárolójegyzékek létrehozásába és kezelésébe az Azure CLI 2.0 segítségével"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2875f4089231ed12a0312b2c2e077938440365c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-private-docker-container-registry-using-the-azure-cli-20"></a><span data-ttu-id="ff908-103">Privát Docker-tárolójegyzék létrehozása az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="ff908-103">Create a private Docker container registry using the Azure CLI 2.0</span></span>
<span data-ttu-id="ff908-104">Az [Azure CLI 2.0](https://github.com/Azure/azure-cli) parancsaival létrehozhat egy tároló-beállításjegyzéket, és kezelheti annak beállításait Linux, Mac vagy Windows rendszerű számítógépéről.</span><span class="sxs-lookup"><span data-stu-id="ff908-104">Use commands in the [Azure CLI 2.0](https://github.com/Azure/azure-cli) to create a container registry and manage its settings from your Linux, Mac, or Windows computer.</span></span> <span data-ttu-id="ff908-105">A tároló-beállításjegyzékeket létrehozhatja és kezelheti az [Azure Portalon](container-registry-get-started-portal.md) vagy programozott módon a tároló-beállításjegyzék [REST API-jával](https://go.microsoft.com/fwlink/p/?linkid=834376) is.</span><span class="sxs-lookup"><span data-stu-id="ff908-105">You can also create and manage container registries using the [Azure portal](container-registry-get-started-portal.md) or programmatically with the Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="ff908-106">Háttér-információkért és a fogalmakkal kapcsolatban lásd [az áttekintést](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="ff908-106">For background and concepts, see [the overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="ff908-107">A Container Registry parancssori felületének parancsaival (`az acr` parancsok) kapcsolatos segítségért adja át a `-h` paramétert egy parancsnak.</span><span class="sxs-lookup"><span data-stu-id="ff908-107">For help on Container Registry CLI commands (`az acr` commands), pass the `-h` parameter to any command.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ff908-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff908-108">Prerequisites</span></span>
* <span data-ttu-id="ff908-109">**Azure CLI 2.0**: A CLI 2.0 telepítéshez és megismeréséhez tekintse meg a [telepítési utasításokat](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ff908-109">**Azure CLI 2.0**: To install and get started with the CLI 2.0, see the [installation instructions](/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="ff908-110">Jelentkezzen be Azure-előfizetésébe az `az login` futtatásával.</span><span class="sxs-lookup"><span data-stu-id="ff908-110">Log in to your Azure subscription by running `az login`.</span></span> <span data-ttu-id="ff908-111">További információkért lásd a [CLI 2.0 használatának első lépéseit](/cli/azure/get-started-with-azure-cli) ismertető témakört.</span><span class="sxs-lookup"><span data-stu-id="ff908-111">For more information, see [Get started with the CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="ff908-112">**Erőforráscsoport**: A tárolójegyzék létrehozása előtt hozzon létre egy [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md#resource-groups), vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ff908-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="ff908-113">Győződjön meg arról, hogy az erőforráscsoport olyan helyen található, ahol a Container Registry szolgáltatás [elérhető](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="ff908-113">Make sure the resource group is in a location where the Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="ff908-114">Ha a CLI 2.0-val szeretne erőforráscsoportot létrehozni, tekintse meg [a CLI 2.0-referenciát](/cli/azure/group).</span><span class="sxs-lookup"><span data-stu-id="ff908-114">To create a resource group using the CLI 2.0, see [the CLI 2.0 reference](/cli/azure/group).</span></span>
* <span data-ttu-id="ff908-115">**Storage-fiók** (nem kötelező): Hozzon létre egy standard Azure [Storage-fiókot](../storage/common/storage-introduction.md) a tárolójegyzékhez a tárolójegyzékkel megegyező helyen.</span><span class="sxs-lookup"><span data-stu-id="ff908-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) to back the container registry in the same location.</span></span> <span data-ttu-id="ff908-116">Ha nem ad meg Storage-fiókot, amikor létrehozza a beállításjegyzéket az `az acr create` paranccsal, a parancs létrehoz egyet.</span><span class="sxs-lookup"><span data-stu-id="ff908-116">If you don't specify a storage account when creating a registry with `az acr create`, the command creates one for you.</span></span> <span data-ttu-id="ff908-117">Ha a CLI 2.0-val szeretne tárfiókot létrehozni, tekintse meg [a CLI 2.0-referenciát](/cli/azure/storage/account).</span><span class="sxs-lookup"><span data-stu-id="ff908-117">To create a storage account using the CLI 2.0, see [the CLI 2.0 reference](/cli/azure/storage/account).</span></span> <span data-ttu-id="ff908-118">A Premium Storage jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ff908-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="ff908-119">**Egyszerű szolgáltatás** (nem kötelező): Ha a parancssori felülettel hoz létre beállításjegyzéket, az alapértelmezés szerint nem lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="ff908-119">**Service principal** (optional): When you create a registry with the CLI, by default it is not set up for access.</span></span> <span data-ttu-id="ff908-120">Igény szerint a beállításjegyzékhez hozzárendelhet egy meglévő Azure Active Directory egyszerű szolgáltatást (vagy létrehozhat és hozzárendelhet egy újat), vagy engedélyezheti a beállításjegyzék rendszergazdai felhasználói fiókját.</span><span class="sxs-lookup"><span data-stu-id="ff908-120">Depending on your needs, you can assign an existing Azure Active Directory service principal to a registry (or create and assign a new one), or enable the registry's admin user account.</span></span> <span data-ttu-id="ff908-121">Ezzel kapcsolatban lásd a cikk későbbi részeit.</span><span class="sxs-lookup"><span data-stu-id="ff908-121">See the sections later in this article.</span></span> <span data-ttu-id="ff908-122">A beállításjegyzék elérésével kapcsolatos további információkat a [tároló-beállításjegyzékkel való hitelesítéssel kapcsolatos cikkben](container-registry-authentication.md) találhat.</span><span class="sxs-lookup"><span data-stu-id="ff908-122">For more information about registry access, see [Authenticate with the container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="ff908-123">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff908-123">Create a container registry</span></span>
<span data-ttu-id="ff908-124">Futtassa az `az acr create` parancsot egy tároló-beállításjegyzék létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ff908-124">Run the `az acr create` command to create a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="ff908-125">A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ff908-125">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="ff908-126">A beállításjegyzék neve a példákban `myRegistry1`, de helyettesítsen be egy saját, egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="ff908-126">The registry name in the examples is `myRegistry1`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="ff908-127">Az alábbi parancs az *alapszintű* termékváltozattal a minimális paramétereket használja a tárolóregisztrációs adatbázis (`myRegistry1`) `myResourceGroup` erőforráscsoportban való létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="ff908-127">The following command uses the minimal parameters to create container registry `myRegistry1` in the resource group `myResourceGroup`, and using the *Basic* sku:</span></span>

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* <span data-ttu-id="ff908-128">A(z) `--storage-account-name` nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="ff908-128">`--storage-account-name` is optional.</span></span> <span data-ttu-id="ff908-129">Ha nincs megadva, a Storage-fiók a beállításjegyzék nevéből és egy időbélyegzőből álló névvel jön létre a megadott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ff908-129">If not specified, a storage account is created with a name consisting of the registry name and a timestamp in the specified resource group.</span></span>

<span data-ttu-id="ff908-130">A tárolóregisztrációs adatbázis létrehozásakor a kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="ff908-130">When the registry is created, the output is similar to the following:</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T18:36:29.124842+00:00",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry
/registries/myRegistry1",
  "location": "southcentralus",
  "loginServer": "myregistry1.azurecr.io",
  "name": "myRegistry1",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myregistry123456789"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```


<span data-ttu-id="ff908-131">Különösen ügyeljen a következőre:</span><span class="sxs-lookup"><span data-stu-id="ff908-131">Take special note:</span></span>

* <span data-ttu-id="ff908-132">`id` – Az előfizetésben lévő beállításjegyzék azonosítója, amelyre akkor van szükség, ha egyszerű szolgáltatást szeretne hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="ff908-132">`id` - Identifier for the registry in your subscription, which you need if you want to assign a service principal.</span></span>
* <span data-ttu-id="ff908-133">`loginServer` – A teljes név, amelyet a [beállításjegyzékbe való bejelentkezéshez](container-registry-authentication.md) ad meg.</span><span class="sxs-lookup"><span data-stu-id="ff908-133">`loginServer` - The fully qualified name you specify to [log in to the registry](container-registry-authentication.md).</span></span> <span data-ttu-id="ff908-134">Ebben a példában a név `myregistry1.exp.azurecr.io` (csak kisbetűkkel).</span><span class="sxs-lookup"><span data-stu-id="ff908-134">In this example, the name is `myregistry1.exp.azurecr.io` (all lowercase).</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="ff908-135">Egyszerű szolgáltatás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ff908-135">Assign a service principal</span></span>
<span data-ttu-id="ff908-136">Azure Active Directory egyszerű szolgáltatások beállításjegyzékhez való hozzárendeléséhez a CLI 2.0 parancsait használhatja.</span><span class="sxs-lookup"><span data-stu-id="ff908-136">Use CLI 2.0 commands to assign an Azure Active Directory service principal to a registry.</span></span> <span data-ttu-id="ff908-137">A példákban szereplő egyszerű szolgáltatáshoz a tulajdonosi szerepkör van hozzárendelve, de [más szerepköröket](../active-directory/role-based-access-control-configure.md) is hozzárendelhet, ha szeretne.</span><span class="sxs-lookup"><span data-stu-id="ff908-137">The service principal in these examples is assigned the Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal-and-assign-access-to-the-registry"></a><span data-ttu-id="ff908-138">Egyszerű szolgáltatás létrehozása és hozzáférés biztosítása a beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="ff908-138">Create a service principal and assign access to the registry</span></span>
<span data-ttu-id="ff908-139">Az alábbi parancs tulajdonosi szerepkör szintű hozzáférést biztosít az új egyszerű szolgáltatás számára a `--scopes` paraméterrel átadott beállításjegyzék-azonosítóhoz.</span><span class="sxs-lookup"><span data-stu-id="ff908-139">In the following command, a new service principal is assigned Owner role access to the registry identifier passed with the `--scopes` parameter.</span></span> <span data-ttu-id="ff908-140">Adjon meg egy erős jelszót a `--password` paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="ff908-140">Specify a strong password with the `--password` parameter.</span></span>

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a><span data-ttu-id="ff908-141">Meglévő egyszerű szolgáltatás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ff908-141">Assign an existing service principal</span></span>
<span data-ttu-id="ff908-142">Ha már rendelkezik egyszerű szolgáltatással, és tulajdonosi szerepkör szintű hozzáférést szeretne számára biztosítani a beállításjegyzékhez, futtasson egy, a következő példához hasonló parancsot.</span><span class="sxs-lookup"><span data-stu-id="ff908-142">If you already have a service principal and want to assign it Owner role access to the registry, run a command similar to the following example.</span></span> <span data-ttu-id="ff908-143">Az egyszerű szolgáltatás alkalmazásazonosítóját az `--assignee` paraméterrel lehet átadni:</span><span class="sxs-lookup"><span data-stu-id="ff908-143">You pass the service principal app ID using the `--assignee` parameter:</span></span>

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a><span data-ttu-id="ff908-144">Rendszergazdai hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="ff908-144">Manage admin credentials</span></span>
<span data-ttu-id="ff908-145">Minden tároló-beállításjegyzékhez automatikusan létrejön egy rendszergazdai fiók, amely alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="ff908-145">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="ff908-146">Az alábbi példák `az acr` parancssori felületi parancsokat mutatnak be a tároló-beállításjegyzék rendszergazdai hitelesítő adatainak kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="ff908-146">The following examples show `az acr` CLI commands to manage the admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="ff908-147">Rendszergazdai hitelesítő adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="ff908-147">Obtain admin user credentials</span></span>
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="ff908-148">Rendszergazdai felhasználó engedélyezése meglévő beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="ff908-148">Enable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="ff908-149">Rendszergazdai felhasználó letiltása meglévő beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="ff908-149">Disable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a><span data-ttu-id="ff908-150">Képek és címkék listázása</span><span class="sxs-lookup"><span data-stu-id="ff908-150">List images and tags</span></span>
<span data-ttu-id="ff908-151">Az `az acr` parancssori felületi parancsokkal lekérdezheti az adattárakban tárolt képeket és címkéket.</span><span class="sxs-lookup"><span data-stu-id="ff908-151">Use the `az acr` CLI commands to query the images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="ff908-152">A Container Registry jelenleg nem támogatja a `docker search` parancsot a képek és címkék lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ff908-152">Currently, Container Registry does not support the `docker search` command to query for images and tags.</span></span>


### <a name="list-repositories"></a><span data-ttu-id="ff908-153">Adattárak listázása</span><span class="sxs-lookup"><span data-stu-id="ff908-153">List repositories</span></span>
<span data-ttu-id="ff908-154">A következő példa egy beállításjegyzék adattárait listázza JSON (JavaScript Object Notation) formátumban:</span><span class="sxs-lookup"><span data-stu-id="ff908-154">The following example lists the repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="ff908-155">Címkék listázása</span><span class="sxs-lookup"><span data-stu-id="ff908-155">List tags</span></span>
<span data-ttu-id="ff908-156">A következő példa a **samples/nginx** adattárban lévő címkéket listázza JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="ff908-156">The following example lists the tags on the **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="ff908-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff908-157">Next steps</span></span>
* [<span data-ttu-id="ff908-158">Az első rendszerkép leküldése a Docker parancssori felületével</span><span class="sxs-lookup"><span data-stu-id="ff908-158">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
