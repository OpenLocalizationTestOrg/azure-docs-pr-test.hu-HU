---
title: "SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban aaaHow toodebug |} Microsoft Docs"
description: "Megtudhatja, hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 846c7b3497c8842947c5b406f4362b9e06785b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-saml-based-single-sign-on-tooapplications-in-azure-active-directory"></a><span data-ttu-id="01c18-103">Hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="01c18-103">How toodebug SAML-based single sign-on tooapplications in Azure Active Directory</span></span>
<span data-ttu-id="01c18-104">Hibakeresési információ egy SAML-alapú alkalmazások integrálása esetén gyakran hasznos toouse egy eszköz, például [Fiddler](http://www.telerik.com/fiddler) toosee hello hello tényleges SAML-jogkivonat toohello alkalmazás kiadott, a SAML kérelem és a hello SAML-válasz.</span><span class="sxs-lookup"><span data-stu-id="01c18-104">When debugging a SAML-based application integration, it is often helpful toouse a tool like [Fiddler](http://www.telerik.com/fiddler) toosee hello SAML request, hello SAML response, and hello actual SAML token that is issued toohello application.</span></span> <span data-ttu-id="01c18-105">Hello SAML-jogkivonat vizsgálatával győződjön meg arról, hogy az összes hello szükséges attribútumok, felhasználónév hello SAML tárgy hello és hello issuer URI a várt módon keresztül érkeznek.</span><span class="sxs-lookup"><span data-stu-id="01c18-105">By examining hello SAML token, you can ensure that all of hello required attributes, hello username in hello SAML subject, and hello issuer URI are coming through as expected.</span></span>

![][1]

<span data-ttu-id="01c18-106">hello Azure ad-hello SAML-jogkivonat tartalmazó esetében általában hello egyet, amely egy HTTP 302 átirányítási után a https://login.windows.net, és a elküldött toohello **válasz URL-CÍMEN** hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="01c18-106">hello response from Azure AD that contains hello SAML token is typically hello one that occurs after an HTTP 302 redirect from https://login.windows.net, and is sent toohello configured **Reply URL** of hello application.</span></span> 

<span data-ttu-id="01c18-107">Válassza ezt a sort, és jelölje be hello megtekintheti hello SAML-jogkivonat **ellenőrök > WebForms** lapon hello jobb oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="01c18-107">You can view hello SAML token by selecting this line and then selecting hello **Inspectors > WebForms** tab in hello right panel.</span></span> <span data-ttu-id="01c18-108">Ott, kattintson a jobb gombbal hello **SAMLResponse** értékét, és válassza ki **tooTextWizard küldése**.</span><span class="sxs-lookup"><span data-stu-id="01c18-108">From there, right-click hello **SAMLResponse** value and select **Send tooTextWizard**.</span></span> <span data-ttu-id="01c18-109">Válassza ki **a Base64** a hello **átalakítási** menü toodecode hello jogkivonatot, és tekintse meg annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="01c18-109">Then select **From Base64** from hello **Transform** menu toodecode hello token and see its contents.</span></span>

<span data-ttu-id="01c18-110">**Megjegyzés:**: toosee hello tartalmát, a HTTP kérelem Fiddler késztethetik, HTTPS-forgalmat, amelyeket érdemes tooconfigure visszafejtése toodo.</span><span class="sxs-lookup"><span data-stu-id="01c18-110">**Note**: toosee hello contents of this HTTP request, Fiddler may prompt you tooconfigure decryption of HTTPS traffic, which you will need toodo.</span></span>

## <a name="related-articles"></a><span data-ttu-id="01c18-111">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="01c18-111">Related Articles</span></span>
* [<span data-ttu-id="01c18-112">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="01c18-112">Article Index for Application Management in Azure Active Directory</span></span>](../active-directory-apps-index.md)
* [<span data-ttu-id="01c18-113">Egyszeri bejelentkezés tooapplications, amelyek nincsenek hello Azure Active Directory alkalmazáskatalógusában konfigurálása</span><span class="sxs-lookup"><span data-stu-id="01c18-113">Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery</span></span>](../active-directory-saas-custom-apps.md)
* [<span data-ttu-id="01c18-114">Hogyan tooCustomize jogcím a kiállított hello SAML-jogkivonat Pre-Integrated alkalmazások</span><span class="sxs-lookup"><span data-stu-id="01c18-114">How tooCustomize Claims Issued in hello SAML Token for Pre-Integrated Apps</span></span>](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png