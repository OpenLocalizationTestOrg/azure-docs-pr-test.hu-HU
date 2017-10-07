---
title: "Windows virtuális gép kiterjesztésű sablonok aaaAuthoring |} Microsoft Docs"
description: "További tudnivalók a bővítményekkel Azure Resource Manager-sablonok készítése Windows virtuális gépek"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 418dd1f7-ded8-45ab-9a5a-a59d245e2555
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: c5156cb3859f7ff86bebda942150d268e57d6486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Windows Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Azure PowerShell futtassa a következő Azure PowerShell-parancsmag hello:

      Get-AzureVMAvailableExtension


Ez a parancsmag adja vissza hello közzétevő neve, a bővítmény neve és verziója az alábbiak szerint:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

A három Tulajdonságok leképezése túl "publisher", "type" és "typeHandlerVersion" rendre a fenti sablon részlet hello.

> [!NOTE]
> Mindig ajánlott toouse hello legújabb bővítmény verzió tooget hello legtöbb frissített funkcióit.
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a>Hello séma hello bővítmény konfigurációs paraméterek azonosítása
hello következő lépés egy bővítmény sablont készít az formátuma tooidentify hello szükséges konfigurációs paramétereket. Mindegyik bővítményt a saját paraméterek támogat.

a minta konfigurációi Windows-bővítmények, toolook lásd: [Windows bővítmények minták](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Tekintse meg a következő tooget egy teljes mértékben teljes sablont a Virtuálisgép-bővítmények toohello.

[Virtuális gép Windows egyéni parancsprogramok futtatására szolgáló bővítmény](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

Hello sablont készít, az Azure PowerShell használatával telepítheti.

