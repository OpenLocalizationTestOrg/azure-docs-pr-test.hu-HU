---
title: "aaaTroubleshooting Windows virtuális gép bővítmény hibák |} Microsoft Docs"
description: "További tudnivalók az Azure Windows virtuális gép bővítmény hibáinak elhárítása"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: d24544743d9e0cb1a68ec9ab7718716f918054f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Azure Windows virtuális gép bővítmény hibáinak elhárítása
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Bővítmény állapotának megtekintése
Az Azure Resource Manager-sablonok az Azure PowerShell-lel hajtható végre. Hello sablon végrehajtása, amennyiben az Azure erőforrás-kezelővel vagy hello parancssori eszközök hello bővítmény állapotát tekintheti meg.

Például:

Az Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Itt egy hello minta kimenet:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Bővítmény hibáinak elhárítása
### <a name="re-running-hello-extension-on-hello-vm"></a>A virtuális gép hello ismételt futtatásával hello bővítmény
Ha futtatja a parancsfájlokat a virtuális gépet az egyéni parancsprogramok futtatására szolgáló bővítmény hello, néha hiba történt, amikor virtuális gép sikeresen létrejött, de hello parancsfájl nem futtatható. Ezek a feltételek hello ajánlott módja toorecover ezt a hibát a tooremove hello bővítménye, és futtassa újra a hello sablon.
Megjegyzés: A jövőben ez a funkció lenne továbbfejlesztett tooremove hello hello bővítmény eltávolításához szükséges.

#### <a name="remove-hello-extension-from-azure-powershell"></a>Azure PowerShell hello-kiterjesztés eltávolítása
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Miután hello kiterjesztés eltávolításra került, hello sablon lehet az Újrafuttatja toorun hello parancsprogramok hello virtuális gép.

