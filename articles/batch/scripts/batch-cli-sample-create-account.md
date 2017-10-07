---
title: "parancssori felület parancsfájl minta - aaaAzure Batch-fiók létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - Batch-fiók létrehozása"
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
ms.openlocfilehash: 62b640eebbafdd1081822a638fd0720121ef067a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-cli"></a>Az Azure CLI hello Batch-fiók létrehozása

Ez a parancsfájl létrehoz egy Azure Batch-fiókot, és bemutatja, hogyan hello fiók tulajdonságait kérdezhetők le, és frissíti.

## <a name="prerequisites"></a>Előfeltételek

Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.

## <a name="batch-account-sample-script"></a>Batch-fiók parancsfájlpéldát

Batch-fiók létrehozásakor alapértelmezés szerint a számítási csomópontok belső rendeli hozzá hello Batch-szolgáltatás. Lefoglalt számítási csomópontok tulajdonos tooa külön core kvóta és hello fiók hitelesítése keresztül megosztott kulcs hitelesítő adatai vagy az Azure Active Dirctory tokent.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Batch-fiók felhasználói előfizetés minta parancsfájl használatával

Is toohave Batch számítási csomópontjainak létrehozása a saját Azure-előfizetésben.
Fiókok kialakítása, amelyek számítási csomópontok a előfizetéssé hitelesíteni kell az Azure Active Directory-tokent keresztül, és hello számítási csomópontok lefoglalt az előfizetési kvóta is beleszámít. toocreate ebben a módban egy fiókot, hello fiók létrehozásakor egy kell adnia a Key Vault hivatkozás.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

Miután lefuttatta a fent mintaparancsfájlok hello valamelyikét, futtassa a következő parancs tooremove hello az erőforráscsoport és az összes kapcsolódó erőforrások (beleértve a Batch-fiókok, Azure Storage-fiókokat és Azure kulcstárolójának).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a Batch-fiók és minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az a batch-fiók létrehozása](https://docs.microsoft.com/cli/azure/batch/account#create) | Hello Batch-fiók létrehozása  |
| [az batch-fiók beállítása](https://docs.microsoft.com/cli/azure/batch/account#set) | Hello Batch-fiók tulajdonságainak frissítése.  |
| [az batch-fiók megjelenítése](https://docs.microsoft.com/cli/azure/batch/account#show) | Hello részleteit kérdezi le a Batch-fiókhoz megadott.  |
| [az kötegelt fióklista kulcsok](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | Lekéri hello hívóbetűk hello a Batch-fiókhoz megadott.  |
| [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) | Megadott hello azonosítja a Batch-fiók a további parancssori felület interakció.  |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account#create) | Tárfiók létrehozása. |
| [az keyvault létrehozása](https://docs.microsoft.com/cli/azure/keyvault#create) | Kulcstároló létrehozása. |
| [az keyvault-szabály beállítása](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | A megadott kulcstároló hello hello biztonsági házirend frissítése. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/group#delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).
