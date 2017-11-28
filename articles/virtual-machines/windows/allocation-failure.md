---
title: "Windows virtuális gép pufferallokációs hibák elhárítása |} Microsoft Docs"
description: "Hozzon létre, újraindításakor vagy átméretezésekor egy Windows Azure-ban fellépő lefoglalási hibák elhárítása"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: cjiang
ms.openlocfilehash: 57925b5a75fd8bcf2a9450025b5dc51eb552353f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a><span data-ttu-id="bd9af-103">Hozzon létre, újraindításakor vagy átméretezésekor Windows-alapú virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="bd9af-103">Troubleshoot allocation failures when you create, restart, or resize Windows VMs in Azure</span></span>
<span data-ttu-id="bd9af-104">Hozzon létre egy virtuális Gépet, indítsa újra a leállított (felszabadított) virtuális gépek vagy átméretezni egy virtuális Gépet, a Microsoft Azure előfizetéshez számítási erőforrásokat foglal le.</span><span class="sxs-lookup"><span data-stu-id="bd9af-104">When you create a VM, restart stopped (deallocated) VMs, or resize a VM, Microsoft Azure allocates compute resources to your subscription.</span></span> <span data-ttu-id="bd9af-105">Alkalmanként jelenhet hibák ezeket a műveleteket--az Azure-előfizetésre vonatkozó korlátok eléri előtt is.</span><span class="sxs-lookup"><span data-stu-id="bd9af-105">You may occasionally receive errors when performing these operations -- even before you reach the Azure subscription limits.</span></span> <span data-ttu-id="bd9af-106">Ez a cikk ismerteti az egyes a közös pufferallokációs hibák okait, és lehetséges javítási javasol.</span><span class="sxs-lookup"><span data-stu-id="bd9af-106">This article explains the causes of some of the common allocation failures and suggests possible remediation.</span></span> <span data-ttu-id="bd9af-107">Az információ is hasznos lehet a szolgáltatásokhoz központi telepítésének tervezése során.</span><span class="sxs-lookup"><span data-stu-id="bd9af-107">The information may also be useful when you plan the deployment of your services.</span></span> <span data-ttu-id="bd9af-108">Emellett [létrehozása, újraindításakor vagy átméretezésekor Linux virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bd9af-108">You can also [troubleshoot allocation failures when you create, restart, or resize Linux VMs in Azure](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

