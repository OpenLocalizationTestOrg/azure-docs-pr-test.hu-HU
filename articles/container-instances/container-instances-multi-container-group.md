---
title: "Tároló példányok - többszörös a tárolócsoport aaaAzure |} Az Azure Docs"
description: "Azure-tároló példányokon - többszörös a tárolócsoport"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 976f578cd2a9bf7f05ab97f24662139bb72062ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-group"></a><span data-ttu-id="81dec-103">A tároló csoport telepítése</span><span class="sxs-lookup"><span data-stu-id="81dec-103">Deploy a container group</span></span>

<span data-ttu-id="81dec-104">Az Azure tároló példányok több tároló telepítését használatával egyetlen állomásra hello telepítésének támogatása egy *a tárolócsoport*.</span><span class="sxs-lookup"><span data-stu-id="81dec-104">Azure Container Instances support hello deployment of multiple containers onto a single host using a *container group*.</span></span> <span data-ttu-id="81dec-105">Ez akkor hasznos, ha egy alkalmazás oldalkocsi naplózási, figyelési vagy bármely egyéb konfigurációs felépítése amikor egy szolgáltatás kell egy második csatolt folyamat.</span><span class="sxs-lookup"><span data-stu-id="81dec-105">This is useful when building an application sidecar for logging, monitoring, or any other configuration where a service needs a second attached process.</span></span> 

<span data-ttu-id="81dec-106">Ez a dokumentum végigvezeti egy egyszerű több tároló oldalkocsi konfigurációs Azure Resource Manager-sablonnal futtatása.</span><span class="sxs-lookup"><span data-stu-id="81dec-106">This document walks through running a simple multi-container sidecar configuration using an Azure Resource Manager template.</span></span>

## <a name="configure-hello-template"></a><span data-ttu-id="81dec-107">Hello sablon konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81dec-107">Configure hello template</span></span>

<span data-ttu-id="81dec-108">Hozzon létre egy fájlt `azuredeploy.json` és a következő json másolási hello bele.</span><span class="sxs-lookup"><span data-stu-id="81dec-108">Create a file named `azuredeploy.json` and copy hello following json into it.</span></span> 

<span data-ttu-id="81dec-109">Ebben a mintában a tároló két tárolók és a nyilvános IP-cím van definiálva.</span><span class="sxs-lookup"><span data-stu-id="81dec-109">In this sample, a container group with two containers and a public IP address is defined.</span></span> <span data-ttu-id="81dec-110">hello első tároló hello csoport internet felé néző alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="81dec-110">hello first container of hello group runs an internet facing application.</span></span> <span data-ttu-id="81dec-111">hello második tároló, hello oldalkocsi, lehetővé teszi egy HTTP-kérelem toohello fő webes alkalmazások hello csoport helyi hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="81dec-111">hello second container, hello sidecar, makes an HTTP request toohello main web application via hello group's local network.</span></span> 

<span data-ttu-id="81dec-112">Oldalkocsi példában lehet bővített tootrigger riasztást, ha egy HTTP-válaszkód eltérő 200 OK kapott.</span><span class="sxs-lookup"><span data-stu-id="81dec-112">This sidecar example could be expanded tootrigger an alert if it received an HTTP response code other than 200 OK.</span></span> 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",    
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

<span data-ttu-id="81dec-113">egy tároló titkos kép beállításjegyzék toouse egy objektum toohello json-dokumentum formátuma a következő hello adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="81dec-113">toouse a private container image registry, add an object toohello json document with hello following format.</span></span>

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-hello-template"></a><span data-ttu-id="81dec-114">Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="81dec-114">Deploy hello template</span></span>

<span data-ttu-id="81dec-115">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="81dec-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="81dec-116">A hello hello sablon üzembe helyezése [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="81dec-116">Deploy hello template with hello [az group deployment create](/cli/azure/group/deployment#create) command.</span></span>

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

<span data-ttu-id="81dec-117">Néhány másodpercen belül egy kezdeti választ kap az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="81dec-117">Within a few seconds, you will receive an initial response from Azure.</span></span> 

## <a name="view-deployment-state"></a><span data-ttu-id="81dec-118">Központi telepítés állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="81dec-118">View deployment state</span></span>

<span data-ttu-id="81dec-119">hello központi telepítését, használatát hello tooview hello állapotának `az container show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="81dec-119">tooview hello state of hello deployment, use hello `az container show` command.</span></span> <span data-ttu-id="81dec-120">Ez a kiépített hello mely hello keresztül érhetők el az alkalmazás nyilvános IP-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="81dec-120">This returns hello provisioned public IP address over which hello application can be accessed.</span></span>

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

<span data-ttu-id="81dec-121">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="81dec-121">Output:</span></span>

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a><span data-ttu-id="81dec-122">Naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="81dec-122">View logs</span></span>   

<span data-ttu-id="81dec-123">Hello napló kimeneti hello segítségével a tároló megtekintése `az container logs` parancsot.</span><span class="sxs-lookup"><span data-stu-id="81dec-123">View hello log output of a container using hello `az container logs` command.</span></span> <span data-ttu-id="81dec-124">Hello `--container-name` argumentum meghatározza, melyik toopull naplók hello tárolót.</span><span class="sxs-lookup"><span data-stu-id="81dec-124">hello `--container-name` argument specifies hello container from which toopull logs.</span></span> <span data-ttu-id="81dec-125">Ebben a példában a hello első tároló van megadva.</span><span class="sxs-lookup"><span data-stu-id="81dec-125">In this example, hello first container is specified.</span></span> 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

<span data-ttu-id="81dec-126">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="81dec-126">Output:</span></span>

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

<span data-ttu-id="81dec-127">toosee hello hello ügyféloldali-car tároló naplózza, hello futtatása azonos parancs megadásával hello második tároló neve.</span><span class="sxs-lookup"><span data-stu-id="81dec-127">toosee hello logs for hello side-car container, run hello same command specifying hello second container name.</span></span>

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

<span data-ttu-id="81dec-128">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="81dec-128">Output:</span></span>

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

<span data-ttu-id="81dec-129">Ahogy látja, az hello oldalkocsi, hogy így egy HTTP-kérelem toohello fő webes alkalmazások hello csoport helyi hálózati tooensure, hogy fut-e keresztül rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="81dec-129">As you can see, hello sidecar is periodically making an HTTP request toohello main web application via hello group's local network tooensure that it is running.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81dec-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81dec-130">Next steps</span></span>

<span data-ttu-id="81dec-131">Ez a dokumentum egy Azure-tárolót példány több tároló üzembe helyezéséhez szükséges hello lépéseket mutatja be.</span><span class="sxs-lookup"><span data-stu-id="81dec-131">This document covered hello steps needed for deploying a multi-container Azure container instance.</span></span> <span data-ttu-id="81dec-132">Az end tooend Azure tároló példányok tapasztalhat lásd: hello Azure tároló példányok oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="81dec-132">For an end tooend Azure Container Instances experience, see hello Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="81dec-133">[Azure tároló példányok oktatóanyag]:./container-instances-tutorial-prepare-app.md</span><span class="sxs-lookup"><span data-stu-id="81dec-133">[Azure Container Instances tutorial]: ./container-instances-tutorial-prepare-app.md</span></span>
