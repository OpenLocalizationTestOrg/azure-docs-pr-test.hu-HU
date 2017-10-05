---
title: "Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot |} Microsoft Docs"
description: "Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot"
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: 67eb41d89328ab57741c419d8b718e19b947dab1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-app-service"></a>Hozzon létre egy App Service

Ez a parancsfájlpélda hoz létre, a függvény alkalmazás használatával az [fogyasztás terv](../functions-scale.md#consumption-plan) kapcsolódó erőforrásokkal, és folyamatosan telepíti a funkciókódot a GitHub-tárházban. Ez a példa lesz szüksége:

* GitHub-tárházban funkciók kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.
* A [személyes hozzáférési jogkivonat (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) a GitHub-fiókjában.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

Ez a minta egy Azure-függvény alkalmazás létrehozza, és telepíti a Githubról funkciókódot.

[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure szolgáltatás")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat. Ezt a parancsfájlt használja a következő:

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. |
| [az functionapp létrehozása](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [az App Service web verziókezelő konfiguráció](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Egy függvény app társítja a Git vagy Mercurial tárházba. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).
