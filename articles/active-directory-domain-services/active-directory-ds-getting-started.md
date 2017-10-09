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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 79cbb21c4a50194f5ad8ca1a4a8493ee4a260a9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="50bf6-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata</span><span class="sxs-lookup"><span data-stu-id="50bf6-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>
<span data-ttu-id="50bf6-104">Ez a cikk bemutatja, hogyan tooenable Azure Active Directory tartományi szolgáltatások (az Azure AD DS) használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="50bf6-104">This article shows how tooenable Azure Active Directory Domain Services (Azure AD DS) using hello Azure portal.</span></span>


<span data-ttu-id="50bf6-105">toolaunch hello **engedélyezése az Azure AD tartományi szolgáltatások** varázsló, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="50bf6-105">toolaunch hello **Enable Azure AD Domain Services** wizard, complete hello following steps:</span></span>

1. <span data-ttu-id="50bf6-106">Nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="50bf6-106">Go toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="50bf6-107">Hello bal oldali ablaktáblában kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="50bf6-107">In hello left pane, click on **New**.</span></span>
3. <span data-ttu-id="50bf6-108">A hello **új** panelen, írja be **tartományi szolgáltatások** hello keresősáv be.</span><span class="sxs-lookup"><span data-stu-id="50bf6-108">In hello **New** blade, type **Domain Services** into hello search bar.</span></span>

    ![Keresse meg a tartományi szolgáltatások](./media/getting-started/search-domain-services.png)

4. <span data-ttu-id="50bf6-110">Kattintson a tooselect **Azure AD tartományi szolgáltatások** hello keresési javaslatok listája.</span><span class="sxs-lookup"><span data-stu-id="50bf6-110">Click tooselect **Azure AD Domain Services** from hello list of search suggestions.</span></span> <span data-ttu-id="50bf6-111">A hello **Azure AD tartományi szolgáltatások** panelen kattintson hello **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="50bf6-111">On hello **Azure AD Domain Services** blade, click hello **Create** button.</span></span>

    ![Tartományi szolgáltatások panel](./media/getting-started/domain-services-blade.png)

5. <span data-ttu-id="50bf6-113">Hello **engedélyezése az Azure AD tartományi szolgáltatások** varázsló elindul.</span><span class="sxs-lookup"><span data-stu-id="50bf6-113">hello **Enable Azure AD Domain Services** wizard is launched.</span></span>


## <a name="task-1-configure-basic-settings"></a><span data-ttu-id="50bf6-114">1. feladat: konfigurálja az alapbeállításokat</span><span class="sxs-lookup"><span data-stu-id="50bf6-114">Task 1: configure basic settings</span></span>
<span data-ttu-id="50bf6-115">A hello **alapjai** lap hello varázsló hello hello által kezelt tartomány DNS-tartománynevet adhat meg.</span><span class="sxs-lookup"><span data-stu-id="50bf6-115">In hello **Basics** page of hello wizard, you can specify hello DNS domain name for hello managed domain.</span></span> <span data-ttu-id="50bf6-116">Hello erőforráscsoportot is beállíthatja, és az Azure-beli hely toowhich hello által felügyelt tartományokhoz kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="50bf6-116">You can also choose hello resource group and Azure location toowhich hello managed domain should be deployed.</span></span>

![Alapvető beállítások konfigurálása](./media/getting-started/domain-services-blade-basics.png)

1. <span data-ttu-id="50bf6-118">Válassza ki a hello **DNS-tartománynév** a felügyelt tartományok.</span><span class="sxs-lookup"><span data-stu-id="50bf6-118">Choose hello **DNS domain name** for your managed domain.</span></span>

   * <span data-ttu-id="50bf6-119">hello hello címtár alapértelmezett tartományneve (az egy **. onmicrosoft.com** utótag) alapértelmezés szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="50bf6-119">hello default domain name of hello directory (with a **.onmicrosoft.com** suffix) is specified by default.</span></span>

   * <span data-ttu-id="50bf6-120">A egy egyéni tartománynevet is megadható.</span><span class="sxs-lookup"><span data-stu-id="50bf6-120">You can also type in a custom domain name.</span></span> <span data-ttu-id="50bf6-121">Ebben a példában hello egyéni tartománynév megadása *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="50bf6-121">In this example, hello custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="50bf6-122">a megadott tartománynév előtagja hello (például *contoso100* a hello *contoso100.com* tartománynevet) kell tartalmaznia a 15 karaktert.</span><span class="sxs-lookup"><span data-stu-id="50bf6-122">hello prefix of your specified domain name (for example, *contoso100* in hello *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="50bf6-123">Nem hozható létre egy felügyelt tartomány 15 karakternél hosszabb előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="50bf6-123">You cannot create a managed domain with a prefix longer than 15 characters.</span></span>
     >
     >

2. <span data-ttu-id="50bf6-124">Győződjön meg arról, hogy hello DNS-tartománynevet választott hello felügyelt tartomány már nem létezik hello virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="50bf6-124">Ensure that hello DNS domain name you have chosen for hello managed domain does not already exist in hello virtual network.</span></span> <span data-ttu-id="50bf6-125">Pontosabban, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="50bf6-125">Specifically, check whether:</span></span>

   * <span data-ttu-id="50bf6-126">Már létezik hello tartomány hello virtuális hálózaton azonos DNS-tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="50bf6-126">You already have a domain with hello same DNS domain name on hello virtual network.</span></span>

   * <span data-ttu-id="50bf6-127">hello tooenable hello által kezelt tartomány tervezett virtuális hálózat a helyszíni hálózat VPN-kapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="50bf6-127">hello virtual network where you plan tooenable hello managed domain has a VPN connection with your on-premises network.</span></span> <span data-ttu-id="50bf6-128">Ebben a forgatókönyvben a ellenőrizze, hogy nincs olyan tartomány hello azonos DNS-tartománynevet a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="50bf6-128">In this scenario, ensure you don't have a domain with hello same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="50bf6-129">Hello virtuális hálózaton van ilyen nevű létező felhőalapú szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="50bf6-129">You have an existing cloud service with that name on hello virtual network.</span></span>

3. <span data-ttu-id="50bf6-130">Válassza ki a hello **a fajta virtuális hálózatot**.</span><span class="sxs-lookup"><span data-stu-id="50bf6-130">Choose hello **type of virtual network**.</span></span> <span data-ttu-id="50bf6-131">Alapértelmezés szerint hello **erőforrás-kezelő** virtuális hálózati típus van kijelölve.</span><span class="sxs-lookup"><span data-stu-id="50bf6-131">By default, hello **Resource Manager** virtual network type is selected.</span></span> <span data-ttu-id="50bf6-132">Ez a fajta virtuális hálózatot az összes felügyelt tartományok az újonnan létrehozott használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="50bf6-132">We recommend using this type of virtual network for all newly created managed domains.</span></span>

4. <span data-ttu-id="50bf6-133">Jelölje be hello Azure **előfizetés** , amelyben szeretné toocreate hello által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="50bf6-133">Select hello Azure **Subscription** in which you would like toocreate hello managed domain.</span></span>

5. <span data-ttu-id="50bf6-134">Jelölje be hello **erőforráscsoport** toowhich hello felügyelt tartományhoz kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="50bf6-134">Select hello **Resource group** toowhich hello managed domain should belong.</span></span> <span data-ttu-id="50bf6-135">Dönthet úgy, vagy hello **hozzon létre új** vagy **meglévő** beállítások tooselect hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="50bf6-135">You can choose either hello **Create new** or **Use existing** options tooselect hello resource group.</span></span>

6. <span data-ttu-id="50bf6-136">Válassza ki a hello Azure **hely** mely hello által felügyelt tartományokhoz kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="50bf6-136">Choose hello Azure **Location** in which hello managed domain should be created.</span></span> <span data-ttu-id="50bf6-137">A hello **hálózati** lap hello varázsló csak virtuális hálózatok tartozó kijelölt toohello helyét látja.</span><span class="sxs-lookup"><span data-stu-id="50bf6-137">On hello **Network** page of hello wizard, you see only virtual networks that belong toohello location you have selected.</span></span>

7. <span data-ttu-id="50bf6-138">Amikor elkészült, kattintson a **OK** a toohello toomove **hálózati** hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="50bf6-138">When you are done, click **OK** toomove on toohello **Network** page of hello wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="50bf6-139">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="50bf6-139">Next step</span></span>
[<span data-ttu-id="50bf6-140">2. feladat: a hálózati beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="50bf6-140">Task 2: configure network settings</span></span>](active-directory-ds-getting-started-network.md)
