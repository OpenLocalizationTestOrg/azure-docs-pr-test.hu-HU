---
title: a Windows Azure CLI aaaUsing hello |} Microsoft Docs
description: "A Windows hello Azure parancssori felület használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: edca4a2701a7dfc0fc54df5649e8a5e7afc95490
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-on-windows"></a>A Windows hello Azure parancssori felület használatával

hello Azure parancssori felület (CLI) biztosít egy parancssor, és a parancsfájl-kezelési környezet létrehozására és Azure-erőforrások kezelésére. hello Azure CLI macOS, a Linux és a Windows operációs rendszereken érhető el. Ezek az operációs rendszerek között hello parancssori felület parancsai azonosak, azonban az operációs rendszer adott parancsfájlkezelő szintaxis eltérőek lehetnek.

Ez a dokumentum részletek hello módon hello Azure parancssori felület telepítve, és az egyes Windows és a részletek szintaktikai szempontok futtatnak. A részletes Azure CLI dokumentációját lásd [Azure CLI dokumentáció]( https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="windows-subsystem-for-linux"></a>Linux rendszerhez készült Windows alrendszer

hello Windows alrendszer Linux (WSL) Ubuntu Linux környezetet biztosít a Windows 10 évforduló és a későbbi kiadások. Ha engedélyezve van, WSL natív Bash élményt, létrehozásának és az Azure CLI-parancsfájlok futtatásakor, amellyel nyújt. WSL natív Bash élményt biztosít, mert az Azure parancssori felület parancsfájlok megoszthatók macOS, a Linux és a Windows között módosítás nélkül.

toouse hello WSL, az Azure CLI hello következő végezze el.

|Tevékenység | Útmutatás |
|---|---|
| WSL engedélyezése | [Telepítse a WSL dokumentáció](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| Hello Azure parancssori felület telepítése |[WSL/Ubuntu 14.04 hello parancssori felület telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

hello Azure CLI Windows natív módon futtatható. Ebben a konfigurációban hello Azure CLI csomag hello Windows operációs rendszerre van telepítve, és PowerShell parancsok futtatását. Ebben a konfigurációban az Azure parancssori felület parancsaiban és parancsfájljaiban futtatható bármely, a Windows támogatott verzióját azonban platform adott parancsfájlkezelő szintaxis kötelező. Ebből kifolyólag parancsfájlok nem feltétlenül lehet megosztani macOS, a Linux és a Windows között módosítás nélkül.

toouse hello Azure CLI Windows, a jelen útmutatást hello csomag telepítésének [telepítés hello CLI Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).

## <a name="docker-image"></a>Docker kép

Docker Windows használatakor a Docker-lemezkép indíthatók el, amely tartalmazza az Azure CLI hello. Ez a rendszerkép kijelentkezés Linux alapul, és egy natív Bash élmény tartalmaz.  Docker a Windows és a hello Azure CLI képet, a parancsfájlok toobe macOS, a Linux és a Windows között megosztott használatakor. 

toouse hello Azure parancssori felület a Docker Windows, győződjön meg arról, hogy Docker for Windows fut, és futtassa a következő parancs hello.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

Ezt követően a Bash munkamenet indul el, amely előre feltölti hello Azure CLI-eszközökkel.

## <a name="next-steps"></a>Következő lépések

[Az Azure virtuális gépek CLI minta](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Az Azure Web Apps CLI-minták](../../app-service-web/app-service-cli-samples.md)

[Az Azure SQL CLI minták](../../sql-database/sql-database-cli-samples.md)
