---
title: "aaaB2B együttműködés a felhasználói jogcímek hozzárendelése az Azure Active Directoryban |} Microsoft Docs"
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
ms.openlocfilehash: 9e26085e91a6004b2f11286ae9c1df133bd47341
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a><span data-ttu-id="862f6-103">B2B együttműködés a felhasználói jogcímek hozzárendelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="862f6-103">B2B collaboration user claims mapping in Azure Active Directory</span></span>

<span data-ttu-id="862f6-104">Az Azure Active Directory (Azure AD) támogatja az SAML-jogkivonat hello B2B együttműködés felhasználók számára kiállított hello jogcímek testreszabása.</span><span class="sxs-lookup"><span data-stu-id="862f6-104">Azure Active Directory (Azure AD) supports customizing hello claims issued in hello SAML token for B2B collaboration users.</span></span> <span data-ttu-id="862f6-105">Amikor a felhasználók hitelesítése toohello alkalmazás, az Azure AD kibocsát egy SAML token toohello alkalmazást, amely tartalmazza az adatokat (vagy a jogcímek), amely egyedileg azonosítja hello felhasználóról.</span><span class="sxs-lookup"><span data-stu-id="862f6-105">When a user authenticates toohello application, Azure AD issues a SAML token toohello app that contains information (or claims) about hello user that uniquely identifies them.</span></span> <span data-ttu-id="862f6-106">Alapértelmezés szerint ez magában foglalja a hello felhasználói felhasználó nevét, e-mail címét, Utónév és Vezetéknév.</span><span class="sxs-lookup"><span data-stu-id="862f6-106">By default, this includes hello user's user name, email address, first name, and last name.</span></span> <span data-ttu-id="862f6-107">Megtekintheti vagy hello hello attribútumok lap SAML-jogkivonat toohello alkalmazást küldi hello jogcímek szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="862f6-107">You can view or edit hello claims sent in hello SAML token toohello application under hello Attributes tab.</span></span>

<span data-ttu-id="862f6-108">Ennek oka két miért szükség lehet a hello SAML-jogkivonat kiadott tooedit hello jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="862f6-108">There are two possible reasons why you might need tooedit hello claims issued in hello SAML token.</span></span>

1. <span data-ttu-id="862f6-109">hello alkalmazás írása egy másik jogcím URI-azonosítók beállítása vagy jogcímértékek toorequire</span><span class="sxs-lookup"><span data-stu-id="862f6-109">hello application has been written toorequire a different set of claim URIs or claim values</span></span>

2. <span data-ttu-id="862f6-110">Az alkalmazás által igényelt hello NameIdentifier jogcím toobe nem hello egyszerű felhasználónév az Azure Active Directoryban tárolja.</span><span class="sxs-lookup"><span data-stu-id="862f6-110">Your application requires hello NameIdentifier claim toobe something other than hello user principal name stored in Azure Active Directory.</span></span>

  ![nézet jogcímek SAML-jogkivonat](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

<span data-ttu-id="862f6-112">Hogyan tooadd és Szerkesztés jogcímek információkért tekintse meg a jogcímek testreszabása, ez a cikk [hello SAML-jogkivonat előzetesen beépített alkalmazások az Azure Active Directoryban a kiállított jogcímek testreszabása](develop/active-directory-saml-claims-customization.md).</span><span class="sxs-lookup"><span data-stu-id="862f6-112">For information on how tooadd and edit claims, check out this article on claims customization, [Customizing claims issued in hello SAML token for pre-integrated apps in Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span></span> <span data-ttu-id="862f6-113">A B2B együttműködés a felhasználók, NameID és UPN kereszt-bérlő leképezési biztonsági okokból nem.</span><span class="sxs-lookup"><span data-stu-id="862f6-113">For B2B collaboration users, mapping NameID and UPN cross-tenant are prevented for security reasons.</span></span>


## <a name="next-steps"></a><span data-ttu-id="862f6-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="862f6-114">Next steps</span></span>

<span data-ttu-id="862f6-115">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="862f6-115">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="862f6-116">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="862f6-116">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="862f6-117">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="862f6-117">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="862f6-118">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="862f6-118">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="862f6-119">B2bB együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="862f6-119">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="862f6-120">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="862f6-120">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="862f6-121">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="862f6-121">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="862f6-122">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="862f6-122">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="862f6-123">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="862f6-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="862f6-124">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="862f6-124">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="862f6-125">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="862f6-125">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
