---
title: "Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: d67e85f91c80efe464fceb1105243bedfba83a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot

Ez a parancsfájlpélda hoz létre, a függvény alkalmazás használatával az [fogyasztás terv](../functions-scale.md#consumption-plan) kapcsolódó erőforrásokkal, és telepíti a funkciókódot a nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül. A Githubból funkciókódot folyamatos kézbesítését, olvassa el a [függvény-alkalmazás létrehozása, és folyamatosan telepítése a Githubból](functions-cli-create-function-app-github-continuous.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

Ez a minta egy Azure-függvény alkalmazás létrehozza, és telepíti a Githubról funkciókódot.

[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "a Githubról telepítés függvény-alkalmazás létrehozása")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat. Ezt a parancsfájlt az alábbi parancsokat használja:

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. |
| [az functionapp létrehozása](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Létrehoz egy Azure függvény alkalmazást. |
| [az App Service web verziókezelő konfiguráció](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Egy függvény app társítja a Git vagy Mercurial tárházba. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).
