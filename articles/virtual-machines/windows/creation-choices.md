---
title: "aaaDifferent módon toocreate egy Windows Azure-ban |} Microsoft Docs"
description: "Hello különböző módokon toocreate egy Windows rendszerű virtuális gép a Resource Manager sorolja fel."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2928d4daa9b44c4d3a5083092a82c9a7f7c69fae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-windows-virtual-machine"></a>A Windows rendszerű virtuális gép különböző módokon toocreate

Az Azure kínál különböző módokon toocreate egy virtuális gép, mert a virtuális gépek különböző felhasználóknak és céloknak vannak kialakítva. Ez azt jelenti, hogy szüksége toomake néhány hello virtuális géppel kapcsolatos döntések és hogyan toocreate azt. Ez a cikk lehetővé teszi az ezeket a döntéseket összegzését, és hivatkozásokat tartalmaz tooinstructions.

## <a name="azure-portal"></a>Azure Portal
Egy egyszerű módon tootry ki egy virtuális gépet hello Azure-portálon a használata, különösen akkor, ha nemrég kezdte az Azure-ral. 

[Hozzon létre egy olyan virtuális géphez a Windows hello-portál használatával](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a>Sablon
Virtuális gépeknél szükség van a több erőforrás kombinációját (például egy rendelkezésre állási készletek és a storage-fiókok). Ahelyett, hogy telepíti, és az egyes erőforrások kezelése külön-külön, létrehozhat egy Azure Resource Manager-sablon, amely üzembe és tesz elérhetővé minden hello erőforrást egyetlen, koordinált műveletben.

* [Windows rendszerű virtuális gép létrehozása egy Resource Manager-sablonnal](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a>Azure PowerShell
Ha szeretné, hogy működik-e a parancs-rendszerhéj, használhatja az Azure PowerShell.

* [Windows rendszerű virtuális gép létrehozása a PowerShell használatával](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a>Visual Studio
Használja a Visual Studio toobuild, kezeléséhez, és hello Azure eszközök rendelkező virtuális gépek telepítése a Visual Studio és hello Azure SDK-t.

[Az Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

