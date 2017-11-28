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
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 108622c565e41777fbb1fc9da9a01168abc7a7fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="13a5b-103">Azure-tárolót beállításjegyzék adattárak</span><span class="sxs-lookup"><span data-stu-id="13a5b-103">Azure container registry repositories</span></span>

<span data-ttu-id="13a5b-104">Azure-tárolót beállításjegyzék lehetővé teszi a toostore tároló képek tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="13a5b-104">Azure container registry allows you toostore container images in repositories.</span></span> <span data-ttu-id="13a5b-105">Lemezképek tárolása tárházak találhatók, akkor elkülönített környezetben csoportok lemezképet (vagy képeket verzióját).</span><span class="sxs-lookup"><span data-stu-id="13a5b-105">By storing images in repositories, you can have groups of images (or version of images) in isolated environments.</span></span> <span data-ttu-id="13a5b-106">A tárolóhelyekkel is megadhat, amikor képek tooyour beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="13a5b-106">You can specify these repositories when you push images tooyour registry.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="13a5b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13a5b-107">Prerequisites</span></span>
* <span data-ttu-id="13a5b-108">**Azure Container Registry** – Létrehozhat egy tároló-beállításjegyzéket Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="13a5b-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="13a5b-109">Például, használja a hello [Azure-portálon](container-registry-get-started-portal.md) vagy hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="13a5b-109">For example, use hello [Azure portal](container-registry-get-started-portal.md) or hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="13a5b-110">**A docker parancssori felület** -tooset egy Docker gazdagép és a hozzáférés hello Docker parancssori felület parancsait, mint a helyi számítógép telepítéséhez [Docker-motorhoz](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="13a5b-110">**Docker CLI** - tooset up your local computer as a Docker host and access hello Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>
* <span data-ttu-id="13a5b-111">**A képfájl lekéréses** - lemezkép lekéréses beállításjegyzékből hello nyilvános Docker Hub, címkével, és leküldeni tooyour beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="13a5b-111">**Pull an image** - Pull an image from hello public Docker Hub registry, tag it, and push it tooyour registry.</span></span> <span data-ttu-id="13a5b-112">Útmutatás a leküldéses és lekéréses képek, lásd: [leküldéses Docker kép tooAzure titkos beállításjegyzék](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="13a5b-112">For guidance on how push and pull images, see [Push Docker image tooAzure private registry](container-registry-get-started-docker-cli.md).</span></span>


## <a name="viewing-repositories-in-hello-portal"></a><span data-ttu-id="13a5b-113">Hello Portal adattárak megtekintése</span><span class="sxs-lookup"><span data-stu-id="13a5b-113">Viewing repositories in hello Portal</span></span>

<span data-ttu-id="13a5b-114">Lemezképek tooyour tároló beállításjegyzék rendelkezik leküldött, miután hello Azure-portálon hello lemezképeit tároló hello adattárak listáját láthatja.</span><span class="sxs-lookup"><span data-stu-id="13a5b-114">Once you have pushed images tooyour container registry, you can see a list of hello repositories hosting hello images in hello Azure portal.</span></span>

<span data-ttu-id="13a5b-115">Ha követte hello hello lépéseit [leküldéses Docker kép tooAzure titkos beállításjegyzék](container-registry-get-started-docker-cli.md) cikk, most rendelkeznie kell egy Nginx-lemezképet a tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="13a5b-115">If you followed hello steps in hello [Push Docker image tooAzure private registry](container-registry-get-started-docker-cli.md) article, you should now have a Nginx image in your container registry.</span></span> <span data-ttu-id="13a5b-116">Hello utasításokat részeként kell megadott hello kép egy névteret.</span><span class="sxs-lookup"><span data-stu-id="13a5b-116">As part of hello instructions, you should have specified a namespace for hello image.</span></span> <span data-ttu-id="13a5b-117">Hello az alábbi példában lévő parancs hello hello NGinx toohello "minták" lemezképtárba leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="13a5b-117">In hello example below, hello command pushes hello NGinx image toohello "samples" repository:</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```
 <span data-ttu-id="13a5b-118">Az Azure Container Registry támogatja a többszintű adattárnévtereket.</span><span class="sxs-lookup"><span data-stu-id="13a5b-118">Azure Container Registry supports multilevel repository namespaces.</span></span> <span data-ttu-id="13a5b-119">Ez a funkció lehetővé teszi, hogy Ön toogroup gyűjtemények képek kapcsolódó tooa adott alkalmazás vagy alkalmazások toospecific fejlesztési vagy működési csapatok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="13a5b-119">This feature enables you toogroup collections of images related tooa specific app, or a collection of apps toospecific development or operational teams.</span></span> <span data-ttu-id="13a5b-120">tooread tároló nyilvántartó, a tárolóhelyekkel kapcsolatos további információkért lásd: [saját Docker-tároló nyilvántartó az Azure-ban](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="13a5b-120">tooread more about repositories in container registries, see [Private Docker container registries in Azure](container-registry-intro.md).</span></span>

<span data-ttu-id="13a5b-121">tooview hello tároló beállításjegyzék adattárak:</span><span class="sxs-lookup"><span data-stu-id="13a5b-121">tooview hello container registry repositories:</span></span>

1. <span data-ttu-id="13a5b-122">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="13a5b-122">Log in toohello Azure portal</span></span>
2. <span data-ttu-id="13a5b-123">A hello **Azure tároló beállításjegyzék** panelen, jelölje be hello beállításjegyzék tooinspect kívánja</span><span class="sxs-lookup"><span data-stu-id="13a5b-123">On hello **Azure Container Registry** blade, select hello registry you wish tooinspect</span></span>
3. <span data-ttu-id="13a5b-124">Hello beállításjegyzék paneljén kattintson **Tárházak** toosee összes hello tárházak találhatók, és a képek listája</span><span class="sxs-lookup"><span data-stu-id="13a5b-124">In hello registry blade, click **Repositories** toosee a list of all hello repositories and their images</span></span>
4. <span data-ttu-id="13a5b-125">(Választható) Egy adott kép toosee címkék kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="13a5b-125">(Optional) Select a specific image toosee tags</span></span>

![A tárolóhelyekkel hello portálon](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a><span data-ttu-id="13a5b-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13a5b-127">Next steps</span></span>
<span data-ttu-id="13a5b-128">Most, hogy tudja, hogy hello alapjai, készen áll a beállításjegyzékkel toostart áll!</span><span class="sxs-lookup"><span data-stu-id="13a5b-128">Now that you know hello basics, you are ready toostart using your registry!</span></span> <span data-ttu-id="13a5b-129">Például a tároló képek tooan telepítése start [Azure Tárolószolgáltatás](https://azure.microsoft.com/documentation/services/container-service/) fürt.</span><span class="sxs-lookup"><span data-stu-id="13a5b-129">For example, start deploying container images tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
