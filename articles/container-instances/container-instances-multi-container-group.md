---
title: "Azure-tároló példányokon - többszörös a tárolócsoport |} Az Azure Docs"
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
ms.openlocfilehash: 140f58582645ea32f77e901eb13364ed145bbecf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-group"></a><span data-ttu-id="8ef3a-103">A tároló csoport telepítése</span><span class="sxs-lookup"><span data-stu-id="8ef3a-103">Deploy a container group</span></span>

<span data-ttu-id="8ef3a-104">Azure-tároló példányokon alakzatot használatával egyetlen állomásra több tároló telepítését támogatja a *a tárolócsoport*.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-104">Azure Container Instances support the deployment of multiple containers onto a single host using a *container group*.</span></span> <span data-ttu-id="8ef3a-105">Ez akkor hasznos, ha egy alkalmazás oldalkocsi naplózási, figyelési vagy bármely egyéb konfigurációs felépítése amikor egy szolgáltatás kell egy második csatolt folyamat.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-105">This is useful when building an application sidecar for logging, monitoring, or any other configuration where a service needs a second attached process.</span></span> 

<span data-ttu-id="8ef3a-106">Ez a dokumentum végigvezeti egy egyszerű több tároló oldalkocsi konfigurációs Azure Resource Manager-sablonnal futtatása.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-106">This document walks through running a simple multi-container sidecar configuration using an Azure Resource Manager template.</span></span>

## <a name="configure-the-template"></a><span data-ttu-id="8ef3a-107">A sablon konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ef3a-107">Configure the template</span></span>

<span data-ttu-id="8ef3a-108">Hozzon létre egy fájlt `azuredeploy.json` és a következő json másolása.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-108">Create a file named `azuredeploy.json` and copy the following json into it.</span></span> 

<span data-ttu-id="8ef3a-109">Ebben a mintában a tároló két tárolók és a nyilvános IP-cím van definiálva.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-109">In this sample, a container group with two containers and a public IP address is defined.</span></span> <span data-ttu-id="8ef3a-110">A csoport első tároló internet felé néző alkalmazás fut.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-110">The first container of the group runs an internet facing application.</span></span> <span data-ttu-id="8ef3a-111">A második tároló, a oldalkocsi egy HTTP kérést küld az elsődleges webes alkalmazás a csoporthoz tartozó helyi hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-111">The second container, the sidecar, makes an HTTP request to the main web application via the group's local network.</span></span> 

<span data-ttu-id="8ef3a-112">Ebben a példában oldalkocsi figyelmeztetést jelenít meg, ha egy HTTP-válaszkód eltérő 200 OK kapott lehetett kibontani.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-112">This sidecar example could be expanded to trigger an alert if it received an HTTP response code other than 200 OK.</span></span> 

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

<span data-ttu-id="8ef3a-113">Egy tároló titkos kép beállításjegyzék használatát, az objektum hozzáadása a json-dokumentum, az alábbi formátumban.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-113">To use a private container image registry, add an object to the json document with the following format.</span></span>

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-the-template"></a><span data-ttu-id="8ef3a-114">A sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="8ef3a-114">Deploy the template</span></span>

<span data-ttu-id="8ef3a-115">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="8ef3a-116">A sablon üzembe helyezése a [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-116">Deploy the template with the [az group deployment create](/cli/azure/group/deployment#create) command.</span></span>

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

<span data-ttu-id="8ef3a-117">Néhány másodpercen belül egy kezdeti választ kap az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-117">Within a few seconds, you will receive an initial response from Azure.</span></span> 

## <a name="view-deployment-state"></a><span data-ttu-id="8ef3a-118">Központi telepítés állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="8ef3a-118">View deployment state</span></span>

<span data-ttu-id="8ef3a-119">A központi telepítési állapotának megtekintéséhez használja a `az container show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-119">To view the state of the deployment, use the `az container show` command.</span></span> <span data-ttu-id="8ef3a-120">Ez visszaad, amelyben az alkalmazás elérhető kiosztott nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-120">This returns the provisioned public IP address over which the application can be accessed.</span></span>

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

<span data-ttu-id="8ef3a-121">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="8ef3a-121">Output:</span></span>

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a><span data-ttu-id="8ef3a-122">Naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="8ef3a-122">View logs</span></span>   

<span data-ttu-id="8ef3a-123">A kimenet egy tároló használatával megtekintheti a `az container logs` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-123">View the log output of a container using the `az container logs` command.</span></span> <span data-ttu-id="8ef3a-124">A `--container-name` argumentum meghatározza a tároló, amelyből való lekérésére naplókat.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-124">The `--container-name` argument specifies the container from which to pull logs.</span></span> <span data-ttu-id="8ef3a-125">Ebben a példában az első tároló van megadva.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-125">In this example, the first container is specified.</span></span> 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

<span data-ttu-id="8ef3a-126">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="8ef3a-126">Output:</span></span>

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

<span data-ttu-id="8ef3a-127">A kiszolgálóoldali-car tároló a naplók megtekintéséhez futtassa a ugyanazzal a paranccsal a második tároló nevének megadását.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-127">To see the logs for the side-car container, run the same command specifying the second container name.</span></span>

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

<span data-ttu-id="8ef3a-128">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="8ef3a-128">Output:</span></span>

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

<span data-ttu-id="8ef3a-129">Ahogy látja, a oldalkocsi HTTP-kérelem, hogy így rendszeres időközönként a fő webalkalmazásnak a csoporthoz tartozó helyi hálózaton keresztül annak érdekében, hogy fut-e.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-129">As you can see, the sidecar is periodically making an HTTP request to the main web application via the group's local network to ensure that it is running.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ef3a-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ef3a-130">Next steps</span></span>

<span data-ttu-id="8ef3a-131">Ez a dokumentum egy Azure-tárolót több tároló példány telepítéséhez szükséges lépéseket mutatja be.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-131">This document covered the steps needed for deploying a multi-container Azure container instance.</span></span> <span data-ttu-id="8ef3a-132">Egy teljes körű Azure tároló példányok élmény érdekében tekintse meg az Azure-tároló példányok.</span><span class="sxs-lookup"><span data-stu-id="8ef3a-132">For an end to end Azure Container Instances experience, see the Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="8ef3a-133">[Azure tároló példányok oktatóanyag]:./container-instances-tutorial-prepare-app.md</span><span class="sxs-lookup"><span data-stu-id="8ef3a-133">[Azure Container Instances tutorial]: ./container-instances-tutorial-prepare-app.md</span></span>
