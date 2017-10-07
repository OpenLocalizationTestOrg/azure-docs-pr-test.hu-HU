---
title: "aaaDeploy tooAzure tároló példányok a hello Azure tároló beállításjegyzék |} Az Azure Docs"
description: "Hello Azure tároló beállításjegyzék tooAzure tároló példányok telepítése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2667f91db8ed92a9ccc9ba722a2b1f5c5ea93886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-container-instances-from-hello-azure-container-registry"></a><span data-ttu-id="06f03-103">Hello Azure tároló beállításjegyzék tooAzure tároló példányok telepítése</span><span class="sxs-lookup"><span data-stu-id="06f03-103">Deploy tooAzure Container Instances from hello Azure Container Registry</span></span>

<span data-ttu-id="06f03-104">hello Azure tároló beállításjegyzék egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek.</span><span class="sxs-lookup"><span data-stu-id="06f03-104">hello Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="06f03-105">Ez a cikk ismerteti, hogyan toodeploy tároló képek tárolt hello Azure tároló beállításjegyzék tooAzure tároló példányok.</span><span class="sxs-lookup"><span data-stu-id="06f03-105">This article covers how toodeploy container images stored in hello Azure Container Registry tooAzure Container Instances.</span></span>

## <a name="using-hello-azure-cli"></a><span data-ttu-id="06f03-106">Hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="06f03-106">Using hello Azure CLI</span></span>

<span data-ttu-id="06f03-107">az Azure parancssori felület hello létrehozása és kezelése az Azure-tároló példányok a tárolók parancsokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="06f03-107">hello Azure CLI includes commands for creating and managing containers in Azure Container Instances.</span></span> <span data-ttu-id="06f03-108">Ha megad egy titkos kép hello `create` parancs hello kép beállításjegyzék jelszó szükséges tooauthenticate hello tároló beállításjegyzék esetében is megadhat.</span><span class="sxs-lookup"><span data-stu-id="06f03-108">If you specify a private image in hello `create` command, you can also specify hello image registry password required tooauthenticate with hello container registry.</span></span>

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

<span data-ttu-id="06f03-109">Hello `create` parancsot is támogatja a hello megadása `registry-login-server` és `registry-username`.</span><span class="sxs-lookup"><span data-stu-id="06f03-109">hello `create` command also supports specifying hello `registry-login-server` and `registry-username`.</span></span> <span data-ttu-id="06f03-110">Hello bejelentkezési kiszolgáló hello Azure tároló beállításjegyzék azonban mindig *registryname*. azurecr.io és hello alapértelmezett felhasználónév az *registryname*, így ezek az értékek kikövetkeztetve a hello kép neve, ha nincs kifejezetten megadva.</span><span class="sxs-lookup"><span data-stu-id="06f03-110">However, hello login server for hello Azure Container Registry is always *registryname*.azurecr.io and hello default username is *registryname*, so these values are inferred from hello image name if not explicitly provided.</span></span>

## <a name="using-an-azure-resource-manager-template"></a><span data-ttu-id="06f03-111">Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="06f03-111">Using an Azure Resource Manager template</span></span>

<span data-ttu-id="06f03-112">Tulajdonságok is megadhatók hello az Azure-tároló beállításjegyzék Azure Resource Manager sablon hello-ot `imageRegistryCredentials` tulajdonság hello tartalmazó csoport definícióban:</span><span class="sxs-lookup"><span data-stu-id="06f03-112">You can specify hello properties of your Azure Container Registry in an Azure Resource Manager template by including hello `imageRegistryCredentials` property in hello container group definition:</span></span>

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

<span data-ttu-id="06f03-113">a tároló beállításjegyzék jelszó tárolása közvetlenül hello sablon tooavoid azt javasoljuk, hogy tárolja azt, a titkos kulcs [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) és hello sablon használatával hello vonatkozó hivatkozást [natív integráció az között hello Azure Resource Manager és a Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="06f03-113">tooavoid storing your container registry password directly in hello template, we recommend that you store it as a secret in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) and reference it in hello template using hello [native integration between hello Azure Resource Manager and Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span>

## <a name="using-hello-azure-portal"></a><span data-ttu-id="06f03-114">Hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="06f03-114">Using hello Azure portal</span></span>

<span data-ttu-id="06f03-115">Karbantartása hello Azure tároló beállításjegyzék tároló lemezképeihez, ha Azure tároló példányát hello Azure-portál használatával könnyen létrehozhat egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="06f03-115">If you maintain container images in hello Azure Container Registry, you can easily create a container in Azure Container Instances using hello Azure portal.</span></span>

1. <span data-ttu-id="06f03-116">Hello Azure-portálon lépjen a tooyour tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="06f03-116">In hello Azure portal, navigate tooyour container registry.</span></span>

2. <span data-ttu-id="06f03-117">Válassza ki a tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="06f03-117">Choose Repositories.</span></span>

    ![hello Azure tároló beállításjegyzék menüjében hello Azure-portálon][acr-menu]

3. <span data-ttu-id="06f03-119">Válassza ki, amelyet a toodeploy hello tárház.</span><span class="sxs-lookup"><span data-stu-id="06f03-119">Choose hello repository that you want toodeploy from.</span></span>

4. <span data-ttu-id="06f03-120">Kattintson a jobb gombbal a hello címke hello tároló kép toodeploy keresi.</span><span class="sxs-lookup"><span data-stu-id="06f03-120">Right-click hello tag for hello container image you want toodeploy.</span></span>

    ![Helyi menü az Azure-tároló példányaival tároló megnyitása][acr-runinstance-contextmenu]

5. <span data-ttu-id="06f03-122">Adja meg egy nevet a hello tároló és hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="06f03-122">Enter a name for hello container and a name for hello resource group.</span></span> <span data-ttu-id="06f03-123">Ha kívánja, hello alapértelmezett értékeit módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="06f03-123">You can also change hello default values if you wish.</span></span>

    ![Azure-tároló példányok menü létrehozása][acr-create-deeplink]

6. <span data-ttu-id="06f03-125">Hello telepítése után a toohello tároló csoporthoz hello értesítések ablaktábla toofind navigálhat az IP-címét és egyéb tulajdonságok is.</span><span class="sxs-lookup"><span data-stu-id="06f03-125">Once hello deployment completes, you can navigate toohello container group from hello notifications pane toofind its IP address and other properties.</span></span>

    ![Az Azure-tároló példányok a tárolócsoport Részletek nézet][aci-detailsview]

## <a name="next-steps"></a><span data-ttu-id="06f03-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06f03-127">Next steps</span></span>

<span data-ttu-id="06f03-128">Megtudhatja, hogyan toobuild tárolók tooa személyes tárolót beállításjegyzék küldje le őket, és telepítheti azokat tooAzure tároló példányokat [hello az oktatóanyagnak az elvégzése](container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="06f03-128">Learn how toobuild containers, push them tooa private container registry, and deploy them tooAzure Container Instances by [completing hello tutorial](container-instances-tutorial-prepare-app.md).</span></span>

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
