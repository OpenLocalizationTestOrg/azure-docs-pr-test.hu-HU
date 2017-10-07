---
title: "egy virtuális Gépet egy statikus nyilvános IP-cím - Azure-portálon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate virtuális gép és a statikus nyilvános IP cím használatával hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f74d2132785f06148757409ee0a44b98d1e4b98e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-portal"></a><span data-ttu-id="39d21-103">Virtuális gép létrehozása egy statikus nyilvános IP-cím hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="39d21-103">Create a VM with a static public IP address using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="39d21-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="39d21-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="39d21-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="39d21-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="39d21-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="39d21-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="39d21-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="39d21-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="39d21-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="39d21-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="39d21-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="39d21-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="39d21-110">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="39d21-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a><span data-ttu-id="39d21-111">Hozzon létre egy virtuális Gépet egy statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="39d21-111">Create a VM with a static public IP</span></span>

<span data-ttu-id="39d21-112">toocreate egy virtuális Gépet egy statikus nyilvános IP-cím hello Azure-portálon teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="39d21-112">toocreate a VM with a static public IP address in hello Azure portal, complete hello following steps:</span></span>

1. <span data-ttu-id="39d21-113">Egy böngészőből keresse meg a toohello [Azure-portálon](https://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="39d21-113">From a browser, navigate toohello [Azure portal](https://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="39d21-114">Hello felső bal oldali sarokban hello portál, kattintson a **új**>>**számítási**>**Windows Server 2012 R2 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="39d21-114">On hello top left hand corner of hello portal, click **New**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span></span>
3. <span data-ttu-id="39d21-115">A hello **telepítési modell kiválasztása** listára, válassza ki **erőforrás-kezelő** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="39d21-115">In hello **Select a deployment model** list, select **Resource Manager** and click **Create**.</span></span>
4. <span data-ttu-id="39d21-116">A hello **alapjai** panelen adja meg a Virtuálisgép-adatok hello alább látható módon, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="39d21-116">In hello **Basics** blade, enter hello VM information as shown below, and then click **OK**.</span></span>
   
    ![Azure portál – alapok](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. <span data-ttu-id="39d21-118">A hello **méret kiválasztása** panelen kattintson **A1 szabványos** az alábbi módon, majd **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="39d21-118">In hello **Choose a size** blade, click **A1 Standard** as shown below, and then click **Select**.</span></span>
   
    ![Azure portál – méret kiválasztása](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. <span data-ttu-id="39d21-120">A hello **beállítások** panelen kattintson a **nyilvános IP-cím**, ezt a hello **nyilvános IP-cím létrehozása** panelen, a **hozzárendelés**, kattintson a **Statikus** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="39d21-120">In hello **Settings** blade, click **Public IP address**, then in hello **Create public IP address** blade, under **Assignment**, click **Static** as shown below.</span></span> <span data-ttu-id="39d21-121">Majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="39d21-121">And then click **OK**.</span></span>
   
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. <span data-ttu-id="39d21-123">A hello **beállítások** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="39d21-123">In hello **Settings** blade, click **OK**.</span></span>
8. <span data-ttu-id="39d21-124">Felülvizsgálati hello **összegzés** panelen, a lent látható módon, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="39d21-124">Review hello **Summary** blade, as shown below, and then click **OK**.</span></span>
   
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. <span data-ttu-id="39d21-126">Figyelje meg hello új csempe az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="39d21-126">Notice hello new tile in your dashboard.</span></span>
   
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. <span data-ttu-id="39d21-128">Hello virtuális gép létrehozása után hello **beállítások** panelen megjelenik a lent látható módon</span><span class="sxs-lookup"><span data-stu-id="39d21-128">Once hello VM is created, hello **Settings** blade will be displayed as shown below</span></span>
    
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

