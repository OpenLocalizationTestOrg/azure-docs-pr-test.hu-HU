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
ms.openlocfilehash: f87bcf33d3b1eb21c7d84814e4c4086f664e293d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="95387-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="95387-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>


## <a name="task-3-configure-administrative-group"></a><span data-ttu-id="95387-104">3. feladat: a felügyeleti csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95387-104">Task 3: configure administrative group</span></span>
<span data-ttu-id="95387-105">A konfigurációs feladat létrehoz egy felügyeleti csoport az Azure AD-címtárát.</span><span class="sxs-lookup"><span data-stu-id="95387-105">In this configuration task, you create an administrative group in your Azure AD directory.</span></span> <span data-ttu-id="95387-106">Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*.</span><span class="sxs-lookup"><span data-stu-id="95387-106">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="95387-107">A csoport tagjai, amelyek a felügyelt tartományra tartományhoz csatlakoztatott számítógépen rendszergazdai jogosultsággal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="95387-107">Members of this group are granted administrative permissions on machines that are domain-joined to the managed domain.</span></span> <span data-ttu-id="95387-108">A tartományhoz csatlakoztatott számítógépeken ehhez a csoporthoz hozzáadni a Rendszergazdák csoport.</span><span class="sxs-lookup"><span data-stu-id="95387-108">On domain-joined machines, this group is added to the administrators group.</span></span> <span data-ttu-id="95387-109">A csoport tagjai emellett használhatja a távoli asztal távolról csatlakozni a tartományhoz csatlakoztatott számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="95387-109">Additionally, members of this group can use Remote Desktop to connect remotely to domain-joined machines.</span></span>

> [!NOTE]
> <span data-ttu-id="95387-110">Nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie az Azure Active Directory tartományi szolgáltatások által létrehozott felügyelt tartományon.</span><span class="sxs-lookup"><span data-stu-id="95387-110">You do not have Domain Administrator or Enterprise Administrator permissions on the managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="95387-111">A felügyelt tartományok ezeket az engedélyeket a szolgáltatás által fenntartott, és nem érhetik el a bérlő belüli felhasználók.</span><span class="sxs-lookup"><span data-stu-id="95387-111">On managed domains, these permissions are reserved by the service and are not made available to users within the tenant.</span></span> <span data-ttu-id="95387-112">A speciális felügyeleti csoport létrehozása a konfigurációs feladat használatával azonban néhány kiemelt műveletek végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="95387-112">However, you can use the special administrative group created in this configuration task to perform some privileged operations.</span></span> <span data-ttu-id="95387-113">Ezek a műveletek közé tartoznak a számítógépek csatlakoztatása a tartományhoz, tartományhoz csatlakozó számítógépeken a felügyeleti csoportba tartozó és a csoportházirend konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="95387-113">These operations include joining computers to the domain, belonging to the administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="95387-114">A varázsló automatikusan létrehozza a felügyeleti csoport az Azure AD-címtárát.</span><span class="sxs-lookup"><span data-stu-id="95387-114">The wizard automatically creates the administrative group in your Azure AD directory.</span></span> <span data-ttu-id="95387-115">Ez a csoport neve "AAD DC rendszergazdák".</span><span class="sxs-lookup"><span data-stu-id="95387-115">This group is called 'AAD DC Administrators'.</span></span> <span data-ttu-id="95387-116">Ha van ilyen nevű meglévő csoport az Azure AD-címtár, a varázsló kiválasztása ehhez a csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="95387-116">If you have an existing group with this name in your Azure AD directory, the wizard selects this group.</span></span> <span data-ttu-id="95387-117">Csoport tagsági használatával konfigurálhatja a **rendszergazdai csoport** varázsló lapja.</span><span class="sxs-lookup"><span data-stu-id="95387-117">You can configure group membership using the **Administrator group** wizard page.</span></span>

1. <span data-ttu-id="95387-118">A csoporttagság konfigurálásához kattintson **AAD DC rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="95387-118">To configure group membership, click **AAD DC Administrators**.</span></span>

    ![Csoporttagság konfigurálása](./media/getting-started/domain-services-blade-admingroup.png)

2. <span data-ttu-id="95387-120">Kattintson a **tagok hozzáadása** gomb az Azure AD-címtárban lévő felhasználók hozzáadásához a rendszergazda csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="95387-120">Click the **Add members** button to add users from your Azure AD directory to the administrator group.</span></span>

3. <span data-ttu-id="95387-121">Amikor elkészült, kattintson a **OK** a áthelyezése a **összegzés** a varázsló.</span><span class="sxs-lookup"><span data-stu-id="95387-121">When you are done, click **OK** to move on to the **Summary** page of the wizard.</span></span>

4. <span data-ttu-id="95387-122">A a **összegzés** oldalon a varázsló, tekintse át a felügyelt tartományra konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="95387-122">On the **Summary** page of the wizard, review the configuration settings for the managed domain.</span></span> <span data-ttu-id="95387-123">Később is visszatérhet módosításokat végezni, a varázsló minden lépésre szükség.</span><span class="sxs-lookup"><span data-stu-id="95387-123">You can go back to any step of the wizard to make changes, if necessary.</span></span> <span data-ttu-id="95387-124">Amikor elkészült, kattintson a **OK** az új felügyelt tartomány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="95387-124">When you are done, click **OK** to create the new managed domain.</span></span>

    ![Összefoglalás](./media/getting-started/domain-services-blade-summary.png)

5. <span data-ttu-id="95387-126">Megjelenik egy értesítés, hogy az Azure AD tartományi szolgáltatásokhoz központi telepítés végrehajtási állapotát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="95387-126">You see a notification that shows the progress of your Azure AD Domain Services deployment.</span></span> <span data-ttu-id="95387-127">Kattintson az értesítésre tekintse meg a központi telepítés részletes folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="95387-127">Click the notification to see detailed progress for the deployment.</span></span>

    ![Értesítés - telepítés folyamatban](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a><span data-ttu-id="95387-129">A felügyelt tartományok kiépítése</span><span class="sxs-lookup"><span data-stu-id="95387-129">Provision your managed domain</span></span>
<span data-ttu-id="95387-130">A felügyelt tartományok kiépítési folyamat egy óráig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="95387-130">The process of provisioning your managed domain can take up to an hour.</span></span>

1. <span data-ttu-id="95387-131">Amíg a telepítés folyamatban van, a a "tartományi szolgáltatások" kereshet a **keresési erőforrások** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="95387-131">While your deployment is in progress, you can search for 'domain services' in the **Search resources** search box.</span></span> <span data-ttu-id="95387-132">Válassza ki **Azure AD tartományi szolgáltatások** a keresési eredmény alapján.</span><span class="sxs-lookup"><span data-stu-id="95387-132">Select **Azure AD Domain Services** from the search result.</span></span> <span data-ttu-id="95387-133">A **Azure AD tartományi szolgáltatások** panel sorolja fel a felügyelt tartományra, amelyek telepítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="95387-133">The **Azure AD Domain Services** blade lists the managed domain that is being provisioned.</span></span>

    ![Található felügyelt tartomány telepítése folyamatban](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="95387-135">Kattintson a nevére, a felügyelt tartomány (például "contoso100.com") a tartománnyal kapcsolatos további részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="95387-135">Click the name of the managed domain (for example, 'contoso100.com') to see more details about the domain.</span></span>

    ![Tartományi szolgáltatások - üzembe helyezési állapota](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="95387-137">A **áttekintése** lapon láthatja, hogy a tartomány jelenleg telepítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="95387-137">The **Overview** tab shows that the domain is currently being provisioned.</span></span> <span data-ttu-id="95387-138">A felügyelt tartományra nem konfigurálható, amíg a teljes mértékben ki van építve.</span><span class="sxs-lookup"><span data-stu-id="95387-138">You cannot configure the managed domain until it is fully provisioned.</span></span> <span data-ttu-id="95387-139">Azt a felügyelt tartomány teljesen kiépített egy órát is tarthat.</span><span class="sxs-lookup"><span data-stu-id="95387-139">It may take up to an hour for your managed domain to be fully provisioned.</span></span>

    ![<span data-ttu-id="95387-140">Tartományi szolgáltatások - áttekintés lap során a kiépítési állapot</span><span class="sxs-lookup"><span data-stu-id="95387-140">Domain Services - Overview tab during the provisioning state</span></span> ](./media/getting-started/domain-services-provisioning-state-details.png)

4. <span data-ttu-id="95387-141">Ha a felügyelt tartományra teljesen ki van építve, a **áttekintése** lapon láthatók a tartomány állapotának **futtató**.</span><span class="sxs-lookup"><span data-stu-id="95387-141">When the managed domain is fully provisioned, the **Overview** tab shows the domain status as **Running**.</span></span>

    ![Tartományi szolgáltatások – Áttekintés lap a teljes kiépítés után](./media/getting-started/domain-services-provisioned.png)

5. <span data-ttu-id="95387-143">Az a **tulajdonságok** lapon látni tartományi vezérlők érhetők el a virtuális hálózat két IP-címet.</span><span class="sxs-lookup"><span data-stu-id="95387-143">On the **Properties** tab, you see two IP addresses at which domain controllers are available for the virtual network.</span></span>

    ![Tartományi szolgáltatások - teljesen kiépítése után tulajdonságai lap](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a><span data-ttu-id="95387-145">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="95387-145">Next step</span></span>
[<span data-ttu-id="95387-146">4. feladat: Az Azure virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="95387-146">Task 4: update the DNS settings for the Azure virtual network</span></span>](active-directory-ds-getting-started-dns.md)
