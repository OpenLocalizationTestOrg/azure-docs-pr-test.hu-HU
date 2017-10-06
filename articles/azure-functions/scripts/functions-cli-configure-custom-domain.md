---
title: "aaaAzure CLI parancsfájl minta - leképezés egy egyéni tartomány tooa függvény app |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - leképezés egy egyéni tartomány tooa függvény alkalmazást az Azure-ban."
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c7cb0a3e132b491250623b945aecf6aea4f57c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-function-app"></a>Egy egyéni tartomány tooa függvény app leképezése

Ez a parancsfájlpélda kapcsolódó erőforrások egy függvény alkalmazást hoz létre, és majd leképezi `www.<yourdomain>` tooit. toomap tooa egyéni tartományt, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben. Az Azure Functions csak akkor támogatja a leképezés az A rekord egyéni tartományt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account#create) | Hello függvény alkalmazás által igényelt tárfiók létrehozása. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Az App Service-csomag szükséges toomap hoz létre egyéni tartományt. |
| [az functionapp létrehozása]() | Létrehoz egy függvény alkalmazást. |
| [az App Service web config állomásnév hozzáadása](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | A Maps egy egyéni tartomány tooa függvény alkalmazást. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További funkciók CLI parancsfájl minták hello található [dokumentáció az Azure Functions]().
