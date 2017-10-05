---
title: "Külső megosztás Office 365 és Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "a jogcímek referencia az Azure Active Directory B2B együttműködés leképezése"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: cad0ce8f745f3d6ca14436fd714b08c60de0e459
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="e2179-103">Külső megosztás Office 365 és Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="e2179-103">Office 365 external sharing and Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="e2179-104">Az Office 365 (OneDrive, SharePoint online-hoz, egységes csoportok stb.) és az Azure Active Directory (Azure AD) B2B együttműködés megosztása külső rendszer technikailag az ugyanaz.</span><span class="sxs-lookup"><span data-stu-id="e2179-104">External sharing in Office 365 (OneDrive, SharePoint Online, Unified Groups, etc.) and Azure Active Directory (Azure AD) B2B collaboration are technically the same thing.</span></span> <span data-ttu-id="e2179-105">Minden külső megosztás (kivéve a onedrive-on vagy a SharePoint Online), többek között a vendégek az Office 365-csoportok, már használja az Azure AD B2B együttműködés meghívó API-k a megosztásához.</span><span class="sxs-lookup"><span data-stu-id="e2179-105">All external sharing (except OneDrive/SharePoint Online), including guests in Office 365 Groups, already uses the Azure AD B2B collaboration invitation APIs for sharing.</span></span>

<span data-ttu-id="e2179-106">A OneDrive vagy a SharePoint Online rendelkezik egy külön meghívó manager.</span><span class="sxs-lookup"><span data-stu-id="e2179-106">OneDrive/SharePoint Online has a separate invitation manager.</span></span> <span data-ttu-id="e2179-107">A onedrive-on vagy a SharePoint Online elindítása előtt az Azure AD fejlesztett támogatását külső megosztás támogatása.</span><span class="sxs-lookup"><span data-stu-id="e2179-107">Support for external sharing in OneDrive/SharePoint Online started before Azure AD developed its support.</span></span> <span data-ttu-id="e2179-108">Idővel OneDrive vagy a SharePoint Online külső megosztás esedékes számos szolgáltatást és a termék használó felhasználók sok több millió tartozó a-épül megosztása mintát.</span><span class="sxs-lookup"><span data-stu-id="e2179-108">Over time, OneDrive/SharePoint Online external sharing has accrued several features and many millions of users who use the product's in-built sharing pattern.</span></span> <span data-ttu-id="e2179-109">Van azonban néhány finom eltérések vannak a onedrive-on vagy a SharePoint Online külső megosztás működése, és az Azure AD B2B együttműködés működéséről között:</span><span class="sxs-lookup"><span data-stu-id="e2179-109">However, there are some subtle differences between how OneDrive/SharePoint Online external sharing works and how Azure AD B2B collaboration works:</span></span>

- <span data-ttu-id="e2179-110">A OneDrive vagy a SharePoint Online hozzáadja a felhasználókat a könyvtár után a felhasználók rendelkeznek sikerült beváltani a meghívást.</span><span class="sxs-lookup"><span data-stu-id="e2179-110">OneDrive/SharePoint Online adds users to the directory after users have redeemed their invitations.</span></span> <span data-ttu-id="e2179-111">Igen előtt érvényesítési, nem látható a felhasználó Azure AD portálon.</span><span class="sxs-lookup"><span data-stu-id="e2179-111">So, before redemption, you don't see the user in Azure AD portal.</span></span> <span data-ttu-id="e2179-112">Ha egy másik hely időközben felkéri a felhasználó, egy új meghívó jön létre.</span><span class="sxs-lookup"><span data-stu-id="e2179-112">If another site invites a user in the meantime, a new invitation is generated.</span></span> <span data-ttu-id="e2179-113">Azonban Azure AD B2B együttműködés használata esetén felhasználót adnak hozzá közvetlenül a meghívót, hogy azok megjelenni everywhere.</span><span class="sxs-lookup"><span data-stu-id="e2179-113">However, when you use Azure AD B2B collaboration, users are added immediately on invitation so that they show up everywhere.</span></span>

- <span data-ttu-id="e2179-114">Az érvényesítési élmény, a onedrive-on vagy a SharePoint Online az Azure AD B2B együttműködés élményt fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="e2179-114">The redemption experience in OneDrive/SharePoint Online looks different from the experience in Azure AD B2B collaboration.</span></span> <span data-ttu-id="e2179-115">Miután egy felhasználó visszaváltja meghívót, az feladatait egyformának.</span><span class="sxs-lookup"><span data-stu-id="e2179-115">After a user redeems an invitation, the experiences look alike.</span></span>

- <span data-ttu-id="e2179-116">Az Azure AD B2B együttműködés meghívott felhasználók is tárolható a onedrive-on vagy a SharePoint Online-ból párbeszédpanelek.</span><span class="sxs-lookup"><span data-stu-id="e2179-116">Azure AD B2B collaboration invited users can be picked from OneDrive/SharePoint Online sharing dialog boxes.</span></span> <span data-ttu-id="e2179-117">OneDrive vagy a SharePoint Online meghívott felhasználók is jelenik meg az Azure AD után azok beváltani a meghívást.</span><span class="sxs-lookup"><span data-stu-id="e2179-117">OneDrive/SharePoint Online invited users also show up in Azure AD after they redeem their invitations.</span></span>

- <span data-ttu-id="e2179-118">Külső megosztás kezelése a onedrive-on vagy a SharePoint Online az Azure AD B2B együttműködés, állítsa be a onedrive-on vagy a SharePoint Online külső megosztási beállítással **csak a könyvtárban már külső felhasználókkal való megosztásának engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="e2179-118">To manage external sharing in OneDrive/SharePoint Online with Azure AD B2B collaboration, set the OneDrive/SharePoint Online external sharing setting to **Only allow sharing with external users already in the directory**.</span></span> <span data-ttu-id="e2179-119">Felhasználók ugorjon a külsőleg megosztott hely, és válassza ki a külső együttműködők, hogy a rendszergazda hozzáad.</span><span class="sxs-lookup"><span data-stu-id="e2179-119">Users can go to externally shared sites and pick from external collaborators that the admin has added.</span></span> <span data-ttu-id="e2179-120">A rendszergazda adhat hozzá a külső együttműködők a B2B együttműködés meghívót API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="e2179-120">The admin can add the external collaborators through the B2B collaboration invitation APIs.</span></span>

![A onedrive-on vagy a SharePoint Online külső megosztásának beállítása](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a><span data-ttu-id="e2179-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2179-122">Next steps</span></span>

<span data-ttu-id="e2179-123">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="e2179-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="e2179-124">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="e2179-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="e2179-125">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e2179-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="e2179-126">Egy szerepkör B2B együttműködés felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2179-126">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="e2179-127">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="e2179-127">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="e2179-128">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="e2179-128">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="e2179-129">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="e2179-129">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="e2179-130">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e2179-130">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="e2179-131">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="e2179-131">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="e2179-132">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="e2179-132">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="e2179-133">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="e2179-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
