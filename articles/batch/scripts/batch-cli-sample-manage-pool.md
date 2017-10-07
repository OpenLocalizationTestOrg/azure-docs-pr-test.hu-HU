---
title: "aaaAzure CLI parancsfájl minta - készletek kezelése a Batch |} Microsoft Docs"
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
ms.openlocfilehash: 6c9ca9515565aff42752231a080943be8e4c810b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Az Azure CLI Azure Batch-készletek kezelése

A parancsfájl bemutatja az egyes hello eszközök hello Azure CLI toocreate érhető el, és hello Azure Batch szolgáltatás a számítási csomópontok készleteinek kezelése.

> [!NOTE]
> Ez a példa hello parancsok létrehozása az Azure virtuális gépek. Futó virtuális gépek fog keletkeznek költségek tooyour fiók. toominimize ezeket a díjakat hello virtuális gépek törléséhez Miután befejezte a futó hello minta. Lásd: [készletek tisztítása](#clean-up-pools).

Kötegelt készletek a felhő konfigurálása (csak Windows), vagy a virtuálisgép-konfiguráció (Windows és Linux) két módon konfigurálhatók. az alábbi hello Mintaparancsfájlok megjelenítése, hogyan toocreate mindkét konfigurációjú készletbe.

## <a name="prerequisites"></a>Előfeltételek

- Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.
- Batch-fiók létrehozása, ha még nem rendelkezik. Lásd: [Batch-fiók létrehozása az Azure CLI hello](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.
- Még nem még fel, ha egy alkalmazás toorun az a kezdő tevékenység konfigurálása Lásd: [alkalmazások tooAzure kötegelt az Azure parancssori felület Hozzáadás](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazás csomag tooAzure feltöltését.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>A felhőalapú szolgáltatás konfigurációs mintaparancsfájl készlet

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>A virtuális gép konfigurációs mintaparancsfájl készlet

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a>Készletek tisztítása

Miután lefuttatta a fenti mintaparancsfájl hello, futtassa a következő parancs toodelete hello készletek hello.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt használja a következő parancsok toocreate hello és kötegelt készletek kezelését.
Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) | Batch-fiók hitelesítése.  |
| [az kötegelt alkalmazás összefoglaló listája](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | A Batch-fiók hello hello a rendelkezésre álló alkalmazások listáját.  |
| [az batch-készlet létrehozása](https://docs.microsoft.com/cli/azure/batch/pool#create) | Hozzon létre virtuális gépek készletét.  |
| [az kötegelt készlet beállítása](https://docs.microsoft.com/cli/azure/batch/pool#set) | Egy készlet tulajdonságainak frissítésére.  |
| [az kötegelt készlet csomópont-ügynök-SKU listája](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | Lista elérhető csomópont ügynök SKU és lemezkép információkat.  |
| [az kötegelt készlet átméretezése](https://docs.microsoft.com/cli/azure/batch/pool#resize) | A megadott alkalmazáskészlet átméretezési hello száma a futó virtuális gépek hello.  |
| [az kötegelt készlet megjelenítése](https://docs.microsoft.com/cli/azure/batch/pool#show) | Egy készlet hello tulajdonságok megjelenítése.  |
| [az batch-készlet törlése](https://docs.microsoft.com/cli/azure/batch/pool#delete) | Törölje a megadott hello készlet.  |
| [az kötegelt készlet automatikus skálázás engedélyezése](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | Engedélyezze az automatikus skálázás egy címkészlet, és egy képletet.  |
| [az kötegelt készlet automatikus skálázás letiltása](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | Tiltsa le az automatikus skálázást a készlet.  |
| [az kötegelt csomópont listája](https://docs.microsoft.com/cli/azure/batch/node#list) | Listázza az összes hello számítási csomópont hello lévő megadott készlet.  |
| [az kötegelt csomópont újraindul](https://docs.microsoft.com/cli/azure/batch/node#reboot) | Hello megadott számítási csomópont újraindítása.  |
| [az batch munkaterület törlése](https://docs.microsoft.com/cli/azure/batch/node#delete) | Törlés felsorolt hello csomópontok hello a megadott készlet.  |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).

