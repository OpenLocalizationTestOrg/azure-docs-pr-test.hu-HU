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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 47507096a6245d4f1ba57a652ddf5167b3776ae9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="9ed3d-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="9ed3d-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>
<span data-ttu-id="9ed3d-104">Ez a cikk bemutatja, hogyan engedélyezze az Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-104">This article shows how to enable Azure Active Directory Domain Services (Azure AD DS) using the Azure portal.</span></span>


<span data-ttu-id="9ed3d-105">Elindíthatja a **engedélyezése az Azure AD tartományi szolgáltatások** varázslóban kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9ed3d-105">To launch the **Enable Azure AD Domain Services** wizard, complete the following steps:</span></span>

1. <span data-ttu-id="9ed3d-106">Nyissa meg az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9ed3d-106">Go to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9ed3d-107">A bal oldali ablaktáblájában kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-107">In the left pane, click on **New**.</span></span>
3. <span data-ttu-id="9ed3d-108">Az a **új** panelen, írja be **tartományi szolgáltatások** azokat a keresési sávon.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-108">In the **New** blade, type **Domain Services** into the search bar.</span></span>

    ![Keresse meg a tartományi szolgáltatások](./media/getting-started/search-domain-services.png)

4. <span data-ttu-id="9ed3d-110">Kattintással jelölje ki **Azure AD tartományi szolgáltatások** keresési javaslatok listája.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-110">Click to select **Azure AD Domain Services** from the list of search suggestions.</span></span> <span data-ttu-id="9ed3d-111">Az a **Azure AD tartományi szolgáltatások** panelen kattintson a **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-111">On the **Azure AD Domain Services** blade, click the **Create** button.</span></span>

    ![Tartományi szolgáltatások panel](./media/getting-started/domain-services-blade.png)

5. <span data-ttu-id="9ed3d-113">A **engedélyezése az Azure AD tartományi szolgáltatások** varázsló elindul.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-113">The **Enable Azure AD Domain Services** wizard is launched.</span></span>


## <a name="task-1-configure-basic-settings"></a><span data-ttu-id="9ed3d-114">1. feladat: konfigurálja az alapbeállításokat</span><span class="sxs-lookup"><span data-stu-id="9ed3d-114">Task 1: configure basic settings</span></span>
<span data-ttu-id="9ed3d-115">Az a **alapjai** lap a varázslóban megadhatja a DNS-tartománynév, a felügyelt tartomány számára.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-115">In the **Basics** page of the wizard, you can specify the DNS domain name for the managed domain.</span></span> <span data-ttu-id="9ed3d-116">Választhatja azt is, az erőforráscsoport és az Azure-beli hely, amelyhez a felügyelt tartományra kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-116">You can also choose the resource group and Azure location to which the managed domain should be deployed.</span></span>

![Alapvető beállítások konfigurálása](./media/getting-started/domain-services-blade-basics.png)

1. <span data-ttu-id="9ed3d-118">Válassza ki a **DNS-tartománynév** a felügyelt tartományok.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-118">Choose the **DNS domain name** for your managed domain.</span></span>

   * <span data-ttu-id="9ed3d-119">A címtár alapértelmezett tartományneve (az egy **. onmicrosoft.com** utótag) alapértelmezés szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-119">The default domain name of the directory (with a **.onmicrosoft.com** suffix) is specified by default.</span></span>

   * <span data-ttu-id="9ed3d-120">A egy egyéni tartománynevet is megadható.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-120">You can also type in a custom domain name.</span></span> <span data-ttu-id="9ed3d-121">Ebben a példában az egyéni tartománynév a *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-121">In this example, the custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="9ed3d-122">A megadott tartománynév előtagja (a *contoso100.com* tartománynévben például a *contoso100* tag) legfeljebb 15 karaktert tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-122">The prefix of your specified domain name (for example, *contoso100* in the *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="9ed3d-123">Nem hozható létre egy felügyelt tartomány 15 karakternél hosszabb előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-123">You cannot create a managed domain with a prefix longer than 15 characters.</span></span>
     >
     >

2. <span data-ttu-id="9ed3d-124">Ellenőrizze, hogy a felügyelt tartomány számára választott DNS-tartománynév még nem létezik a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-124">Ensure that the DNS domain name you have chosen for the managed domain does not already exist in the virtual network.</span></span> <span data-ttu-id="9ed3d-125">Pontosabban, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="9ed3d-125">Specifically, check whether:</span></span>

   * <span data-ttu-id="9ed3d-126">Már létezik-e tartomány ugyanezzel a DNS-tartománynévvel a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-126">You already have a domain with the same DNS domain name on the virtual network.</span></span>

   * <span data-ttu-id="9ed3d-127">A virtuális hálózat, ahol a felügyelt tartományra engedélyezni szeretné a helyszíni hálózat VPN-kapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-127">The virtual network where you plan to enable the managed domain has a VPN connection with your on-premises network.</span></span> <span data-ttu-id="9ed3d-128">Ebben a forgatókönyvben győződjön meg arról, nincs olyan tartomány ugyanezzel a DNS-tartománynévvel a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-128">In this scenario, ensure you don't have a domain with the same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="9ed3d-129">Létezik-e egy már meglévő felhőszolgáltatás ugyanezen a néven a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-129">You have an existing cloud service with that name on the virtual network.</span></span>

3. <span data-ttu-id="9ed3d-130">Válassza ki a **a fajta virtuális hálózatot**.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-130">Choose the **type of virtual network**.</span></span> <span data-ttu-id="9ed3d-131">Alapértelmezés szerint a **erőforrás-kezelő** virtuális hálózati típus van kijelölve.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-131">By default, the **Resource Manager** virtual network type is selected.</span></span> <span data-ttu-id="9ed3d-132">Ez a fajta virtuális hálózatot az összes felügyelt tartományok az újonnan létrehozott használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-132">We recommend using this type of virtual network for all newly created managed domains.</span></span>

4. <span data-ttu-id="9ed3d-133">Válassza ki az Azure **előfizetés** a szeretné létrehozni a felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-133">Select the Azure **Subscription** in which you would like to create the managed domain.</span></span>

5. <span data-ttu-id="9ed3d-134">Válassza ki a **erőforráscsoport** a felügyelt tartományra tartozik kell.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-134">Select the **Resource group** to which the managed domain should belong.</span></span> <span data-ttu-id="9ed3d-135">Ön választhatja a **hozzon létre új** vagy **meglévő** lehetőséget választhat ki az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-135">You can choose either the **Create new** or **Use existing** options to select the resource group.</span></span>

6. <span data-ttu-id="9ed3d-136">Válassza ki az Azure **hely** a a felügyelt tartományra meg kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-136">Choose the Azure **Location** in which the managed domain should be created.</span></span> <span data-ttu-id="9ed3d-137">Az a **hálózati** oldalon a varázsló csak virtuális hálózatok, amelyek a kijelölt helyre látja.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-137">On the **Network** page of the wizard, you see only virtual networks that belong to the location you have selected.</span></span>

7. <span data-ttu-id="9ed3d-138">Amikor elkészült, kattintson a **OK** a áthelyezése a **hálózati** a varázsló.</span><span class="sxs-lookup"><span data-stu-id="9ed3d-138">When you are done, click **OK** to move on to the **Network** page of the wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="9ed3d-139">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="9ed3d-139">Next step</span></span>
[<span data-ttu-id="9ed3d-140">2. feladat: a hálózati beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ed3d-140">Task 2: configure network settings</span></span>](active-directory-ds-getting-started-network.md)
