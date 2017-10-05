---
title: "Használatával Célállapotkonfiguráció a virtuálisgép-méretezési csoportok |} Microsoft Docs"
description: "Az Azure DSC-bővítményt a virtuálisgép-méretezési használatával beállítása"
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
ms.openlocfilehash: b61b0acf3072569ab733a13defb465c921d26187
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a><span data-ttu-id="487e8-103">Az Azure DSC-bővítményt a virtuálisgép-méretezési használatával beállítása</span><span class="sxs-lookup"><span data-stu-id="487e8-103">Using Virtual Machine Scale Sets with the Azure DSC Extension</span></span>
<span data-ttu-id="487e8-104">[Virtuálisgép-méretezési csoportok](virtual-machine-scale-sets-overview.md) együtt a [Azure kívánt állapot konfigurációs szolgáltatása (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bővítmény kezelő.</span><span class="sxs-lookup"><span data-stu-id="487e8-104">[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) can be used with the [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extension handler.</span></span> <span data-ttu-id="487e8-105">Virtuálisgép-méretezési csoportok nyújtanak olyan telepíthetnek és kezelhetnek olyan nagy számú virtuális gépet, és rugalmasan méretezhető és betölteni válaszul.</span><span class="sxs-lookup"><span data-stu-id="487e8-105">Virtual machine scale sets provide a way to deploy and manage large numbers of virtual machines, and can elastically scale in and out in response to load.</span></span> <span data-ttu-id="487e8-106">A DSC ismét online elérhető, a termelési szoftvert futtatnak, mert a virtuális gépek konfigurálására szolgál.</span><span class="sxs-lookup"><span data-stu-id="487e8-106">DSC is used to configure the VMs as they come online so they are running the production software.</span></span>

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a><span data-ttu-id="487e8-107">Központi telepítése virtuális gépek és virtuálisgép-méretezési csoportok közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="487e8-107">Differences between deploying to Virtual Machines and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="487e8-108">Az alapul szolgáló sablon-szerkezet egy virtuálisgép-méretezési csoport egy egy virtuális némileg eltérő.</span><span class="sxs-lookup"><span data-stu-id="487e8-108">The underlying template structure for a virtual machine scale set is slightly different from a single VM.</span></span> <span data-ttu-id="487e8-109">Egy virtuális pontosabban, a "virtuális gép" csomópontban bővítmények telepíti.</span><span class="sxs-lookup"><span data-stu-id="487e8-109">Specifically, a single VM deploys extensions under the "virtualMachines" node.</span></span> <span data-ttu-id="487e8-110">Nincs "kiterjesztésekkel" típusú bejegyzés ahol DSC hozzáadódik a sablon</span><span class="sxs-lookup"><span data-stu-id="487e8-110">There is an entry of type "extensions" where DSC is added to the template</span></span>

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

<span data-ttu-id="487e8-111">A virtuális gép méretezési készlet csomópont "Tulajdonságok" szakasza az a "VirtualMachineProfile", "extensionProfile" attribútum.</span><span class="sxs-lookup"><span data-stu-id="487e8-111">A virtual machine scale set node has a "properties" section with the "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="487e8-112">A DSC hozzáadása az "kiterjesztésekkel"</span><span class="sxs-lookup"><span data-stu-id="487e8-112">DSC is added under "extensions"</span></span>

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a><span data-ttu-id="487e8-113">A virtuálisgép-méretezési csoport működése</span><span class="sxs-lookup"><span data-stu-id="487e8-113">Behavior for a Virtual Machine Scale Set</span></span>
<span data-ttu-id="487e8-114">A virtuálisgép-méretezési csoport viselkedés megegyezik a működése egy virtuális.</span><span class="sxs-lookup"><span data-stu-id="487e8-114">The behavior for a virtual machine scale set is identical to the behavior for a single VM.</span></span> <span data-ttu-id="487e8-115">Amikor egy új virtuális Gépet hoz létre, akkor a DSC-bővítményt automatikusan ki van építve.</span><span class="sxs-lookup"><span data-stu-id="487e8-115">When a new VM is created, it is automatically provisioned with the DSC extension.</span></span> <span data-ttu-id="487e8-116">A WMF újabb verziója szükséges a bővítmény által, ha a virtuális gép újraindul, mielőtt online állapotba kerüljön.</span><span class="sxs-lookup"><span data-stu-id="487e8-116">If a newer version of the WMF is required by the extension, the VM reboots before coming online.</span></span> <span data-ttu-id="487e8-117">Online állapotban, ha a DSC-konfiguráció .zip tölti le, és építse ki azt a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="487e8-117">Once it is online, it downloads the DSC configuration .zip and provision it on the VM.</span></span> <span data-ttu-id="487e8-118">További részletek találhatók [az Azure DSC-bővítmény áttekintése](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="487e8-118">More details can be found in [the Azure DSC Extension Overview](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="487e8-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="487e8-119">Next steps</span></span>
<span data-ttu-id="487e8-120">Vizsgálja meg a [Azure Resource Manager sablon a DSC-bővítmény](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="487e8-120">Examine the [Azure Resource Manager template for the DSC extension](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="487e8-121">Ismerje meg, hogy a [DSC-bővítményt biztonságosan kezeli a hitelesítő adatok](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="487e8-121">Learn how the [DSC extension securely handles credentials](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="487e8-122">Az Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása az Azure célállapot-konfiguráció bővítmény kezelő](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="487e8-122">For more information on the Azure DSC extension handler, see [Introduction to the Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="487e8-123">További információ a PowerShell DSC [látogasson el a PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="487e8-123">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

