---
title: "klasszikus Linux virtuális gépek beállítja aaaAvailability |} Microsoft Docs"
description: "Konfigurálja a rendelkezésre állási készlet egy új vagy meglévő Linux virtuális gép hello klasszikus üzembe helyezési modellel hello Azure-portál és az Azure PowerShell használatával."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b8624315-beca-4ec7-8441-2e98b166b548
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: cynthn
ms.openlocfilehash: 8d8d041e3540e42a1921f5665469a2fdcaa30a29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-availability-set-for-linux-virtual-machines-in-hello-classic-deployment-model"></a><span data-ttu-id="d34e5-103">Hogyan tooconfigure rendelkezésre állási készlet hello klasszikus üzembe helyezési modellben a Linux virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="d34e5-103">How tooconfigure an availability set for Linux virtual machines in hello classic deployment model</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="d34e5-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d34e5-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d34e5-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="d34e5-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d34e5-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="d34e5-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="d34e5-107">Emellett [rendelkezésre állási csoportok konfigurálása](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) a Resource Manager üzembe helyezések.</span><span class="sxs-lookup"><span data-stu-id="d34e5-107">You can also [configure availability sets](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) in Resource Manager deployments.</span></span>

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]