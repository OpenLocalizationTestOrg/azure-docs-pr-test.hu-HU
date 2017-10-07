---
title: "Windows-alapú virtuális gépek Azure-adatok aaaInject |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan tooinject egyéni adatokat az Azure virtuális gépre, ha hello példány jön létre, és hogyan toolocate hello Windows vagy Linux egyéni adatokat."
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 48759f76-eaa0-4202-ada0-706d3f9a9467
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 2b2a12e5d0942fa957387ace7e38a353dbf27197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="injecting-custom-data-into-an-azure-virtual-machine"></a><span data-ttu-id="89d2e-103">Egyéni adatok hogy Azure virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="89d2e-103">Injecting custom data into an Azure virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="89d2e-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="89d2e-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="89d2e-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="89d2e-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="89d2e-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="89d2e-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="89d2e-107">Hello Resource Manager modellt hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával kapcsolatos információkért lásd: [Itt](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="89d2e-107">For information about using hello Custom Script Extension with hello Resource Manager model, see [here](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-inject-custom-data](../../../../includes/virtual-machines-common-classic-inject-custom-data.md)]
