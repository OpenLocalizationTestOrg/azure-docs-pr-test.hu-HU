---
title: "parancssori felület parancsfájl minta - fut egy feladat kötegelt aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötegelt feladat fut"
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
ms.openlocfilehash: f73a7cb341e550fd1c92f92201e20b27fa20d238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Azure Batch Azure parancssori felülettel futó feladatok

Ezt a parancsfájlt hoz létre egy kötegelt, és hozzáadja a feladatok toohello feladat sorozata. Azt is bemutatja, hogyan toomonitor egy feladatot, és a feladatokat. Végül azt illusztrálja, hogyan tooquery hello hatékonyan hello feladat feladatokkal kapcsolatos adatokat a Batch szolgáltatás.

## <a name="prerequisites"></a>Előfeltételek

- Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.
- Batch-fiók létrehozása, ha még nem rendelkezik. Lásd: [Batch-fiók létrehozása az Azure CLI hello](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.
- Még nem még fel, ha egy alkalmazás toorun az a kezdő tevékenység konfigurálása Lásd: [alkalmazások tooAzure kötegelt az Azure parancssori felület Hozzáadás](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazás csomag tooAzure feltöltését.
- Konfigurálja a címkészletet, mely hello feladat futni fog. Lásd: [kezelése Azure Batch készletek Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) egy parancsfájlt hoz létre a készlet egy felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció számára.

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a>Feladat tisztítása

Miután lefuttatta a fenti mintaparancsfájl hello, futtassa a következő parancs tooremove hello a feladatot, és a feladatokat. Vegye figyelembe, hogy hello készlet toobe külön-külön törölni kell. Lásd: [kezelése Azure Batch készletek Azure parancssori felülettel](./batch-cli-sample-manage-pool.md) létrehozása és törlése készletek olvashat.

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello a kötegelt és a feladatokat. Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) | Batch-fiók hitelesítése.  |
| [hozza létre az kötegelt](https://docs.microsoft.com/cli/azure/batch/job#create) | Létrehoz egy kötegelt feladatot.  |
| [az kötegelt feladat beállítása](https://docs.microsoft.com/cli/azure/batch/job#set) | A kötegelt frissítések tulajdonságai.  |
| [az kötegelt feladat megjelenítése](https://docs.microsoft.com/cli/azure/batch/job#show) | Lekérdezi a megadott kötegelt részleteit.  |
| [az kötegelt feladat létrehozása](https://docs.microsoft.com/cli/azure/batch/task#create) | Egy feladat toohello megadott kötegelt hozzáadja.  |
| [az kötegelt feladat megjelenítése](https://docs.microsoft.com/cli/azure/batch/task#show) | Egy feladat hello lekéri hello részleteit megadott kötegelt.  |
| [az kötegelt tevékenység listája](https://docs.microsoft.com/cli/azure/batch/task#list) | Hello megadott feladathoz hozzárendelt hello feladatokat foglalja össze.  |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).
