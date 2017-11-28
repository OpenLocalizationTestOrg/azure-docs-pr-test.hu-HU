---
title: "egy új több-bérlős alkalmazás aaaHow tooconfigure |} Microsoft Docs"
description: "Hogyan tooconfigure egyszeri bejelentkezést az egyéni alkalmazás fejlesztés és regisztrálása az Azure ad-val."
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
ms.openlocfilehash: 4d3499d8885933516d6597fa9f87bcf88cd5a428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-new-multi-tenant-application"></a><span data-ttu-id="cbbeb-103">Hogyan tooconfigure új több-bérlős alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cbbeb-103">How tooconfigure a new multi-tenant application</span></span>

<span data-ttu-id="cbbeb-104">Az alkalmazás összevont egyszeri bejelentkezést (SSO) engedélyezése automatikusan engedélyezi az Azure AD keresztül OpenID Connect, az SAML 2.0-s vagy a WS-Fed összevonását.</span><span class="sxs-lookup"><span data-stu-id="cbbeb-104">Enabling federated single sign-on (SSO) in your app is automatically enabled when federating through Azure AD for OpenID Connect, SAML 2.0, or WS-Fed.</span></span> <span data-ttu-id="cbbeb-105">Ha a végfelhasználók toosign annak ellenére, hogy már rendelkezik az Azure AD egy meglévő munkamenetben, valószínű, az alkalmazás nincs megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="cbbeb-105">If your end users are having toosign in despite already having an existing session with Azure AD, it’s likely your app may be misconfigured.</span></span>

* <span data-ttu-id="cbbeb-106">Ha az adal-t/MSAL használata esetén ellenőrizze, hogy **PromptBehavior** túl beállítása**automatikus** helyett **mindig**.</span><span class="sxs-lookup"><span data-stu-id="cbbeb-106">If you’re using ADAL/MSAL, make sure you have **PromptBehavior** set too**Auto** rather than **Always**.</span></span>

* <span data-ttu-id="cbbeb-107">A mobilalkalmazások most létrehozása, ha szükség lehet további konfigurációs tooenable által felügyelt vagy nem közvetítőalapú egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cbbeb-107">If you’re building a mobile app, you may need additional configurations tooenable brokered or non-brokered SSO.</span></span>

<span data-ttu-id="cbbeb-108">Lásd: Android, [engedélyezése kereszt-alkalmazás SSO Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span><span class="sxs-lookup"><span data-stu-id="cbbeb-108">For Android, see [Enabling Cross App SSO in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span></span><br>

<span data-ttu-id="cbbeb-109">Az iOS számára, lásd: [Cross-SSO alkalmazás engedélyezése IOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span><span class="sxs-lookup"><span data-stu-id="cbbeb-109">For iOS, see [Enabling Cross App SSO in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbbeb-110">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbbeb-110">Next steps</span></span>

[<span data-ttu-id="cbbeb-111">Az Azure AD-egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cbbeb-111">Azure AD SSO</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[<span data-ttu-id="cbbeb-112">Közötti App SSO Android engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cbbeb-112">Enabling Cross App SSO in Android</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[<span data-ttu-id="cbbeb-113">Cross-SSO App engedélyezése iOS-ban</span><span class="sxs-lookup"><span data-stu-id="cbbeb-113">Enabling Cross App SSO in iOS</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[<span data-ttu-id="cbbeb-114">Alkalmazások tooAzureAD integrálása</span><span class="sxs-lookup"><span data-stu-id="cbbeb-114">Integrating Apps tooAzureAD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[<span data-ttu-id="cbbeb-115">Hozzájárulás és Permissioning AzureAD v2.0 az összevont alkalmazások</span><span class="sxs-lookup"><span data-stu-id="cbbeb-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="cbbeb-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="cbbeb-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
