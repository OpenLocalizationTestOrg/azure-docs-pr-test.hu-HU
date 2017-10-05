---
title: "Windows klasszikus virtuális gépek rendelkezésre állási készletek |} Microsoft Docs"
description: "Konfigurálja a rendelkezésre állási készlet egy új vagy meglévő Windows rendszerű virtuális gép a klasszikus üzembe helyezési modellel, az Azure portál és az Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c3b7fdec-fb59-4412-a4f4-f3a0b9c62e93
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn
ms.openlocfilehash: a5cbbdf402ee06a34a339b193b0cdd5c952d6248
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-an-availability-set-for-windows-virtual-machines-in-the-classic-deployment-model"></a><span data-ttu-id="780b1-103">Rendelkezésre állási készlet Windows virtuális gépek a klasszikus üzembe helyezési modellel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="780b1-103">How to configure an availability set for Windows virtual machines in the classic deployment model</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="780b1-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="780b1-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="780b1-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="780b1-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="780b1-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="780b1-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="780b1-107">Emellett [rendelkezésre állási csoportok konfigurálása](../tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Resource Manager üzembe helyezések.</span><span class="sxs-lookup"><span data-stu-id="780b1-107">You can also [configure availability sets](../tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in Resource Manager deployments.</span></span>

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]

