---
title: "egy Azure-függvény, amely a tooan Azure Cosmos DB aaaCreate |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - tooan Azure Cosmos DB kapcsolódó Azure-függvény létrehozása"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: 0fbc1ebec2dfd480e0cf3ca64f9febcec8af9a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-that-connects-tooan-azure-cosmos-db"></a>Egy Azure-függvény, amely a tooan Azure Cosmos DB létrehozása

Ez a parancsfájlpélda hoz létre egy Azure-függvény alkalmazást, és tooan Azure Cosmos DB adatbázishoz kapcsolódik.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

Ez a minta létrehoz egy Azure-függvény alkalmazást, és hozzáadja egy Cosmos DB végpont és a hozzáférési kulcs tooapp beállításait.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects tooan Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

Hello parancsfájl minta futtatása után hello kövesse parancs lehet használt tooremove hello erőforráscsoportban, és az összes kapcsolódó erőforrásokat.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az bejelentkezés](https://docs.microsoft.com/cli/azure/#login) | Bejelentkezési tooAzure. |
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Hozzon létre egy erőforráscsoportot, amelynek a helye |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account) | Create a storage account |
| [az functionapp létrehozása](https://docs.microsoft.com/cli/azure/functionapp#create) | Hozzon létre egy új funkció alkalmazást |
| [az documentdb létrehozása](https://docs.microsoft.com/cli/azure/documentdb#create) | A documentdb-adatbázis létrehozása |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/group#delete) | A fölöslegessé vált elemek eltávolítása |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).




