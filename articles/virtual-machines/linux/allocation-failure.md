---
title: "Linux virtuális gép pufferallokációs hibák elhárítása |} Microsoft Docs"
description: "Hozzon létre, újraindításakor vagy átméretezésekor egy Linux virtuális Gépet az Azure-ban fellépő lefoglalási hibák elhárítása"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cjiang
ms.openlocfilehash: c65ede134971c034006781e058c05a82ffb68a19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a><span data-ttu-id="ac9c4-103">Hozzon létre, újraindításakor vagy átméretezésekor Linux virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="ac9c4-103">Troubleshoot allocation failures when you create, restart, or resize Linux VMs in Azure</span></span>
<span data-ttu-id="ac9c4-104">Hozzon létre egy virtuális Gépet, indítsa újra a leállított (felszabadított) virtuális gépek vagy átméretezni egy virtuális Gépet, a Microsoft Azure előfizetéshez számítási erőforrásokat foglal le.</span><span class="sxs-lookup"><span data-stu-id="ac9c4-104">When you create a VM, restart stopped (deallocated) VMs, or resize a VM, Microsoft Azure allocates compute resources to your subscription.</span></span> <span data-ttu-id="ac9c4-105">Alkalmanként jelenhet hibák ezeket a műveleteket--az Azure-előfizetésre vonatkozó korlátok eléri előtt is.</span><span class="sxs-lookup"><span data-stu-id="ac9c4-105">You may occasionally receive errors when performing these operations -- even before you reach the Azure subscription limits.</span></span> <span data-ttu-id="ac9c4-106">Ez a cikk ismerteti az egyes a közös pufferallokációs hibák okait, és lehetséges javítási javasol.</span><span class="sxs-lookup"><span data-stu-id="ac9c4-106">This article explains the causes of some of the common allocation failures and suggests possible remediation.</span></span> <span data-ttu-id="ac9c4-107">Az információ is hasznos lehet a szolgáltatásokhoz központi telepítésének tervezése során.</span><span class="sxs-lookup"><span data-stu-id="ac9c4-107">The information may also be useful when you plan the deployment of your services.</span></span> <span data-ttu-id="ac9c4-108">Emellett [létrehozása, újraindításakor vagy átméretezésekor Windows-alapú virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ac9c4-108">You can also [troubleshoot allocation failures when you create, restart, or resize Windows VMs in Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

