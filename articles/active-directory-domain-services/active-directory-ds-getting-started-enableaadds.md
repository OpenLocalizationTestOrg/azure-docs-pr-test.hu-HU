---
title: "Active Directory Domain Services: Az Active Directory Domain Services engedélyezése | Microsoft Docs"
description: "Az Azure Active Directory Domain Services engedélyezése a klasszikus Azure portál használatával"
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
ms.openlocfilehash: ed72325ca9db99405c6173eb882a92f80cd77f47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a><span data-ttu-id="15597-103">Az Azure Active Directory Domain Services engedélyezése a klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="15597-103">Enable Azure Active Directory Domain Services using the Azure classic portal</span></span>

## <a name="task-3-enable-azure-active-directory-domain-services"></a><span data-ttu-id="15597-104">3. feladat: Az Azure Active Directory Domain Services engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15597-104">Task 3: enable Azure Active Directory Domain Services</span></span>
<span data-ttu-id="15597-105">Ennek a feladatnak a keretében engedélyezi az Azure Active Directory Domain Servicest (Azure AD DS) a címtár számára a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="15597-105">In this task, you enable Azure Active Directory Domain Services (Azure AD DS) for your directory by doing the following steps:</span></span>

1. <span data-ttu-id="15597-106">Nyissa meg a [klasszikus Azure portált](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="15597-106">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="15597-107">A bal oldali panelen válassza az **Active Directory** gombot.</span><span class="sxs-lookup"><span data-stu-id="15597-107">In the left pane, select the **Active Directory** button.</span></span>
3. <span data-ttu-id="15597-108">Válassza ki az Azure Active Directory (Azure AD) bérlőt (címtárat), amelyiken engedélyezni kívánja az Azure AD DS szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="15597-108">Select the Azure Active Directory (Azure AD) tenant (directory) for which you want to enable Azure AD DS.</span></span>

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="15597-110">Az **előzetes verzió** címtár oldalán kattintson a **Konfigurálás** lapra.</span><span class="sxs-lookup"><span data-stu-id="15597-110">On the **preview directory** page, click the **Configure** tab.</span></span>

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="15597-112">A **tartományi szolgáltatások** alatt váltsa át a **Tartományi szolgáltatások engedélyezése a címtáron** beállítást **Igen** értékre.</span><span class="sxs-lookup"><span data-stu-id="15597-112">Under **domain services**, change the **Enable domain services for this directory** option to **Yes**.</span></span>  
    <span data-ttu-id="15597-113">További Active Directory Domain Services konfigurációs beállítások jelennek meg az oldalon.</span><span class="sxs-lookup"><span data-stu-id="15597-113">Additional Azure Active Directory Domain Services configuration options appear on the page.</span></span>

    ![Tartományi szolgáltatások engedélyezése](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > <span data-ttu-id="15597-115">Amikor engedélyezi az Active Directory Domain Servicest a bérlő számára, az Azure AD létrehozza és tárolja a felhasználók hitelesítéséhez szükséges Kerberos és NTLM hitelesítő adatok kivonatait.</span><span class="sxs-lookup"><span data-stu-id="15597-115">When you enable Azure Active Directory Domain Services for your tenant, Azure AD generates and stores the Kerberos and NTLM credential hashes that are required for authenticating users.</span></span>
   >
   >
6. <span data-ttu-id="15597-116">Adja meg a **tartományi szolgáltatások DNS-tartománynevét**.</span><span class="sxs-lookup"><span data-stu-id="15597-116">Specify the **DNS domain name of domain services**.</span></span>

   * <span data-ttu-id="15597-117">Alapértelmezés szerint a címtár alapértelmezett tartományneve (azaz az **.onmicrosoft.com** utótaggal végződő) lesz kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="15597-117">The default domain name of the directory (with a **.onmicrosoft.com** suffix) is selected by default.</span></span>

   * <span data-ttu-id="15597-118">A lista tartalmazza az Azure AD címtárhoz konfigurált összes tartományt, beleértve az ellenőrzött és a nem ellenőrzött tartományokat egyaránt, amelyeket a **Tartományok** lapon konfigurált.</span><span class="sxs-lookup"><span data-stu-id="15597-118">The list contains all domains that have been configured for your Azure AD directory, including both verified and unverified domains that you configure on the **Domains** tab.</span></span>

   * <span data-ttu-id="15597-119">Egy egyéni tartománynevet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="15597-119">You can also enter a custom domain name.</span></span> <span data-ttu-id="15597-120">Ebben a példában az egyéni tartománynév a *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="15597-120">In this example, the custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="15597-121">A megadott tartománynév előtagja (a *contoso100.com* tartománynévben például a *contoso100* tag) legfeljebb 15 karaktert tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="15597-121">The prefix of your specified domain name (for example, *contoso100* in the *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="15597-122">Nem hozhat létre olyan Active Directory Domain Services tartományt, amelynek az előtagja 15 karakternél többet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="15597-122">You cannot create an Azure Active Directory Domain Services domain with a prefix containing more than 15 characters.</span></span>
     >
     >
7. <span data-ttu-id="15597-123">Ellenőrizze, hogy a felügyelt tartomány számára választott DNS-tartománynév még nem létezik a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="15597-123">Ensure that the DNS domain name you have chosen for the managed domain does not already exist in the virtual network.</span></span> <span data-ttu-id="15597-124">A következőket ellenőrizze:</span><span class="sxs-lookup"><span data-stu-id="15597-124">Specifically, check to see whether:</span></span>

   * <span data-ttu-id="15597-125">Már létezik-e tartomány ugyanezzel a DNS-tartománynévvel a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="15597-125">You already have a domain with the same DNS domain name on the virtual network.</span></span>

   * <span data-ttu-id="15597-126">A kiválasztott virtuális hálózat rendelkezik-e VPN-kapcsolattal a helyszíni hálózattal, és hogy már létezik-e tartomány ugyanezzel a DNS-tartománynévvel a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="15597-126">The virtual network you've selected has a VPN connection with your on-premises network, and you have a domain with the same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="15597-127">Létezik-e egy már meglévő felhőszolgáltatás ugyanezen a néven a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="15597-127">You have an existing cloud service with that name on the virtual network.</span></span>
8. <span data-ttu-id="15597-128">Válasszon egy virtuális hálózatot, amelyen szeretné, ha az Active Directory Domain Services elérhető lenne.</span><span class="sxs-lookup"><span data-stu-id="15597-128">Select a virtual network on which you want Azure Active Directory Domain Services to be available.</span></span> <span data-ttu-id="15597-129">Válassza ki a létrehozott virtuális hálózatot és a kijelölt alhálózatot a **Tartományi szolgáltatások csatlakoztatása a virtuális hálózathoz** legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="15597-129">Select the virtual network and dedicated subnet you created in the **Connect domain services to this virtual network** drop-down list.</span></span> <span data-ttu-id="15597-130">Ügyeljen a következőkre is:</span><span class="sxs-lookup"><span data-stu-id="15597-130">Also consider the following:</span></span>

   * <span data-ttu-id="15597-131">Ellenőrizze, hogy a megadott virtuális hálózat az Active Directory Domain Services által támogatott Azure-régióba tartozik-e.</span><span class="sxs-lookup"><span data-stu-id="15597-131">Ensure that the virtual network that you have specified belongs to an Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="15597-132">Az Azure-régiók listájáért, ahol az Active Directory Domain Services elérhető, lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="15597-132">To ascertain the Azure regions where Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>

   * <span data-ttu-id="15597-133">Az Active Directory Domain Services által nem támogatott régiókba tartozó virtuális hálózatok nem jelennek meg a legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="15597-133">Virtual networks that belong to a region where Azure Active Directory Domain Services is not supported do not show up in the drop-down list.</span></span>

   * <span data-ttu-id="15597-134">Az Active Directory Domain Serviceshez a virtuális hálózaton belüli kijelölt alhálózatot használjon.</span><span class="sxs-lookup"><span data-stu-id="15597-134">Use a dedicated subnet within the virtual network for Azure Active Directory Domain Services.</span></span> <span data-ttu-id="15597-135">*Ne* válassza az átjáró alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="15597-135">Do *not* select the gateway subnet.</span></span> <span data-ttu-id="15597-136">Lásd: [hálózati szempontok](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="15597-136">See [networking considerations](active-directory-ds-networking.md).</span></span>

   * <span data-ttu-id="15597-137">Az Azure Resource Manager használatával létrehozott virtuális hálózatok szintén nem jelennek meg a legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="15597-137">Similarly, virtual networks that were created by using Azure Resource Manager do not appear in the drop-down list.</span></span> <span data-ttu-id="15597-138">A Resource Manager-alapú virtuális hálózatokat az Active Directory Domain Services jelenleg nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="15597-138">Resource Manager-based virtual networks are not currently supported by Azure Active Directory Domain Services.</span></span>
9. <span data-ttu-id="15597-139">Az Active Directory Domain Services engedélyezéséhez kattintson a **Mentés** gombra a feladatpanelen a lap alján.</span><span class="sxs-lookup"><span data-stu-id="15597-139">To enable Azure Active Directory Domain Services, in the task pane at the bottom of the page, click **Save**.</span></span>
    * <span data-ttu-id="15597-140">Amíg a rendszer engedélyezi az Active Directory Domain Servicest a címtáron, az oldal *Függőben* állapotot mutat.</span><span class="sxs-lookup"><span data-stu-id="15597-140">While Azure Active Directory Domain Services is being enabled for your directory, the page displays a status of *Pending*.</span></span>

        ![A Tartományi szolgáltatások engedélyezése ablak](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > <span data-ttu-id="15597-142">Az Active Directory Domain Services biztosítja a felügyelt tartományok magas rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="15597-142">Azure Active Directory Domain Services provides high availability for your managed domain.</span></span> <span data-ttu-id="15597-143">Miután engedélyezte az Active Directory Domain Servicest, az IP-címek, amelyeken a tartományi szolgáltatások a virtuális hálózaton elérhetők, egyenként jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="15597-143">After you enable Azure Active Directory Domain Services, the IP addresses at which domain services are available on the virtual network are displayed one at a time.</span></span> <span data-ttu-id="15597-144">Az első után a második IP-cím is hamarosan megjelenik, amint a szolgáltatás engedélyezi a magas rendelkezésre állást a tartomány számára.</span><span class="sxs-lookup"><span data-stu-id="15597-144">The second IP address is displayed shortly after the first, as soon the service enables high availability for your domain.</span></span> <span data-ttu-id="15597-145">Ha a magas rendelkezésre állás konfigurálva van és aktív a tartományon, két IP-címet kell látnia a **Configure** (Konfigurálás) lap **domain services** (tartományi szolgáltatások) szakaszában.</span><span class="sxs-lookup"><span data-stu-id="15597-145">When high availability is configured and active for your domain, you should see two IP addresses in the **domain services** section of the **Configure** tab.</span></span>
        >
        >
    * <span data-ttu-id="15597-146">Mintegy 20–30 perc múltán a **Konfigurálás** lap **IP-cím** mezőjében láthatja az első IP-címet, amelyen a tartományi szolgáltatások elérhetők a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="15597-146">After about 20 to 30 minutes, the first IP address at which domain services are available on your virtual network in the **IP address** field on the **Configure** page.</span></span>

        ![A Tartományi szolgáltatások ablak, rajta az első kiosztott IP-címmel](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * <span data-ttu-id="15597-148">Ha a magas rendelkezésre állás működik a tartomány esetén, két IP-cím látható a lapon.</span><span class="sxs-lookup"><span data-stu-id="15597-148">When high availability is operational for your domain, two IP addresses are displayed on the page.</span></span> <span data-ttu-id="15597-149">A felügyelt tartomány a kiválasztott virtuális hálózaton ezen a két IP-címen érhető el.</span><span class="sxs-lookup"><span data-stu-id="15597-149">Your managed domain is available on your selected virtual network at these two IP addresses.</span></span>

10. <span data-ttu-id="15597-150">Jegyezze fel a két IP-címet a virtuális hálózat DNS-beállításainak frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="15597-150">Note the two IP addresses so that you can update the DNS settings for your virtual network.</span></span> <span data-ttu-id="15597-151">Ez lehetővé teszi, hogy a virtuális hálózaton lévő virtuális gépek a tartományhoz kapcsolódhassanak a csatlakozás tartományhoz és hasonló műveletek céljából.</span><span class="sxs-lookup"><span data-stu-id="15597-151">Doing so enables virtual machines on the virtual network to connect to the domain for operations such as domain join.</span></span>

    ![A Tartományi szolgáltatások ablak, rajta a két kiosztott IP-címmel](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> <span data-ttu-id="15597-153">Az Azure AD-bérlő méretétől (például a felhasználók vagy csoportok számától) függően a felügyelt tartományhoz való szinkronizálás eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="15597-153">Depending on the size of your Azure AD tenant (for example, the number of users or groups), synchronization to your managed domain takes a while.</span></span> <span data-ttu-id="15597-154">Ez a szinkronizálási folyamat a háttérben zajlik.</span><span class="sxs-lookup"><span data-stu-id="15597-154">This synchronization process happens in the background.</span></span> <span data-ttu-id="15597-155">A több tízezer objektumot tartalmazó nagy méretű bérlők esetében az összes felhasználó, csoporttagság és hitelesítő adat szinkronizálása egy vagy két napot is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="15597-155">For large tenants with tens of thousands of objects, it might take a day or two for all users, group memberships, and credentials to be synchronized.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="15597-156">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="15597-156">Next step</span></span>
[<span data-ttu-id="15597-157">4. feladat: Az Azure virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="15597-157">Task 4: update the DNS settings for the Azure virtual network</span></span>](active-directory-ds-getting-started-update-dns.md)
