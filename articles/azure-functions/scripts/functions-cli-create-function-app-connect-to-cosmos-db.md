---
title: "Egy Azure Cosmos DB kapcsolódó Azure-függvény létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták –, amely egy Azure Cosmos DB csatlakozik Azure-függvény létrehozása"
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
ms.openlocfilehash: ba7e934f71824493f29b001cea6dd1c567ef3414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Hozzon létre egy Azure-függvény, amely egy Azure Cosmos DB csatlakozik

Ez a parancsfájlpélda hoz létre egy Azure-függvény alkalmazást, és egy Azure Cosmos DB adatbázishoz kapcsolódik.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

Ez a minta egy Azure-függvény alkalmazást hoz létre, és Alkalmazásbeállítások ad hozzá egy Cosmos DB végpont és a hozzáférési kulcsot.

[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh ", amely egy Azure Cosmos DB csatlakozik Azure-függvény létrehozása")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, és az összes kapcsolódó erőforrásokat.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az bejelentkezés](https://docs.microsoft.com/cli/azure/#login) | Bejelentkezés az Azure-bA. |
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Hozzon létre egy erőforráscsoportot, amelynek a helye |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account) | Create a storage account |
| [az functionapp létrehozása](https://docs.microsoft.com/cli/azure/functionapp#create) | Hozzon létre egy új funkció alkalmazást |
| [az documentdb létrehozása](https://docs.microsoft.com/cli/azure/documentdb#create) | A documentdb-adatbázis létrehozása |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/group#delete) | A fölöslegessé vált elemek eltávolítása |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).




