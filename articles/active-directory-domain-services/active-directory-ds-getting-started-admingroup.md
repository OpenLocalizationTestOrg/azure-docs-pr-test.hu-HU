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
ms.openlocfilehash: 8bde872a13bc9960d1e62c74017ff78a8953a0a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="e9dee-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata</span><span class="sxs-lookup"><span data-stu-id="e9dee-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>


## <a name="task-3-configure-administrative-group"></a><span data-ttu-id="e9dee-104">3. feladat: a felügyeleti csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9dee-104">Task 3: configure administrative group</span></span>
<span data-ttu-id="e9dee-105">A konfigurációs feladat létrehoz egy felügyeleti csoport az Azure AD-címtárát.</span><span class="sxs-lookup"><span data-stu-id="e9dee-105">In this configuration task, you create an administrative group in your Azure AD directory.</span></span> <span data-ttu-id="e9dee-106">Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*.</span><span class="sxs-lookup"><span data-stu-id="e9dee-106">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="e9dee-107">A csoport tagjai a gépeken, amelyek a tartományhoz csatlakoztatott toohello által kezelt tartomány rendszergazdai jogosultsággal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="e9dee-107">Members of this group are granted administrative permissions on machines that are domain-joined toohello managed domain.</span></span> <span data-ttu-id="e9dee-108">A tartományhoz csatlakoztatott számítógépeken ez a csoport toohello rendszergazdák csoport jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e9dee-108">On domain-joined machines, this group is added toohello administrators group.</span></span> <span data-ttu-id="e9dee-109">Emellett a csoport tagjai is használ a távoli asztal tooconnect távolról toodomain csatlakoztatott számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="e9dee-109">Additionally, members of this group can use Remote Desktop tooconnect remotely toodomain-joined machines.</span></span>

> [!NOTE]
> <span data-ttu-id="e9dee-110">A felügyelt tartományra hello Azure Active Directory tartományi szolgáltatások által létrehozott nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e9dee-110">You do not have Domain Administrator or Enterprise Administrator permissions on hello managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="e9dee-111">A felügyelt tartományok ezek az engedélyek hello szolgáltatás által fenntartott és az nem elérhető toousers hello bérlői belül.</span><span class="sxs-lookup"><span data-stu-id="e9dee-111">On managed domains, these permissions are reserved by hello service and are not made available toousers within hello tenant.</span></span> <span data-ttu-id="e9dee-112">Azonban használhat hello speciális felügyeleti csoport létrehozása a konfigurációs feladat tooperform a néhány privilegizált műveleteket.</span><span class="sxs-lookup"><span data-stu-id="e9dee-112">However, you can use hello special administrative group created in this configuration task tooperform some privileged operations.</span></span> <span data-ttu-id="e9dee-113">Ezek a műveletek közé tartoznak a számítógépek toohello tartományhoz való csatlakozás, tartományhoz csatlakoztatott számítógépeken toohello felügyeleti csoporthoz tartozó és a csoportházirend konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="e9dee-113">These operations include joining computers toohello domain, belonging toohello administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="e9dee-114">hello varázsló automatikusan létrehozza a hello felügyeleti csoport, az Azure AD-címtárát.</span><span class="sxs-lookup"><span data-stu-id="e9dee-114">hello wizard automatically creates hello administrative group in your Azure AD directory.</span></span> <span data-ttu-id="e9dee-115">Ez a csoport neve "AAD DC rendszergazdák".</span><span class="sxs-lookup"><span data-stu-id="e9dee-115">This group is called 'AAD DC Administrators'.</span></span> <span data-ttu-id="e9dee-116">Ha van ilyen nevű meglévő csoport az Azure AD-címtár, hello varázsló kiválasztása ehhez a csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="e9dee-116">If you have an existing group with this name in your Azure AD directory, hello wizard selects this group.</span></span> <span data-ttu-id="e9dee-117">Csoporttagság hello használatával konfigurálhat **rendszergazdai csoport** varázsló lapja.</span><span class="sxs-lookup"><span data-stu-id="e9dee-117">You can configure group membership using hello **Administrator group** wizard page.</span></span>

1. <span data-ttu-id="e9dee-118">tooconfigure csoporttagság, kattintson a **AAD DC rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="e9dee-118">tooconfigure group membership, click **AAD DC Administrators**.</span></span>

    ![Csoporttagság konfigurálása](./media/getting-started/domain-services-blade-admingroup.png)

2. <span data-ttu-id="e9dee-120">Kattintson a hello **tagok hozzáadása** gombra kattint, az Azure AD directory toohello rendszergazda-csoport tooadd felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="e9dee-120">Click hello **Add members** button tooadd users from your Azure AD directory toohello administrator group.</span></span>

3. <span data-ttu-id="e9dee-121">Amikor elkészült, kattintson a **OK** a toohello toomove **összegzés** hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="e9dee-121">When you are done, click **OK** toomove on toohello **Summary** page of hello wizard.</span></span>

4. <span data-ttu-id="e9dee-122">A hello **összegzés** hello varázsló, felülvizsgálati hello a hello által kezelt tartomány beállításait.</span><span class="sxs-lookup"><span data-stu-id="e9dee-122">On hello **Summary** page of hello wizard, review hello configuration settings for hello managed domain.</span></span> <span data-ttu-id="e9dee-123">Lépjen vissza tooany lépés hello varázsló toomake változásokat, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="e9dee-123">You can go back tooany step of hello wizard toomake changes, if necessary.</span></span> <span data-ttu-id="e9dee-124">Amikor elkészült, kattintson a **OK** toocreate hello új felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="e9dee-124">When you are done, click **OK** toocreate hello new managed domain.</span></span>

    ![Összefoglalás](./media/getting-started/domain-services-blade-summary.png)

5. <span data-ttu-id="e9dee-126">Megjelenik egy értesítés, hogy hello az Azure AD tartományi szolgáltatásokhoz központi telepítés végrehajtási állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e9dee-126">You see a notification that shows hello progress of your Azure AD Domain Services deployment.</span></span> <span data-ttu-id="e9dee-127">Kattintson a hello értesítés toosee részletes hello telepítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="e9dee-127">Click hello notification toosee detailed progress for hello deployment.</span></span>

    ![Értesítés - telepítés folyamatban](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a><span data-ttu-id="e9dee-129">A felügyelt tartományok kiépítése</span><span class="sxs-lookup"><span data-stu-id="e9dee-129">Provision your managed domain</span></span>
<span data-ttu-id="e9dee-130">a felügyelt tartományok kiépítés hello folyamat akár tooan óra is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="e9dee-130">hello process of provisioning your managed domain can take up tooan hour.</span></span>

1. <span data-ttu-id="e9dee-131">Amíg a telepítés van folyamatban, hello tartományi szolgáltatásokban keresheti **keresési erőforrások** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="e9dee-131">While your deployment is in progress, you can search for 'domain services' in hello **Search resources** search box.</span></span> <span data-ttu-id="e9dee-132">Válassza ki **Azure AD tartományi szolgáltatások** hello keresési eredmény alapján.</span><span class="sxs-lookup"><span data-stu-id="e9dee-132">Select **Azure AD Domain Services** from hello search result.</span></span> <span data-ttu-id="e9dee-133">Hello **Azure AD tartományi szolgáltatások** panel megjeleníti hello által felügyelt tartományokhoz, amelyek telepítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="e9dee-133">hello **Azure AD Domain Services** blade lists hello managed domain that is being provisioned.</span></span>

    ![Található felügyelt tartomány telepítése folyamatban](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="e9dee-135">Kattintson hello hello felügyelt tartomány (például "contoso100.com") toosee hello tartománnyal kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="e9dee-135">Click hello name of hello managed domain (for example, 'contoso100.com') toosee more details about hello domain.</span></span>

    ![Tartományi szolgáltatások - üzembe helyezési állapota](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="e9dee-137">Hello **áttekintése** lapon láthatók, melyek hello tartományhoz jelenleg folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="e9dee-137">hello **Overview** tab shows that hello domain is currently being provisioned.</span></span> <span data-ttu-id="e9dee-138">Hello által kezelt tartomány teljesen kiépítéséig nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="e9dee-138">You cannot configure hello managed domain until it is fully provisioned.</span></span> <span data-ttu-id="e9dee-139">A felügyelt tartományra toobe teljesen kiépítve tooan órát igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="e9dee-139">It may take up tooan hour for your managed domain toobe fully provisioned.</span></span>

    ![<span data-ttu-id="e9dee-140">Tartományi szolgáltatások - áttekintés lap során hello üzembe helyezési állapota</span><span class="sxs-lookup"><span data-stu-id="e9dee-140">Domain Services - Overview tab during hello provisioning state</span></span> ](./media/getting-started/domain-services-provisioning-state-details.png)

4. <span data-ttu-id="e9dee-141">Amikor hello által kezelt tartomány teljesen ki van építve, hello **áttekintése** lapon láthatók hello tartomány állapotának **futtató**.</span><span class="sxs-lookup"><span data-stu-id="e9dee-141">When hello managed domain is fully provisioned, hello **Overview** tab shows hello domain status as **Running**.</span></span>

    ![Tartományi szolgáltatások – Áttekintés lap a teljes kiépítés után](./media/getting-started/domain-services-provisioned.png)

5. <span data-ttu-id="e9dee-143">A hello **tulajdonságok** lapon látni tartományi tartományvezérlők elérhetők hello virtuális hálózat két IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e9dee-143">On hello **Properties** tab, you see two IP addresses at which domain controllers are available for hello virtual network.</span></span>

    ![Tartományi szolgáltatások - teljesen kiépítése után tulajdonságai lap](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a><span data-ttu-id="e9dee-145">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="e9dee-145">Next step</span></span>
[<span data-ttu-id="e9dee-146">4. feladat: hello hello Azure-beli virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="e9dee-146">Task 4: update hello DNS settings for hello Azure virtual network</span></span>](active-directory-ds-getting-started-dns.md)
