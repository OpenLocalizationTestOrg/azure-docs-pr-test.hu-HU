---
title: "Active Directory Domain Services: Az Active Directory Domain Services engedélyezése | Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 6263eb1849808a7c85e572e1046bc9039362dd9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a><span data-ttu-id="7bfc4-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="7bfc4-103">Enable Azure Active Directory Domain Services using hello Azure classic portal</span></span>

## <a name="task-3-enable-azure-active-directory-domain-services"></a><span data-ttu-id="7bfc4-104">3. feladat: Az Azure Active Directory Domain Services engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7bfc4-104">Task 3: enable Azure Active Directory Domain Services</span></span>
<span data-ttu-id="7bfc4-105">Ebben a feladatban engedélyezi Azure Active Directory tartományi szolgáltatások (az Azure AD DS) a címtáron a hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="7bfc4-105">In this task, you enable Azure Active Directory Domain Services (Azure AD DS) for your directory by doing hello following steps:</span></span>

1. <span data-ttu-id="7bfc4-106">Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7bfc4-106">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="7bfc4-107">Hello bal oldali ablaktáblában jelöljön ki hello **Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-107">In hello left pane, select hello **Active Directory** button.</span></span>
3. <span data-ttu-id="7bfc4-108">Válassza ki a kívánt Azure Active Directory tartományi szolgáltatások tooenable Azure Active Directory (Azure AD) hello bérlőt (címtárat).</span><span class="sxs-lookup"><span data-stu-id="7bfc4-108">Select hello Azure Active Directory (Azure AD) tenant (directory) for which you want tooenable Azure AD DS.</span></span>

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="7bfc4-110">A hello **preview directory** hello kattintson **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-110">On hello **preview directory** page, click hello **Configure** tab.</span></span>

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="7bfc4-112">A **tartományi szolgáltatások**, hello módosítása **engedélyezése tartományi szolgáltatásokat a címtárhoz** beállítás túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-112">Under **domain services**, change hello **Enable domain services for this directory** option too**Yes**.</span></span>  
    <span data-ttu-id="7bfc4-113">További Azure Active Directory tartományi szolgáltatások konfigurációs beállítások hello oldalon jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-113">Additional Azure Active Directory Domain Services configuration options appear on hello page.</span></span>

    ![Tartományi szolgáltatások engedélyezése](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > <span data-ttu-id="7bfc4-115">Ha engedélyezi az Azure Active Directory tartományi szolgáltatások a bérlő számára, az Azure AD hoz létre, és hello Kerberos és NTLM hitelesítő adatok kivonatait felhasználók hitelesítéséhez szükséges tárolja.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-115">When you enable Azure Active Directory Domain Services for your tenant, Azure AD generates and stores hello Kerberos and NTLM credential hashes that are required for authenticating users.</span></span>
   >
   >
6. <span data-ttu-id="7bfc4-116">Adja meg a hello **DNS-tartománynevet a tartományi szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-116">Specify hello **DNS domain name of domain services**.</span></span>

   * <span data-ttu-id="7bfc4-117">hello hello címtár alapértelmezett tartományneve (az egy **. onmicrosoft.com** utótag) alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-117">hello default domain name of hello directory (with a **.onmicrosoft.com** suffix) is selected by default.</span></span>

   * <span data-ttu-id="7bfc4-118">hello lista tartalmazza az Azure AD címtárhoz konfigurált összes tartomány, többek között, mindkét ellenőrzése és a nem ellenőrzött tartományok hello konfigurált **tartományok** fülre.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-118">hello list contains all domains that have been configured for your Azure AD directory, including both verified and unverified domains that you configure on hello **Domains** tab.</span></span>

   * <span data-ttu-id="7bfc4-119">Egy egyéni tartománynevet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-119">You can also enter a custom domain name.</span></span> <span data-ttu-id="7bfc4-120">Ebben a példában hello egyéni tartománynév megadása *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-120">In this example, hello custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="7bfc4-121">a megadott tartománynév előtagja hello (például *contoso100* a hello *contoso100.com* tartománynevet) kell tartalmaznia a 15 karaktert.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-121">hello prefix of your specified domain name (for example, *contoso100* in hello *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="7bfc4-122">Nem hozhat létre olyan Active Directory Domain Services tartományt, amelynek az előtagja 15 karakternél többet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-122">You cannot create an Azure Active Directory Domain Services domain with a prefix containing more than 15 characters.</span></span>
     >
     >
7. <span data-ttu-id="7bfc4-123">Győződjön meg arról, hogy hello DNS-tartománynevet választott hello felügyelt tartomány már nem létezik hello virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-123">Ensure that hello DNS domain name you have chosen for hello managed domain does not already exist in hello virtual network.</span></span> <span data-ttu-id="7bfc4-124">Pontosabban ellenőrizze hogy toosee:</span><span class="sxs-lookup"><span data-stu-id="7bfc4-124">Specifically, check toosee whether:</span></span>

   * <span data-ttu-id="7bfc4-125">Már létezik hello tartomány hello virtuális hálózaton azonos DNS-tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-125">You already have a domain with hello same DNS domain name on hello virtual network.</span></span>

   * <span data-ttu-id="7bfc4-126">hello választott virtuális hálózathoz van a helyszíni hálózat VPN-kapcsolattal, és a tartomány hello rendelkezik a helyszíni hálózaton azonos DNS-tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-126">hello virtual network you've selected has a VPN connection with your on-premises network, and you have a domain with hello same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="7bfc4-127">Hello virtuális hálózaton van ilyen nevű létező felhőalapú szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-127">You have an existing cloud service with that name on hello virtual network.</span></span>
8. <span data-ttu-id="7bfc4-128">Válasszon egy virtuális hálózatot kívánja Azure Active Directory tartományi szolgáltatások toobe érhető el.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-128">Select a virtual network on which you want Azure Active Directory Domain Services toobe available.</span></span> <span data-ttu-id="7bfc4-129">Válassza ki a hello virtuális hálózat és a dedikált alhálózati hello létrehozott **Connect tartományszolgáltatási toothis virtuális hálózati** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-129">Select hello virtual network and dedicated subnet you created in hello **Connect domain services toothis virtual network** drop-down list.</span></span> <span data-ttu-id="7bfc4-130">Is vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="7bfc4-130">Also consider hello following:</span></span>

   * <span data-ttu-id="7bfc4-131">Győződjön meg arról, Ön által megadott hello virtuális hálózat tartozik tooan Azure-régió, Azure Active Directory tartományi szolgáltatások által támogatott.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-131">Ensure that hello virtual network that you have specified belongs tooan Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="7bfc4-132">tooascertain hello Azure-régiók Azure Active Directory tartományi szolgáltatások esetén érhető el, lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="7bfc4-132">tooascertain hello Azure regions where Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>

   * <span data-ttu-id="7bfc4-133">Virtuális hálózatok tartozó tooa régió, ahol nem támogatott az Azure Active Directory tartományi szolgáltatások nem hello legördülő listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-133">Virtual networks that belong tooa region where Azure Active Directory Domain Services is not supported do not show up in hello drop-down list.</span></span>

   * <span data-ttu-id="7bfc4-134">Használjon egy dedikált alhálózati hello virtuális hálózaton belül az Azure Active Directory tartományi szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-134">Use a dedicated subnet within hello virtual network for Azure Active Directory Domain Services.</span></span> <span data-ttu-id="7bfc4-135">Tegye *nem* válasszon hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-135">Do *not* select hello gateway subnet.</span></span> <span data-ttu-id="7bfc4-136">Lásd: [hálózati szempontok](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="7bfc4-136">See [networking considerations](active-directory-ds-networking.md).</span></span>

   * <span data-ttu-id="7bfc4-137">Hasonlóképpen Azure Resource Manager használatával létrehozott virtuális hálózatok nem hello legördülő listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-137">Similarly, virtual networks that were created by using Azure Resource Manager do not appear in hello drop-down list.</span></span> <span data-ttu-id="7bfc4-138">A Resource Manager-alapú virtuális hálózatokat az Active Directory Domain Services jelenleg nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-138">Resource Manager-based virtual networks are not currently supported by Azure Active Directory Domain Services.</span></span>
9. <span data-ttu-id="7bfc4-139">Azure Active Directory tartományi szolgáltatások, tooenable hello munkaablakban hello lapon hello alján kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-139">tooenable Azure Active Directory Domain Services, in hello task pane at hello bottom of hello page, click **Save**.</span></span>
    * <span data-ttu-id="7bfc4-140">Azure Active Directory tartományi szolgáltatások engedélyezése a címtáron van, közben hello lap mezőjében a *függőben lévő*.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-140">While Azure Active Directory Domain Services is being enabled for your directory, hello page displays a status of *Pending*.</span></span>

        ![A Tartományi szolgáltatások engedélyezése ablak](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > <span data-ttu-id="7bfc4-142">Az Active Directory Domain Services biztosítja a felügyelt tartományok magas rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-142">Azure Active Directory Domain Services provides high availability for your managed domain.</span></span> <span data-ttu-id="7bfc4-143">Miután engedélyezte az Azure Active Directory tartományi szolgáltatások, a tartományi szolgáltatások elérhetőek a virtuális hálózati hello hello IP-címek megjelenített egyszerre csak egy.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-143">After you enable Azure Active Directory Domain Services, hello IP addresses at which domain services are available on hello virtual network are displayed one at a time.</span></span> <span data-ttu-id="7bfc4-144">hello második IP-cím jelenik meg hamarosan hello után először, amint hello szolgáltatás lehetővé teszi, hogy magas rendelkezésre állást a tartományra.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-144">hello second IP address is displayed shortly after hello first, as soon hello service enables high availability for your domain.</span></span> <span data-ttu-id="7bfc4-145">Ha magas rendelkezésre állás konfigurálva van és aktív a tartományon, megjelenítheti a hello két IP-címek **tartományi szolgáltatások** hello szakasza **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-145">When high availability is configured and active for your domain, you should see two IP addresses in hello **domain services** section of hello **Configure** tab.</span></span>
        >
        >
    * <span data-ttu-id="7bfc4-146">Too30 körülbelül 20 perc elteltével hello tartományi szolgáltatások érhetők el a virtuális hálózaton hello az első IP-cím **IP-cím** található hello **konfigurálása** lap.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-146">After about 20 too30 minutes, hello first IP address at which domain services are available on your virtual network in hello **IP address** field on hello **Configure** page.</span></span>

        ![A Tartományi szolgáltatások ablak, rajta az első kiosztott IP-címmel](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * <span data-ttu-id="7bfc4-148">Ha magas rendelkezésre állás a tartomány működési, két IP-címek hello oldalon jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-148">When high availability is operational for your domain, two IP addresses are displayed on hello page.</span></span> <span data-ttu-id="7bfc4-149">A felügyelt tartomány a kiválasztott virtuális hálózaton ezen a két IP-címen érhető el.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-149">Your managed domain is available on your selected virtual network at these two IP addresses.</span></span>

10. <span data-ttu-id="7bfc4-150">Vegye figyelembe a hello két IP-címet, hogy a virtuális hálózat hello DNS-beállítások segítségével frissítheti.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-150">Note hello two IP addresses so that you can update hello DNS settings for your virtual network.</span></span> <span data-ttu-id="7bfc4-151">Így lehetővé teszi, hogy a virtuális gépek hello virtuális hálózati tooconnect toohello tartományban műveletek, például a tartományhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-151">Doing so enables virtual machines on hello virtual network tooconnect toohello domain for operations such as domain join.</span></span>

    ![A Tartományi szolgáltatások ablak, rajta a két kiosztott IP-címmel](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> <span data-ttu-id="7bfc4-153">Attól függően, hogy az Azure AD-bérlő (például hello száma felhasználókat és csoportokat) hello méretét a szinkronizálás tooyour által kezelt tartomány eltart egy ideig.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-153">Depending on hello size of your Azure AD tenant (for example, hello number of users or groups), synchronization tooyour managed domain takes a while.</span></span> <span data-ttu-id="7bfc4-154">Ez a szinkronizálási folyamat hello háttérben történik.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-154">This synchronization process happens in hello background.</span></span> <span data-ttu-id="7bfc4-155">Az objektumok tízezreit tartalmazó nagy bérlők egy vagy két összes felhasználó, csoporttagságot és hitelesítő adatok toobe szinkronizált napot is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="7bfc4-155">For large tenants with tens of thousands of objects, it might take a day or two for all users, group memberships, and credentials toobe synchronized.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="7bfc4-156">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="7bfc4-156">Next step</span></span>
[<span data-ttu-id="7bfc4-157">4. feladat: hello hello Azure-beli virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="7bfc4-157">Task 4: update hello DNS settings for hello Azure virtual network</span></span>](active-directory-ds-getting-started-update-dns.md)
