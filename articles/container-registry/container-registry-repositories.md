---
title: "Azure-tárolót beállításjegyzék adattárak |} Microsoft Docs"
description: "Azure-tároló beállításjegyzék adattárak Docker lemezképek használata"
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
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 06b809c31cecef1714f60d04657eb74c611be8cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="e98f5-103">Azure-tárolót beállításjegyzék adattárak</span><span class="sxs-lookup"><span data-stu-id="e98f5-103">Azure container registry repositories</span></span>

<span data-ttu-id="e98f5-104">Azure-tárolót beállításjegyzék tároló lemezképek tárolása adattárak teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="e98f5-104">Azure container registry allows you to store container images in repositories.</span></span> <span data-ttu-id="e98f5-105">Lemezképek tárolása tárházak találhatók, akkor elkülönített környezetben csoportok lemezképet (vagy képeket verzióját).</span><span class="sxs-lookup"><span data-stu-id="e98f5-105">By storing images in repositories, you can have groups of images (or version of images) in isolated environments.</span></span> <span data-ttu-id="e98f5-106">A tárolóhelyekkel történő képek leküldése a beállításjegyzék megadhat.</span><span class="sxs-lookup"><span data-stu-id="e98f5-106">You can specify these repositories when you push images to your registry.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="e98f5-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e98f5-107">Prerequisites</span></span>
* <span data-ttu-id="e98f5-108">**Azure Container Registry** – Létrehozhat egy tároló-beállításjegyzéket Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="e98f5-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="e98f5-109">Ehhez például használhatja az [Azure Portalt](container-registry-get-started-portal.md) vagy az [Azure CLI 2.0-t](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e98f5-109">For example, use the [Azure portal](container-registry-get-started-portal.md) or the [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="e98f5-110">**A Docker parancssori felülete** – Ha szeretné helyi számítógépét Docker-gazdagépként beállítani és elérni a Docker parancssori felületének parancsait, telepítse a [Docker Engine-t](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="e98f5-110">**Docker CLI** - To set up your local computer as a Docker host and access the Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>
* <span data-ttu-id="e98f5-111">**A képfájl lekéréses** - lemezkép nyilvános Docker Hub beállításjegyzékből való lekérésére, címkével, és hogy a beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="e98f5-111">**Pull an image** - Pull an image from the public Docker Hub registry, tag it, and push it to your registry.</span></span> <span data-ttu-id="e98f5-112">Útmutatás a leküldéses és lekéréses képek, lásd: [leküldéses Docker kép Azure személyes beállítási](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e98f5-112">For guidance on how push and pull images, see [Push Docker image to Azure private registry](container-registry-get-started-docker-cli.md).</span></span>


## <a name="viewing-repositories-in-the-portal"></a><span data-ttu-id="e98f5-113">A portálon megtekintik adattárak</span><span class="sxs-lookup"><span data-stu-id="e98f5-113">Viewing repositories in the Portal</span></span>

<span data-ttu-id="e98f5-114">A tároló beállításjegyzék képek rendelkezik leküldött, miután az Azure-portálon a lemezképeket tároló adattárak listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="e98f5-114">Once you have pushed images to your container registry, you can see a list of the repositories hosting the images in the Azure portal.</span></span>

<span data-ttu-id="e98f5-115">Ha követte a lépéseket a [leküldéses Docker kép Azure személyes beállítási](container-registry-get-started-docker-cli.md) cikk, most rendelkeznie kell egy Nginx-lemezképet a tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="e98f5-115">If you followed the steps in the [Push Docker image to Azure private registry](container-registry-get-started-docker-cli.md) article, you should now have a Nginx image in your container registry.</span></span> <span data-ttu-id="e98f5-116">Utasításokat részeként kell adta meg a lemezkép egy névteret.</span><span class="sxs-lookup"><span data-stu-id="e98f5-116">As part of the instructions, you should have specified a namespace for the image.</span></span> <span data-ttu-id="e98f5-117">Az alábbi példában a parancs a "minták" tárházba NGinx kép leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="e98f5-117">In the example below, the command pushes the NGinx image to the "samples" repository:</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```
 <span data-ttu-id="e98f5-118">Az Azure Container Registry támogatja a többszintű adattárnévtereket.</span><span class="sxs-lookup"><span data-stu-id="e98f5-118">Azure Container Registry supports multilevel repository namespaces.</span></span> <span data-ttu-id="e98f5-119">Ezzel a szolgáltatással egy adott alkalmazáshoz vagy alkalmazások gyűjteményéhez kapcsolódó rendszerképek gyűjteményeit csoportba rendezheti az egyes fejlesztői és üzemeltetői csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="e98f5-119">This feature enables you to group collections of images related to a specific app, or a collection of apps to specific development or operational teams.</span></span> <span data-ttu-id="e98f5-120">További lásd: a tároló nyilvántartó adattárak [saját Docker-tároló nyilvántartó az Azure-ban](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="e98f5-120">To read more about repositories in container registries, see [Private Docker container registries in Azure](container-registry-intro.md).</span></span>

<span data-ttu-id="e98f5-121">A tároló beállításjegyzék adattárak megtekintése:</span><span class="sxs-lookup"><span data-stu-id="e98f5-121">To view the container registry repositories:</span></span>

1. <span data-ttu-id="e98f5-122">Jelentkezzen be az Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="e98f5-122">Log in to the Azure portal</span></span>
2. <span data-ttu-id="e98f5-123">Az a **Azure tároló beállításjegyzék** panelen válassza ki a megvizsgálni kívánt beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="e98f5-123">On the **Azure Container Registry** blade, select the registry you wish to inspect</span></span>
3. <span data-ttu-id="e98f5-124">A beállításjegyzék paneljén kattintson **Tárházak** a tárolóhelyekkel és a képek listájának megjelenítéséhez</span><span class="sxs-lookup"><span data-stu-id="e98f5-124">In the registry blade, click **Repositories** to see a list of all the repositories and their images</span></span>
4. <span data-ttu-id="e98f5-125">(Választható) Válasszon ki egy adott lemezképet címkék megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="e98f5-125">(Optional) Select a specific image to see tags</span></span>

![A tárolóhelyekkel a portálon](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a><span data-ttu-id="e98f5-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e98f5-127">Next steps</span></span>
<span data-ttu-id="e98f5-128">Most, hogy elsajátította az alapokat, készen áll a beállításjegyzéke használatára.</span><span class="sxs-lookup"><span data-stu-id="e98f5-128">Now that you know the basics, you are ready to start using your registry!</span></span> <span data-ttu-id="e98f5-129">Például üzembe helyezhet tárolórendszerképeket egy [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/)-fürtön.</span><span class="sxs-lookup"><span data-stu-id="e98f5-129">For example, start deploying container images to an [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
