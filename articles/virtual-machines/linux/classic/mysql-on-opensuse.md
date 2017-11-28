---
title: "egy OpenSUSE virtuális gépen MySQL aaaInstall |} Microsoft Docs"
description: "Tooinstall MySQL az Azure-ban OpenSUSE Linux VMirtual géphez tudnivalók."
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
ms.openlocfilehash: 8f96d29f29cb9c466dd7fdaf92b378783fdaacd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a><span data-ttu-id="b82f6-103">A MySQL telepítése Azure-ban működő, OpenSUSE Linux rendszerű virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="b82f6-103">Install MySQL on a virtual machine running OpenSUSE Linux in Azure</span></span>
<span data-ttu-id="b82f6-104">[MySQL] [ MySQL] egy népszerű, nyílt forráskódú SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b82f6-104">[MySQL][MySQL] is a popular, open-source SQL database.</span></span> <span data-ttu-id="b82f6-105">Az oktatóanyag bemutatja, hogyan toocreate OpenSUSE Linuxot futtató virtuális gép telepítse MySQL.</span><span class="sxs-lookup"><span data-stu-id="b82f6-105">This tutorial shows you how toocreate a virtual machine running OpenSUSE Linux, then install MySQL.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="b82f6-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b82f6-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b82f6-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="b82f6-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="b82f6-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="b82f6-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a><span data-ttu-id="b82f6-109">OpenSUSE Linuxot futtató virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b82f6-109">Create a virtual machine running OpenSUSE Linux</span></span>
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-hello-virtual-machine"></a><span data-ttu-id="b82f6-110">Telepítése és futtatása a MySQL hello virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="b82f6-110">Install and run MySQL on hello virtual machine</span></span>
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="b82f6-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b82f6-111">Next steps</span></span>
<span data-ttu-id="b82f6-112">MySQL kapcsolatos részletekért lásd: hello [MySQL dokumentáció][MySQLDocs].</span><span class="sxs-lookup"><span data-stu-id="b82f6-112">For details about MySQL, see hello [MySQL Documentation][MySQLDocs].</span></span>

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

