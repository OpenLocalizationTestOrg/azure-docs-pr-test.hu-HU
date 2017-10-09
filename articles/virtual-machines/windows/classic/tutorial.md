---
title: "egy virtuális Gépet az Azure-portálon hello aaaCreate |} Microsoft Docs"
description: "Windows virtuális gép létrehozása az Azure-portálon hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 3848a3d06a7ce377633db78ea385826ed40f94fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-running-windows-in-hello-azure-portal"></a>A Windows hello Azure-portálon futó virtuális gép létrehozása
> [!div class="op_single_selector"]
> * [Azure Portal](tutorial.md)
> * [PowerShell: Klasszikus telepítési](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager telepítési modell használatával a következő lépésekkel](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello segítségével **Azure-portálon**.

Az oktatóanyag bemutatja, hogyan toocreate egy Azure virtuális gép (VM) fut a Windows hello Azure-portálon. A Windows Server lemezkép használjuk példaként, de ez csak egy hello a számos lemezképek Azure ajánlatokat. Vegye figyelembe, hogy a rendszerképek függ-e az előfizetés. Windows asztali rendszerképek például elérhető tooMSDN előfizetők lehet.

Ez a szakasz bemutatja, hogyan toouse hello **irányítópult** az Azure portál tooselect hello és majd a hello virtuális gép létrehozása.

Virtuális gépek használatával is létrehozhat [a saját lemezképek](createupload-vhd.md). Ezzel és más módszerekkel kapcsolatos toolearn lásd [különböző módokon toocreate egy Windows rendszerű virtuális gép](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on hello classic portal. -->

## <a id="createvirtualmachine"></a>Hello virtuális gép létrehozása
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hello Resource Manager üzembe helyezési modellben virtuális gép létrehozása](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a hello Azure-portálon.
* Jelentkezzen be a virtuális gép toohello. Útmutatásért lásd: [jelentkezzen be Windows Server rendszerű tooa virtuális gép](connect-logon.md).
* Csatlakoztassa a lemezadatokat toostore. Csatolhat is üres és a lemezek, amelyek adatokat tartalmaznak. Útmutatásért lásd: hello [adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolása](attach-disk.md).
