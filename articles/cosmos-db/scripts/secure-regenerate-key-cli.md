---
title: "Az Azure CLI parancsfájl-újragenerálása Azure Cosmos DB fiókkulcs |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - Azure Cosmos DB fiókkulcs újragenerálása"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: b667aed1ecdf403654ab86b2b7601d5cbddaa725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-the-azure-cli"></a>Egy Azure Cosmos DB-fiókot az Azure parancssori felület használatával kulcs újragenerálása

Ez a minta újragenerálja az Azure parancssori felület használatával Azure Cosmos DB fiókkulcs bármilyen típusú. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-regenerate-keys/secure-cosmosdb-regenerate-keys.sh?highlight=27-31 "Regenerate Azure Cosmos DB account keys")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az cosmosdb létrehozása](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Egy Azure Cosmos DB fiók frissíti. |
| [az cosmosdb újragenerálása-kulcs](/cli/azure/cosmosdb/regenerate-key) | Regeneratates Azure Cosmos DB kulcsait. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Cosmos DB CLI parancsfájl minták megtalálhatók a [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).