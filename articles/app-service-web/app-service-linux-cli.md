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
# <a name="manage-web-app-on-linux-using-azure-cli"></a>Azure parancssori felület használatával Linux webalkalmazás kezelése

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Ez a cikk hello parancsokkal képes toocreate és egy webalkalmazást az Azure CLI 2.0 verziót használja Linux kezelése.
Megkezdheti a hello hello CLI két módon új verzióját használja:

* [Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen.
* Használatával [Azure felhőben rendszerhéj (előzetes verzió)](../cloud-shell/overview.md)


## <a name="create-a-linux-app-service-plan"></a>A Linux App Service-csomag létrehozása

a Linux App Service-csomag toocreate, a következő parancs hello is használhatja:

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a>Hozzon létre egy egyéni Docker-tároló webalkalmazás

toocreate egy webalkalmazást és konfigurálni kell egy egyéni Docker-tároló toorun hello a következő parancs használható:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-hello-docker-container-logging"></a>Hello Docker-tároló naplózási aktiválása

tooactivate hello Docker-tároló naplózási, hello a következő parancsot használja:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-hello-custom-docker-container-for-an-existing-web-app-on-linux-app"></a>A Linux-alkalmazás egy már meglévő webalkalmazás hello egyéni Docker-tároló módosítása

toochange egy korábban létrehozott alkalmazást, hello aktuális Docker kép tooa új lemezképet, a következő parancs hello is használhatja:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a>A titkos beállításjegyzékből Docker lemezképek használatával

Beállíthatja, hogy az alkalmazás-toouse lemezképek titkos beállításjegyzékből. Tooprovide hello URL-cím van szüksége a beállításjegyzék, felhasználónevét és jelszavát. Ez a érhető el a következő parancs hello használata:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Folyamatos egyéni Docker-lemezképek központi telepítésének engedélyezése

A parancs a következő hello hello CD funkciónak, és hello webhook url-cím beszerzése. Az URL-cím lehet használt tooconfigure meg DockerHub vagy Azure tároló beállításjegyzék repók.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a>A Linux-alkalmazás a beépített futásidejű keretrendszerek egyikével webalkalmazás létrehozása

a Linux-alkalmazást, hogy használhatja-e a következő parancs hello PHP 5.6 webalkalmazás toocreate.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a>A Linux-alkalmazás egy már meglévő webalkalmazás a keretrendszer verziójának módosítása

egy korábban létrehozott alkalmazást, hello jelenlegi keretrendszer verzióját tooNode.js 6.11, toochange hello a következő parancsot is használhatja:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a>A webalkalmazás Git-telepítésekhez beállítása

tooset Git központi telepítések az alkalmazások, a következő parancs hello is használhatja:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](app-service-linux-intro.md)
* [Az Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [Azure-felhőbe rendszerhéj (előzetes verzió)](../cloud-shell/overview.md)
* [Átmeneti környezet az Azure App Service beállítása](./web-sites-staged-publishing.md)
* [Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux rendszeren](./app-service-linux-ci-cd.md)
