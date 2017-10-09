---
title: "Active Directory Domain Services: Virtuális hálózat létrehozása vagy kiválasztása | Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 212c741b20e846742d94f70342c4263ce8984153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a><span data-ttu-id="0af69-103">Virtuális hálózat létrehozása vagy kiválasztása az Azure Active Directory Domain Services-hez</span><span class="sxs-lookup"><span data-stu-id="0af69-103">Create or select a virtual network for Azure Active Directory Domain Services</span></span>
## <a name="before-you-begin"></a><span data-ttu-id="0af69-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0af69-104">Before you begin</span></span>
<span data-ttu-id="0af69-105">Tekintse meg a túl[Azure Active Directory tartományi szolgáltatások hálózati szempontjai](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="0af69-105">Refer too[Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>

## <a name="task-2-create-an-azure-virtual-network"></a><span data-ttu-id="0af69-106">2. feladat: Azure-alapú virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0af69-106">Task 2: Create an Azure virtual network</span></span>
<span data-ttu-id="0af69-107">hello következő konfigurációs feladat toocreate van egy Azure virtuális hálózatra és egy alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="0af69-107">hello next configuration task is toocreate an Azure virtual network and a subnet within it.</span></span> <span data-ttu-id="0af69-108">Engedélyezze az Azure Active Directory Domain Services-t a virtuális hálózatának ezen az alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="0af69-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="0af69-109">Ha egy meglévő virtuális hálózatot, hogy szeretne-e toouse, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="0af69-109">If you have an existing virtual network that you’d prefer toouse, you can skip this step.</span></span>

> [!NOTE]
> <span data-ttu-id="0af69-110">Győződjön meg arról, hogy hello Azure virtuális hálózat létrehozása vagy kiválasztása az Azure Active Directory tartományi szolgáltatások toouse tooan Azure-régió, Azure Active Directory tartományi szolgáltatások által támogatott tartozik.</span><span class="sxs-lookup"><span data-stu-id="0af69-110">Make sure that hello Azure virtual network you create or choose toouse with Azure Active Directory Domain Services belongs tooan Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="0af69-111">tooascertain hello Azure-régiók, amelyben az Azure Active Directory tartományi szolgáltatások című [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="0af69-111">tooascertain hello Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
>
><span data-ttu-id="0af69-112">Megjegyzés: hello neve hello virtuális hálózati tooensure hello megfelelő virtuális hálózatot válassza ki, ha engedélyezte az Azure Active Directory tartományi szolgáltatások egy későbbi konfigurációs lépésben.</span><span class="sxs-lookup"><span data-stu-id="0af69-112">Note hello name of hello virtual network tooensure that you select hello right virtual network when you enable Azure Active Directory Domain Services in a subsequent configuration step.</span></span>


<span data-ttu-id="0af69-113">toocreate kívánt tooenable Azure Active Directory tartományi szolgáltatások, Azure virtuális hálózat kövesse az alábbi konfigurációs utasításokat:</span><span class="sxs-lookup"><span data-stu-id="0af69-113">toocreate an Azure virtual network in which you want tooenable Azure Active Directory Domain Services, follow these configuration instructions:</span></span>

1. <span data-ttu-id="0af69-114">Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0af69-114">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="0af69-115">Hello bal oldali ablaktáblában jelöljön ki **hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="0af69-115">In hello left pane, select **Networks**.</span></span>

    <span data-ttu-id="0af69-116">![Networks (Hálózatok) csomópont](./media/active-directory-domain-services-getting-started/networks-node.png)</span><span class="sxs-lookup"><span data-stu-id="0af69-116">![Networks node](./media/active-directory-domain-services-getting-started/networks-node.png)</span></span>  
    <span data-ttu-id="0af69-117">Hello **virtuális hálózatok** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="0af69-117">hello **Virtual Networks** window opens.</span></span>
3. <span data-ttu-id="0af69-118">Hello munkaablakban hello ablak hello alján, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="0af69-118">In hello task pane at hello bottom of hello window, click **New**.</span></span>

    ![Virtual Networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. <span data-ttu-id="0af69-120">Kattintson a **Network Services** (Hálózati szolgáltatások), majd a **Virtual Network** (Virtuális hálózat) elemre.</span><span class="sxs-lookup"><span data-stu-id="0af69-120">Click **Network Services**, and then select **Virtual Network**.</span></span>

    ![Virtuális hálózat – gyors létrehozás](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. <span data-ttu-id="0af69-122">Kattintson egy virtuális hálózati toocreate **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="0af69-122">toocreate a virtual network, click **Quick Create**.</span></span>

6. <span data-ttu-id="0af69-123">Adjon meg egy **neve** a virtuális hálózat, és fontolja meg az alábbiak hello:</span><span class="sxs-lookup"><span data-stu-id="0af69-123">Specify a **Name** for your virtual network, and consider doing hello following:</span></span>
    * <span data-ttu-id="0af69-124">Kiválaszthatja a tooconfigure **Címtéren** vagy **virtuális gépek maximális számát** ehhez a hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="0af69-124">You can choose tooconfigure **Address space** or **Maximum VM count** for this network.</span></span>
    * <span data-ttu-id="0af69-125">Meghagyhatja hello **DNS-kiszolgáló** beállítását **nincs** most.</span><span class="sxs-lookup"><span data-stu-id="0af69-125">You can leave hello **DNS server** setting as **None** for now.</span></span> <span data-ttu-id="0af69-126">Azure Active Directory tartományi szolgáltatások engedélyezése után hello beállítás frissítheti.</span><span class="sxs-lookup"><span data-stu-id="0af69-126">You can update hello setting after you enable Azure Active Directory Domain Services.</span></span>
7. <span data-ttu-id="0af69-127">A hello **hely** legördülő listára, válassza ki a támogatott Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="0af69-127">In hello **Location** drop-down list, select a supported Azure region.</span></span>  
    <span data-ttu-id="0af69-128">tooascertain hello Azure-régiók, amelyben az Azure Active Directory tartományi szolgáltatások című [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="0af69-128">tooascertain hello Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
8. <span data-ttu-id="0af69-129">toocreate a virtuális hálózaton, kattintson a **hozzon létre egy virtuális hálózatot**.</span><span class="sxs-lookup"><span data-stu-id="0af69-129">toocreate your virtual network, click **Create a Virtual Network**.</span></span>

    ![Virtuális hálózat létrehozása az Azure Active Directory Domain Services-hez](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. <span data-ttu-id="0af69-131">Miután létrehozta a virtuális hálózaton, válassza ki a virtuális hálózat hello hello nevét, és kattintson a hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="0af69-131">After you've created a virtual network, select hello name of hello virtual network, and then click hello **Configure** tab.</span></span>

    ![Alhálózat létrehozása](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. <span data-ttu-id="0af69-133">A **virtuális hálózat**, kattintson a **alhálózat hozzáadása**, majd adja meg az alhálózat hello nevű **AaddsSubnet**.</span><span class="sxs-lookup"><span data-stu-id="0af69-133">Under **virtual network address spaces**, click **add subnet**, and then specify a subnet with hello name **AaddsSubnet**.</span></span>

    ![Alhálózat létrehozása az Azure Active Directory Domain Services-hez](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. <span data-ttu-id="0af69-135">toocreate hello alhálózati, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0af69-135">toocreate hello subnet, click **Save**.</span></span>


## <a name="next-step"></a><span data-ttu-id="0af69-136">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="0af69-136">Next step</span></span>
[<span data-ttu-id="0af69-137">3. feladat: Az Active Directory Domain Services engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0af69-137">Task 3: enable Azure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-enableaadds.md)
