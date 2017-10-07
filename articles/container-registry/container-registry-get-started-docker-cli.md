---
title: "aaaPush Docker kép tooprivate Azure beállításjegyzék |} Microsoft Docs"
description: "Leküldéses és lekéréses Docker képek tooa személyes tárolót beállításjegyzék az Azure-ban hello Docker parancssori felületén"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a81a6f4bfcb23642a89ac7631348d40e2f4911a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="push-your-first-image-tooa-private-docker-container-registry-using-hello-docker-cli"></a><span data-ttu-id="d933e-103">Leküldéses az első kép tooa titkos Docker tároló beállításjegyzék hello Docker parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d933e-103">Push your first image tooa private Docker container registry using hello Docker CLI</span></span>
<span data-ttu-id="d933e-104">Egy Azure-tárolót beállításjegyzék tárolja és kezeli a saját [Docker](http://hub.docker.com) tároló képek, hasonló toohello módon [Docker Hub](https://hub.docker.com/) nyilvános Docker-lemezképet tárolja.</span><span class="sxs-lookup"><span data-stu-id="d933e-104">An Azure container registry stores and manages private [Docker](http://hub.docker.com) container images, similar toohello way [Docker Hub](https://hub.docker.com/) stores public Docker images.</span></span> <span data-ttu-id="d933e-105">Hello használata [Docker parancssori felületet](https://docs.docker.com/engine/reference/commandline/cli/) (a Docker parancssori felületén) a [bejelentkezési](https://docs.docker.com/engine/reference/commandline/login/), [leküldéses](https://docs.docker.com/engine/reference/commandline/push/), [lekéréses](https://docs.docker.com/engine/reference/commandline/pull/), és más műveleteket végez a tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="d933e-105">You use hello [Docker Command-Line Interface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) for [login](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), and other operations on your container registry.</span></span>

<span data-ttu-id="d933e-106">További háttér és fogalmak [hello áttekintése](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="d933e-106">For more background and concepts, see [hello overview](container-registry-intro.md)</span></span>



## <a name="prerequisites"></a><span data-ttu-id="d933e-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d933e-107">Prerequisites</span></span>
* <span data-ttu-id="d933e-108">**Azure Container Registry** – Létrehozhat egy tároló-beállításjegyzéket Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="d933e-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="d933e-109">Például, használja a hello [Azure-portálon](container-registry-get-started-portal.md) vagy hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d933e-109">For example, use hello [Azure portal](container-registry-get-started-portal.md) or hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="d933e-110">**A docker parancssori felület** -tooset egy Docker gazdagép és a hozzáférés hello Docker parancssori felület parancsait, mint a helyi számítógép telepítéséhez [Docker-motorhoz](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="d933e-110">**Docker CLI** - tooset up your local computer as a Docker host and access hello Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>

## <a name="log-in-tooa-registry"></a><span data-ttu-id="d933e-111">Jelentkezzen be tooa beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="d933e-111">Log in tooa registry</span></span>
<span data-ttu-id="d933e-112">Futtatás `docker login` tooyour tároló beállításjegyzék rendelkező toolog a [beállításjegyzék hitelesítő adatok](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="d933e-112">Run `docker login` toolog in tooyour container registry with your [registry credentials](container-registry-authentication.md).</span></span>

<span data-ttu-id="d933e-113">hello alábbi példa továbbítja hello Azonosítót és jelszót egy Azure Active Directory [egyszerű](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="d933e-113">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="d933e-114">Például előfordulhat, hogy rendelt hozzá egy szolgáltatás egyszerű tooyour beállításjegyzék az automation-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="d933e-114">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> <span data-ttu-id="d933e-115">Győződjön meg arról, hogy toospecify hello beállításjegyzék teljesen minősített neve (összes, kisbetű).</span><span class="sxs-lookup"><span data-stu-id="d933e-115">Make sure toospecify hello fully qualified registry name (all lowercase).</span></span> <span data-ttu-id="d933e-116">Ebben a példában ez `myregistry.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="d933e-116">In this example, it is `myregistry.azurecr.io`.</span></span>

## <a name="steps-toopull-and-push-an-image"></a><span data-ttu-id="d933e-117">Lépéseket toopull és leküldéses kép</span><span class="sxs-lookup"><span data-stu-id="d933e-117">Steps toopull and push an image</span></span>
<span data-ttu-id="d933e-118">hello letöltések hello Nginx kép beállításjegyzékből hello nyilvános Docker Hub, azt a saját Azure-tárolót beállításjegyzék leküldéses értesítések tooyour beállításjegyzék, majd kéri le újra címkék példa kövesse.</span><span class="sxs-lookup"><span data-stu-id="d933e-118">hello follow example downloads hello Nginx image from hello public Docker Hub registry, tags it for your private Azure container registry, pushes it tooyour registry, then pulls it again.</span></span>

<span data-ttu-id="d933e-119">**1. Az Nginx hello Docker hivatalos kép lekéréses**</span><span class="sxs-lookup"><span data-stu-id="d933e-119">**1. Pull hello Docker official image for Nginx**</span></span>

<span data-ttu-id="d933e-120">Első lekéréses hello nyilvános Nginx kép tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d933e-120">First pull hello public Nginx image tooyour local computer.</span></span>

```
docker pull nginx
```
<span data-ttu-id="d933e-121">**2. Indítsa el a hello Nginx tároló**</span><span class="sxs-lookup"><span data-stu-id="d933e-121">**2. Start hello Nginx container**</span></span>

<span data-ttu-id="d933e-122">hello következő indítja el a hello helyi Nginx tároló interaktív porton 8080-as, így Nginx toosee kimenetét.</span><span class="sxs-lookup"><span data-stu-id="d933e-122">hello following command starts hello local Nginx container interactively on port 8080, allowing you toosee output from Nginx.</span></span> <span data-ttu-id="d933e-123">Eltávolítja a hello fut a tárolót egyszer leállt.</span><span class="sxs-lookup"><span data-stu-id="d933e-123">It removes hello running container once stopped.</span></span>

```
docker run -it --rm -p 8080:80 nginx
```

<span data-ttu-id="d933e-124">Keresse meg a túl[8080](http://localhost:8080) tooview hello futtató tároló.</span><span class="sxs-lookup"><span data-stu-id="d933e-124">Browse too[http://localhost:8080](http://localhost:8080) tooview hello running container.</span></span> <span data-ttu-id="d933e-125">Megjelenik egy egyet a következő képernyő hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="d933e-125">You see a screen similar toohello following one.</span></span>

![Nginx egy helyi számítógépen](./media/container-registry-get-started-docker-cli/nginx.png)

<span data-ttu-id="d933e-127">toostop futó tároló hello megadásához nyomja meg a [CTRL] + [C].</span><span class="sxs-lookup"><span data-stu-id="d933e-127">toostop hello running container, press [CTRL]+[C].</span></span>

<span data-ttu-id="d933e-128">**3. A beállításjegyzék-alias hello lemezkép létrehozása**</span><span class="sxs-lookup"><span data-stu-id="d933e-128">**3. Create an alias of hello image in your registry**</span></span>

<span data-ttu-id="d933e-129">hello parancsban hoz létre hello lemezkép alias teljes elérési útja tooyour beállításjegyzékbeli.</span><span class="sxs-lookup"><span data-stu-id="d933e-129">hello following command creates an alias of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="d933e-130">Ez a példa meghatározza, hogy hello `samples` névtér tooavoid zsúfoltságát hello beállításjegyzék hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="d933e-130">This example specifies hello `samples` namespace tooavoid clutter in hello root of hello registry.</span></span>

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

<span data-ttu-id="d933e-131">**4. Leküldéses hello kép tooyour beállításjegyzék**</span><span class="sxs-lookup"><span data-stu-id="d933e-131">**4. Push hello image tooyour registry**</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="d933e-132">**5. A beállításjegyzék lekéréses hello lemezkép**</span><span class="sxs-lookup"><span data-stu-id="d933e-132">**5. Pull hello image from your registry**</span></span>

```
docker pull myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="d933e-133">**6. A beállításjegyzék-tól kezdődnek hello Nginx tároló**</span><span class="sxs-lookup"><span data-stu-id="d933e-133">**6. Start hello Nginx container from your registry**</span></span>

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="d933e-134">Keresse meg a túl[8080](http://localhost:8080) tooview hello futtató tároló.</span><span class="sxs-lookup"><span data-stu-id="d933e-134">Browse too[http://localhost:8080](http://localhost:8080) tooview hello running container.</span></span>

<span data-ttu-id="d933e-135">toostop futó tároló hello megadásához nyomja meg a [CTRL] + [C].</span><span class="sxs-lookup"><span data-stu-id="d933e-135">toostop hello running container, press [CTRL]+[C].</span></span>

<span data-ttu-id="d933e-136">**7. (Választható) Hello kép eltávolítása**</span><span class="sxs-lookup"><span data-stu-id="d933e-136">**7. (Optional) Remove hello image**</span></span>

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a><span data-ttu-id="d933e-137">Egyidejű korlátok</span><span class="sxs-lookup"><span data-stu-id="d933e-137">Concurrent Limits</span></span>
<span data-ttu-id="d933e-138">Bizonyos forgatókönyvekben a párhuzamos hívások végrehajtása hibát eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="d933e-138">In some scenarios, executing calls concurrently might result in errors.</span></span> <span data-ttu-id="d933e-139">a következő táblázat hello hello korlátok számos egyidejű hívás az Azure-tárolót beállításjegyzékbeli "Leküldéses" és "Pull" műveleteket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="d933e-139">hello following table contains hello limits of concurrent calls with "Push" and "Pull" operations on Azure container registry:</span></span>

| <span data-ttu-id="d933e-140">Művelet</span><span class="sxs-lookup"><span data-stu-id="d933e-140">Operation</span></span>  | <span data-ttu-id="d933e-141">Korlát</span><span class="sxs-lookup"><span data-stu-id="d933e-141">Limit</span></span>                                  |
| ---------- | -------------------------------------- |
| <span data-ttu-id="d933e-142">LEKÉRÉS</span><span class="sxs-lookup"><span data-stu-id="d933e-142">PULL</span></span>       | <span data-ttu-id="d933e-143">Egyidejű too10 mentése kéri le. egy beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="d933e-143">Up too10 concurrent pulls per registry</span></span> |
| <span data-ttu-id="d933e-144">LEKÜLDÉSES ÉRTESÍTÉSEK</span><span class="sxs-lookup"><span data-stu-id="d933e-144">PUSH</span></span>       | <span data-ttu-id="d933e-145">Egyidejű too5 be leküldéses értesítések beállításjegyzék száma</span><span class="sxs-lookup"><span data-stu-id="d933e-145">Up too5 concurrent pushes per registry</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d933e-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d933e-146">Next steps</span></span>
<span data-ttu-id="d933e-147">Most, hogy tudja, hogy hello alapjai, készen áll a beállításjegyzékkel toostart áll!</span><span class="sxs-lookup"><span data-stu-id="d933e-147">Now that you know hello basics, you are ready toostart using your registry!</span></span> <span data-ttu-id="d933e-148">Például a tároló képek tooan telepítése start [Azure Tárolószolgáltatás](https://azure.microsoft.com/documentation/services/container-service/) fürt.</span><span class="sxs-lookup"><span data-stu-id="d933e-148">For example, start deploying container images tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
