---
title: "aaaCreate az Docker-tároló beállításjegyzék titkos - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerkedés a létrehozása és kezelése a saját Docker-tároló nyilvántartó, hello Azure CLI 2.0"
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
ms.openlocfilehash: f0d876a70b71a5e1bd564fbc9198f693dfe8a347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-cli-20"></a><span data-ttu-id="7e9f0-103">Hozza létre a titkos Docker tároló beállításkulcs hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="7e9f0-103">Create a private Docker container registry using hello Azure CLI 2.0</span></span>
<span data-ttu-id="7e9f0-104">Hello parancsokat használhatja [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate egy tároló beállításjegyzék és a beállítások kezelése a Linux, Mac vagy Windows számítógépről.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-104">Use commands in hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate a container registry and manage its settings from your Linux, Mac, or Windows computer.</span></span> <span data-ttu-id="7e9f0-105">Is hozhat létre, és hello segítségével kezeléséhez tároló [Azure-portálon](container-registry-get-started-portal.md) vagy programozott módon hello tároló beállításjegyzék [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-105">You can also create and manage container registries using hello [Azure portal](container-registry-get-started-portal.md) or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="7e9f0-106">Háttér és fogalmak: [hello áttekintése](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="7e9f0-106">For background and concepts, see [hello overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="7e9f0-107">A Súgó a tároló beállításjegyzék parancssori felület parancsait (`az acr` parancsok), adja át a hello `-h` paraméter tooany parancsot.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-107">For help on Container Registry CLI commands (`az acr` commands), pass hello `-h` parameter tooany command.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7e9f0-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7e9f0-108">Prerequisites</span></span>
* <span data-ttu-id="7e9f0-109">**Az Azure CLI 2.0**: tooinstall és hello CLI 2.0 első lépések című hello [telepítési utasításokat](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-109">**Azure CLI 2.0**: tooinstall and get started with hello CLI 2.0, see hello [installation instructions](/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="7e9f0-110">Jelentkezzen be Azure-előfizetés tooyour futtatásával `az login`.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-110">Log in tooyour Azure subscription by running `az login`.</span></span> <span data-ttu-id="7e9f0-111">További információkért lásd: [Ismerkedés a hello CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-111">For more information, see [Get started with hello CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="7e9f0-112">**Erőforráscsoport**: A tárolójegyzék létrehozása előtt hozzon létre egy [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md#resource-groups), vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="7e9f0-113">Ellenőrizze, hogy hello erőforráscsoport van egy helyet, ahol hello tároló beállításszolgáltatás [elérhető](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-113">Make sure hello resource group is in a location where hello Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="7e9f0-114">egy erőforrás csoport használatával toocreate hello CLI 2.0, lásd: [hello CLI 2.0 hivatkozás](/cli/azure/group).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-114">toocreate a resource group using hello CLI 2.0, see [hello CLI 2.0 reference](/cli/azure/group).</span></span>
* <span data-ttu-id="7e9f0-115">**A tárfiók** (nem kötelező): hozzon létre egy szabványos Azure [tárfiók](../storage/common/storage-introduction.md) tooback hello tároló hello rendszerleíró adatbázis ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) tooback hello container registry in hello same location.</span></span> <span data-ttu-id="7e9f0-116">Ha nem adja meg a storage-fiók létrehozásakor a beállításjegyzéket `az acr create`, hello parancs létrehoz egyet.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-116">If you don't specify a storage account when creating a registry with `az acr create`, hello command creates one for you.</span></span> <span data-ttu-id="7e9f0-117">a tárolási fiók használatával toocreate CLI 2.0 hello című [hello CLI 2.0 hivatkozás](/cli/azure/storage/account).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-117">toocreate a storage account using hello CLI 2.0, see [hello CLI 2.0 reference](/cli/azure/storage/account).</span></span> <span data-ttu-id="7e9f0-118">A Premium Storage jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="7e9f0-119">**Szolgáltatás egyszerű** (nem kötelező): Ha beállításjegyzékbeli hello CLI létrehozásához, alapértelmezés szerint nincs beállítva a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-119">**Service principal** (optional): When you create a registry with hello CLI, by default it is not set up for access.</span></span> <span data-ttu-id="7e9f0-120">Igényeitől függően rendelje hozzá egy meglévő Azure Active Directory szolgáltatás egyszerű tooa beállításjegyzék (vagy hozzon létre, és rendelje hozzá egy új), vagy engedélyezze a hello beállításjegyzék rendszergazda felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-120">Depending on your needs, you can assign an existing Azure Active Directory service principal tooa registry (or create and assign a new one), or enable hello registry's admin user account.</span></span> <span data-ttu-id="7e9f0-121">A cikk későbbi részében hello szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-121">See hello sections later in this article.</span></span> <span data-ttu-id="7e9f0-122">Beállításjegyzék-hozzáféréssel kapcsolatos további információkért lásd: [hitelesítés hello tároló rendszerleíró](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-122">For more information about registry access, see [Authenticate with hello container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="7e9f0-123">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e9f0-123">Create a container registry</span></span>
<span data-ttu-id="7e9f0-124">Futtassa a hello `az acr create` parancs toocreate tároló beállításjegyzékbeli.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-124">Run hello `az acr create` command toocreate a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="7e9f0-125">A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-125">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="7e9f0-126">hello beállításjegyzék neve hello példákban `myRegistry1`, de helyettesítse a saját, egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-126">hello registry name in hello examples is `myRegistry1`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="7e9f0-127">a következő parancsot használja hello minimális paraméterek toocreate tároló beállításjegyzék hello `myRegistry1` hello erőforráscsoportban `myResourceGroup`, és hello segítségével *alapvető* termékváltozat:</span><span class="sxs-lookup"><span data-stu-id="7e9f0-127">hello following command uses hello minimal parameters toocreate container registry `myRegistry1` in hello resource group `myResourceGroup`, and using hello *Basic* sku:</span></span>

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* <span data-ttu-id="7e9f0-128">A(z) `--storage-account-name` nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-128">`--storage-account-name` is optional.</span></span> <span data-ttu-id="7e9f0-129">Ha nem megadott, a storage-fiók létrejön a neve, amely hello beállításjegyzék neve és hello időbélyegzőnek megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-129">If not specified, a storage account is created with a name consisting of hello registry name and a timestamp in hello specified resource group.</span></span>

<span data-ttu-id="7e9f0-130">Hello beállításjegyzék létrehozásakor hello kimenete hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="7e9f0-130">When hello registry is created, hello output is similar toohello following:</span></span>

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


<span data-ttu-id="7e9f0-131">Különösen ügyeljen a következőre:</span><span class="sxs-lookup"><span data-stu-id="7e9f0-131">Take special note:</span></span>

* <span data-ttu-id="7e9f0-132">`id`-Előfizetését, ha azt szeretné, hogy a szolgáltatás egyszerű tooassign igénylő hello beállításjegyzék azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-132">`id` - Identifier for hello registry in your subscription, which you need if you want tooassign a service principal.</span></span>
* <span data-ttu-id="7e9f0-133">`loginServer`-hello teljesen minősített név túl meg[toohello beállításjegyzék-e jelentkezni](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-133">`loginServer` - hello fully qualified name you specify too[log in toohello registry](container-registry-authentication.md).</span></span> <span data-ttu-id="7e9f0-134">Ebben a példában hello neve: `myregistry1.exp.azurecr.io` (kisbetűket).</span><span class="sxs-lookup"><span data-stu-id="7e9f0-134">In this example, hello name is `myregistry1.exp.azurecr.io` (all lowercase).</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="7e9f0-135">Egyszerű szolgáltatás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7e9f0-135">Assign a service principal</span></span>
<span data-ttu-id="7e9f0-136">Parancssori felület 2.0 parancsok tooassign egy Azure Active Directory szolgáltatás egyszerű tooa beállításjegyzék használja.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-136">Use CLI 2.0 commands tooassign an Azure Active Directory service principal tooa registry.</span></span> <span data-ttu-id="7e9f0-137">hello szolgáltatás egyszerű ezekben a példákban szerepét hello tulajdonos, de rendelhet [más szerepkörök](../active-directory/role-based-access-control-configure.md) irányíthatók.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-137">hello service principal in these examples is assigned hello Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal-and-assign-access-toohello-registry"></a><span data-ttu-id="7e9f0-138">Egy egyszerű szolgáltatás létrehozása és hozzárendelése hozzáférés toohello beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="7e9f0-138">Create a service principal and assign access toohello registry</span></span>
<span data-ttu-id="7e9f0-139">A következő parancs hello, egy új szolgáltatás egyszerű tulajdonos szerepkör hozzáférés toohello beállításjegyzék-azonosító hello átadni kapja `--scopes` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-139">In hello following command, a new service principal is assigned Owner role access toohello registry identifier passed with hello `--scopes` parameter.</span></span> <span data-ttu-id="7e9f0-140">Adjon meg egy erős jelszót a hello `--password` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-140">Specify a strong password with hello `--password` parameter.</span></span>

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a><span data-ttu-id="7e9f0-141">Meglévő egyszerű szolgáltatás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7e9f0-141">Assign an existing service principal</span></span>
<span data-ttu-id="7e9f0-142">Ha már rendelkezik egy egyszerű szolgáltatást, és szeretné, hogy tooassign azt tulajdonos szerepkör hozzáférés toohello beállításjegyzék, futtassa a következő példa egy parancshoz hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-142">If you already have a service principal and want tooassign it Owner role access toohello registry, run a command similar toohello following example.</span></span> <span data-ttu-id="7e9f0-143">Hello szolgáltatás egyszerű Alkalmazásazonosító hello segítségével át `--assignee` paraméter:</span><span class="sxs-lookup"><span data-stu-id="7e9f0-143">You pass hello service principal app ID using hello `--assignee` parameter:</span></span>

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a><span data-ttu-id="7e9f0-144">Rendszergazdai hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="7e9f0-144">Manage admin credentials</span></span>
<span data-ttu-id="7e9f0-145">Minden tároló-beállításjegyzékhez automatikusan létrejön egy rendszergazdai fiók, amely alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-145">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="7e9f0-146">a következő példák szemléltetik hello `az acr` parancssori felület parancsai toomanage hello rendszergazdai hitelesítő adatait a tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-146">hello following examples show `az acr` CLI commands toomanage hello admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="7e9f0-147">Rendszergazdai hitelesítő adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="7e9f0-147">Obtain admin user credentials</span></span>
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="7e9f0-148">Rendszergazdai felhasználó engedélyezése meglévő beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="7e9f0-148">Enable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="7e9f0-149">Rendszergazdai felhasználó letiltása meglévő beállításjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="7e9f0-149">Disable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a><span data-ttu-id="7e9f0-150">Képek és címkék listázása</span><span class="sxs-lookup"><span data-stu-id="7e9f0-150">List images and tags</span></span>
<span data-ttu-id="7e9f0-151">Használjon hello `az acr` parancssori felület parancsai tooquery hello képek és címkék tárházban.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-151">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="7e9f0-152">Jelenleg, tároló-beállításjegyzék nem támogatja a hello `docker search` parancs tooquery képek és címkék.</span><span class="sxs-lookup"><span data-stu-id="7e9f0-152">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>


### <a name="list-repositories"></a><span data-ttu-id="7e9f0-153">Adattárak listázása</span><span class="sxs-lookup"><span data-stu-id="7e9f0-153">List repositories</span></span>
<span data-ttu-id="7e9f0-154">hello alábbi példa felsorolja a beállításjegyzék (JavaScript Object Notation) JSON formátumban hello adattárak:</span><span class="sxs-lookup"><span data-stu-id="7e9f0-154">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="7e9f0-155">Címkék listázása</span><span class="sxs-lookup"><span data-stu-id="7e9f0-155">List tags</span></span>
<span data-ttu-id="7e9f0-156">hello alábbi példa felsorolja a hello hello címkék **minták/nginx** tárház JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="7e9f0-156">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="7e9f0-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e9f0-157">Next steps</span></span>
* [<span data-ttu-id="7e9f0-158">Leküldéses az első kép hello Docker parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="7e9f0-158">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
