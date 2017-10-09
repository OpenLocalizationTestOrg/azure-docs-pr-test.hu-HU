---
title: "aaaAzure CLI parancsfájl minta - alkalmazás hozzáadása a kötegben |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötegben alkalmazás hozzáadása"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: cb33b3a7b30610011b19954a987995cc5f0257c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-applications-tooazure-batch-with-azure-cli"></a>Az Azure parancssori felület kötegben. alkalmazásokat tooAzure hozzáadása

Ez a parancsfájl bemutatja, hogyan tooset az alkalmazáshoz egy Azure Batch-készlet vagy feladat való használatra. az alkalmazás tooset csomag a végrehajtható fájlt, és függőségeit, .zip fájlba. A példa hello végrehajtható zip a fájl neve "saját-alkalmazás-exe.zip".

## <a name="prerequisites"></a>Előfeltételek

- Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.
- Batch-fiók létrehozása, ha még nem rendelkezik. Lásd: [Batch-fiók létrehozása az Azure CLI hello](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Alkalmazás törlése

Miután lefuttatta a fenti mintaparancsfájl hello, futtassa a következő parancsok tooremove hello az alkalmazások és a feltöltött alkalmazás csomagokat.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello, az alkalmazás és az alkalmazáscsomag feltöltése.
Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [az kötegelt alkalmazás létrehozása](https://docs.microsoft.com/cli/azure/batch/application#create) | Alkalmazást hoz létre.  |
| [az kötegelt alkalmazás beállítása](https://docs.microsoft.com/cli/azure/batch/application#set) | Alkalmazás tulajdonságainak frissítése.  |
| [az kötegelt alkalmazáscsomag létrehozása](https://docs.microsoft.com/cli/azure/batch/application/package#create) | Hozzáad egy alkalmazás csomag toohello megadott alkalmazást.  |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).
