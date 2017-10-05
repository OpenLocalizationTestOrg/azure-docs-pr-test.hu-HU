---
title: "Az Azure CLI-parancsfájlt minták – a kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – a kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 37046967bd5ab0d797d1c66690db7200fb4805e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a>A kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása

Ez a parancsfájlpélda hoz létre egy Azure függvény alkalmazást, mert a függvények tárolója. A függvény App létre kell hozni a [fogyasztás terv](../functions-scale.md#consumption-plan), ami ideális eseményvezérelt kiszolgáló nélküli munkaterheléseket.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

Ezt a parancsfájlt hoz létre egy Azure-függvény alkalmazás használata a [fogyasztás terv](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "fogyasztás tervezze egy Azure-függvény létrehozása")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat. Ezt a parancsfájlt az alábbi parancsokat használja:

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az storage-fiók létrehozása](/cli/azure/storage/account#create) | Létrehoz egy Azure Storage-fiókot. |
| [az functionapp létrehozása](https://docs.microsoft.com/cli/azure/functionapp#create) | Létrehoz egy Azure-függvényt. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).
