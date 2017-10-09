---
title: "egy Azure virtuális gép sablonját aaaDownload hello |} Microsoft Docs"
description: "Töltse le a virtuális gép toohelp hello Resource Manager üzembe helyezési modellel központi telepítések végrehajthassa hello templatefor"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 86fd05f67409019b5e5c9023881745047860eee1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-template-for-a-vm"></a><span data-ttu-id="6cf14-103">A virtuális gépek hello sablon letöltése</span><span class="sxs-lookup"><span data-stu-id="6cf14-103">Download hello template for a VM</span></span>
<span data-ttu-id="6cf14-104">Ha a virtuális gép létrehozása az Azure-ban hello portál vagy PowerShell-lel, erőforrás-kezelő sablon automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="6cf14-104">When you create a VM in Azure using hello portal or PowerShell, a Resource Manager template is automatically created for you.</span></span> <span data-ttu-id="6cf14-105">A sablon tooquickly duplikált a központi telepítés is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6cf14-105">You can use this template tooquickly duplicate a deployment.</span></span> <span data-ttu-id="6cf14-106">hello sablon összes hello erőforrás egy erőforráscsoportban található információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6cf14-106">hello template contains information about all of hello resources in a resource group.</span></span> <span data-ttu-id="6cf14-107">A virtuális gép esetén ez azt jelenti, hello sablon tartalmaz mindent, az erőforráscsoport, beleértve a hálózati erőforrások hello VM hello elősegítésére jön létre.</span><span class="sxs-lookup"><span data-stu-id="6cf14-107">For a virtual machine, this means hello template contains everything that is created in support of hello VM in that resource group, including hello networking resources.</span></span>

## <a name="download-hello-template-using-hello-portal"></a><span data-ttu-id="6cf14-108">Hello portálon hello sablon letöltése</span><span class="sxs-lookup"><span data-stu-id="6cf14-108">Download hello template using hello portal</span></span>
1. <span data-ttu-id="6cf14-109">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6cf14-109">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6cf14-110">Egy hello menüben válassza **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="6cf14-110">One hello hub menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="6cf14-111">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="6cf14-111">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="6cf14-112">Válassza ki **automatizálási parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="6cf14-112">Select **Automation script**.</span></span>
5. <span data-ttu-id="6cf14-113">Válassza ki **letöltése** , és mentse a hello .zip fájl tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6cf14-113">Select **Download** and save hello .zip file tooyour local computer.</span></span>
6. <span data-ttu-id="6cf14-114">Nyissa meg hello .zip fájlt, és bontsa ki a hello fájlok tooa mappa.</span><span class="sxs-lookup"><span data-stu-id="6cf14-114">Open hello .zip file and extract hello files tooa folder.</span></span> <span data-ttu-id="6cf14-115">hello .zip-fájlt fogja tartalmazni:</span><span class="sxs-lookup"><span data-stu-id="6cf14-115">hello .zip file will contain:</span></span>
   
   * <span data-ttu-id="6cf14-116">Deploy.ps1</span><span class="sxs-lookup"><span data-stu-id="6cf14-116">deploy.ps1</span></span>
   * <span data-ttu-id="6cf14-117">Deploy.SH</span><span class="sxs-lookup"><span data-stu-id="6cf14-117">deploy.sh</span></span> 
   * <span data-ttu-id="6cf14-118">deployer.RB</span><span class="sxs-lookup"><span data-stu-id="6cf14-118">deployer.rb</span></span>
   * <span data-ttu-id="6cf14-119">DeploymentHelper.cs</span><span class="sxs-lookup"><span data-stu-id="6cf14-119">DeploymentHelper.cs</span></span>
   * <span data-ttu-id="6cf14-120">Parameters.JSON</span><span class="sxs-lookup"><span data-stu-id="6cf14-120">parameters.json</span></span>
   * <span data-ttu-id="6cf14-121">Template.JSON</span><span class="sxs-lookup"><span data-stu-id="6cf14-121">template.json</span></span>

<span data-ttu-id="6cf14-122">hello template.json fájl hello sablont.</span><span class="sxs-lookup"><span data-stu-id="6cf14-122">hello template.json file is hello template.</span></span>

## <a name="download-hello-template-using-powershell"></a><span data-ttu-id="6cf14-123">PowerShell-lel hello sablon letöltése</span><span class="sxs-lookup"><span data-stu-id="6cf14-123">Download hello template using PowerShell</span></span>
<span data-ttu-id="6cf14-124">Hello .JSON kiterjesztésű sablonfájl hello segítségével is letöltheti [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="6cf14-124">You can also download hello .json template file using hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet.</span></span> <span data-ttu-id="6cf14-125">Használhatja a hello `-path` paraméter tooprovide hello fájlnév és hello .JSON kiterjesztésű fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="6cf14-125">You can use hello `-path` parameter tooprovide hello filename and path for hello .json file.</span></span> <span data-ttu-id="6cf14-126">Ez a példa bemutatja, hogyan toodownload hello hello erőforráscsoport sablonjának nevű **myResourceGroup** toohello **C:\users\public\downloads** mappát a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6cf14-126">This example shows how toodownload hello template for hello resource group named **myResourceGroup** toohello **C:\users\public\downloads** folder on your local computer.</span></span>

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a><span data-ttu-id="6cf14-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6cf14-127">Next steps</span></span>
<span data-ttu-id="6cf14-128">toolearn sablonokkal, erőforrásokat üzembe helyezi bővebben lásd: [Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="6cf14-128">toolearn more about deploying resources using templates, see [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

