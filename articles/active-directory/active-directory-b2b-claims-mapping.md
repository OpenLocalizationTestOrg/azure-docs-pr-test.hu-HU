---
title: "B2B együttműködés a felhasználói jogcímek hozzárendelése az Azure Active Directoryban |} Microsoft Docs"
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 5f8559450b24effd40a38879aeae3a8dd03944a3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a><span data-ttu-id="0a17d-103">B2B együttműködés a felhasználói jogcímek hozzárendelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="0a17d-103">B2B collaboration user claims mapping in Azure Active Directory</span></span>

<span data-ttu-id="0a17d-104">Az Azure Active Directory (Azure AD) támogatja a SAML-jogkivonat B2B együttműködés felhasználók számára kiállított jogcímek testreszabása.</span><span class="sxs-lookup"><span data-stu-id="0a17d-104">Azure Active Directory (Azure AD) supports customizing the claims issued in the SAML token for B2B collaboration users.</span></span> <span data-ttu-id="0a17d-105">Amikor egy felhasználó hitelesíti magát az alkalmazást, az Azure AD bocsát ki egy SAML-jogkivonat a az alkalmazás, amely tartalmazza az adatokat (vagy jogcímeket), amely egyedileg azonosítja a felhasználóról.</span><span class="sxs-lookup"><span data-stu-id="0a17d-105">When a user authenticates to the application, Azure AD issues a SAML token to the app that contains information (or claims) about the user that uniquely identifies them.</span></span> <span data-ttu-id="0a17d-106">Alapértelmezés szerint ez tartalmazza a felhasználó felhasználónevét, e-mail címét, Utónév és Vezetéknév.</span><span class="sxs-lookup"><span data-stu-id="0a17d-106">By default, this includes the user's user name, email address, first name, and last name.</span></span> <span data-ttu-id="0a17d-107">Megtekintheti vagy szerkesztheti a jogcímek küldése az alkalmazásnak az attribútumok lapon a SAML-jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="0a17d-107">You can view or edit the claims sent in the SAML token to the application under the Attributes tab.</span></span>

<span data-ttu-id="0a17d-108">Előfordulhat, hogy miért szerkesztése a SAML-jogkivonat kiadott jogcímeket kell két lehetséges oka van.</span><span class="sxs-lookup"><span data-stu-id="0a17d-108">There are two possible reasons why you might need to edit the claims issued in the SAML token.</span></span>

1. <span data-ttu-id="0a17d-109">Az alkalmazás írt eltérő szabályzatkészletet jogcím URI-azonosítók, vagy a jogcímértékek</span><span class="sxs-lookup"><span data-stu-id="0a17d-109">The application has been written to require a different set of claim URIs or claim values</span></span>

2. <span data-ttu-id="0a17d-110">Az alkalmazás által a NameIdentifier jogcímet, mint a felhasználó egyszerű felhasználóneve, az Azure Active Directoryban tárolja.</span><span class="sxs-lookup"><span data-stu-id="0a17d-110">Your application requires the NameIdentifier claim to be something other than the user principal name stored in Azure Active Directory.</span></span>

  ![nézet jogcímek SAML-jogkivonat](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

<span data-ttu-id="0a17d-112">Tájékoztatást adhat hozzá és szerkeszthet a jogcímeket, tekintse meg a jogcímek testreszabása, ez a cikk [előre integrált alkalmazások az Azure Active Directoryban a SAML-jogkivonat kiállított jogcímek testreszabása](develop/active-directory-saml-claims-customization.md).</span><span class="sxs-lookup"><span data-stu-id="0a17d-112">For information on how to add and edit claims, check out this article on claims customization, [Customizing claims issued in the SAML token for pre-integrated apps in Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span></span> <span data-ttu-id="0a17d-113">A B2B együttműködés a felhasználók, NameID és UPN kereszt-bérlő leképezési biztonsági okokból nem.</span><span class="sxs-lookup"><span data-stu-id="0a17d-113">For B2B collaboration users, mapping NameID and UPN cross-tenant are prevented for security reasons.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0a17d-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a17d-114">Next steps</span></span>

<span data-ttu-id="0a17d-115">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="0a17d-115">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="0a17d-116">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="0a17d-116">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="0a17d-117">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="0a17d-117">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="0a17d-118">Egy szerepkör B2B együttműködés felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0a17d-118">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="0a17d-119">B2bB együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="0a17d-119">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="0a17d-120">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="0a17d-120">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="0a17d-121">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="0a17d-121">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="0a17d-122">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a17d-122">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="0a17d-123">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="0a17d-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="0a17d-124">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="0a17d-124">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="0a17d-125">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="0a17d-125">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
