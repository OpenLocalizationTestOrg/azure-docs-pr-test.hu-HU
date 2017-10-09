---
title: "aaaEnable vagy letiltását Azure VM figyelése"
description: "Ismerteti, hogyan tooEnable vagy tiltsa le a Azure VM figyelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 39c2211e4e5f3ad364513b52c5acc70c00b432fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a>Engedélyezheti vagy tilthatja le, Azure virtuális gép figyelése

Ez a szakasz ismerteti, hogyan tooenable vagy a figyelés letiltásakor a virtuális gépek Azure-on futó. Engedélyezheti vagy letilthatja a figyelési hello portál vagy az Azure parancssori felület Mac, Linux és Windows (hello Azure CLI) használatával.

> [!IMPORTANT]
> Ez a dokumentum ismerteti a hello Linux diagnosztikai bővítmény, amely elavult 2.3 verzióját. 2.3 verziója lesz támogatott 2018. június 30-ig.
>
> Linux diagnosztikai bővítmény helyette engedélyezhető hello 3.0-s verziója. További információkért lásd: [dokumentáció hello](./diagnostic-extension.md).

## <a name="enable--disable-monitoring-through-hello-azure-portal"></a>Engedélyezi / letiltja a figyelt hello Azure-portálon

Az Azure virtuális gép, amely biztosít a példány adatait 1 perces időszakokban figyelését is engedélyezheti. (a tároló módosításai érvényesek). Részletes diagnosztikai adatokat hello VM hello portál diagramjait vagy azon keresztül hello API majd érhető el. Alapértelmezés szerint az Azure portál lehetővé teszi a gazdagép alapú felügyelete, metrikák korlátozott számú. Engedélyezheti, hogy egy virtuális Gépet, miközben hello virtuális gép fut vagy leállított állapotban belül a metrikák figyelését.

* Nyissa meg hello Azure portál, [https://portal.azure.com](https://portal.azure.com).
* A bal oldali navigációs hello kattintson a virtuális gépek.
* Hello lista virtuális gépeken válasszon ki egy fut vagy leállt példányt. hello "Virtuális gép" panel nyílik meg.
* Kattintson az összes beállítást.
* Kattintson a diagnosztika.
* Állapot tooOn módosítása vagy ki. Ezen a panelen hello szinten tooenable szeretné a virtuális gép részletei figyelést is kiválaszthat.

![Engedélyezi / letiltja a figyelés hello Azure-portálon keresztül.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a>Engedélyezi / letiltja a megfigyelés az Azure parancssori felület

egy Azure virtuális gép tooenable figyelését.

* Hozzon létre egy fájlt (például PrivateConfig.json):

```json
{
        "storageAccountName":"hello storage account tooreceive data",
        "storageAccountKey":"hello key of hello account"
}
```

* Azure parancssori felületen keresztül hello bővítmény engedélyezése.

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

További információkért lásd: [Linux diagnosztikai bővítmény használatával tooMonitor Linux virtuális gép teljesítmény- és diagnosztikai adatokat](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
