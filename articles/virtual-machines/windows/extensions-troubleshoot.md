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
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a><span data-ttu-id="ff1c9-103">Azure Windows virtuális gép bővítmény hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="ff1c9-103">Troubleshooting Azure Windows VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="ff1c9-104">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="ff1c9-104">Viewing extension status</span></span>
<span data-ttu-id="ff1c9-105">Az Azure Resource Manager-sablonok az Azure PowerShell-lel hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="ff1c9-105">Azure Resource Manager templates can be executed from Azure PowerShell.</span></span> <span data-ttu-id="ff1c9-106">Hello sablon végrehajtása, amennyiben az Azure erőforrás-kezelővel vagy hello parancssori eszközök hello bővítmény állapotát tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ff1c9-106">Once hello template is executed, hello extension status can be viewed from Azure Resource Explorer or hello command line tools.</span></span>

<span data-ttu-id="ff1c9-107">Például:</span><span class="sxs-lookup"><span data-stu-id="ff1c9-107">Here is an example:</span></span>

<span data-ttu-id="ff1c9-108">Az Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ff1c9-108">Azure PowerShell:</span></span>

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

<span data-ttu-id="ff1c9-109">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="ff1c9-109">Here is hello sample output:</span></span>

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
  <span data-ttu-id="ff1c9-110">]</span><span class="sxs-lookup"><span data-stu-id="ff1c9-110">]</span></span>

## <a name="troubleshooting-extension-failures"></a><span data-ttu-id="ff1c9-111">Bővítmény hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="ff1c9-111">Troubleshooting extension failures</span></span>
### <a name="re-running-hello-extension-on-hello-vm"></a><span data-ttu-id="ff1c9-112">A virtuális gép hello ismételt futtatásával hello bővítmény</span><span class="sxs-lookup"><span data-stu-id="ff1c9-112">Re-running hello extension on hello VM</span></span>
<span data-ttu-id="ff1c9-113">Ha futtatja a parancsfájlokat a virtuális gépet az egyéni parancsprogramok futtatására szolgáló bővítmény hello, néha hiba történt, amikor virtuális gép sikeresen létrejött, de hello parancsfájl nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="ff1c9-113">If you are running scripts on hello VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but hello script has failed.</span></span> <span data-ttu-id="ff1c9-114">Ezek a feltételek hello ajánlott módja toorecover ezt a hibát a tooremove hello bővítménye, és futtassa újra a hello sablon.</span><span class="sxs-lookup"><span data-stu-id="ff1c9-114">Under these conditons, hello recommended way toorecover from this error is tooremove hello extension and rerun hello template again.</span></span>
<span data-ttu-id="ff1c9-115">Megjegyzés: A jövőben ez a funkció lenne továbbfejlesztett tooremove hello hello bővítmény eltávolításához szükséges.</span><span class="sxs-lookup"><span data-stu-id="ff1c9-115">Note: In future, this functionality would be enhanced tooremove hello need for uninstalling hello extension.</span></span>

#### <a name="remove-hello-extension-from-azure-powershell"></a><span data-ttu-id="ff1c9-116">Azure PowerShell hello-kiterjesztés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ff1c9-116">Remove hello extension from Azure PowerShell</span></span>
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

<span data-ttu-id="ff1c9-117">Miután hello kiterjesztés eltávolításra került, hello sablon lehet az Újrafuttatja toorun hello parancsprogramok hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ff1c9-117">Once hello extension has been removed, hello template can be re-executed toorun hello scripts on hello VM.</span></span>

