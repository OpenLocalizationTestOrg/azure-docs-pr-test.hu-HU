---
title: "Az Azure CLI parancsfájl minta - leképezés funkció alkalmazásokhoz az egyéni tartomány |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - leképezés funkció alkalmazásokhoz az Azure-ban egyéni tartományt."
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
ms.openlocfilehash: 6fcea6d32f9dd25b0fafb4f895f60d8320ac9df8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Egyéni tartomány leképezése egy függvény alkalmazást

Ez a parancsfájlpélda kapcsolódó erőforrások egy függvény alkalmazást hoz létre, és majd leképezi `www.<yourdomain>` rá. Hozzárendelése egyéni tartományhoz, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben. Az Azure Functions csak akkor támogatja a leképezés az A rekord egyéni tartományt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "egy egyéni tartomány leképezése egy függvény alkalmazást")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account#create) | Létrehoz egy tárfiókot, a függvény alkalmazás által igényelt. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Az App Service-csomag kell rendelni egy egyéni tartományt hoz létre. |
| [az functionapp létrehozása]() | Létrehoz egy függvény alkalmazást. |
| [az App Service web config állomásnév hozzáadása](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Egy függvény app rendeli hozzá egyéni tartományt. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További funkciók CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions]().
