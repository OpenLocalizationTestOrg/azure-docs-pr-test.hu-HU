---
title: "aaaOffice 365 külső megosztás és az Azure Active Directory B2B együttműködés |} Microsoft Docs"
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
ms.openlocfilehash: 60452b27b328453eda729bd839c982b479cb6f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="8cd99-103">Külső megosztás Office 365 és Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="8cd99-103">Office 365 external sharing and Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="8cd99-104">Az Office 365 (OneDrive, SharePoint online-hoz, egységes csoportok stb.), és Azure Active Directory (Azure AD) B2B együttműködés technikailag megosztása külső hello ugyanazt jelentik.</span><span class="sxs-lookup"><span data-stu-id="8cd99-104">External sharing in Office 365 (OneDrive, SharePoint Online, Unified Groups, etc.) and Azure Active Directory (Azure AD) B2B collaboration are technically hello same thing.</span></span> <span data-ttu-id="8cd99-105">Minden külső megosztás (kivéve a onedrive-on vagy a SharePoint Online), beleértve a vendégek Office 365-csoportokat, már használja hello Azure AD B2B együttműködés meghívó API-k megosztása.</span><span class="sxs-lookup"><span data-stu-id="8cd99-105">All external sharing (except OneDrive/SharePoint Online), including guests in Office 365 Groups, already uses hello Azure AD B2B collaboration invitation APIs for sharing.</span></span>

<span data-ttu-id="8cd99-106">A OneDrive vagy a SharePoint Online rendelkezik egy külön meghívó manager.</span><span class="sxs-lookup"><span data-stu-id="8cd99-106">OneDrive/SharePoint Online has a separate invitation manager.</span></span> <span data-ttu-id="8cd99-107">A onedrive-on vagy a SharePoint Online elindítása előtt az Azure AD fejlesztett támogatását külső megosztás támogatása.</span><span class="sxs-lookup"><span data-stu-id="8cd99-107">Support for external sharing in OneDrive/SharePoint Online started before Azure AD developed its support.</span></span> <span data-ttu-id="8cd99-108">Idővel OneDrive vagy a SharePoint Online külső megosztás esedékes számos szolgáltatást és hello termék használó felhasználók sok több millió tartozó a-épül megosztása mintát.</span><span class="sxs-lookup"><span data-stu-id="8cd99-108">Over time, OneDrive/SharePoint Online external sharing has accrued several features and many millions of users who use hello product's in-built sharing pattern.</span></span> <span data-ttu-id="8cd99-109">Van azonban néhány finom eltérések vannak a onedrive-on vagy a SharePoint Online külső megosztás működése, és az Azure AD B2B együttműködés működéséről között:</span><span class="sxs-lookup"><span data-stu-id="8cd99-109">However, there are some subtle differences between how OneDrive/SharePoint Online external sharing works and how Azure AD B2B collaboration works:</span></span>

- <span data-ttu-id="8cd99-110">A OneDrive vagy a SharePoint Online felhasználók toohello könyvtárat ad után a felhasználók rendelkeznek sikerült beváltani a meghívást.</span><span class="sxs-lookup"><span data-stu-id="8cd99-110">OneDrive/SharePoint Online adds users toohello directory after users have redeemed their invitations.</span></span> <span data-ttu-id="8cd99-111">Igen előtt érvényesítési, nem látható hello felhasználói Azure AD portálon.</span><span class="sxs-lookup"><span data-stu-id="8cd99-111">So, before redemption, you don't see hello user in Azure AD portal.</span></span> <span data-ttu-id="8cd99-112">Egy másik hely addig felkéri hello felhasználóként, egy új meghívó jön létre.</span><span class="sxs-lookup"><span data-stu-id="8cd99-112">If another site invites a user in hello meantime, a new invitation is generated.</span></span> <span data-ttu-id="8cd99-113">Azonban Azure AD B2B együttműködés használata esetén felhasználót adnak hozzá közvetlenül a meghívót, hogy azok megjelenni everywhere.</span><span class="sxs-lookup"><span data-stu-id="8cd99-113">However, when you use Azure AD B2B collaboration, users are added immediately on invitation so that they show up everywhere.</span></span>

- <span data-ttu-id="8cd99-114">hello érvényesítési élmény a onedrive-on vagy a SharePoint Online az Azure AD B2B együttműködés hello élmény fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="8cd99-114">hello redemption experience in OneDrive/SharePoint Online looks different from hello experience in Azure AD B2B collaboration.</span></span> <span data-ttu-id="8cd99-115">Miután egy felhasználó visszaváltja meghívót, hello lép egyformának.</span><span class="sxs-lookup"><span data-stu-id="8cd99-115">After a user redeems an invitation, hello experiences look alike.</span></span>

- <span data-ttu-id="8cd99-116">Az Azure AD B2B együttműködés meghívott felhasználók is tárolható a onedrive-on vagy a SharePoint Online-ból párbeszédpanelek.</span><span class="sxs-lookup"><span data-stu-id="8cd99-116">Azure AD B2B collaboration invited users can be picked from OneDrive/SharePoint Online sharing dialog boxes.</span></span> <span data-ttu-id="8cd99-117">OneDrive vagy a SharePoint Online meghívott felhasználók is jelenik meg az Azure AD után azok beváltani a meghívást.</span><span class="sxs-lookup"><span data-stu-id="8cd99-117">OneDrive/SharePoint Online invited users also show up in Azure AD after they redeem their invitations.</span></span>

- <span data-ttu-id="8cd99-118">külső toomanage megosztása a onedrive-on vagy a SharePoint Online az Azure AD B2B együttműködés beállítása OneDrive vagy a SharePoint Online hello külső megosztási beállítás túl**csak a hello könyvtárban már külső felhasználókkal való megosztásának engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="8cd99-118">toomanage external sharing in OneDrive/SharePoint Online with Azure AD B2B collaboration, set hello OneDrive/SharePoint Online external sharing setting too**Only allow sharing with external users already in hello directory**.</span></span> <span data-ttu-id="8cd99-119">Felhasználók folytathatja tooexternally megosztott hely, és mentse a külső együttműködők, amely rendszergazdai hello hozzá van adva.</span><span class="sxs-lookup"><span data-stu-id="8cd99-119">Users can go tooexternally shared sites and pick from external collaborators that hello admin has added.</span></span> <span data-ttu-id="8cd99-120">Üdvözöljük a rendszergazdákat is hozzáadhat hello külső együttműködők hello B2B együttműködés meghívó API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="8cd99-120">hello admin can add hello external collaborators through hello B2B collaboration invitation APIs.</span></span>

![hello OneDrive vagy a SharePoint Online külső megosztásának beállítása](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a><span data-ttu-id="8cd99-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8cd99-122">Next steps</span></span>

<span data-ttu-id="8cd99-123">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="8cd99-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="8cd99-124">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="8cd99-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="8cd99-125">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8cd99-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="8cd99-126">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8cd99-126">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="8cd99-127">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="8cd99-127">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="8cd99-128">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="8cd99-128">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="8cd99-129">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="8cd99-129">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="8cd99-130">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8cd99-130">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="8cd99-131">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="8cd99-131">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="8cd99-132">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="8cd99-132">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="8cd99-133">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="8cd99-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
