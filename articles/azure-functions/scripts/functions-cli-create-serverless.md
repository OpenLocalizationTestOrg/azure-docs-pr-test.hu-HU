---
title: "aaaAzure CLI parancsfájl minta - kiszolgáló nélküli végrehajtásra függvény-alkalmazás létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: 7ec872b642e50827896a73a9e43bcc87233e15c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a>A kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása

Ez a parancsfájlpélda hoz létre egy Azure függvény alkalmazást, mert a függvények tárolója. hello függvény App létre hello [fogyasztás terv](../functions-scale.md#consumption-plan), ami ideális eseményvezérelt kiszolgáló nélküli munkaterheléseket.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

Ezt a parancsfájlt hoz létre az Azure-függvény alkalmazást hello [fogyasztás terv](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját. A parancsfájl a következő parancsok hello:

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az storage-fiók létrehozása](/cli/azure/storage/account#create) | Létrehoz egy Azure Storage-fiókot. |
| [az functionapp létrehozása](https://docs.microsoft.com/cli/azure/functionapp#create) | Létrehoz egy Azure-függvényt. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).
