---
title: "aaaRedeploy Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "Hogyan tooredeploy Windows virtuális gépek Azure toomitigate RDP-kapcsolaton ad ki."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 903d9d5bf241075931ee4b746690c553d808a58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-windows-virtual-machine-toonew-azure-node"></a><span data-ttu-id="e0a63-103">Telepítse újra a Windows virtuális gép toonew Azure csomópont</span><span class="sxs-lookup"><span data-stu-id="e0a63-103">Redeploy Windows virtual machine toonew Azure node</span></span>
<span data-ttu-id="e0a63-104">Ha Ön rendelkezik lett nehézségekkel hibaelhárítás a távoli asztal (RDP) kapcsolat vagy az alkalmazás eléréséhez tooWindows-alapú Azure virtuális gép (VM) újratelepíteni a virtuális gép segíthet hello.</span><span class="sxs-lookup"><span data-stu-id="e0a63-104">If you have been facing difficulties troubleshooting Remote Desktop (RDP) connection or application access tooWindows-based Azure virtual machine (VM), redeploying hello VM may help.</span></span> <span data-ttu-id="e0a63-105">A virtuális gép újbóli telepítésének, hello VM tooa új csomópont belül hello Azure-infrastruktúra helyezi át, és majd bekapcsolja azt vissza, a konfigurációs beállításokat és a kapcsolódó erőforrások megőrzése.</span><span class="sxs-lookup"><span data-stu-id="e0a63-105">When you redeploy a VM, it moves hello VM tooa new node within hello Azure infrastructure and then powers it back on, retaining all your configuration options and associated resources.</span></span> <span data-ttu-id="e0a63-106">Ez a cikk bemutatja, hogyan tooredeploy a virtuális gépet az Azure PowerShell vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e0a63-106">This article shows you how tooredeploy a VM using Azure PowerShell or hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="e0a63-107">Miután a virtuális gép újbóli telepítésének, hello mennyiségű ideiglenes lemezes elvesztését, és dinamikus IP-címek társított virtuális hálózati illesztő frissítése.</span><span class="sxs-lookup"><span data-stu-id="e0a63-107">After you redeploy a VM, hello temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 


## <a name="using-azure-powershell"></a><span data-ttu-id="e0a63-108">Az Azure PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="e0a63-108">Using Azure PowerShell</span></span>
<span data-ttu-id="e0a63-109">Győződjön meg arról, hogy hello rendelkezik a legújabb Azure PowerShell 1.x van telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="e0a63-109">Make sure you have hello latest Azure PowerShell 1.x installed on your machine.</span></span> <span data-ttu-id="e0a63-110">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e0a63-110">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="e0a63-111">hello következő példa telepíti hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="e0a63-111">hello following example deploys hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="e0a63-112">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0a63-112">Next steps</span></span>
<span data-ttu-id="e0a63-113">Csatlakozás a virtuális gép tooyour problémát tapasztal, ha található segítséget [RDP-kapcsolatok hibáinak elhárítása](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [részletes hibaelhárítási RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0a63-113">If you are having issues connecting tooyour VM, you can find specific help on [troubleshooting RDP connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [detailed RDP troubleshooting steps](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e0a63-114">Ha nem fér hozzá a virtuális gép futó alkalmazást, is olvasható [problémák elhárítása alkalmazás](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0a63-114">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

