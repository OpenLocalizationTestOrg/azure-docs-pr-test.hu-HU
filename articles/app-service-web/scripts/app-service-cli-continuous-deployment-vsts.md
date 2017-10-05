---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a folyamatos üzembe helyezés, a Visual Studio Team Services |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a folyamatos üzembe helyezés, a Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 2b983616757ca3c4226c12876f5fd4c285067318
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>A folyamatos üzembe helyezés, a Visual Studio Team Services-webalkalmazás létrehozása

Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja a Visual Studio Team Services tárházból folyamatos üzembe helyezést. Az ebben a példában a következőkre lesz szüksége:

* A Visual Studio Team Services tárház alkalmazás kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.
* A [személyes hozzáférési jogkivonat (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) a Visual Studio Team Services-fiókhoz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "a folyamatos üzembe helyezés, a Visual Studio Team Services-webalkalmazás létrehozása")]

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
