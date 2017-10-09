---
title: "aaaTroubleshooting Windows virtuális gép pufferallokációs hibák |} Microsoft Docs"
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
ms.openlocfilehash: d0cc75ac60d952d8e4310cebc37654dc4f80857f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a><span data-ttu-id="78b95-103">Hozzon létre, újraindításakor vagy átméretezésekor Windows-alapú virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="78b95-103">Troubleshoot allocation failures when you create, restart, or resize Windows VMs in Azure</span></span>
<span data-ttu-id="78b95-104">Hozzon létre egy virtuális Gépet, indítsa újra a leállított (felszabadított) virtuális gépek vagy átméretezni egy virtuális Gépet, a Microsoft Azure számítási erőforrásokat tooyour előfizetés foglal le.</span><span class="sxs-lookup"><span data-stu-id="78b95-104">When you create a VM, restart stopped (deallocated) VMs, or resize a VM, Microsoft Azure allocates compute resources tooyour subscription.</span></span> <span data-ttu-id="78b95-105">Alkalmanként jelenhet hibák ezeket a műveleteket--még mielőtt hello Azure előfizetési korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="78b95-105">You may occasionally receive errors when performing these operations -- even before you reach hello Azure subscription limits.</span></span> <span data-ttu-id="78b95-106">Ez a cikk ismerteti a hello okok egyes hello közös pufferallokációs hibák, és lehetséges javítási javasol.</span><span class="sxs-lookup"><span data-stu-id="78b95-106">This article explains hello causes of some of hello common allocation failures and suggests possible remediation.</span></span> <span data-ttu-id="78b95-107">hello információkat is lehetnek hasznosak, ha azt tervezi, hogy a szolgáltatások hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="78b95-107">hello information may also be useful when you plan hello deployment of your services.</span></span> <span data-ttu-id="78b95-108">Emellett [létrehozása, újraindításakor vagy átméretezésekor Linux virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="78b95-108">You can also [troubleshoot allocation failures when you create, restart, or resize Linux VMs in Azure](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

