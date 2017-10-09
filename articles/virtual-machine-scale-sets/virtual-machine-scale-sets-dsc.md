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
# <a name="using-virtual-machine-scale-sets-with-hello-azure-dsc-extension"></a><span data-ttu-id="b76b7-103">Hello Azure DSC-bővítményt a virtuálisgép-méretezési készlet használata</span><span class="sxs-lookup"><span data-stu-id="b76b7-103">Using Virtual Machine Scale Sets with hello Azure DSC Extension</span></span>
<span data-ttu-id="b76b7-104">[Virtuálisgép-méretezési csoportok](virtual-machine-scale-sets-overview.md) hello használható [Azure kívánt állapot konfigurációs szolgáltatása (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bővítmény kezelő.</span><span class="sxs-lookup"><span data-stu-id="b76b7-104">[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) can be used with hello [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extension handler.</span></span> <span data-ttu-id="b76b7-105">Virtuálisgép-méretezési csoportok adjon meg egy módon toodeploy nagyszámú virtuális gépek és a válasz tooload rugalmasan méretezhető növelésére és csökkentésére.</span><span class="sxs-lookup"><span data-stu-id="b76b7-105">Virtual machine scale sets provide a way toodeploy and manage large numbers of virtual machines, and can elastically scale in and out in response tooload.</span></span> <span data-ttu-id="b76b7-106">DSC használt tooconfigure hello virtuális gépek esetén, mert a azok ismét online elérhető, így hello éles szoftvert futtatnak.</span><span class="sxs-lookup"><span data-stu-id="b76b7-106">DSC is used tooconfigure hello VMs as they come online so they are running hello production software.</span></span>

## <a name="differences-between-deploying-toovirtual-machines-and-virtual-machine-scale-sets"></a><span data-ttu-id="b76b7-107">TooVirtual gépek és virtuálisgép-méretezési csoportok telepítése közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="b76b7-107">Differences between deploying tooVirtual Machines and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="b76b7-108">a virtuálisgép-méretezési csoport hello alapjául szolgáló sablon struktúra némileg eltér egy virtuális.</span><span class="sxs-lookup"><span data-stu-id="b76b7-108">hello underlying template structure for a virtual machine scale set is slightly different from a single VM.</span></span> <span data-ttu-id="b76b7-109">Pontosabban egy virtuális telepíti a bővítmények hello "virtuális gép" csomópont alatt.</span><span class="sxs-lookup"><span data-stu-id="b76b7-109">Specifically, a single VM deploys extensions under hello "virtualMachines" node.</span></span> <span data-ttu-id="b76b7-110">Nincs "kiterjesztésekkel" típusú bejegyzés ahol DSC toohello sablon szerepel-e</span><span class="sxs-lookup"><span data-stu-id="b76b7-110">There is an entry of type "extensions" where DSC is added toohello template</span></span>

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

<span data-ttu-id="b76b7-111">A virtuális gép méretezési készlet csomópont "Tulajdonságok" szakasza a hello "VirtualMachineProfile", "extensionProfile" attribútum.</span><span class="sxs-lookup"><span data-stu-id="b76b7-111">A virtual machine scale set node has a "properties" section with hello "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="b76b7-112">A DSC hozzáadása az "kiterjesztésekkel"</span><span class="sxs-lookup"><span data-stu-id="b76b7-112">DSC is added under "extensions"</span></span>

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a><span data-ttu-id="b76b7-113">A virtuálisgép-méretezési csoport működése</span><span class="sxs-lookup"><span data-stu-id="b76b7-113">Behavior for a Virtual Machine Scale Set</span></span>
<span data-ttu-id="b76b7-114">a virtuálisgép-méretezési csoport hello viselkedése azonos toohello működése egy virtuális.</span><span class="sxs-lookup"><span data-stu-id="b76b7-114">hello behavior for a virtual machine scale set is identical toohello behavior for a single VM.</span></span> <span data-ttu-id="b76b7-115">Amikor egy új virtuális Gépet hoz létre, azt hello DSC-bővítményt automatikusan ki van építve.</span><span class="sxs-lookup"><span data-stu-id="b76b7-115">When a new VM is created, it is automatically provisioned with hello DSC extension.</span></span> <span data-ttu-id="b76b7-116">WMF hello-kiterjesztés által igényelt hello újabb verzióját, hello VM újraindul, mielőtt online állapotba kerüljön.</span><span class="sxs-lookup"><span data-stu-id="b76b7-116">If a newer version of hello WMF is required by hello extension, hello VM reboots before coming online.</span></span> <span data-ttu-id="b76b7-117">Amikor online, hello DSC konfigurációs .zip tölti le, és építse ki azt a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="b76b7-117">Once it is online, it downloads hello DSC configuration .zip and provision it on hello VM.</span></span> <span data-ttu-id="b76b7-118">További részletek találhatók [hello Azure DSC-bővítmény áttekintése](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b76b7-118">More details can be found in [hello Azure DSC Extension Overview](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b76b7-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b76b7-119">Next steps</span></span>
<span data-ttu-id="b76b7-120">Vizsgálja meg a hello [Azure Resource Manager sablon hello DSC-bővítmény](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b76b7-120">Examine hello [Azure Resource Manager template for hello DSC extension](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="b76b7-121">Megtudhatja, hogyan hello [DSC-bővítményt biztonságosan kezeli a hitelesítő adatok](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b76b7-121">Learn how hello [DSC extension securely handles credentials](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="b76b7-122">Hello Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b76b7-122">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="b76b7-123">További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="b76b7-123">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

