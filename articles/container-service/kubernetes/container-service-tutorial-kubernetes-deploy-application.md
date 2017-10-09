---
title: "aaaAzure Tárolószolgáltatás oktatóanyag - alkalmazás központi telepítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - alkalmazás központi telepítése"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7e2fa06d359caf83e684df3966624a6e9a8e7efa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-applications-in-kubernetes"></a><span data-ttu-id="fe5ab-104">A Kubernetes alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="fe5ab-104">Run applications in Kubernetes</span></span>

<span data-ttu-id="fe5ab-105">Ebben az oktatóanyagban négy hét része, egy minta-alkalmazás központi telepítése egy Kubernetes fürtbe.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-105">In this tutorial, part four of seven, a sample application is deployed into a Kubernetes cluster.</span></span> <span data-ttu-id="fe5ab-106">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="fe5ab-106">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe5ab-107">Kubernetes fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="fe5ab-107">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="fe5ab-108">Futtatja az alkalmazást Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fe5ab-108">Run application in Kubernetes</span></span>
> * <span data-ttu-id="fe5ab-109">Hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="fe5ab-109">Test hello application</span></span>

<span data-ttu-id="fe5ab-110">A következő útmutatókból az alkalmazás van méretezhető, frissítése és az Operations Management Suite konfigurált toomonitor hello Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-110">In subsequent tutorials, this application is scaled out, updated, and Operations Management Suite configured toomonitor hello Kubernetes cluster.</span></span>

<span data-ttu-id="fe5ab-111">Ez az oktatóanyag azt feltételezi, hogy alapszinten megértse, Kubernetes fogalmakat, Kubernetes részletes információkat lásd: hello [Kubernetes dokumentáció](https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="fe5ab-111">This tutorial assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see hello [Kubernetes documentation](https://kubernetes.io/docs/home/).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fe5ab-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fe5ab-112">Before you begin</span></span>

<span data-ttu-id="fe5ab-113">Az előző oktatóanyagokat egy alkalmazás egy tároló lemezképpel csomagolása, a lemezkép: feltöltött tooAzure tároló beállításjegyzék és Kubernetes fürt létrejött.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-113">In previous tutorials, an application was packaged into a container image, this image was uploaded tooAzure Container Registry, and a Kubernetes cluster was created.</span></span> <span data-ttu-id="fe5ab-114">Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="fe5ab-114">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="fe5ab-115">Legalább az oktatóanyag egy Kubernetes fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-115">At minimum, this tutorial requires a Kubernetes cluster.</span></span>

## <a name="get-manifest-file"></a><span data-ttu-id="fe5ab-116">A jegyzékfájlnak beolvasása</span><span class="sxs-lookup"><span data-stu-id="fe5ab-116">Get manifest file</span></span>

<span data-ttu-id="fe5ab-117">Ebben az oktatóanyagban [Kubernetes objektumok](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) Kubernetes jegyzékfájl vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-117">For this tutorial, [Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) are deployed using a Kubernetes manifest.</span></span> <span data-ttu-id="fe5ab-118">A Kubernetes jegyzék Kubernetes objektum központi telepítési és konfigurálási utasításait tartalmazó YAM vagy JSON formátumú fájl.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-118">A Kubernetes manifest is a YAML or JSON formatted file containing Kubernetes object deployment and configuration instructions.</span></span>

<span data-ttu-id="fe5ab-119">hello Alkalmazásjegyzék-fájl az oktatóanyag előző oktatóprogram klónozása megtörtént hello Azure szavazattal alkalmazás tárházban érhető el.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-119">hello application manifest file for this tutorial is available in hello Azure Vote application repo, which was cloned in a previous tutorial.</span></span> <span data-ttu-id="fe5ab-120">Ha még nem tette meg, klónozza a hello tárház a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fe5ab-120">If you have not already done so, clone hello repo with hello following command:</span></span> 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="fe5ab-121">hello jegyzékfájl hello következő hello klónozott tárház könyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-121">hello manifest file is found in hello following directory of hello cloned repo.</span></span>

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a><span data-ttu-id="fe5ab-122">A jegyzékfájl frissítése</span><span class="sxs-lookup"><span data-stu-id="fe5ab-122">Update manifest file</span></span>

<span data-ttu-id="fe5ab-123">Ha Azure-tároló beállításjegyzék toostore hello tároló lemezképek használatával, hello jegyzék igények toobe frissített hello ACR loginServer neve.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-123">If using Azure Container Registry toostore hello container images, hello manifest needs toobe updated with hello ACR loginServer name.</span></span>

<span data-ttu-id="fe5ab-124">Első hello ACR bejelentkezési kiszolgálónevet hello [az acr lista](/cli/azure/acr#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-124">Get hello ACR login server name with hello [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="fe5ab-125">hello minta jegyzékfájl már korábban létrehozott tárházba nevű *microsoft*.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-125">hello sample manifest has been pre-created with a repository name of *microsoft*.</span></span> <span data-ttu-id="fe5ab-126">Nyissa meg szövegszerkesztőben hello fájlt, és cserélje le a hello *microsoft* hello bejelentkezési kiszolgálónevet a ACR példány érték.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-126">Open hello file with any text editor, and replace hello *microsoft* value with hello login server name of your ACR instance.</span></span>

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a><span data-ttu-id="fe5ab-127">Alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="fe5ab-127">Deploy application</span></span>

<span data-ttu-id="fe5ab-128">Használjon hello [kubectl létrehozása](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) toorun hello alkalmazás parancsot.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-128">Use hello [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command toorun hello application.</span></span> <span data-ttu-id="fe5ab-129">Ez a parancs elemez hello manifest fájlt, és meghatározott hello Kubernetes objektumok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-129">This command parses hello manifest file and create hello defined Kubernetes objects.</span></span>

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

<span data-ttu-id="fe5ab-130">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="fe5ab-130">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a><span data-ttu-id="fe5ab-131">Alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="fe5ab-131">Test application</span></span>

<span data-ttu-id="fe5ab-132">A [Kubernetes szolgáltatás](https://kubernetes.io/docs/concepts/services-networking/service/) jön létre, amely közzétesz hello alkalmazás toohello internet.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-132">A [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created which exposes hello application toohello internet.</span></span> <span data-ttu-id="fe5ab-133">A folyamat eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-133">This process can take a few minutes.</span></span> 

<span data-ttu-id="fe5ab-134">toomonitor folyamatban, használjon hello [kubectl beolvasása szolgáltatás](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) hello parancsot `--watch` argumentum.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-134">toomonitor progress, use hello [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) command with hello `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="fe5ab-135">Kezdetben hello **külső IP-** a hello *azure-szavazat-front* szolgáltatás jelenik meg *függőben lévő*.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-135">Initially, hello **EXTERNAL-IP** for hello *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="fe5ab-136">Miután hello külső IP-cím megváltozott *függőben lévő* tooan *IP-cím*, használjon `CTRL-C` toostop hello kubectl figyelési folyamat.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-136">Once hello EXTERNAL-IP address has changed from *pending* tooan *IP address*, use `CTRL-C` toostop hello kubectl watch process.</span></span>

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

<span data-ttu-id="fe5ab-137">toosee hello alkalmazás, Tallózás toohello külső IP-címet.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-137">toosee hello application, browse toohello external IP address.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a><span data-ttu-id="fe5ab-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe5ab-139">Next steps</span></span>

<span data-ttu-id="fe5ab-140">Ebben az oktatóanyagban hello Azure szavazattal alkalmazás telepített tooan Azure tároló szolgáltatás Kubernetes fürt volt.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-140">In this tutorial, hello Azure vote application was deployed tooan Azure Container Service Kubernetes cluster.</span></span> <span data-ttu-id="fe5ab-141">Befejeződött a következő feladatokból áll:</span><span class="sxs-lookup"><span data-stu-id="fe5ab-141">Tasks completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="fe5ab-142">Kubernetes fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="fe5ab-142">Download Kubernetes manifest files</span></span>
> * <span data-ttu-id="fe5ab-143">Kubernetes hello alkalmazást futtat</span><span class="sxs-lookup"><span data-stu-id="fe5ab-143">Run hello application in Kubernetes</span></span>
> * <span data-ttu-id="fe5ab-144">Tesztelt hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="fe5ab-144">Tested hello application</span></span>

<span data-ttu-id="fe5ab-145">Előzetes toohello oktatóanyag következő toolearn egy Kubernetes alkalmazás és az alapjául szolgáló Kubernetes infrastruktúra hello méretezésével kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="fe5ab-145">Advance toohello next tutorial toolearn about scaling both a Kubernetes application and hello underlying Kubernetes infrastructure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="fe5ab-146">Skála Kubernetes alkalmazás- és infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="fe5ab-146">Scale Kubernetes application and infrastructure</span></span>](./container-service-tutorial-kubernetes-scale.md)