---
title: "aaaClaims-t támogató alkalmazások – Azure AD alkalmazás Proxy |} Microsoft Docs"
description: "Hogyan toopublish helyszíni fogadja el az AD FS-jogcímek biztonságos távoli hozzáférést a felhasználók által az ASP.NET-alkalmazások."
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
ms.openlocfilehash: 7be633225de700226c7c94815eb91b3de2b61cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a><span data-ttu-id="c9169-103">Az alkalmazásproxy jogcímbarát alkalmazásokkal való munka</span><span class="sxs-lookup"><span data-stu-id="c9169-103">Working with claims-aware apps in Application Proxy</span></span>
<span data-ttu-id="c9169-104">[Jogcímbarát alkalmazások](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) hajtsa végre egy átirányítási toohello biztonsági jogkivonat szolgáltatás (STS).</span><span class="sxs-lookup"><span data-stu-id="c9169-104">[Claims-aware apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) perform a redirection toohello Security Token Service (STS).</span></span> <span data-ttu-id="c9169-105">hello STS hello felhasználói jogkivonat cserébe kéri a hitelesítő adatokat, és ezután átirányítja a felhasználókat hello toohello alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="c9169-105">hello STS requests credentials from hello user in exchange for a token and then redirects hello user toohello application.</span></span> <span data-ttu-id="c9169-106">Néhány módon tooenable alkalmazásproxy toowork ezek átirányítással van.</span><span class="sxs-lookup"><span data-stu-id="c9169-106">There are a few ways tooenable Application Proxy toowork with these redirects.</span></span> <span data-ttu-id="c9169-107">Ez a cikk tooconfigure a központi telepítés az jogcímbarát alkalmazáshoz használni.</span><span class="sxs-lookup"><span data-stu-id="c9169-107">Use this article tooconfigure your deployment for claims-aware apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c9169-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c9169-108">Prerequisites</span></span>
<span data-ttu-id="c9169-109">Győződjön meg arról, hogy hello STS hello jogcímbarát alkalmazáshoz átirányítja a felhasználókat a helyszíni hálózaton kívülről elérhető toois.</span><span class="sxs-lookup"><span data-stu-id="c9169-109">Make sure that hello STS that hello claims-aware app redirects toois available outside of your on-premises network.</span></span> <span data-ttu-id="c9169-110">Elérhetővé teheti hello STS proxyn keresztül, az ilyen, vagy kívül kapcsolatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9169-110">You can make hello STS available by exposing it through a proxy or by allowing outside connections.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="c9169-111">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="c9169-111">Publish your application</span></span>

1. <span data-ttu-id="c9169-112">Az alkalmazás toohello utasításokat leírtak szerint közzétételére [alkalmazások közzétételére az alkalmazásproxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c9169-112">Publish your application according toohello instructions described in [Publish applications with Application Proxy](application-proxy-publish-azure-portal.md).</span></span>
2. <span data-ttu-id="c9169-113">Keresse meg a toohello alkalmazáslap hello portál, és válassza a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9169-113">Navigate toohello application page in hello portal and select **Single sign-on**.</span></span>
3. <span data-ttu-id="c9169-114">Ha úgy döntött, hogy **Azure Active Directory** , a **előhitelesítési módszer**, jelölje be **az Azure AD az egyszeri bejelentkezés le van tiltva** , a **belső Hitelesítési módszer**.</span><span class="sxs-lookup"><span data-stu-id="c9169-114">If you chose **Azure Active Directory** as your **Preauthentication Method**, select **Azure AD single sign-on disabled** as your **Internal Authentication Method**.</span></span> <span data-ttu-id="c9169-115">Ha úgy döntött, hogy **csatlakoztatott** , a **előhitelesítési módszer**, toochange semmi nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9169-115">If you chose **Passthrough** as your **Preauthentication Method**, you don't need toochange anything.</span></span>

## <a name="configure-adfs"></a><span data-ttu-id="c9169-116">AD FS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9169-116">Configure ADFS</span></span>

<span data-ttu-id="c9169-117">Az AD FS a jogcímbarát alkalmazások az alábbi két módszer egyikével állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="c9169-117">You can configure ADFS for claims-aware apps in one of two ways.</span></span> <span data-ttu-id="c9169-118">hello első az egyéni tartományok használatával.</span><span class="sxs-lookup"><span data-stu-id="c9169-118">hello first is by using custom domains.</span></span> <span data-ttu-id="c9169-119">hello második WS-Federation jelenti.</span><span class="sxs-lookup"><span data-stu-id="c9169-119">hello second is with WS-Federation.</span></span> 

### <a name="option-1-custom-domains"></a><span data-ttu-id="c9169-120">1. lehetőség: Egyéni tartományok</span><span class="sxs-lookup"><span data-stu-id="c9169-120">Option 1: Custom domains</span></span>

<span data-ttu-id="c9169-121">Ha az összes hello belső URL-címeket, az alkalmazások: teljesen minősített tartománynevek (FQDN), majd konfigurálhatja [egyéni tartományok](active-directory-application-proxy-custom-domains.md) az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="c9169-121">If all hello internal URLs for your applications are fully qualified domain names (FQDNs), then you can configure [custom domains](active-directory-application-proxy-custom-domains.md) for your applications.</span></span> <span data-ttu-id="c9169-122">Használjon hello egyéni tartományok toocreate külső URL-címek, amelyek megegyeznek a belső URL-címek hello hello.</span><span class="sxs-lookup"><span data-stu-id="c9169-122">Use hello custom domains toocreate external URLs that are hello same as hello internal URLs.</span></span> <span data-ttu-id="c9169-123">Ha a külső URL-címek megegyeznek a belső URL-címeket, majd hello STS átirányítások működik, hogy a felhasználók is a helyi vagy távoli.</span><span class="sxs-lookup"><span data-stu-id="c9169-123">When your external URLs match your internal URLs, then hello STS redirections work whether your users are on-premises or remote.</span></span> 

### <a name="option-2-ws-federation"></a><span data-ttu-id="c9169-124">2. lehetőség: WS-Federation</span><span class="sxs-lookup"><span data-stu-id="c9169-124">Option 2: WS-Federation</span></span>

1. <span data-ttu-id="c9169-125">Nyissa meg az AD FS kezelése.</span><span class="sxs-lookup"><span data-stu-id="c9169-125">Open ADFS Management.</span></span>
2. <span data-ttu-id="c9169-126">Nyissa meg túl**függő entitás Megbízhatóságai**, kattintson a jobb gombbal az alkalmazásproxy közzétett hello alkalmazás, és válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="c9169-126">Go too**Relying Party Trusts**, right-click on hello app you are publishing with Application Proxy, and choose **Properties**.</span></span>  

   ![Függő entitás Megbízhatóságai kattintson a jobb gombbal az alkalmazás neve – képernyőkép](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. <span data-ttu-id="c9169-128">A hello **végpontok** lap **típusú végpont**, jelölje be **WS-Federation**.</span><span class="sxs-lookup"><span data-stu-id="c9169-128">On hello **Endpoints** tab, under **Endpoint type**, select **WS-Federation**.</span></span>
4. <span data-ttu-id="c9169-129">Alatt **megbízható URL-cím**, adja meg a megadott hello az alkalmazásproxy hello URL-cím **külső URL-cím** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9169-129">Under **Trusted URL**, enter hello URL you entered in hello Application Proxy under **External URL** and click **OK**.</span></span>  

   ![Adja hozzá a végpont - állítsa be a megbízható URL-cím érték – képernyőkép](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a><span data-ttu-id="c9169-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9169-131">Next steps</span></span>
* <span data-ttu-id="c9169-132">[Egyszeri bejelentkezés engedélyezése a](application-proxy-sso-overview.md) , amelyek nem jogcímbarát alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="c9169-132">[Enable single-sign on](application-proxy-sso-overview.md) for applications that aren't claims-aware</span></span>
* [<span data-ttu-id="c9169-133">Natív ügyfél alkalmazások toointeract proxy alkalmazások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c9169-133">Enable native client apps toointeract with proxy applications</span></span>](active-directory-application-proxy-native-client.md)


