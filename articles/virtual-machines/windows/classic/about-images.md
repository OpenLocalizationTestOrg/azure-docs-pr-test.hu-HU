---
title: "a Windows virtuális gépek aaaAbout képek |} Microsoft Docs"
description: "További tudnivalók a lemezképek Windows virtuális gépek Azure-ban való használatának módját."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: c7cfa1d018a5e99d5b68f559ec9ae1f14e4dec8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-images-for-windows-virtual-machines"></a>A Windows virtuális gépek képek kapcsolatos
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Információ keresése és lemezképek használatával hello Resource Manager modellben: [Itt](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Lemezképek használata

Hello Azure PowerShell-modulra és hello Azure portál toomanage hello képek elérhető tooyour Azure-előfizetés is használhatja. hello Azure PowerShell-modul több parancs beállításokat biztosít, így pontosan mit tudja toosee szeretné, vagy tegye. hello Azure-portálon grafikus felhasználói Felülettel biztosít sok hello mindennapos felügyeleti feladatokat.

Íme néhány példa hello Azure PowerShell-modult használó.

* **Az összes kép lekérése**:`Get-AzureVMImage`érhető el az aktuális előfizetésben hello lemezképek listáját adja vissza: a lemezképeket és az Azure vagy partnerek által kínáltak mellett. lehet, hogy nagy hello listájában. Hogyan hello a következő példák szemléltetik tooget rövidebb listáját.
* **Első kép családok**:`Get-AzureVMImage | select ImageFamily` kép családok listájának lekérése jelenít meg karakterláncok **ImageFamily** tulajdonság.
* **Egy megadott termékcsalád összes képek beolvasása**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Virtuálisgép-rendszerképek található**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` hello DataDiskConfiguration tulajdonságot, amelynek csak érvényes tooVM képek szűréssel működik ez a parancsmag. Ebben a példában is szűrők hello kimeneti tooonly hello címke és a lemezkép nevét.
* **Általános lemezkép mentéséhez**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **A speciális képének mentéséhez**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > hello OSState paraméter szükséges toocreate Virtuálisgép-lemezképet, amely magában foglalja a hello operációsrendszer-lemez, és adatlemezt csatolni. Ha nem hello paraméter, hello parancsmag létrehoz egy operációsrendszer-lemezképben. hello paraméter hello érték azt jelzi, hogy hello kép általánosítva van, vagy kifejezetten, alapján e hello operációsrendszer-lemez van készítve ismételt felhasználásra.

* **Lemezkép törlése**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Következő lépések
Emellett [létrehozása a Windows hello Azure-portálon gépek](tutorial.md).
