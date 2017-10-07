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
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a><span data-ttu-id="c3ed3-103">Azure Linux virtuális gép bővítmény hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="c3ed3-103">Troubleshooting Azure Linux VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="c3ed3-104">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="c3ed3-104">Viewing extension status</span></span>
<span data-ttu-id="c3ed3-105">Az Azure Resource Manager-sablonok hello Azure CLI hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="c3ed3-105">Azure Resource Manager templates can be executed from hello  Azure CLI.</span></span> <span data-ttu-id="c3ed3-106">Hello sablon végrehajtása, amennyiben az Azure erőforrás-kezelővel vagy hello parancssori eszközök hello bővítmény állapotát tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c3ed3-106">Once hello template is executed, hello extension status can be viewed from Azure Resource Explorer or hello command line tools.</span></span>

<span data-ttu-id="c3ed3-107">Például:</span><span class="sxs-lookup"><span data-stu-id="c3ed3-107">Here is an example:</span></span>

<span data-ttu-id="c3ed3-108">Az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="c3ed3-108">Azure CLI:</span></span>

      azure vm get-instance-view


<span data-ttu-id="c3ed3-109">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="c3ed3-109">Here is hello sample output:</span></span>

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
  <span data-ttu-id="c3ed3-110">]</span><span class="sxs-lookup"><span data-stu-id="c3ed3-110">]</span></span>

## <a name="troubleshooting-extenson-failures"></a><span data-ttu-id="c3ed3-111">Extenson hibáinak elhárítása:</span><span class="sxs-lookup"><span data-stu-id="c3ed3-111">Troubleshooting Extenson failures:</span></span>
### <a name="re-running-hello-extension-on-hello-vm"></a><span data-ttu-id="c3ed3-112">A virtuális gép hello ismételt futtatásával hello bővítmény</span><span class="sxs-lookup"><span data-stu-id="c3ed3-112">Re-running hello extension on hello VM</span></span>
<span data-ttu-id="c3ed3-113">Ha futtatja a parancsfájlokat a virtuális gépet az egyéni parancsprogramok futtatására szolgáló bővítmény hello, néha hiba történt, amikor virtuális gép sikeresen létrejött, de hello parancsfájl nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="c3ed3-113">If you are running scripts on hello VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but hello script has failed.</span></span> <span data-ttu-id="c3ed3-114">Ezek a feltételek hello ajánlott módja toorecover ezt a hibát a tooremove hello bővítménye, és futtassa újra a hello sablon.</span><span class="sxs-lookup"><span data-stu-id="c3ed3-114">Under these conditons, hello recommended way toorecover from this error is tooremove hello extension and rerun hello template again.</span></span>
<span data-ttu-id="c3ed3-115">Megjegyzés: A jövőben ez a funkció lenne továbbfejlesztett tooremove hello hello bővítmény eltávolításához szükséges.</span><span class="sxs-lookup"><span data-stu-id="c3ed3-115">Note: In future, this functionality would be enhanced tooremove hello need for uninstalling hello extension.</span></span>

#### <a name="remove-hello-extension-from-azure-cli"></a><span data-ttu-id="c3ed3-116">Az Azure parancssori felület hello-kiterjesztés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c3ed3-116">Remove hello extension from Azure CLI</span></span>
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

<span data-ttu-id="c3ed3-117">Ahol "publsher-name" megfelel-e toohello bővítménytípus hello kimenetből "get-példány-nézet az azure virtuális gép", és hello bővítmény erőforrás hello sablonból hello neve</span><span class="sxs-lookup"><span data-stu-id="c3ed3-117">Where "publsher-name" corresponds toohello extension type from hello output of "azure vm get-instance-view" and name is hello name of hello extension resource from hello template</span></span>

<span data-ttu-id="c3ed3-118">Miután hello kiterjesztés eltávolításra került, hello sablon lehet az Újrafuttatja toorun hello parancsprogramok hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c3ed3-118">Once hello extension has been removed, hello template can be re-executed toorun hello scripts on hello VM.</span></span>

