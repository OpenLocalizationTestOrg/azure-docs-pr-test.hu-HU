---
title: "Az Azure AD application gallery egy több-bérlős alkalmazás hozzáadása |} Microsoft Docs"
description: "Azt ismerteti, hogyan listázhatja a Azure AD Application Gallery a saját fejlesztésű több-bérlős alkalmazásához"
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
ms.openlocfilehash: 208f0d40bd7a8e8f35f16e1fcb09c305d833dbb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a><span data-ttu-id="1eebb-103">Az Azure AD application gallery egy több-bérlős alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1eebb-103">How to add a multi-tenant application to the Azure AD application gallery</span></span>

## <a name="what-is-the-azure-ad-application-gallery"></a><span data-ttu-id="1eebb-104">Mi az az Azure AD Application Gallery?</span><span class="sxs-lookup"><span data-stu-id="1eebb-104">What is the Azure AD Application Gallery?</span></span>

<span data-ttu-id="1eebb-105">Az Azure AD Application Gallery kiváló módja a az alkalmazás Azure Active Directory-ügyfelek számára a bővíthetők a hatás és a felhasználók elérését a piactéren az alkalmazás összes millióit elé.</span><span class="sxs-lookup"><span data-stu-id="1eebb-105">The Azure AD Application Gallery is a great way to get your application in front of all the millions of Azure Active Directory customers to broaden the impact and reach of your application in the marketplace.</span></span> <span data-ttu-id="1eebb-106">A következő lépések azt ismertetik, hogyan listázhatja a Azure AD Application Gallery az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1eebb-106">The below steps explain how you can list your application in the Azure AD Application Gallery.</span></span>

## <a name="if-your-application-supports-saml-or-openidconnect"></a><span data-ttu-id="1eebb-107">Ha az alkalmazás támogatja az SAML-alapú vagy OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="1eebb-107">If your application supports SAML or OpenIDConnect</span></span>
<span data-ttu-id="1eebb-108">Ha egy több-bérlős alkalmazást szeretné az Azure AD Application Gallery listájában, először ellenőrizze, az alkalmazás támogatja a következő egyszeri bejelentkezési technológiák egyikét:</span><span class="sxs-lookup"><span data-stu-id="1eebb-108">If you have a multi-tenant application you'd like to list in the Azure AD Application Gallery, you must first make sure that your application supports one of the following single sign-on technologies:</span></span>

1. <span data-ttu-id="1eebb-109">**OpenID Connect** -közvetlen integráció használja OpenID Connect hitelesítési és az Azure AD hozzájárulás API konfigurálása az Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="1eebb-109">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and the Azure AD consent API for configuration.</span></span> <span data-ttu-id="1eebb-110">Ha integrációs csak indításakor, és az alkalmazás nem támogatja az SAML-alapú, majd azt az ajánlott mód.</span><span class="sxs-lookup"><span data-stu-id="1eebb-110">If you are just starting an integration and your application does not support SAML, then this is the recommend mode.</span></span>
2. <span data-ttu-id="1eebb-111">**SAML** – az alkalmazás már rendelkezik az ügyfélgépek konfigurálására harmadik fél Identitásszolgáltatók az SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="1eebb-111">**SAML** – Your application already has the ability to configure third-party identity providers using the SAML protocol.</span></span>

<span data-ttu-id="1eebb-112">Ha az alkalmazás támogatja ezeket egyszeri bejelentkezési módok egyikét, és szeretné több-bérlős alkalmazás szerepeltetése az Azure AD Application Gallery, követheti a dokumentum az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="1eebb-112">If your application supports one of these single sign-on modes and you'd like to list your multi-tenant application in the Azure AD Application Gallery, you can follow the steps in the document below.</span></span> <span data-ttu-id="1eebb-113">Ha gyorsan el szeretné küldeni az e-mailek  **waadpartners@microsoft.com** .</span><span class="sxs-lookup"><span data-stu-id="1eebb-113">To get started quickly send an email to **waadpartners@microsoft.com**.</span></span>

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a><span data-ttu-id="1eebb-114">Ha az alkalmazás nem támogatja az SAML-alapú vagy OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="1eebb-114">If your application does not support SAML or OpenIDConnect</span></span>
<span data-ttu-id="1eebb-115">Akkor is, ha az alkalmazás nem támogatja e két beállítás közül, azt továbbra is integrálható, a gyűjtemény, a jelszót az egyszeri bejelentkezés technológiával.</span><span class="sxs-lookup"><span data-stu-id="1eebb-115">Even if your application does not support one of these modes, we can still integrate it into our gallery using our Password Single Sign-on technology.</span></span> <span data-ttu-id="1eebb-116">Milyen eltérések felfedezése, ezt a beállítást, ha az e-mailt küldhet  **waadpartners@microsoft.com** .</span><span class="sxs-lookup"><span data-stu-id="1eebb-116">If you'd like to explore this option, you can send an email to **waadpartners@microsoft.com**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1eebb-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1eebb-117">Next steps</span></span>
[<span data-ttu-id="1eebb-118">Hogyan alkalmazás szerepeltetése az Azure Active Directory alkalmazáskatalógusában</span><span class="sxs-lookup"><span data-stu-id="1eebb-118">How to list your application in the Azure Active Directory application gallery</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
