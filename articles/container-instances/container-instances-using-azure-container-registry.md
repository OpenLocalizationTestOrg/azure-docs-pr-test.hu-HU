---
title: "aaaDeploy tooAzure tároló példányok a hello Azure tároló beállításjegyzék |} Az Azure Docs"
description: "Hello Azure tároló beállításjegyzék tooAzure tároló példányok telepítése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2667f91db8ed92a9ccc9ba722a2b1f5c5ea93886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-container-instances-from-hello-azure-container-registry"></a>Hello Azure tároló beállításjegyzék tooAzure tároló példányok telepítése

hello Azure tároló beállításjegyzék egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek. Ez a cikk ismerteti, hogyan toodeploy tároló képek tárolt hello Azure tároló beállításjegyzék tooAzure tároló példányok.

## <a name="using-hello-azure-cli"></a>Hello Azure parancssori felület használatával

az Azure parancssori felület hello létrehozása és kezelése az Azure-tároló példányok a tárolók parancsokat tartalmaz. Ha megad egy titkos kép hello `create` parancs hello kép beállításjegyzék jelszó szükséges tooauthenticate hello tároló beállításjegyzék esetében is megadhat.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

Hello `create` parancsot is támogatja a hello megadása `registry-login-server` és `registry-username`. Hello bejelentkezési kiszolgáló hello Azure tároló beállításjegyzék azonban mindig *registryname*. azurecr.io és hello alapértelmezett felhasználónév az *registryname*, így ezek az értékek kikövetkeztetve a hello kép neve, ha nincs kifejezetten megadva.

## <a name="using-an-azure-resource-manager-template"></a>Azure Resource Manager-sablonnal

Tulajdonságok is megadhatók hello az Azure-tároló beállításjegyzék Azure Resource Manager sablon hello-ot `imageRegistryCredentials` tulajdonság hello tartalmazó csoport definícióban:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

a tároló beállításjegyzék jelszó tárolása közvetlenül hello sablon tooavoid azt javasoljuk, hogy tárolja azt, a titkos kulcs [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) és hello sablon használatával hello vonatkozó hivatkozást [natív integráció az között hello Azure Resource Manager és a Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-hello-azure-portal"></a>Hello Azure-portál használatával

Karbantartása hello Azure tároló beállításjegyzék tároló lemezképeihez, ha Azure tároló példányát hello Azure-portál használatával könnyen létrehozhat egy tárolót.

1. Hello Azure-portálon lépjen a tooyour tároló beállításjegyzék.

2. Válassza ki a tárházak találhatók.

    ![hello Azure tároló beállításjegyzék menüjében hello Azure-portálon][acr-menu]

3. Válassza ki, amelyet a toodeploy hello tárház.

4. Kattintson a jobb gombbal a hello címke hello tároló kép toodeploy keresi.

    ![Helyi menü az Azure-tároló példányaival tároló megnyitása][acr-runinstance-contextmenu]

5. Adja meg egy nevet a hello tároló és hello erőforráscsoport nevét. Ha kívánja, hello alapértelmezett értékeit módosíthatja.

    ![Azure-tároló példányok menü létrehozása][acr-create-deeplink]

6. Hello telepítése után a toohello tároló csoporthoz hello értesítések ablaktábla toofind navigálhat az IP-címét és egyéb tulajdonságok is.

    ![Az Azure-tároló példányok a tárolócsoport Részletek nézet][aci-detailsview]

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan toobuild tárolók tooa személyes tárolót beállításjegyzék küldje le őket, és telepítheti azokat tooAzure tároló példányokat [hello az oktatóanyagnak az elvégzése](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
