---
title: "aaaUsing kívánt állapot konfigurációs rendelkező virtuális gép méretezési csoportok |} Microsoft Docs"
description: "Hello Azure DSC-bővítményt a virtuálisgép-méretezési készlet használata"
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: a35f1ca6700aa4889978032aa512882db50d6573
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-virtual-machine-scale-sets-with-hello-azure-dsc-extension"></a>Hello Azure DSC-bővítményt a virtuálisgép-méretezési készlet használata
[Virtuálisgép-méretezési csoportok](virtual-machine-scale-sets-overview.md) hello használható [Azure kívánt állapot konfigurációs szolgáltatása (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bővítmény kezelő. Virtuálisgép-méretezési csoportok adjon meg egy módon toodeploy nagyszámú virtuális gépek és a válasz tooload rugalmasan méretezhető növelésére és csökkentésére. DSC használt tooconfigure hello virtuális gépek esetén, mert a azok ismét online elérhető, így hello éles szoftvert futtatnak.

## <a name="differences-between-deploying-toovirtual-machines-and-virtual-machine-scale-sets"></a>TooVirtual gépek és virtuálisgép-méretezési csoportok telepítése közötti különbségek
a virtuálisgép-méretezési csoport hello alapjául szolgáló sablon struktúra némileg eltér egy virtuális. Pontosabban egy virtuális telepíti a bővítmények hello "virtuális gép" csomópont alatt. Nincs "kiterjesztésekkel" típusú bejegyzés ahol DSC toohello sablon szerepel-e

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

A virtuális gép méretezési készlet csomópont "Tulajdonságok" szakasza a hello "VirtualMachineProfile", "extensionProfile" attribútum. A DSC hozzáadása az "kiterjesztésekkel"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>A virtuálisgép-méretezési csoport működése
a virtuálisgép-méretezési csoport hello viselkedése azonos toohello működése egy virtuális. Amikor egy új virtuális Gépet hoz létre, azt hello DSC-bővítményt automatikusan ki van építve. WMF hello-kiterjesztés által igényelt hello újabb verzióját, hello VM újraindul, mielőtt online állapotba kerüljön. Amikor online, hello DSC konfigurációs .zip tölti le, és építse ki azt a virtuális gép hello. További részletek találhatók [hello Azure DSC-bővítmény áttekintése](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Következő lépések
Vizsgálja meg a hello [Azure Resource Manager sablon hello DSC-bővítmény](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Megtudhatja, hogyan hello [DSC-bővítményt biztonságosan kezeli a hitelesítő adatok](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Hello Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview). 

