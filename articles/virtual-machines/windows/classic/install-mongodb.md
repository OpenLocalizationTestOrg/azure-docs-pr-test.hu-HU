---
title: a Windows Azure-ban a MongoDB aaaInstall |} Microsoft Docs
description: "Ismerje meg, hogyan tooinstall egy Azure virtuális gépen MongoDB hozza létre a Windows Server rendszert futtató hello klasszikus üzembe helyezési modellel."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: aafd86b4c49c6a97ae8a1a2191ae375b6c47483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="7583f-103">A MongoDB telepítése a Windows Azure-ban</span><span class="sxs-lookup"><span data-stu-id="7583f-103">Install MongoDB on a Windows VM in Azure</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7583f-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7583f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="7583f-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="7583f-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="7583f-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="7583f-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="7583f-107">tooinstall és hello erőforrás-kezelő telepítési modellel MongoDB konfigurálása, lásd: [Ez a cikk](../install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="7583f-107">tooinstall and configure MongoDB using hello Resource Manager deployment model, see [this article](../install-mongodb.md).</span></span>

<span data-ttu-id="7583f-108">[MongoDB] [ MongoDB] egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7583f-108">[MongoDB][MongoDB] is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="7583f-109">Ez a cikk végigvezeti a Windows Server virtuális gép (VM) hello lépésein [Azure-portálon][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="7583f-109">This article guides you through creating a Windows Server virtual machine (VM) using hello [Azure portal][AzurePortal].</span></span> <span data-ttu-id="7583f-110">Kell létrehozni és csatolni a adatok lemez toohello VM telepítése és konfigurálása a MongoDB előtt.</span><span class="sxs-lookup"><span data-stu-id="7583f-110">You then create and attach a data disk toohello VM before installing and configuring MongoDB.</span></span> <span data-ttu-id="7583f-111">Ha egy meglévő virtuális Gépen, hogy szeretné-e toouse Azure-ban, de is ugorhat rögtön túl[telepítése és konfigurálása a MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="7583f-111">If you have an existing VM in Azure that you would like toouse, you can jump straight too[installing and configuring MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span></span>

## <a name="create-a-virtual-machine-running-windows-server"></a><span data-ttu-id="7583f-112">Windows Servert futtató virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="7583f-112">Create a virtual machine running Windows Server</span></span>
<span data-ttu-id="7583f-113">Hajtsa végre az ezen utasításokat toocreate egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7583f-113">Follow these instructions toocreate a virtual machine.</span></span>

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> <span data-ttu-id="7583f-114">Végpont hozzáadása a mongodb-protokolltámogatással hello virtuális gép létrehozása során, és konfigurálja az alábbiak szerint: nevezze el, mint a **Mongo**, használjon **TCP** , hello protokoll, és állítsa be mindkét hello nyilvános vagy privát portokkal túl**27017**.</span><span class="sxs-lookup"><span data-stu-id="7583f-114">You can add an endpoint for MongoDB while creating hello virtual machine, and configure it as follows: name it as **Mongo**, use **TCP** as hello protocol, and set both hello public and private ports too**27017**.</span></span>
>
>

## <a name="attach-a-data-disk"></a><span data-ttu-id="7583f-115">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="7583f-115">Attach a data disk</span></span>
<span data-ttu-id="7583f-116">tooprovide tárolási hello virtuális gép esetén adatlemezzel és majd inicializálnia, hogy a Windows használni tudja.</span><span class="sxs-lookup"><span data-stu-id="7583f-116">tooprovide storage for hello virtual machine, attach a data disk and then initialize it so that Windows can use it.</span></span> <span data-ttu-id="7583f-117">Ha már rendelkezik adatlemezt, csatolhat a meglévő lemezt, vagy csatolhat egy üres lemez.</span><span class="sxs-lookup"><span data-stu-id="7583f-117">If you already have a data disk, you can attach that existing disk, or you can attach an empty disk.</span></span>

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

<span data-ttu-id="7583f-118">Hello lemez inicializálása, lásd: "How to: a Windows Server új adatlemezt inicializálása" a [hogyan tooattach az adatok lemezre tooa Windows rendszerű virtuális gép](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="7583f-118">For instructions on initializing hello disk, see "How to: Initialize a new data disk in Windows Server" in [How tooattach a data disk tooa Windows virtual machine](attach-disk.md).</span></span>

## <a name="install-and-run-mongodb-on-hello-virtual-machine"></a><span data-ttu-id="7583f-119">Telepítése és futtatása a MongoDB hello virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="7583f-119">Install and run MongoDB on hello virtual machine</span></span>
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a><span data-ttu-id="7583f-120">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="7583f-120">Summary</span></span>
<span data-ttu-id="7583f-121">Ebben az oktatóanyagban megtanulta, hogyan toocreate egy Windows Server rendszerű virtuális gép távolról csatlakozzon tooit, és adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="7583f-121">In this tutorial, you learned how toocreate a virtual machine running Windows Server, remotely connect tooit, and attach a data disk.</span></span>  <span data-ttu-id="7583f-122">Azt is megtanulta, hogyan tooinstall, és konfigurálja a MongoDB hello Windows-alapú virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="7583f-122">You also learned how tooinstall and configure MongoDB on hello Windows-based virtual machine.</span></span> <span data-ttu-id="7583f-123">Most már elérheti az MongoDB hello Windows-alapú virtuális gépen, a következő speciális hello témakörei hello [MongoDB dokumentációt][MongoDocs].</span><span class="sxs-lookup"><span data-stu-id="7583f-123">You can now access MongoDB on hello Windows-based virtual machine, by following hello advanced topics in hello [MongoDB documentation][MongoDocs].</span></span>

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

