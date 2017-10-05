---
title: "MySQL telepítése egy OpenSUSE virtuális gépen |} Microsoft Docs"
description: "Ismerje meg, MySQL telepítsen egy OpenSUSE Linux VMirtual gépet az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 01b798a25575b66f89057315ce80d6cc0cde53b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a><span data-ttu-id="3f0fe-103">A MySQL telepítése Azure-ban működő, OpenSUSE Linux rendszerű virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="3f0fe-103">Install MySQL on a virtual machine running OpenSUSE Linux in Azure</span></span>
<span data-ttu-id="3f0fe-104">[MySQL] [ MySQL] egy népszerű, nyílt forráskódú SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3f0fe-104">[MySQL][MySQL] is a popular, open-source SQL database.</span></span> <span data-ttu-id="3f0fe-105">Ez az oktatóanyag bemutatja, hogyan OpenSUSE Linuxot futtató virtuális gép létrehozása, majd telepítse a MySQL.</span><span class="sxs-lookup"><span data-stu-id="3f0fe-105">This tutorial shows you how to create a virtual machine running OpenSUSE Linux, then install MySQL.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3f0fe-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3f0fe-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3f0fe-107">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="3f0fe-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="3f0fe-108">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="3f0fe-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a><span data-ttu-id="3f0fe-109">OpenSUSE Linuxot futtató virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f0fe-109">Create a virtual machine running OpenSUSE Linux</span></span>
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a><span data-ttu-id="3f0fe-110">Telepítse, MySQL futtassa a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="3f0fe-110">Install and run MySQL on the virtual machine</span></span>
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="3f0fe-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f0fe-111">Next steps</span></span>
<span data-ttu-id="3f0fe-112">MySQL kapcsolatos részletekért lásd: a [MySQL dokumentáció][MySQLDocs].</span><span class="sxs-lookup"><span data-stu-id="3f0fe-112">For details about MySQL, see the [MySQL Documentation][MySQLDocs].</span></span>

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

