---
title: "Az Azure Active Directory tartományi szolgáltatások: Első lépések |} Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata"
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
ms.openlocfilehash: 7695dabb11df8d9dcfdac24996edf021af2e7f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="f8a99-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata</span><span class="sxs-lookup"><span data-stu-id="f8a99-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="f8a99-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f8a99-104">Before you begin</span></span>
<span data-ttu-id="f8a99-105">Tekintse meg a túl[Azure Active Directory tartományi szolgáltatások hálózati szempontjai](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="f8a99-105">Refer too[Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>


## <a name="task-2-configure-network-settings"></a><span data-ttu-id="f8a99-106">2. feladat: a hálózati beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8a99-106">Task 2: configure network settings</span></span>
<span data-ttu-id="f8a99-107">hello következő konfigurációs feladat toocreate van egy Azure virtuális hálózatra és egy dedikált alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="f8a99-107">hello next configuration task is toocreate an Azure virtual network and a dedicated subnet within it.</span></span> <span data-ttu-id="f8a99-108">Engedélyezze az Azure Active Directory Domain Services-t a virtuális hálózatának ezen az alhálózatán.</span><span class="sxs-lookup"><span data-stu-id="f8a99-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="f8a99-109">Előfordulhat, hogy válasszon egy meglévő virtuális hálózathoz, és hozzon létre dedikált hello alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="f8a99-109">You may also pick an existing virtual network and create hello dedicated subnet within it.</span></span>

1. <span data-ttu-id="f8a99-110">Kattintson a **virtuális hálózati** tooselect egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="f8a99-110">Click **Virtual network** tooselect a virtual network.</span></span>
2. <span data-ttu-id="f8a99-111">A hello **válasszon virtuális hálózati** panelen, megjelenik az összes meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="f8a99-111">On hello **Choose virtual network** blade, you see all existing virtual networks.</span></span> <span data-ttu-id="f8a99-112">Csak hello virtuális hálózatok toohello erőforráscsoport és az Azure-beli hely hello a kijelölt tartozó látja **alapjai** varázsló lapja.</span><span class="sxs-lookup"><span data-stu-id="f8a99-112">You see only hello virtual networks that belong toohello resource group and Azure location you have selected on hello **Basics** wizard page.</span></span>

3. <span data-ttu-id="f8a99-113">Válasszon hello virtuális hálózatot, amelyben a Azure AD tartományi szolgáltatások engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="f8a99-113">Choose hello virtual network in which Azure AD Domain Services should be enabled.</span></span> <span data-ttu-id="f8a99-114">Kattintson a **hozzon létre új**, ha szeretné toocreate egy új virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="f8a99-114">Click **Create new**, if you prefer toocreate a new virtual network.</span></span> <span data-ttu-id="f8a99-115">Erősen ajánlott egy dedikált alhálózati használata az Azure AD tartományi szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f8a99-115">We highly recommend using a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="f8a99-116">Ha egy meglévő virtuális hálózatot választ [hozzon létre egy külön alhálózatot hello virtuális hálózatok kiterjesztéssel](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) , majd válassza ki a alhálózaton használni.</span><span class="sxs-lookup"><span data-stu-id="f8a99-116">If you pick an existing virtual network, [create a dedicated subnet using hello virtual networks extension](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) and then pick that subnet.</span></span> 

    ![Válassza ki a virtuális hálózat](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. <span data-ttu-id="f8a99-118">Kattintson a **alhálózati** toopick hello dedikált alhálózatot a virtuális hálózaton, mely tooenable belül az új kezelt tartományban található.</span><span class="sxs-lookup"><span data-stu-id="f8a99-118">Click **Subnet** toopick hello dedicated subnet in this virtual network, within which tooenable your new managed domain.</span></span> <span data-ttu-id="f8a99-119">A hello **alhálózati létrehozása** panelen hello alhálózat nevét adja meg, és kattintson a **OK** befejezése.</span><span class="sxs-lookup"><span data-stu-id="f8a99-119">In hello **Create subnet** blade, specify a name for hello subnet, and click **OK** when you're done.</span></span> <span data-ttu-id="f8a99-120">Hozzon létre például egy alhálózat hello neve "DomainServices", így könnyen más rendszergazdák toounderstand mi hello alhálózaton belül történik.</span><span class="sxs-lookup"><span data-stu-id="f8a99-120">For example, create a subnet with hello name 'DomainServices', making it easy for other administrators toounderstand what is deployed within hello subnet.</span></span>

    ![Válassza ki az alhálózatot hello virtuális hálózaton belül](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > <span data-ttu-id="f8a99-122">**Egy alhálózat kiválasztására vonatkozó irányelvek**</span><span class="sxs-lookup"><span data-stu-id="f8a99-122">**Guidelines for selecting a subnet**</span></span>
  > 1. <span data-ttu-id="f8a99-123">Használja a dedikált alhálózatot az Azure AD tartományi szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f8a99-123">Use a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="f8a99-124">Ne telepítsen más virtuális gépek toothis alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="f8a99-124">Do not deploy any other virtual machines toothis subnet.</span></span> <span data-ttu-id="f8a99-125">Ez a konfiguráció lehetővé teszi tooconfigure hálózati biztonsági csoportokkal (NSG-k) a munkaterhelések virtuális gépek számára a felügyelt tartományok megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="f8a99-125">This configuration enables you tooconfigure network security groups (NSGs) for your workloads/virtual machines without disrupting your managed domain.</span></span> <span data-ttu-id="f8a99-126">További információkért lásd: [hálózat az Azure Active Directory tartományi szolgáltatások szempontjai](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="f8a99-126">For details, see [networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>
  2. <span data-ttu-id="f8a99-127">Csak akkor válassza hello átjáró-alhálózatot az Azure AD tartományi szolgáltatások telepítése, mert már nem támogatott konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f8a99-127">Do not select hello Gateway subnet for deploying Azure AD Domain Services, because it is not a supported configuration.</span></span>
  3. <span data-ttu-id="f8a99-128">Győződjön meg arról, választott hello alhálózaton nincs elegendő elérhető címek lemezterület - legalább 3-5 elérhető IP-címek.</span><span class="sxs-lookup"><span data-stu-id="f8a99-128">Ensure that hello subnet you've selected has sufficient available address space - at least 3-5 available IP addresses.</span></span>
  >

5. <span data-ttu-id="f8a99-129">Amikor elkészült, kattintson a **OK** a toohello toomove **rendszergazdai csoport** hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="f8a99-129">When you are done, click **OK** toomove on toohello **Administrator group** page of hello wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="f8a99-130">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f8a99-130">Next step</span></span>
[<span data-ttu-id="f8a99-131">3. feladat: a felügyeleti csoport konfigurálása és Azure AD tartományi szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f8a99-131">Task 3: configure administrative group and enable Azure AD Domain Services</span></span>](active-directory-ds-getting-started-admingroup.md)
