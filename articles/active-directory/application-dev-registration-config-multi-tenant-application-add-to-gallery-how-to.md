---
title: "egy több-bérlős alkalmazás toohello az Azure AD application gallery aaaHow tooadd |} Microsoft Docs"
description: "Azt ismerteti, hogyan listázhatja a hello Azure AD Application Gallery saját fejlesztésű több-bérlős alkalmazásához"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2dc6e0d783835d2639a7e6dda172110ee860a977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-multi-tenant-application-toohello-azure-ad-application-gallery"></a><span data-ttu-id="88bf7-103">Hogyan tooadd egy több-bérlős alkalmazás toohello az Azure AD application gallery</span><span class="sxs-lookup"><span data-stu-id="88bf7-103">How tooadd a multi-tenant application toohello Azure AD application gallery</span></span>

## <a name="what-is-hello-azure-ad-application-gallery"></a><span data-ttu-id="88bf7-104">Mi az Azure AD Application Gallery hello?</span><span class="sxs-lookup"><span data-stu-id="88bf7-104">What is hello Azure AD Application Gallery?</span></span>

<span data-ttu-id="88bf7-105">hello Azure AD Application Gallery egy kiváló módja tooget az alkalmazás Azure Active Directory felhasználók toobroaden hello hatással lehet az összes hello több millió elé, és az alkalmazás hello piactéren elérni.</span><span class="sxs-lookup"><span data-stu-id="88bf7-105">hello Azure AD Application Gallery is a great way tooget your application in front of all hello millions of Azure Active Directory customers toobroaden hello impact and reach of your application in hello marketplace.</span></span> <span data-ttu-id="88bf7-106">hello következő lépések azt ismertetik, hogyan listázhatja hello Azure AD Application Gallery az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="88bf7-106">hello below steps explain how you can list your application in hello Azure AD Application Gallery.</span></span>

## <a name="if-your-application-supports-saml-or-openidconnect"></a><span data-ttu-id="88bf7-107">Ha az alkalmazás támogatja az SAML-alapú vagy OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="88bf7-107">If your application supports SAML or OpenIDConnect</span></span>
<span data-ttu-id="88bf7-108">Ha azt szeretné, hogy az Azure AD Application Gallery hello toolist egy több-bérlős-alkalmazást, először győződjön meg arról, hogy az alkalmazás támogatja a következő egyszeri bejelentkezési technológiák hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="88bf7-108">If you have a multi-tenant application you'd like toolist in hello Azure AD Application Gallery, you must first make sure that your application supports one of hello following single sign-on technologies:</span></span>

1. <span data-ttu-id="88bf7-109">**OpenID Connect** - közvetlen integráció az Azure AD OpenID Connect használata a hitelesítéshez és az Azure AD hozzájárulási API konfiguráció hello.</span><span class="sxs-lookup"><span data-stu-id="88bf7-109">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and hello Azure AD consent API for configuration.</span></span> <span data-ttu-id="88bf7-110">Ha az alkalmazás nem támogatja az SAML-alapú integrációs csak indításakor, majd ez érték hello ajánlott módja.</span><span class="sxs-lookup"><span data-stu-id="88bf7-110">If you are just starting an integration and your application does not support SAML, then this is hello recommend mode.</span></span>
2. <span data-ttu-id="88bf7-111">**SAML** – az alkalmazás már rendelkezik hello képességét tooconfigure harmadik fél Identitásszolgáltatók hello SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="88bf7-111">**SAML** – Your application already has hello ability tooconfigure third-party identity providers using hello SAML protocol.</span></span>

<span data-ttu-id="88bf7-112">Ha az alkalmazás támogatja ezeket egyszeri bejelentkezési módok egyikét, és azt szeretné, hogy toolist az Azure AD Application Gallery hello több-bérlős alkalmazást, a lépésekkel hello hello dokumentum alatt.</span><span class="sxs-lookup"><span data-stu-id="88bf7-112">If your application supports one of these single sign-on modes and you'd like toolist your multi-tenant application in hello Azure AD Application Gallery, you can follow hello steps in hello document below.</span></span> <span data-ttu-id="88bf7-113">gyorsan tooget küldjön egy e-mailt túl**waadpartners@microsoft.com**.</span><span class="sxs-lookup"><span data-stu-id="88bf7-113">tooget started quickly send an email too**waadpartners@microsoft.com**.</span></span>

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a><span data-ttu-id="88bf7-114">Ha az alkalmazás nem támogatja az SAML-alapú vagy OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="88bf7-114">If your application does not support SAML or OpenIDConnect</span></span>
<span data-ttu-id="88bf7-115">Akkor is, ha az alkalmazás nem támogatja e két beállítás közül, azt továbbra is integrálható, a gyűjtemény, a jelszót az egyszeri bejelentkezés technológiával.</span><span class="sxs-lookup"><span data-stu-id="88bf7-115">Even if your application does not support one of these modes, we can still integrate it into our gallery using our Password Single Sign-on technology.</span></span> <span data-ttu-id="88bf7-116">Ha azt szeretné, hogy tooexplore ezt a beállítást, küldhet e-mailt túl**waadpartners@microsoft.com**.</span><span class="sxs-lookup"><span data-stu-id="88bf7-116">If you'd like tooexplore this option, you can send an email too**waadpartners@microsoft.com**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="88bf7-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88bf7-117">Next steps</span></span>
[<span data-ttu-id="88bf7-118">Hogyan toolist az alkalmazás hello Azure Active Directory alkalmazáskatalógusában</span><span class="sxs-lookup"><span data-stu-id="88bf7-118">How toolist your application in hello Azure Active Directory application gallery</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
