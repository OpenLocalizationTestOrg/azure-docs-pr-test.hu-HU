---
title: "Webalkalmazás az Azure CLI 2.0 verziót használja Linux aaaManage |} Microsoft Docs"
description: "Webalkalmazás Linux Azure parancssori felület használatával kezelheti."
keywords: "az Azure app service, webalkalmazás, cli, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.openlocfilehash: 5e8e0da8a362450c56d2e87e087f77598ec874ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-web-app-on-linux-using-azure-cli"></a><span data-ttu-id="15ba9-104">Azure parancssori felület használatával Linux webalkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="15ba9-104">Manage Web App on Linux using Azure CLI</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="15ba9-105">Ez a cikk hello parancsokkal képes toocreate és egy webalkalmazást az Azure CLI 2.0 verziót használja Linux kezelése.</span><span class="sxs-lookup"><span data-stu-id="15ba9-105">Using hello commands in this article you are able toocreate and manage a Web App on Linux using Azure CLI 2.0.</span></span>
<span data-ttu-id="15ba9-106">Megkezdheti a hello hello CLI két módon új verzióját használja:</span><span class="sxs-lookup"><span data-stu-id="15ba9-106">You can start using hello new version of hello CLI in two ways:</span></span>

* <span data-ttu-id="15ba9-107">[Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="15ba9-107">[Installing Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span>
* <span data-ttu-id="15ba9-108">Használatával [Azure felhőben rendszerhéj (előzetes verzió)](../cloud-shell/overview.md)</span><span class="sxs-lookup"><span data-stu-id="15ba9-108">Using [Azure Cloud Shell (Preview)](../cloud-shell/overview.md)</span></span>


## <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="15ba9-109">A Linux App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="15ba9-109">Create a Linux App Service Plan</span></span>

<span data-ttu-id="15ba9-110">a Linux App Service-csomag toocreate, a következő parancs hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="15ba9-110">toocreate a Linux App Service Plan, you can use hello following command:</span></span>

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a><span data-ttu-id="15ba9-111">Hozzon létre egy egyéni Docker-tároló webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="15ba9-111">Create a custom Docker container Web App</span></span>

<span data-ttu-id="15ba9-112">toocreate egy webalkalmazást és konfigurálni kell egy egyéni Docker-tároló toorun hello a következő parancs használható:</span><span class="sxs-lookup"><span data-stu-id="15ba9-112">toocreate a web app and configuring it toorun a custom Docker container, you can use hello following command:</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-hello-docker-container-logging"></a><span data-ttu-id="15ba9-113">Hello Docker-tároló naplózási aktiválása</span><span class="sxs-lookup"><span data-stu-id="15ba9-113">Activate hello Docker container logging</span></span>

<span data-ttu-id="15ba9-114">tooactivate hello Docker-tároló naplózási, hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="15ba9-114">tooactivate hello Docker container logging, you can use hello following command:</span></span>

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-hello-custom-docker-container-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="15ba9-115">A Linux-alkalmazás egy már meglévő webalkalmazás hello egyéni Docker-tároló módosítása</span><span class="sxs-lookup"><span data-stu-id="15ba9-115">Change hello custom Docker container for an existing Web App on Linux App</span></span>

<span data-ttu-id="15ba9-116">toochange egy korábban létrehozott alkalmazást, hello aktuális Docker kép tooa új lemezképet, a következő parancs hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="15ba9-116">toochange a previously created app, from hello current Docker image tooa new image, you can use hello following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a><span data-ttu-id="15ba9-117">A titkos beállításjegyzékből Docker lemezképek használatával</span><span class="sxs-lookup"><span data-stu-id="15ba9-117">Using Docker images from a private registry</span></span>

<span data-ttu-id="15ba9-118">Beállíthatja, hogy az alkalmazás-toouse lemezképek titkos beállításjegyzékből.</span><span class="sxs-lookup"><span data-stu-id="15ba9-118">You can configure your app toouse images from a private registry.</span></span> <span data-ttu-id="15ba9-119">Tooprovide hello URL-cím van szüksége a beállításjegyzék, felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="15ba9-119">You need tooprovide hello url for your registry, user name, and password.</span></span> <span data-ttu-id="15ba9-120">Ez a érhető el a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="15ba9-120">This can be achieved using hello following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a><span data-ttu-id="15ba9-121">Folyamatos egyéni Docker-lemezképek központi telepítésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15ba9-121">Enable continuous deployments for custom Docker images</span></span>

<span data-ttu-id="15ba9-122">A parancs a következő hello hello CD funkciónak, és hello webhook url-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="15ba9-122">With hello following command you can enable hello CD functionality, and get hello webhook url.</span></span> <span data-ttu-id="15ba9-123">Az URL-cím lehet használt tooconfigure meg DockerHub vagy Azure tároló beállításjegyzék repók.</span><span class="sxs-lookup"><span data-stu-id="15ba9-123">This url can be used tooconfigure you DockerHub or Azure Container Registry repos.</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a><span data-ttu-id="15ba9-124">A Linux-alkalmazás a beépített futásidejű keretrendszerek egyikével webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="15ba9-124">Create a Web App on Linux App using one of our built-in runtime frameworks</span></span>

<span data-ttu-id="15ba9-125">a Linux-alkalmazást, hogy használhatja-e a következő parancs hello PHP 5.6 webalkalmazás toocreate.</span><span class="sxs-lookup"><span data-stu-id="15ba9-125">toocreate a PHP 5.6 Web App on Linux App that, you can use hello following command.</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="15ba9-126">A Linux-alkalmazás egy már meglévő webalkalmazás a keretrendszer verziójának módosítása</span><span class="sxs-lookup"><span data-stu-id="15ba9-126">Change framework version for an existing Web App on Linux App</span></span>

<span data-ttu-id="15ba9-127">egy korábban létrehozott alkalmazást, hello jelenlegi keretrendszer verzióját tooNode.js 6.11, toochange hello a következő parancsot is használhatja:</span><span class="sxs-lookup"><span data-stu-id="15ba9-127">toochange a previously created app, from hello current framework version tooNode.js 6.11, you can use hello following command:</span></span>

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a><span data-ttu-id="15ba9-128">A webalkalmazás Git-telepítésekhez beállítása</span><span class="sxs-lookup"><span data-stu-id="15ba9-128">Set up Git deployments for your Web App</span></span>

<span data-ttu-id="15ba9-129">tooset Git központi telepítések az alkalmazások, a következő parancs hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="15ba9-129">tooset up Git deployments for your app, you can use hello following command:</span></span>

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a><span data-ttu-id="15ba9-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15ba9-130">Next steps</span></span>
* [<span data-ttu-id="15ba9-131">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="15ba9-131">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="15ba9-132">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="15ba9-132">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [<span data-ttu-id="15ba9-133">Azure-felhőbe rendszerhéj (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="15ba9-133">Azure Cloud Shell (Preview)</span></span>](../cloud-shell/overview.md)
* [<span data-ttu-id="15ba9-134">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="15ba9-134">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="15ba9-135">Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="15ba9-135">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
