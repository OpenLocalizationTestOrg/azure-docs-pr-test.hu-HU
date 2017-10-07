---
title: "Linux virtuális gép aaaTroubleshooting bővítmény hibák |} Microsoft Docs"
description: "További tudnivalók az Azure Linux virtuális gép bővítmény hibáinak elhárítása"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: f05d93f3-42fc-4a09-9798-d92f7929c417
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 29a0ca34207421e0014380000a313d3c44e7e594
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Azure Linux virtuális gép bővítmény hibáinak elhárítása
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Bővítmény állapotának megtekintése
Az Azure Resource Manager-sablonok hello Azure CLI hajthatók végre. Hello sablon végrehajtása, amennyiben az Azure erőforrás-kezelővel vagy hello parancssori eszközök hello bővítmény állapotát tekintheti meg.

Például:

Az Azure parancssori felület:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Extenson hibáinak elhárítása:
### <a name="re-running-hello-extension-on-hello-vm"></a>A virtuális gép hello ismételt futtatásával hello bővítmény
Ha futtatja a parancsfájlokat a virtuális gépet az egyéni parancsprogramok futtatására szolgáló bővítmény hello, néha hiba történt, amikor virtuális gép sikeresen létrejött, de hello parancsfájl nem futtatható. Ezek a feltételek hello ajánlott módja toorecover ezt a hibát a tooremove hello bővítménye, és futtassa újra a hello sablon.
Megjegyzés: A jövőben ez a funkció lenne továbbfejlesztett tooremove hello hello bővítmény eltávolításához szükséges.

#### <a name="remove-hello-extension-from-azure-cli"></a>Az Azure parancssori felület hello-kiterjesztés eltávolítása
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Ahol "publsher-name" megfelel-e toohello bővítménytípus hello kimenetből "get-példány-nézet az azure virtuális gép", és hello bővítmény erőforrás hello sablonból hello neve

Miután hello kiterjesztés eltávolításra került, hello sablon lehet az Újrafuttatja toorun hello parancsprogramok hello virtuális gép.

