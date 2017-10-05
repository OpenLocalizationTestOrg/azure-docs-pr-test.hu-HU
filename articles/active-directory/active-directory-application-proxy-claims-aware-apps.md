---
title: "Jogcímbarát alkalmazások – az Azure AD alkalmazás Proxy |} Microsoft Docs"
description: "Fogadja el az AD FS-jogcímek az biztonságos távoli hozzáférést a felhasználók által a helyszínen ASP.NET alkalmazások közzétételének módját."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 5784222608b01509fc4ff84b1a8792cbcfea89e6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a><span data-ttu-id="46e58-103">Az alkalmazásproxy jogcímbarát alkalmazásokkal való munka</span><span class="sxs-lookup"><span data-stu-id="46e58-103">Working with claims-aware apps in Application Proxy</span></span>
<span data-ttu-id="46e58-104">[Jogcímbarát alkalmazások](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) végre átirányítás a biztonsági jogkivonat szolgáltatás (STS).</span><span class="sxs-lookup"><span data-stu-id="46e58-104">[Claims-aware apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) perform a redirection to the Security Token Service (STS).</span></span> <span data-ttu-id="46e58-105">Az STS cserébe jogkivonat a felhasználó kéri a hitelesítő adatokat, és ezután átirányítja a felhasználót az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="46e58-105">The STS requests credentials from the user in exchange for a token and then redirects the user to the application.</span></span> <span data-ttu-id="46e58-106">Néhány módon történő együttműködésre ezek átirányítások alkalmazásproxy engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="46e58-106">There are a few ways to enable Application Proxy to work with these redirects.</span></span> <span data-ttu-id="46e58-107">Ez a cikk segítségével konfigurálhatja a jogcímbarát alkalmazások központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="46e58-107">Use this article to configure your deployment for claims-aware apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="46e58-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="46e58-108">Prerequisites</span></span>
<span data-ttu-id="46e58-109">Győződjön meg arról, hogy az STS, amely átirányítja a jogcímbarát alkalmazáshoz érhető el a helyszíni hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="46e58-109">Make sure that the STS that the claims-aware app redirects to is available outside of your on-premises network.</span></span> <span data-ttu-id="46e58-110">Elérhetővé teheti az STS megjelenítése révén olyan proxyn keresztül, vagy külső kapcsolatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="46e58-110">You can make the STS available by exposing it through a proxy or by allowing outside connections.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="46e58-111">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="46e58-111">Publish your application</span></span>

1. <span data-ttu-id="46e58-112">Az ismertetett utasításoknak megfelelően az alkalmazás közzétételére [alkalmazások közzétételére az alkalmazásproxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="46e58-112">Publish your application according to the instructions described in [Publish applications with Application Proxy](application-proxy-publish-azure-portal.md).</span></span>
2. <span data-ttu-id="46e58-113">Nyissa meg az alkalmazás oldalát a portálon, és válasszon **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="46e58-113">Navigate to the application page in the portal and select **Single sign-on**.</span></span>
3. <span data-ttu-id="46e58-114">Ha úgy döntött, hogy **Azure Active Directory** , a **előhitelesítési módszer**, jelölje be **az Azure AD az egyszeri bejelentkezés le van tiltva** , a **belső Hitelesítési módszer**.</span><span class="sxs-lookup"><span data-stu-id="46e58-114">If you chose **Azure Active Directory** as your **Preauthentication Method**, select **Azure AD single sign-on disabled** as your **Internal Authentication Method**.</span></span> <span data-ttu-id="46e58-115">Ha úgy döntött, hogy **csatlakoztatott** , a **előhitelesítési módszer**, nem kell bármit módosíthat.</span><span class="sxs-lookup"><span data-stu-id="46e58-115">If you chose **Passthrough** as your **Preauthentication Method**, you don't need to change anything.</span></span>

## <a name="configure-adfs"></a><span data-ttu-id="46e58-116">AD FS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="46e58-116">Configure ADFS</span></span>

<span data-ttu-id="46e58-117">Az AD FS a jogcímbarát alkalmazások az alábbi két módszer egyikével állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="46e58-117">You can configure ADFS for claims-aware apps in one of two ways.</span></span> <span data-ttu-id="46e58-118">Az egyik egyéni tartományok használatával.</span><span class="sxs-lookup"><span data-stu-id="46e58-118">The first is by using custom domains.</span></span> <span data-ttu-id="46e58-119">A második pedig a WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="46e58-119">The second is with WS-Federation.</span></span> 

### <a name="option-1-custom-domains"></a><span data-ttu-id="46e58-120">1. lehetőség: Egyéni tartományok</span><span class="sxs-lookup"><span data-stu-id="46e58-120">Option 1: Custom domains</span></span>

<span data-ttu-id="46e58-121">Ha minden belső URL-címéből az alkalmazások teljes tartománynevek (FQDN), majd konfigurálhatja [egyéni tartományok](active-directory-application-proxy-custom-domains.md) az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="46e58-121">If all the internal URLs for your applications are fully qualified domain names (FQDNs), then you can configure [custom domains](active-directory-application-proxy-custom-domains.md) for your applications.</span></span> <span data-ttu-id="46e58-122">Az egyéni tartományok segítségével hozzon létre a külső URL-címek, amelyek ugyanaz, mint a belső URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="46e58-122">Use the custom domains to create external URLs that are the same as the internal URLs.</span></span> <span data-ttu-id="46e58-123">Ha a külső URL-címek felel meg a belső URL-címeket, akkor az STS-átirányítások munkahelyi, hogy a felhasználók is a helyszíni vagy távoli.</span><span class="sxs-lookup"><span data-stu-id="46e58-123">When your external URLs match your internal URLs, then the STS redirections work whether your users are on-premises or remote.</span></span> 

### <a name="option-2-ws-federation"></a><span data-ttu-id="46e58-124">2. lehetőség: WS-Federation</span><span class="sxs-lookup"><span data-stu-id="46e58-124">Option 2: WS-Federation</span></span>

1. <span data-ttu-id="46e58-125">Nyissa meg az AD FS kezelése.</span><span class="sxs-lookup"><span data-stu-id="46e58-125">Open ADFS Management.</span></span>
2. <span data-ttu-id="46e58-126">Ugrás a **függő entitás Megbízhatóságai**, kattintson a jobb gombbal az alkalmazásproxy közzétett alkalmazás, és válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="46e58-126">Go to **Relying Party Trusts**, right-click on the app you are publishing with Application Proxy, and choose **Properties**.</span></span>  

   ![Függő entitás Megbízhatóságai kattintson a jobb gombbal az alkalmazás neve – képernyőkép](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. <span data-ttu-id="46e58-128">Az a **végpontok** lap **típusú végpont**, jelölje be **WS-Federation**.</span><span class="sxs-lookup"><span data-stu-id="46e58-128">On the **Endpoints** tab, under **Endpoint type**, select **WS-Federation**.</span></span>
4. <span data-ttu-id="46e58-129">A **megbízható URL-cím**, az URL-címét az alkalmazásproxy alatt megadott **külső URL-cím** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="46e58-129">Under **Trusted URL**, enter the URL you entered in the Application Proxy under **External URL** and click **OK**.</span></span>  

   ![Adja hozzá a végpont - állítsa be a megbízható URL-cím érték – képernyőkép](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a><span data-ttu-id="46e58-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46e58-131">Next steps</span></span>
* <span data-ttu-id="46e58-132">[Egyszeri bejelentkezés engedélyezése a](application-proxy-sso-overview.md) , amelyek nem jogcímbarát alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="46e58-132">[Enable single-sign on](application-proxy-sso-overview.md) for applications that aren't claims-aware</span></span>
* [<span data-ttu-id="46e58-133">Proxy alkalmazások együttműködhet natív ügyfél alkalmazások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="46e58-133">Enable native client apps to interact with proxy applications</span></span>](active-directory-application-proxy-native-client.md)


