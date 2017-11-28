---
title: "aaaManaging egyéni tartománynevek az Azure Active Directoryban |} Microsoft Docs"
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
ms.openlocfilehash: 4b6d06fecf3be0621be51c38a1330eafdc1b4d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a><span data-ttu-id="a9ea7-103">Az Azure Active Directoryban egyéni tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="a9ea7-103">Managing custom domain names in your Azure Active Directory</span></span>
<span data-ttu-id="a9ea7-104">A tartomány neve lehet sok címtárerőforrásokkal fontos azonosítója részeként:</span><span class="sxs-lookup"><span data-stu-id="a9ea7-104">A domain name can be an important identifier for many directory resources, as part of:</span></span>

* <span data-ttu-id="a9ea7-105">A felhasználói név vagy e-mail cím egy felhasználó számára</span><span class="sxs-lookup"><span data-stu-id="a9ea7-105">A user name or email address for a user</span></span>
* <span data-ttu-id="a9ea7-106">a csoport hello cím</span><span class="sxs-lookup"><span data-stu-id="a9ea7-106">hello address for a group</span></span>
* <span data-ttu-id="a9ea7-107">hello app ID URI az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a9ea7-107">hello app ID URI for an application</span></span>

<span data-ttu-id="a9ea7-108">Az Azure Active Directory (Azure AD) erőforrás tartalmazhatnak toobe tulajdonában hello directory hello erőforrást tartalmaz, amely már ellenőrizve tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-108">A resource in Azure Active Directory (Azure AD) can include a domain name that is already verified toobe owned by hello directory that contains hello resource.</span></span> <span data-ttu-id="a9ea7-109">Csak egy globális rendszergazda Azure AD-ben végrehajthat tartomány feladatokat.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-109">Only a global administrator can perform domain management tasks in Azure AD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9ea7-110">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-110">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="a9ea7-111">Hogyan toomanage a tartománynevek hello Azure AD felügyeleti központban, lásd: [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a9ea7-111">For how toomanage your domain names in hello Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a><span data-ttu-id="a9ea7-112">Állítsa be az Azure AD-címtár hello elsődleges tartományi neve</span><span class="sxs-lookup"><span data-stu-id="a9ea7-112">Set hello primary domain name for your Azure AD directory</span></span>
<span data-ttu-id="a9ea7-113">A könyvtár jön létre, hello kezdeti tartománynevet, például "contoso.onmicrosoft.com," esetén is hello elsődleges tartománynevet a címtáron.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-113">When your directory is created, hello initial domain name, such as ‘contoso.onmicrosoft.com,’ is also hello primary domain name for your directory.</span></span> <span data-ttu-id="a9ea7-114">hello elsődleges tartománya hello alapértelmezett tartomány nevét az új felhasználó új felhasználó létrehozásakor a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), vagy más portálokon például hello Office 365 felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-114">hello primary domain is hello default domain name for a new user when you create a new user in hello [Azure classic portal](https://manage.windowsazure.com/), or other portals such as hello Office 365 admin portal.</span></span> <span data-ttu-id="a9ea7-115">Ez leegyszerűsíti a hello folyamat egy rendszergazda toocreate új felhasználók hello portálon.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-115">This streamlines hello process for an administrator toocreate new users in hello portal.</span></span>

<span data-ttu-id="a9ea7-116">toochange hello elsődleges tartományt a könyvtár nevét:</span><span class="sxs-lookup"><span data-stu-id="a9ea7-116">toochange hello primary domain name for your directory:</span></span>

1. <span data-ttu-id="a9ea7-117">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) , amely az Azure AD-címtár globális rendszergazdájának felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-117">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with a user account that is a global administrator of your Azure AD directory.</span></span>
2. <span data-ttu-id="a9ea7-118">Válassza ki **Active Directory** hello bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-118">Select **Active Directory** on hello left navigation bar.</span></span>
3. <span data-ttu-id="a9ea7-119">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-119">Open your directory.</span></span>
4. <span data-ttu-id="a9ea7-120">Jelölje be hello **tartományok** fülre.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-120">Select hello **Domains** tab.</span></span>
5. <span data-ttu-id="a9ea7-121">Jelölje be hello **módosítsa elsődleges** hello parancssáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-121">Select hello **Change primary** button on hello command bar.</span></span>
6. <span data-ttu-id="a9ea7-122">Válassza ki a megjeleníteni kívánt toobe hello új elsődleges tartományt a címtáron hello tartományt.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-122">Select hello domain that you want toobe hello new primary domain for your directory.</span></span>

<span data-ttu-id="a9ea7-123">Elsődleges tartománynév hello a directory toobe módosíthatja bármely ellenőrzött egyéni tartomány, amely nincs összevonva.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-123">You can change hello primary domain name for your directory toobe any verified custom domain that is not federated.</span></span> <span data-ttu-id="a9ea7-124">Változó hello elsődleges tartományt a könyvtár nem változik meg a hello felhasználónevek bármely létező felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-124">Changing hello primary domain for your directory will not change hello user names for any existing users.</span></span>

## <a name="add-custom-domain-names-tooyour-azure-ad"></a><span data-ttu-id="a9ea7-125">Az egyéni tartomány nevét tooyour az Azure AD hozzá</span><span class="sxs-lookup"><span data-stu-id="a9ea7-125">Add custom domain names tooyour Azure AD</span></span>
<span data-ttu-id="a9ea7-126">Másolatot too900 egyéni tartomány nevét tooeach Azure AD-címtár is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-126">You can add up too900 custom domain names tooeach Azure AD directory.</span></span> <span data-ttu-id="a9ea7-127">folyamat túl hello[további egyéni tartománynév hozzáadása](active-directory-add-domain.md) van hello azonos hello első egyéni tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-127">hello process too[add an additional custom domain name](active-directory-add-domain.md) is hello same for hello first custom domain name.</span></span>

## <a name="add-subdomains-of-a-custom-domain"></a><span data-ttu-id="a9ea7-128">Altartományok az egyéni tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9ea7-128">Add subdomains of a custom domain</span></span>
<span data-ttu-id="a9ea7-129">Ha azt szeretné, hogy egy harmadik szintű tartomány nevét, például a "europe.contoso.com" tooyour directory tooadd, először adja hozzá, és ellenőrizze hello második szintű tartomány, például contoso.com. hello altartomány Azure AD által automatikusan fog ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-129">If you want tooadd a third-level domain name such as ‘europe.contoso.com’ tooyour directory, you should first add and verify hello second-level domain, such as contoso.com. hello subdomain will be automatically verified by Azure AD.</span></span> <span data-ttu-id="a9ea7-130">ellenőrizte, hogy a most felvett altartomány hello toosee, frissítési hello lap, amely tartalmazza a könyvtárban hello tartományok hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-130">toosee that hello subdomain that you just added has been verified, refresh hello page in hello browser that lists hello domains in your directory.</span></span>

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a><span data-ttu-id="a9ea7-131">Milyen toodo, ha módosítja az egyéni tartománynevet hello DNS-regisztráló webhelyén</span><span class="sxs-lookup"><span data-stu-id="a9ea7-131">What toodo if you change hello DNS registrar for your custom domain name</span></span>
<span data-ttu-id="a9ea7-132">Ha módosítja az egyéni tartománynevet hello DNS-regisztráló webhelyén, folytathatja az egyéni tartománynevet, és az Azure AD maga megszakítás nélkül, és további konfigurációs feladatok nélkül toouse.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-132">If you change hello DNS registrar for your custom domain name, you can continue toouse your custom domain name with Azure AD itself without interruption and without additional configuration tasks.</span></span> <span data-ttu-id="a9ea7-133">Az egyéni tartománynevet és az Office 365 használatakor, Intune, illetve egyéb szolgáltatások egyéni tartománynevekkel az Azure ad-ben, tekintse meg a toohello dokumentációját szolgáltatások esetében.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-133">If you use your custom domain name with Office 365, Intune, or other services that rely on custom domain names in Azure AD, refer toohello documentation for those services.</span></span>

## <a name="delete-a-custom-domain-name"></a><span data-ttu-id="a9ea7-134">Egyéni tartománynév törlése</span><span class="sxs-lookup"><span data-stu-id="a9ea7-134">Delete a custom domain name</span></span>
<span data-ttu-id="a9ea7-135">Egy egyéni tartománynevet törölheti az Azure AD-ből, ha a szervezet már nem használja ezt a nevet, vagy ha egy másik Azure AD-val a tartománynevet kell toouse.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-135">You can delete a custom domain name from your Azure AD if your organization no longer uses that domain name, or if you need toouse that domain name with another Azure AD.</span></span>

<span data-ttu-id="a9ea7-136">egy egyéni tartománynevet toodelete, akkor előbb ellenőrizze, hogy a címtárban nincsenek erőforrások támaszkodnak hello tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-136">toodelete a custom domain name, you must first ensure that no resources in your directory rely on hello domain name.</span></span> <span data-ttu-id="a9ea7-137">A címtárban lévő tartománynév nem törölhető, ha:</span><span class="sxs-lookup"><span data-stu-id="a9ea7-137">You can't delete a domain name from your directory if:</span></span>

* <span data-ttu-id="a9ea7-138">Minden olyan felhasználó, egy felhasználónevet, e-mail címét vagy hello tartománynevet tartalmazó proxycímmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-138">Any user has a user name, email address, or proxy address that include hello domain name.</span></span>
* <span data-ttu-id="a9ea7-139">Bármely csoport rendelkezik egy e-mail címet, vagy a proxykiszolgáló címét, amely tartalmazza a hello tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-139">Any group has an email address or proxy address that includes hello domain name.</span></span>
* <span data-ttu-id="a9ea7-140">Az Azure AD bármely alkalmazásnak egy app ID URI, amely tartalmazza a hello tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-140">Any application in your Azure AD has an app ID URI that includes hello domain name.</span></span>

<span data-ttu-id="a9ea7-141">Módosít, vagy minden ilyen erőforrás törlése az Azure AD-címtárát hello egyéni tartománynév törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-141">You must change or delete any such resource in your Azure AD directory before you can delete hello custom domain name.</span></span>

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a><span data-ttu-id="a9ea7-142">PowerShell vagy a Graph API toomanage tartománynevek</span><span class="sxs-lookup"><span data-stu-id="a9ea7-142">Use PowerShell or Graph API toomanage domain names</span></span>
<span data-ttu-id="a9ea7-143">A legtöbb kezelési feladatot az Azure Active Directoryban tartománynevek is elvégezhető, Microsoft PowerShell használatával, vagy az Azure AD Graph API-val programozott módon.</span><span class="sxs-lookup"><span data-stu-id="a9ea7-143">Most management tasks for domain names in Azure Active Directory can also be completed using Microsoft PowerShell, or programmatically using Azure AD Graph API.</span></span>

* [<span data-ttu-id="a9ea7-144">Az Azure AD PowerShell toomanage tartománynevek használatával</span><span class="sxs-lookup"><span data-stu-id="a9ea7-144">Using PowerShell toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [<span data-ttu-id="a9ea7-145">Az Azure AD Graph API toomanage tartománynevek használatával</span><span class="sxs-lookup"><span data-stu-id="a9ea7-145">Using Graph API toomanage domain names in Azure AD</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a><span data-ttu-id="a9ea7-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9ea7-146">Next steps</span></span>
* [<span data-ttu-id="a9ea7-147">Tartománynevek megismerése az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="a9ea7-147">Learn about domain names in Azure AD</span></span>](active-directory-add-domain-concepts.md)
* [<span data-ttu-id="a9ea7-148">Egyéni tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="a9ea7-148">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)

