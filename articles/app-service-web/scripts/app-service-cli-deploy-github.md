---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a Githubról telepítés |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a Githubról telepítés"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: sample
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 61e9d65319cecf3ea4e9152ebdf1035566aad74c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-with-deployment-from-github"></a>Hozzon létre egy webalkalmazást telepítése a Githubból

Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd központilag telepíti a webes alkalmazás kód egy nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül. Lásd a GitHub-telepítéshez olyan folyamatos üzembe helyezés, [webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról](app-service-cli-continuous-deployment-github.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "webalkalmazás létrehozása a Githubról telepítés")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése 

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. |
| [az alkalmazás-kulcs létrehozása](https://docs.microsoft.com/cli/azure/webapp#create) | Létrehoz egy Azure-webalkalmazásban. |
| [az webalkalmazás központi telepítési forrás konfigurációs](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat. |
| [az alkalmazás kulcs Tallózás](https://docs.microsoft.com/cli/azure/webapp#browse) | Azure-webalkalmazás megnyitása a böngészőben. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).
