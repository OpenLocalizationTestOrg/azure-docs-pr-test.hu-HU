---
title: "AAA \"Azure Batch számítási csomópont környezeti változók |} Microsoft dokumentumok\""
description: "Számítási csomópont környezeti változó hivatkozás az Azure Batch használatával."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/05/2017
ms.author: tamram
ms.openlocfilehash: 860f34b530579a81fbd5cf8ffa31df79d917c080
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-compute-node-environment-variables"></a>Azure Batch számítási csomópont környezeti változók
Hello [Azure Batch szolgáltatás](https://azure.microsoft.com/services/batch/) állítja be a következő környezeti változók a számítási csomópontok hello. Ezek a környezeti változók feladat parancssorokat, és a hello programok is hivatkozni, és parancsfájlok hello parancssorokat futtathatja.

Környezeti változók kötegelt használatával kapcsolatos további információkért lásd: [környezeti beállítások feladatok](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Környezeti változó látható

Ezek a környezeti változók csak hello hello kontextusában láthatók **feladat felhasználói**, hello hello csomóponton, amely alatt a feladat végrehajtása a felhasználói fiók. Fogja *nem* tapasztalja, ha Ön [távoli csatlakozás](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) tooa számítási csomópont keresztül Remote Desktop Protocol (RDP) vagy a Secure Shell (SSH) és a lista hello környezeti változókat. Ennek az az oka hello távoli kapcsolathoz használt felhasználói fiók nem hello. ugyanaz, mint a hello hello feladat által használt fiókot.

## <a name="command-line-expansion-of-environment-variables"></a>Környezeti változók parancssori bővítése

a feladatok által végrehajtott hello parancssorokat számítási csomópontok tegye nem futtathatók a rendszerhéj. Ezért sorok az nem natív módon kihasználni a rendszerhéj-szolgáltatások, például a környezeti változók bővítésének (Ez magában foglalja a hello `PATH`). tootake előny ilyen funkciók kell **hello rendszerhéj meghívása** hello parancssorban. Például elindíthatja `cmd.exe` Windows a számítási csomópontok vagy `/bin/sh` Linux csomópontján:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Környezeti változók

| Változó neve                     | Leírás                                                              | Rendelkezésre állás | Példa |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | hello nevét, amely a feladat hello hello Batch-fiókhoz tartozik.                  | Minden feladat.   | mybatchaccount |
| AZ_BATCH_CERTIFICATES_DIR       | A címtárhoz az hello [feladatütemezési munkakönyvtár] [ files_dirs] Linux rendszer tárolja a tanúsítványok a számítási csomópontok. Vegye figyelembe, hogy az e környezeti változó nem alkalmazza a tooWindows számítási csomópontok.                                                  | Minden feladat.   |  /mnt/Batch/Tasks/workitems/batchjob001/Job-1/task001/certs |
| AZ_BATCH_JOB_ID                 | hello azonosítója, amely a feladat hello hello feladat tartozik. | Minden olyan feladat, kivéve a feladat indítása. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | hello feladat előkészítése hello elérési útját [feladat directory] [ files_dirs] hello csomóponton. | Kezdő tevékenység és a feladat előkészítése tevékenységet kívül az összes feladatot. Csak akkor érhető el, ha a feladat előkészítése tevékenység hello feladat van konfigurálva. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | hello feladat előkészítése hello elérési útját [feladatütemezési munkakönyvtár] [ files_dirs] hello csomóponton. | Kezdő tevékenység és a feladat előkészítése tevékenységet kívül az összes feladatot. Csak akkor érhető el, ha a feladat előkészítése tevékenység hello feladat van konfigurálva. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_NODE_ID                | feladat hello hello csomópont hello azonosítója hozzá van rendelve. | Minden feladat. | TVM-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_ROOT_DIR          | az összes hello gyökér hello elérési útját [Batch-könyvtárak] [ files_dirs] hello csomóponton. | Minden feladat. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | hello hello elérési útját [megosztott könyvtár] [ files_dirs] hello csomóponton. Minden olyan feladat, amely a csomóponton végre rendelkezik olvasási/írási hozzáférést toothis directory. Feladatokat, amelyek a többi csomóponton végre nem rendelkeznek a távelérési toothis könyvtár (nincs olyan "megosztott" hálózati könyvtár). | Minden feladat. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | hello hello elérési útját [indítsa el a feladat directory] [ files_dirs] hello csomóponton. | Minden feladat. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | futó feladat hello hello készlettől hello azonosítója. | Minden feladat. | batchpool001 |
| AZ_BATCH_TASK_DIR               | hello hello elérési útját [feladat directory] [ files_dirs] hello csomóponton. Ez a könyvtár tartalmaz hello `stdout.txt` és `stderr.txt` hello tevékenységhez, és hello AZ_BATCH_TASK_WORKING_DIR. | Minden feladat. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | hello azonosítója hello aktuális feladatot. | Minden olyan feladat, kivéve a feladat indítása. | task001 |
| AZ_BATCH_TASK_WORKING_DIR       | hello hello elérési útját [feladatütemezési munkakönyvtár] [ files_dirs] hello csomóponton. jelenleg fut a feladat hello rendelkezik olvasási/írási hozzáférést toothis directory. | Minden feladat. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | csomópontok és a csomópont tooa lefoglalt magok számát hello listája [többpéldányos feladat][multi_instance]. Csomópontok és -magok felsorolt hello formátumban`numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, ahol a csomópontok hello számát követi egy vagy több csomópont IP-címek hello magok száma az egyes. |  Többpéldányos elsődleges és részfeladatok. |`2 10.0.0.4 1 10.0.0.5 1` |
| AZ_BATCH_NODE_LIST              | hello csomópontlista tooa kiosztott [többpéldányos feladat] [ multi_instance] hello formátumban `nodeIP;nodeIP`. | Többpéldányos elsődleges és részfeladatok. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_HOST_LIST              | hello csomópontlista tooa kiosztott [többpéldányos feladat] [ multi_instance] hello formátumban `nodeIP,nodeIP`. | Többpéldányos elsődleges és részfeladatok. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_MASTER_NODE            | hello IP-cím és port hello a számítási csomópont, mely hello elsődleges feladat egy [többpéldányos feladat] [ multi_instance] futtatja. | Többpéldányos elsődleges és részfeladatok. | `10.0.0.4:6000`|
| AZ_BATCH_TASK_SHARED_DIR | A könyvtár elérési útja, amely azonos az elsődleges feladat hello és minden vonatkozó részfeladatnál annak regisztrálása egy [többpéldányos feladat][multi_instance]. hello elérési út létezik minden csomóponton, amelyen hello többpéldányos feladat fut. a, és olvasási/írási elérhető toohello feladat parancsok ezen a csomóponton futó (mindkét hello [koordinációs parancs] [ coord_cmd] és hello [alkalmazás parancs][app_cmd]). Résztevékenység vagy egy elsődleges feladatot, amely a többi csomóponton végre nincs távelérés toothis könyvtár (nincs olyan "megosztott" hálózati könyvtár). | Többpéldányos elsődleges és részfeladatok. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Megadja, hogy hello aktuális csomópont hello fő csomópont egy [többpéldányos feladat][multi_instance]. A lehetséges értékek: `true` és `false`.| Többpéldányos elsődleges és részfeladatok. | `true` |
| AZ_BATCH_NODE_IS_DEDICATED | Ha `true`, hello aktuális csomópont dedikált csomópontja. Ha `false`, ez egy [alacsony prioritású csomópont](batch-low-pri-vms.md). | Minden feladat. | `true` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
