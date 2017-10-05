---
title: "Az Azure Active Directoryban egyéni tartománynevek kezelése |} Microsoft Docs"
description: "Felügyeleti fogalmakat és használati útmutatók az Azure Active Directoryban egyéni tartományok kezelése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 5ae19bb370064de96cf466ca09b13d02563d65a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="1a722-103">Az Azure Active Directoryban egyéni tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="1a722-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="1a722-104">A tartomány neve lehet sok címtárerőforrásokkal fontos azonosítója részeként:</span><span class="sxs-lookup"><span data-stu-id="1a722-104">A domain name can be an important identifier for many directory resources, as part of:</span></span>

* <span data-ttu-id="1a722-105">A felhasználói név vagy e-mail cím egy felhasználó számára</span><span class="sxs-lookup"><span data-stu-id="1a722-105">A user name or email address for a user</span></span>
* <span data-ttu-id="1a722-106">A cím egy csoporthoz</span><span class="sxs-lookup"><span data-stu-id="1a722-106">The address for a group</span></span>
* <span data-ttu-id="1a722-107">A app ID URI az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="1a722-107">The app ID URI for an application</span></span>

<span data-ttu-id="1a722-108">Az Azure Active Directory (Azure AD) erőforrás neve, a rendszer már ellenőrzi a könyvtárban, amely tartalmazza az erőforrás tulajdonosa lehet tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="1a722-108">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified to be owned by the directory that contains the resource.</span></span> <span data-ttu-id="1a722-109">Csak egy globális rendszergazda Azure AD-ben végrehajthat tartomány feladatokat.</span><span class="sxs-lookup"><span data-stu-id="1a722-109">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a722-110">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="1a722-110">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="1a722-111">Az Azure AD felügyeleti központban a tartománynevek kezelése című történő [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1a722-111">For how to manage your domain names in the Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="1a722-112">Állítsa be az elsődleges tartomány nevét az Azure AD-címtár</span><span class="sxs-lookup"><span data-stu-id="1a722-112">Set the primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="1a722-113">A könyvtár jön létre, a kezdeti tartománynevet, például "contoso.onmicrosoft.com," esetén is az elsődleges tartomány nevét a címtáron.</span><span class="sxs-lookup"><span data-stu-id="1a722-113">When your directory is created, the initial domain name, such as ‘contoso.onmicrosoft.com,’ is also the primary domain name for your directory.</span></span> <span data-ttu-id="1a722-114">Az elsődleges tartomány esetén egy új felhasználó számára az alapértelmezett tartomány nevét az új felhasználó létrehozása a [a klasszikus Azure portálon](https://manage.windowsazure.com/), vagy más portálokon, például az Office 365 felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="1a722-114">The primary domain is the default domain name for a new user when you create a new user in the [Azure classic portal](https://manage.windowsazure.com/), or other portals such as the Office 365 admin portal.</span></span> <span data-ttu-id="1a722-115">Ez leegyszerűsíti és felgyorsítja a rendszergazda számára hozzon létre új felhasználók a portálon.</span><span class="sxs-lookup"><span data-stu-id="1a722-115">This streamlines the process for an administrator to create new users in the portal.</span></span>

<span data-ttu-id="1a722-116">Az elsődleges tartomány nevét a címtáron módosítása:</span><span class="sxs-lookup"><span data-stu-id="1a722-116">To change the primary domain name for your directory:</span></span>

1. <span data-ttu-id="1a722-117">Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com/) az Azure AD-címtár globális rendszergazdájának felhasználói fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1a722-117">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with a user account that is a global administrator of your Azure AD directory.</span></span>
2. <span data-ttu-id="1a722-118">Válassza ki **Active Directory** a bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="1a722-118">Select **Active Directory** on the left navigation bar.</span></span>
3. <span data-ttu-id="1a722-119">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="1a722-119">Open your directory.</span></span>
4. <span data-ttu-id="1a722-120">Válassza ki a **tartományok** fülre.</span><span class="sxs-lookup"><span data-stu-id="1a722-120">Select the **Domains** tab.</span></span>
5. <span data-ttu-id="1a722-121">Válassza ki a **módosítsa elsődleges** gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="1a722-121">Select the **Change primary** button on the command bar.</span></span>
6. <span data-ttu-id="1a722-122">Válassza ki a tartományt, amelyet az új elsődleges tartományt a címtáron.</span><span class="sxs-lookup"><span data-stu-id="1a722-122">Select the domain that you want to be the new primary domain for your directory.</span></span>

<span data-ttu-id="1a722-123">Módosíthatja a címtáron bármely, a ellenőrzött egyéni tartományt is, hogy az nem összevont elsődleges tartományneve.</span><span class="sxs-lookup"><span data-stu-id="1a722-123">You can change the primary domain name for your directory to be any verified custom domain that is not federated.</span></span> <span data-ttu-id="1a722-124">Az elsődleges tartomány a címtáron módosítása nem módosítja a felhasználónevek, a meglévő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="1a722-124">Changing the primary domain for your directory will not change the user names for any existing users.</span></span>

## <a name="add-custom-domain-names-to-your-azure-ad"></a><span data-ttu-id="1a722-125">Egyéni tartománynevek hozzáadása az Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a722-125">Add custom domain names to your Azure AD</span></span>
<span data-ttu-id="1a722-126">Minden Azure AD-címtár legfeljebb 900 egyéni tartományneveket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="1a722-126">You can add up to 900 custom domain names to each Azure AD directory.</span></span> <span data-ttu-id="1a722-127">A folyamat [további egyéni tartománynév hozzáadása](active-directory-add-domain.md) megegyezik az első egyéni tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="1a722-127">The process to [add an additional custom domain name](active-directory-add-domain.md) is the same for the first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="1a722-128">Altartományok az egyéni tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a722-128">Add subdomains of a custom domain</span></span>
<span data-ttu-id="1a722-129">Ha szeretne egy harmadik szintű tartomány neve, pl. "europe.contoso.com" adni a könyvtárhoz, először adja hozzá, és ellenőrizze a második szintű tartomány, például contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1a722-129">If you want to add a third-level domain name such as ‘europe.contoso.com’ to your directory, you should first add and verify the second-level domain, such as contoso.com.</span></span> <span data-ttu-id="1a722-130">Az altartomány Azure AD által automatikusan fog ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="1a722-130">The subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="1a722-131">Jelenik meg, hogy a most felvett altartomány ellenőrzése megtörtént, frissítse a lapot a böngészőben, amely felsorolja azokat a tartományokat a címtárban.</span><span class="sxs-lookup"><span data-stu-id="1a722-131">To see that the subdomain that you just added has been verified, refresh the page in the browser that lists the domains in your directory.</span></span>

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="1a722-132">Mi a teendő, ha a DNS-regisztráló webhelyén módosítja az egyéni tartománynevet</span><span class="sxs-lookup"><span data-stu-id="1a722-132">What to do if you change the DNS registrar for your custom domain name</span></span>
<span data-ttu-id="1a722-133">Ha módosítja a DNS-regisztráló webhelyén az egyéni tartománynevet, továbbra is az egyéni tartománynevet használja az Azure AD maga megszakítás nélkül, és további konfigurációs feladatok nélkül.</span><span class="sxs-lookup"><span data-stu-id="1a722-133">If you change the DNS registrar for your custom domain name, you can continue to use your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="1a722-134">Az egyéni tartománynevet és az Office 365 használatakor, Intune, illetve egyéb szolgáltatások egyéni tartománynevekkel az Azure ad-ben, tekintse meg a szolgáltatások dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="1a722-134">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer to the documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="1a722-135">Egyéni tartománynév törlése</span><span class="sxs-lookup"><span data-stu-id="1a722-135">Delete a custom domain name</span></span>
<span data-ttu-id="1a722-136">Egy egyéni tartománynevet törölheti az Azure AD-ből, ha a szervezet már nem használja ezt a nevet, vagy ha ezt a nevet használja egy másik Azure AD-val kell.</span><span class="sxs-lookup"><span data-stu-id="1a722-136">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need to use that domain name with another Azure AD.</span></span>

<span data-ttu-id="1a722-137">Törölni egy egyéni tartománynevet, akkor előbb ellenőrizze, hogy a címtárban nincsenek erőforrások támaszkodjon a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="1a722-137">To delete a custom domain name, you must first ensure that no resources in your directory rely on the domain name.</span></span> <span data-ttu-id="1a722-138">A címtárban lévő tartománynév nem törölhető, ha:</span><span class="sxs-lookup"><span data-stu-id="1a722-138">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="1a722-139">Minden olyan felhasználó, egy felhasználónevet, e-mail címét vagy a tartománynevet tartalmazó proxycímmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1a722-139">Any user has a user name, email address, or proxy address that include the domain name.</span></span>
* <span data-ttu-id="1a722-140">Bármely csoport rendelkezik egy e-mail címet, vagy a proxykiszolgáló címét, amely a tartomány nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1a722-140">Any group has an email address or proxy address that includes the domain name.</span></span>
* <span data-ttu-id="1a722-141">Az Azure AD bármely alkalmazásnak egy app ID URI, amely a tartomány nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1a722-141">Any application in your Azure AD has an app ID URI that includes the domain name.</span></span>

<span data-ttu-id="1a722-142">Módosít, vagy minden ilyen erőforrás törlése az Azure AD-címtárát, az egyéni tartománynév törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="1a722-142">You must change or delete any such resource in your Azure AD directory before you can delete the custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a><span data-ttu-id="1a722-143">Használja a Powershellt vagy a Graph API tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="1a722-143">Use PowerShell or Graph API to manage domain names</span></span>
<span data-ttu-id="1a722-144">A legtöbb kezelési feladatot az Azure Active Directoryban tartománynevek is elvégezhető, Microsoft PowerShell használatával, vagy az Azure AD Graph API-val programozott módon.</span><span class="sxs-lookup"><span data-stu-id="1a722-144">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="1a722-145">Tartománynevek kezelése az Azure AD PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="1a722-145">Using PowerShell to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="1a722-146">Tartománynevek kezelése az Azure AD Graph API segítségével</span><span class="sxs-lookup"><span data-stu-id="1a722-146">Using Graph API to manage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="1a722-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a722-147">Next steps</span></span>
* [<span data-ttu-id="1a722-148">Tartománynevek megismerése az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="1a722-148">Learn about domain names in Azure AD</span></span>](active-directory-add-domain-concepts.md)
* [<span data-ttu-id="1a722-149">Egyéni tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="1a722-149">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)

