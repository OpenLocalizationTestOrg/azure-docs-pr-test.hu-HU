---
title: "Az Azure Active Directory tartományi szolgáltatások: Első lépések |} Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="f8127-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="f8127-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="f8127-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f8127-104">Before you begin</span></span>
<span data-ttu-id="f8127-105">Tekintse át a [Hálózati megfontolások az Azure Active Directory Domain Services-hez](active-directory-ds-networking.md) című dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="f8127-105">Refer to [Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>


## <a name="task-2-configure-network-settings"></a><span data-ttu-id="f8127-106">2. feladat: a hálózati beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8127-106">Task 2: configure network settings</span></span>
<span data-ttu-id="f8127-107">A következő konfigurációs feladat, ha egy Azure virtuális hálózatra és egy dedikált alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="f8127-107">The next configuration task is to create an Azure virtual network and a dedicated subnet within it.</span></span> <span data-ttu-id="f8127-108">Engedélyezze az Azure Active Directory Domain Services-t a virtuális hálózatának ezen az alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="f8127-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="f8127-109">Is válasszon egy meglévő virtuális hálózathoz, és hozza létre az alhálózatot, amelyek dedikált belül.</span><span class="sxs-lookup"><span data-stu-id="f8127-109">You may also pick an existing virtual network and create the dedicated subnet within it.</span></span>

1. <span data-ttu-id="f8127-110">Kattintson a **virtuális hálózati** virtuális hálózat kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="f8127-110">Click **Virtual network** to select a virtual network.</span></span>
2. <span data-ttu-id="f8127-111">Az a **válasszon virtuális hálózati** panelen, megjelenik az összes meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="f8127-111">On the **Choose virtual network** blade, you see all existing virtual networks.</span></span> <span data-ttu-id="f8127-112">Csak a virtuális hálózatok, amelyek a kijelölt erőforráscsoport és Azure-beli hely megjelenik a **alapjai** varázsló lapja.</span><span class="sxs-lookup"><span data-stu-id="f8127-112">You see only the virtual networks that belong to the resource group and Azure location you have selected on the **Basics** wizard page.</span></span>

3. <span data-ttu-id="f8127-113">Válassza ki a virtuális hálózatot, amelyben a Azure AD tartományi szolgáltatások engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="f8127-113">Choose the virtual network in which Azure AD Domain Services should be enabled.</span></span> <span data-ttu-id="f8127-114">Kattintson a **hozzon létre új**, ha szeretné létrehozni az új virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="f8127-114">Click **Create new**, if you prefer to create a new virtual network.</span></span> <span data-ttu-id="f8127-115">Erősen ajánlott egy dedikált alhálózati használata az Azure AD tartományi szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f8127-115">We highly recommend using a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="f8127-116">Ha egy meglévő virtuális hálózatot választ [hozzon létre egy külön alhálózatot a virtuális hálózatok kiterjesztéssel](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) , majd válassza ki a alhálózaton használni.</span><span class="sxs-lookup"><span data-stu-id="f8127-116">If you pick an existing virtual network, [create a dedicated subnet using the virtual networks extension](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) and then pick that subnet.</span></span> 

    ![Válassza ki a virtuális hálózat](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. <span data-ttu-id="f8127-118">Kattintson a **alhálózati** a dedikált alhálózat kiválasztása a virtuális hálózaton belül, amely lehetővé teszi az új felügyelt tartomány számára.</span><span class="sxs-lookup"><span data-stu-id="f8127-118">Click **Subnet** to pick the dedicated subnet in this virtual network, within which to enable your new managed domain.</span></span> <span data-ttu-id="f8127-119">Az a **hozzon létre alhálózatot** panelen adja meg az alhálózat nevét, és kattintson a **OK** befejezése.</span><span class="sxs-lookup"><span data-stu-id="f8127-119">In the **Create subnet** blade, specify a name for the subnet, and click **OK** when you're done.</span></span> <span data-ttu-id="f8127-120">Hozzon létre például egy alhálózat neve "DomainServices", így könnyen megérteni, hogy mi történik, az alhálózaton belüli más rendszergazdák számára.</span><span class="sxs-lookup"><span data-stu-id="f8127-120">For example, create a subnet with the name 'DomainServices', making it easy for other administrators to understand what is deployed within the subnet.</span></span>

    ![Válassza ki az alhálózatot a virtuális hálózaton belül](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > <span data-ttu-id="f8127-122">**Egy alhálózat kiválasztására vonatkozó irányelvek**</span><span class="sxs-lookup"><span data-stu-id="f8127-122">**Guidelines for selecting a subnet**</span></span>
  > 1. <span data-ttu-id="f8127-123">Használja a dedikált alhálózatot az Azure AD tartományi szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f8127-123">Use a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="f8127-124">Más virtuális gépek nem telepíti az alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="f8127-124">Do not deploy any other virtual machines to this subnet.</span></span> <span data-ttu-id="f8127-125">Ez a konfiguráció lehetővé teszi hálózati biztonsági csoportokkal (NSG-k) konfigurálja a munkaterhelések virtuális gépek számára a felügyelt tartományok megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="f8127-125">This configuration enables you to configure network security groups (NSGs) for your workloads/virtual machines without disrupting your managed domain.</span></span> <span data-ttu-id="f8127-126">További információkért lásd: [hálózat az Azure Active Directory tartományi szolgáltatások szempontjai](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="f8127-126">For details, see [networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>
  2. <span data-ttu-id="f8127-127">Ne válassza az átjáró alhálózatának az Azure AD tartományi szolgáltatások telepítése, mert nem támogatott konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f8127-127">Do not select the Gateway subnet for deploying Azure AD Domain Services, because it is not a supported configuration.</span></span>
  3. <span data-ttu-id="f8127-128">Győződjön meg arról, hogy a kijelölt alhálózat rendelkezik-e elegendő elérhető címterület - legalább 3-5 elérhető IP-címek.</span><span class="sxs-lookup"><span data-stu-id="f8127-128">Ensure that the subnet you've selected has sufficient available address space - at least 3-5 available IP addresses.</span></span>
  >

5. <span data-ttu-id="f8127-129">Amikor elkészült, kattintson a **OK** a áthelyezése a **rendszergazdai csoport** a varázsló.</span><span class="sxs-lookup"><span data-stu-id="f8127-129">When you are done, click **OK** to move on to the **Administrator group** page of the wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="f8127-130">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f8127-130">Next step</span></span>
[<span data-ttu-id="f8127-131">3. feladat: a felügyeleti csoport konfigurálása és Azure AD tartományi szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f8127-131">Task 3: configure administrative group and enable Azure AD Domain Services</span></span>](active-directory-ds-getting-started-admingroup.md)
