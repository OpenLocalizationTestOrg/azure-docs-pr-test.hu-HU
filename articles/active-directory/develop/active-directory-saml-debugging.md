---
title: "SAML-alapú egyszeri bejelentkezés az Azure Active Directoryban alkalmazások hibakeresése |} Microsoft Docs"
description: "Ismerje meg, SAML-alapú egyszeri bejelentkezés az Azure Active Directoryban alkalmazások hibakeresése "
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
ms.openlocfilehash: 31447d597296bac57481dc2acb4a95ee3a104161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a><span data-ttu-id="fa6e6-103">SAML-alapú egyszeri bejelentkezés az Azure Active Directoryban alkalmazások hibakeresése</span><span class="sxs-lookup"><span data-stu-id="fa6e6-103">How to debug SAML-based single sign-on to applications in Azure Active Directory</span></span>
<span data-ttu-id="fa6e6-104">Amikor egy SAML-alapú alkalmazások integrálása hibakeresés, akkor célszerű gyakran hasonló eszközök használata [Fiddler](http://www.telerik.com/fiddler) a SAML-kérelmet, SAML-válasz és az alkalmazás számára kiállított tényleges SAML-jogkivonatra.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-104">When debugging a SAML-based application integration, it is often helpful to use a tool like [Fiddler](http://www.telerik.com/fiddler) to see the SAML request, the SAML response, and the actual SAML token that is issued to the application.</span></span> <span data-ttu-id="fa6e6-105">A SAML-jogkivonat vizsgálatával biztosítható, hogy a szükséges attribútumokat, a felhasználónevet a SAML-tulajdonos, és a kiállító URI mindegyikét érkeznek várt módon.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-105">By examining the SAML token, you can ensure that all of the required attributes, the username in the SAML subject, and the issuer URI are coming through as expected.</span></span>

![][1]

<span data-ttu-id="fa6e6-106">Az Azure ad-, amely tartalmazza a SAML-jogkivonat esetében általában a egy másikat, amely egy HTTP 302 átirányítási után a https://login.windows.net következik be, majd továbbítja a konfigurált való **válasz URL-CÍMEN** az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-106">The response from Azure AD that contains the SAML token is typically the one that occurs after an HTTP 302 redirect from https://login.windows.net, and is sent to the configured **Reply URL** of the application.</span></span> 

<span data-ttu-id="fa6e6-107">Ehhez jelölje ki a sort, és jelölje be a SAML-jogkivonat megtekintheti a **ellenőrök > WebForms** fülre a jobb oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-107">You can view the SAML token by selecting this line and then selecting the **Inspectors > WebForms** tab in the right panel.</span></span> <span data-ttu-id="fa6e6-108">Ott, kattintson a jobb gombbal a **SAMLResponse** értékét, és válassza ki **TextWizard küldeni**.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-108">From there, right-click the **SAMLResponse** value and select **Send to TextWizard**.</span></span> <span data-ttu-id="fa6e6-109">Válassza ki **a Base64** a a **átalakítási** menü dekódolni a jogkivonatot, és tekintse meg annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-109">Then select **From Base64** from the **Transform** menu to decode the token and see its contents.</span></span>

<span data-ttu-id="fa6e6-110">**Megjegyzés:**: a HTTP-kérelem tartalmának megtekintéséhez Fiddler késztethetik, konfigurálhatja a HTTPS forgalmat, amely ehhez szüksége lesz visszafejtése.</span><span class="sxs-lookup"><span data-stu-id="fa6e6-110">**Note**: To see the contents of this HTTP request, Fiddler may prompt you to configure decryption of HTTPS traffic, which you will need to do.</span></span>

## <a name="related-articles"></a><span data-ttu-id="fa6e6-111">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="fa6e6-111">Related Articles</span></span>
* [<span data-ttu-id="fa6e6-112">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="fa6e6-112">Article Index for Application Management in Azure Active Directory</span></span>](../active-directory-apps-index.md)
* [<span data-ttu-id="fa6e6-113">Egyszeri bejelentkezés konfigurálása az Azure Active Directory alkalmazáskatalógusában nem szereplő alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="fa6e6-113">Configuring single sign-on to applications that are not in the Azure Active Directory application gallery</span></span>](../active-directory-saas-custom-apps.md)
* [<span data-ttu-id="fa6e6-114">A SAML-jogkivonat előzetesen beépített alkalmazások számára kiállított jogcímek testreszabása</span><span class="sxs-lookup"><span data-stu-id="fa6e6-114">How to Customize Claims Issued in the SAML Token for Pre-Integrated Apps</span></span>](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png