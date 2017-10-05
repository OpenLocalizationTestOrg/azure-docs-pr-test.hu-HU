---
title: "Webalkalmazás az Azure CLI 2.0 verziót használja Linux kezelése |} Microsoft Docs"
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
ms.openlocfilehash: 04aceecf0cb4cad5c838b7254bf7079a36bbd0d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-web-app-on-linux-using-azure-cli"></a><span data-ttu-id="2b3c4-104">Azure parancssori felület használatával Linux webalkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="2b3c4-104">Manage Web App on Linux using Azure CLI</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="2b3c4-105">Ez a cikk a parancsokkal is létrehozását és kezelését egy webalkalmazást az Azure CLI 2.0 verziót használja Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-105">Using the commands in this article you are able to create and manage a Web App on Linux using Azure CLI 2.0.</span></span>
<span data-ttu-id="2b3c4-106">Megkezdheti az új verzió a CLI két módon:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-106">You can start using the new version of the CLI in two ways:</span></span>

* <span data-ttu-id="2b3c4-107">[Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-107">[Installing Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span>
* <span data-ttu-id="2b3c4-108">Használatával [Azure felhőben rendszerhéj (előzetes verzió)](../cloud-shell/overview.md)</span><span class="sxs-lookup"><span data-stu-id="2b3c4-108">Using [Azure Cloud Shell (Preview)](../cloud-shell/overview.md)</span></span>


## <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="2b3c4-109">A Linux App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-109">Create a Linux App Service Plan</span></span>

<span data-ttu-id="2b3c4-110">A Linux App Service-csomag létrehozásához használhatja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-110">To create a Linux App Service Plan, you can use the following command:</span></span>

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a><span data-ttu-id="2b3c4-111">Hozzon létre egy egyéni Docker-tároló webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2b3c4-111">Create a custom Docker container Web App</span></span>

<span data-ttu-id="2b3c4-112">Hozzon létre egy webalkalmazást, és konfigurálja úgy, hogy egy egyéni Docker-tároló futtassa, használhatja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-112">To create a web app and configuring it to run a custom Docker container, you can use the following command:</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-the-docker-container-logging"></a><span data-ttu-id="2b3c4-113">A Docker-tároló naplózási aktiválása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-113">Activate the Docker container logging</span></span>

<span data-ttu-id="2b3c4-114">A Docker-tároló naplózási aktiválásához használhatja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-114">To activate the Docker container logging, you can use the following command:</span></span>

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-the-custom-docker-container-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="2b3c4-115">Az egyéni Docker-tároló egy már meglévő webalkalmazás Linux alkalmazásában módosítása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-115">Change the custom Docker container for an existing Web App on Linux App</span></span>

<span data-ttu-id="2b3c4-116">Az aktuális Docker kép új lemezképet egy korábban létrehozott alkalmazás módosításához használhatja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-116">To change a previously created app, from the current Docker image to a new image, you can use the following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a><span data-ttu-id="2b3c4-117">A titkos beállításjegyzékből Docker lemezképek használatával</span><span class="sxs-lookup"><span data-stu-id="2b3c4-117">Using Docker images from a private registry</span></span>

<span data-ttu-id="2b3c4-118">Beállíthatja, hogy az alkalmazás képeket használandó titkos beállításjegyzékből.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-118">You can configure your app to use images from a private registry.</span></span> <span data-ttu-id="2b3c4-119">Meg kell adnia az URL-címet a beállításjegyzék, felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-119">You need to provide the url for your registry, user name, and password.</span></span> <span data-ttu-id="2b3c4-120">Ez a érhető el a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-120">This can be achieved using the following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a><span data-ttu-id="2b3c4-121">Folyamatos egyéni Docker-lemezképek központi telepítésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2b3c4-121">Enable continuous deployments for custom Docker images</span></span>

<span data-ttu-id="2b3c4-122">A következő paranccsal engedélyezheti a CD-funkciókat, és a webhook url-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-122">With the following command you can enable the CD functionality, and get the webhook url.</span></span> <span data-ttu-id="2b3c4-123">Az URL-cím akkor DockerHub vagy Azure tároló beállításjegyzék repók konfigurálásához használható.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-123">This url can be used to configure you DockerHub or Azure Container Registry repos.</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a><span data-ttu-id="2b3c4-124">A Linux-alkalmazás a beépített futásidejű keretrendszerek egyikével webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-124">Create a Web App on Linux App using one of our built-in runtime frameworks</span></span>

<span data-ttu-id="2b3c4-125">A PHP 5.6 webalkalmazás létrehozása a Linux-alkalmazást, amely, a következő parancsot használhatja.</span><span class="sxs-lookup"><span data-stu-id="2b3c4-125">To create a PHP 5.6 Web App on Linux App that, you can use the following command.</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="2b3c4-126">A Linux-alkalmazás egy már meglévő webalkalmazás a keretrendszer verziójának módosítása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-126">Change framework version for an existing Web App on Linux App</span></span>

<span data-ttu-id="2b3c4-127">Ha módosítani szeretné egy korábban létrehozott alkalmazást, a jelenlegi keretrendszer verziószáma a Node.js 6.11, használhatja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-127">To change a previously created app, from the current framework version to Node.js 6.11, you can use the following command:</span></span>

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a><span data-ttu-id="2b3c4-128">A webalkalmazás Git-telepítésekhez beállítása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-128">Set up Git deployments for your Web App</span></span>

<span data-ttu-id="2b3c4-129">Az alkalmazás Git-telepítésekhez beállításához használhatja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b3c4-129">To set up Git deployments for your app, you can use the following command:</span></span>

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a><span data-ttu-id="2b3c4-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b3c4-130">Next steps</span></span>
* [<span data-ttu-id="2b3c4-131">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="2b3c4-131">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="2b3c4-132">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="2b3c4-132">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [<span data-ttu-id="2b3c4-133">Azure-felhőbe rendszerhéj (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="2b3c4-133">Azure Cloud Shell (Preview)</span></span>](../cloud-shell/overview.md)
* [<span data-ttu-id="2b3c4-134">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="2b3c4-134">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="2b3c4-135">Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2b3c4-135">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
