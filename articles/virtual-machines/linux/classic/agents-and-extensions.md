---
title: "<span data-ttu-id=\"cf83a-101\">Linux ügynök és Virtuálisgép-bővítmények az Azure-ban |} Microsoft Docs</span><span class=\"sxs-lookup\"><span data-stu-id=\"cf83a-101\">Linux VM agent and extensions in Azure | Microsoft Docs</span></span>"
description: "<span data-ttu-id=\"cf83a-102\">Adja meg az ügynök és a bővítmények áttekintését, és hogyan telepítheti az ügynököt, a Linux virtuális gép a klasszikus telepítési modell használatával.</span><span class=\"sxs-lookup\"><span data-stu-id=\"cf83a-102\">Gives an overview of the agent and extensions, and how to install the agent, using the classic deployment model on a Linux VM.</span></span>"
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b41e3b21-9132-4d8d-804d-34920b2d0942
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: rasquill
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06b802c408ea5d1b2b40d05321e1a0014e99ca8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="about-the-virtual-machine-agent-and-extensions-for-linux"></a><span data-ttu-id="cf83a-103">A virtuális gép ügynökének és a Linux-bővítmények</span><span class="sxs-lookup"><span data-stu-id="cf83a-103">About the virtual machine agent and extensions for Linux</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cf83a-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cf83a-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cf83a-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="cf83a-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="cf83a-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="cf83a-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="cf83a-107">Virtuális gép az ügynökök és a bővítmények az erőforrás-kezelő használatával kapcsolatos információkért lásd: [Itt](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf83a-107">For information about VM agents and extensions using Resource Manager, see [here](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-agents-and-extensions](../../../../includes/virtual-machines-common-classic-agents-and-extensions.md)]
