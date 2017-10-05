---
title: "Az Azure CLI-parancsfájlt minta - Batch-fiók létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: 698978fd717091c49a1375e222f46f4325431223
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-batch-account-with-the-azure-cli"></a>Batch-fiók létrehozása az Azure parancssori felülettel

Ezt a parancsfájlt hoz létre az Azure Batch-fiók, és megjeleníti a fiók a különféle tulajdonságainak lekérdezése, és frissíti.

## <a name="prerequisites"></a>Előfeltételek

Telepítse az Azure parancssori felület használatával a utasításokat a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.

## <a name="batch-account-sample-script"></a>Batch-fiók parancsfájlpéldát

Batch-fiók létrehozásakor alapértelmezés szerint a számítási csomópontok belső rendeli hozzá a Batch szolgáltatás. Lefoglalt számítási csomópontok részt vesznek egy külön core kvótát, és a fiók hitelesítése keresztül megosztott kulcs hitelesítő adatai vagy az Azure Active Dirctory tokent.

[!code-azurecli[fő](../../../cli_scripts/batch/create-account/create-account.sh "fiók létrehozása")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Batch-fiók felhasználói előfizetés minta parancsfájl használatával

Úgy is dönthet, hogy a Batch számítási csomópontjainak létrehozása a saját Azure-előfizetésben rendelkezik.
Fiókok kialakítása, amelyek számítási csomópontok a előfizetéssé hitelesíteni kell az Azure Active Directory-tokent keresztül, és a számítási csomópontok lefoglalt az előfizetési kvóta is beleszámít. Fiók létrehozása az ebben a módban, a Key Vault hivatkozás egy kell adnia a fiók létrehozásakor.

[!code-azurecli[fő](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "előfizetést felhasználói fiók létrehozása")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

Miután a fenti minta parancsfájlok valamelyike, futtassa a következő parancs futtatásával távolítsa el az erőforráscsoportot, és az összes kapcsolódó erőforrások (beleértve a Batch-fiókok, Azure Storage-fiókokat és Azure kulcstárolójának).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat egy erőforráscsoport, a Batch-fiók és minden kapcsolódó erőforrás létrehozásához. Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az a batch-fiók létrehozása](https://docs.microsoft.com/cli/azure/batch/account#create) | A Batch-fiók létrehozása  |
| [az batch-fiók beállítása](https://docs.microsoft.com/cli/azure/batch/account#set) | A Batch-fiók tulajdonságainak frissítése.  |
| [az batch-fiók megjelenítése](https://docs.microsoft.com/cli/azure/batch/account#show) | Lekérdezi a megadott Batch-fiók adatait.  |
| [az kötegelt fióklista kulcsok](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | Lekérdezi a megadott Batch-fiók hozzáférési kulcsainak listázása.  |
| [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) | A megadott Batch-fiók további CLI interakció azonosítja.  |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account#create) | Tárfiók létrehozása. |
| [az keyvault létrehozása](https://docs.microsoft.com/cli/azure/keyvault#create) | Kulcstároló létrehozása. |
| [az keyvault-szabály beállítása](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | A biztonsági házirend a megadott kulcstároló frissítése. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/group#delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További kötegelt CLI parancsfájl minták megtalálhatók a [Azure Batch CLI dokumentáció](../batch-cli-samples.md).
