---
title: "Az Azure CLI parancsfájl minta - skálázási világszerte egy nagy-availabilty architektúrával webalkalmazás |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási világszerte egy nagy-availabilty architektúrával webes alkalmazás"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c368bdc48f197ff5b491d1796d85abfd339051a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>A webalkalmazás skálázása világszerte egy magas rendelkezésre állású architektúra

Ebben a forgatókönyvben létre fog hozni egy erőforráscsoport, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil, illetve két traffic manager-végpont. A gyakorlatban befejezése után egy magas rendelkezésre állású fog architektúra, amely lehetővé teszi, hogy globális alapján a legkisebb hálózati késést a webalkalmazás rendelkezésre állását biztosítja.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "földrajzi méretezési")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat egy erőforráscsoport, webalkalmazás, traffic manager-profil és minden kapcsolódó erőforrás létrehozásához. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára. |
| [az alkalmazás-kulcs létrehozása](https://docs.microsoft.com/cli/azure/webapp#create) | Létrehoz egy Azure-webalkalmazásban. |
| [az hálózati forgalmat-manager-profil létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | Az Azure Traffic Manager-profil létrehozása. |
| [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | Az Azure Traffic Manager-profilt ad hozzá egy végpontot. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).
