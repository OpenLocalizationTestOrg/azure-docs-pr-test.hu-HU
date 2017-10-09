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
# <a name="deploy-a-container-group"></a>A tároló csoport telepítése

Az Azure tároló példányok több tároló telepítését használatával egyetlen állomásra hello telepítésének támogatása egy *a tárolócsoport*. Ez akkor hasznos, ha egy alkalmazás oldalkocsi naplózási, figyelési vagy bármely egyéb konfigurációs felépítése amikor egy szolgáltatás kell egy második csatolt folyamat. 

Ez a dokumentum végigvezeti egy egyszerű több tároló oldalkocsi konfigurációs Azure Resource Manager-sablonnal futtatása.

## <a name="configure-hello-template"></a>Hello sablon konfigurálása

Hozzon létre egy fájlt `azuredeploy.json` és a következő json másolási hello bele. 

Ebben a mintában a tároló két tárolók és a nyilvános IP-cím van definiálva. hello első tároló hello csoport internet felé néző alkalmazást futtat. hello második tároló, hello oldalkocsi, lehetővé teszi egy HTTP-kérelem toohello fő webes alkalmazások hello csoport helyi hálózaton keresztül. 

Oldalkocsi példában lehet bővített tootrigger riasztást, ha egy HTTP-válaszkód eltérő 200 OK kapott. 

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

egy tároló titkos kép beállításjegyzék toouse egy objektum toohello json-dokumentum formátuma a következő hello adja hozzá.

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-hello-template"></a>Hello sablon üzembe helyezése

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

A hello hello sablon üzembe helyezése [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) parancsot.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Néhány másodpercen belül egy kezdeti választ kap az Azure-ból. 

## <a name="view-deployment-state"></a>Központi telepítés állapotának megtekintése

hello központi telepítését, használatát hello tooview hello állapotának `az container show` parancsot. Ez a kiépített hello mely hello keresztül érhetők el az alkalmazás nyilvános IP-címet adja vissza.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Kimenet:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Naplók megtekintése   

Hello napló kimeneti hello segítségével a tároló megtekintése `az container logs` parancsot. Hello `--container-name` argumentum meghatározza, melyik toopull naplók hello tárolót. Ebben a példában a hello első tároló van megadva. 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Kimenet:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

toosee hello hello ügyféloldali-car tároló naplózza, hello futtatása azonos parancs megadásával hello második tároló neve.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Kimenet:

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

Ahogy látja, az hello oldalkocsi, hogy így egy HTTP-kérelem toohello fő webes alkalmazások hello csoport helyi hálózati tooensure, hogy fut-e keresztül rendszeres időközönként.

## <a name="next-steps"></a>Következő lépések

Ez a dokumentum egy Azure-tárolót példány több tároló üzembe helyezéséhez szükséges hello lépéseket mutatja be. Az end tooend Azure tároló példányok tapasztalhat lásd: hello Azure tároló példányok oktatóanyag.

> [!div class="nextstepaction"]
> [Azure tároló példányok oktatóanyag]:./container-instances-tutorial-prepare-app.md
