---
title: "Azure Active Directory B2C: Egyéni szabályzatok kezdőlapja | Microsoft Docs"
description: "Fogyasztói alkalmazások fejlesztése az Azure Active Directory B2C-vel egyéni szabályzatok használatával"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f2079f53-a637-4f2d-b3a0-61a9647ad433
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 5/06/2017
ms.author: parakhj
ms.openlocfilehash: 28a39f282488b81fc9e2ab7841b5f2cb4cd58715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications-using-custom-policies"></a><span data-ttu-id="44c34-103">Azure Active Directory B2C: Felhasználók regisztrálása és bejelentkezése az alkalmazásokba egyéni szabályzatok használatával</span><span class="sxs-lookup"><span data-stu-id="44c34-103">Azure Active Directory B2C: Sign up and sign in consumers in your applications using custom policies</span></span>
<span data-ttu-id="44c34-104">Egyéni házirendek olyan konfigurációs fájlokat, amelyek meghatározzák az Azure AD B2C-bérlő hello viselkedését.</span><span class="sxs-lookup"><span data-stu-id="44c34-104">Custom policies are configuration files that define hello behavior of your Azure AD B2C tenant.</span></span> <span data-ttu-id="44c34-105">El teljesen szerkesztheti az identitás fejlesztői toocomplete közel korlátlan számú feladatok.</span><span class="sxs-lookup"><span data-stu-id="44c34-105">They can be fully edited by an identity developer toocomplete a near unlimited number of tasks.</span></span>

## <a name="how-tooarticles"></a><span data-ttu-id="44c34-106">Hogyan-tooarticles</span><span class="sxs-lookup"><span data-stu-id="44c34-106">How-tooarticles</span></span>
<span data-ttu-id="44c34-107">Megtudhatja, hogyan toouse Azure Active Directory B2C funkciók:</span><span class="sxs-lookup"><span data-stu-id="44c34-107">Learn how toouse specific Azure Active Directory B2C features:</span></span>

* [<span data-ttu-id="44c34-108">Első lépések</span><span class="sxs-lookup"><span data-stu-id="44c34-108">Get Started</span></span>](active-directory-b2c-overview-custom.md)
* <span data-ttu-id="44c34-109">OIDC-szolgáltatók, például az [Azure AD](active-directory-b2c-setup-aad-custom.md) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="44c34-109">Configure OIDC Providers such as [Azure AD](active-directory-b2c-setup-aad-custom.md).</span></span>
* <span data-ttu-id="44c34-110">SAML-szolgáltatók, például a [Salesforce](active-directory-b2c-setup-sf-app-custom.md) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="44c34-110">Configure SAML Providers such as [Salesforce](active-directory-b2c-setup-sf-app-custom.md).</span></span>
* <span data-ttu-id="44c34-111">RESTful API-k integrálása:</span><span class="sxs-lookup"><span data-stu-id="44c34-111">Integrate RESTful APIs:</span></span>
    * <span data-ttu-id="44c34-112">[További jogcímek beszerzése](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="44c34-112">[Obtain additional claims](active-directory-b2c-rest-api-step-custom.md).</span></span>
    * <span data-ttu-id="44c34-113">[Felhasználó által megadott adatok érvényesítése](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="44c34-113">[Validate user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>
* <span data-ttu-id="44c34-114">Bejelentkezés testreszabása:</span><span class="sxs-lookup"><span data-stu-id="44c34-114">Customize Login:</span></span>
    * [<span data-ttu-id="44c34-115">Felhasználó által megadott adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44c34-115">Configure User Input</span></span>](active-directory-b2c-configure-signup-self-asserted-custom.md)
    * [<span data-ttu-id="44c34-116">Felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="44c34-116">Customize UI</span></span>](active-directory-b2c-ui-customization-custom.md)
    * [<span data-ttu-id="44c34-117">Jogkivonat testreszabása</span><span class="sxs-lookup"><span data-stu-id="44c34-117">Customize Token</span></span>](active-directory-b2c-reference-manage-sso-and-token-configuration.md)
* <span data-ttu-id="44c34-118">Hibaelhárítás [naplókat gyűjtve az Application Insights használatával](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="44c34-118">Troubleshoot by [collecting logs using Application Insights](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="whats-new"></a><span data-ttu-id="44c34-119">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="44c34-119">What's new</span></span>
<span data-ttu-id="44c34-120">Térjen vissza ide gyakran kapcsolatos változásokról toohello Azure Active Directory B2C toolearn.</span><span class="sxs-lookup"><span data-stu-id="44c34-120">Check back here often toolearn about future changes toohello Azure Active Directory B2C.</span></span> <span data-ttu-id="44c34-121">A frissítésekről az @AzureAD néven Twitter-üzeneteket is küldünk.</span><span class="sxs-lookup"><span data-stu-id="44c34-121">We'll also tweet about any updates by using @AzureAD.</span></span>



