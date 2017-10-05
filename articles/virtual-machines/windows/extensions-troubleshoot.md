---
title: "Windows virtuális gép bővítmény hibáinak elhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 4dba196e1b838f2092b30972fb070514bd2a4b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a><span data-ttu-id="047b9-103">Azure Windows virtuális gép bővítmény hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="047b9-103">Troubleshooting Azure Windows VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="047b9-104">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="047b9-104">Viewing extension status</span></span>
<span data-ttu-id="047b9-105">Az Azure Resource Manager-sablonok az Azure PowerShell-lel hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="047b9-105">Azure Resource Manager templates can be executed from Azure PowerShell.</span></span> <span data-ttu-id="047b9-106">A sablon végrehajtása, amennyiben a bővítmény állapotát tekintheti meg az Azure erőforrás-kezelővel vagy a parancssori eszközöket.</span><span class="sxs-lookup"><span data-stu-id="047b9-106">Once the template is executed, the extension status can be viewed from Azure Resource Explorer or the command line tools.</span></span>

<span data-ttu-id="047b9-107">Például:</span><span class="sxs-lookup"><span data-stu-id="047b9-107">Here is an example:</span></span>

<span data-ttu-id="047b9-108">Az Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="047b9-108">Azure PowerShell:</span></span>

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

<span data-ttu-id="047b9-109">Itt látható a minta kimenete:</span><span class="sxs-lookup"><span data-stu-id="047b9-109">Here is the sample output:</span></span>

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
  <span data-ttu-id="047b9-110">]</span><span class="sxs-lookup"><span data-stu-id="047b9-110">]</span></span>

## <a name="troubleshooting-extension-failures"></a><span data-ttu-id="047b9-111">Bővítmény hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="047b9-111">Troubleshooting extension failures</span></span>
### <a name="re-running-the-extension-on-the-vm"></a><span data-ttu-id="047b9-112">Ismételt futtatásával a bővítményt a virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="047b9-112">Re-running the extension on the VM</span></span>
<span data-ttu-id="047b9-113">Ha futtatja parancsfájlok a virtuális Gépre egyéni parancsprogramok futtatására szolgáló bővítmény használatával, néha futtatható hiba történt, amikor a virtuális gép sikeresen létrejött, de a parancsfájl futtatása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="047b9-113">If you are running scripts on the VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but the script has failed.</span></span> <span data-ttu-id="047b9-114">Ezek a feltételek alapján ajánlott módja a hiba helyreállításához, távolítsa el a bővítményt, és futtassa újra a sablont.</span><span class="sxs-lookup"><span data-stu-id="047b9-114">Under these conditons, the recommended way to recover from this error is to remove the extension and rerun the template again.</span></span>
<span data-ttu-id="047b9-115">Megjegyzés: A jövőben ez a funkció javítani fogja a szükséges a bővítmény eltávolítása eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="047b9-115">Note: In future, this functionality would be enhanced to remove the need for uninstalling the extension.</span></span>

#### <a name="remove-the-extension-from-azure-powershell"></a><span data-ttu-id="047b9-116">Távolítsa el a kiterjesztés Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="047b9-116">Remove the extension from Azure PowerShell</span></span>
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

<span data-ttu-id="047b9-117">Ha a kiterjesztés eltávolításra került, a sablon lehet az Újrafuttatja a parancsfájlok futtatásához a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="047b9-117">Once the extension has been removed, the template can be re-executed to run the scripts on the VM.</span></span>

