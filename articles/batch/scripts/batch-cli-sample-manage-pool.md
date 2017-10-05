---
title: "Az Azure CLI parancsfájl minta - kötegben készletek kezelése |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötegben készletek kezelése"
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
ms.openlocfilehash: 2556b02459886390b803407c5cb828687229a44e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Az Azure CLI Azure Batch-készletek kezelése

Ezen parancsfájl bemutatja az egyes létrehozásához és az Azure Batch szolgáltatás a számítási csomópontok készleteinek kezelése az Azure parancssori felület elérhető eszközök.

> [!NOTE]
> Ez a példa a parancsok létrehozása az Azure virtuális gépek. Futó virtuális gépek fog keletkeznek költségek a fiókjához. Ezek a költségek minimalizálása érdekében törölje a virtuális gépek Miután befejezte a minta futtatása. Lásd: [készletek tisztítása](#clean-up-pools).

Kötegelt készletek a felhő konfigurálása (csak Windows), vagy a virtuálisgép-konfiguráció (Windows és Linux) két módon konfigurálhatók. Az alábbi minta parancsfájlok létrehozását mutatják be készletek mindkét konfigurációjú.

## <a name="prerequisites"></a>Előfeltételek

- Telepítse az Azure parancssori felület használatával a utasításokat a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.
- Batch-fiók létrehozása, ha még nem rendelkezik. Lásd: [Batch-fiók létrehozása az Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.
- Ha még nem még meg egy kezdő tevékenység-ről futtatva alkalmazások konfigurálása. Lásd: [hozzáadása az Azure CLI Azure Batch-alkalmazások](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazáscsomag feltöltését az Azure-bA.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>A felhőalapú szolgáltatás konfigurációs mintaparancsfájl készlet

[!code-azurecli[fő](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "felhő készletek kezelése")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>A virtuális gép konfigurációs mintaparancsfájl készlet

[!code-azurecli[fő](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "virtuálisgép-készletek kezelése")]

## <a name="clean-up-pools"></a>Készletek tisztítása

Miután lefuttatta a fenti minta parancsfájlt, a következő parancsot a készleteket.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok létrehozása és módosítása a Batch-készletek.
Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) | Batch-fiók hitelesítése.  |
| [az kötegelt alkalmazás összefoglaló listája](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | A Batch-fiók a rendelkezésre álló alkalmazások listáját.  |
| [az batch-készlet létrehozása](https://docs.microsoft.com/cli/azure/batch/pool#create) | Hozzon létre virtuális gépek készletét.  |
| [az kötegelt készlet beállítása](https://docs.microsoft.com/cli/azure/batch/pool#set) | Egy készlet tulajdonságainak frissítésére.  |
| [az kötegelt készlet csomópont-ügynök-SKU listája](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | Lista elérhető csomópont ügynök SKU és lemezkép információkat.  |
| [az kötegelt készlet átméretezése](https://docs.microsoft.com/cli/azure/batch/pool#resize) | A program átméretezi a futó virtuális gépek a megadott készlet számát.  |
| [az kötegelt készlet megjelenítése](https://docs.microsoft.com/cli/azure/batch/pool#show) | Egy készlet tulajdonságok megjelenítése.  |
| [az batch-készlet törlése](https://docs.microsoft.com/cli/azure/batch/pool#delete) | A megadott készlet törlése.  |
| [az kötegelt készlet automatikus skálázás engedélyezése](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | Engedélyezze az automatikus skálázás egy címkészlet, és egy képletet.  |
| [az kötegelt készlet automatikus skálázás letiltása](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | Tiltsa le az automatikus skálázást a készlet.  |
| [az kötegelt csomópont listája](https://docs.microsoft.com/cli/azure/batch/node#list) | A megadott készlet a számítási csomópont listában.  |
| [az kötegelt csomópont újraindul](https://docs.microsoft.com/cli/azure/batch/node#reboot) | A megadott számítási csomópont újraindítása.  |
| [az batch munkaterület törlése](https://docs.microsoft.com/cli/azure/batch/node#delete) | A felsorolt csomópontok törlése a megadott készlet.  |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További kötegelt CLI parancsfájl minták megtalálhatók a [Azure Batch CLI dokumentáció](../batch-cli-samples.md).
