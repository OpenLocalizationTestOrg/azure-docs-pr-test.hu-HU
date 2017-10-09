---
title: "aaaApplication lapján nem jelennek meg helyesen az alkalmazásproxy-alkalmazás |} Microsoft Docs"
description: "Amikor hello lap nem megfelelően jelennek meg az alkalmazás Proxy alkalmazás útmutatást integrálva van az Azure AD"
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
ms.openlocfilehash: f4abaa4e94c512868f2085affe59cac443784a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a><span data-ttu-id="10148-103">Alkalmazás lapján nem jelennek meg helyesen az alkalmazásproxy-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="10148-103">Application page does not display correctly for an Application Proxy application</span></span>

<span data-ttu-id="10148-104">Ez a cikk segítséget Azure Active Directory Alkalmazásproxyjával alkalmazások tootroubleshoot problémái toohello lap Navigálás, de hello oldalon valami mégsem helyes-e.</span><span class="sxs-lookup"><span data-stu-id="10148-104">This article help you tootroubleshoot issues with Azure Active Directory Application Proxy applications when you navigate toohello page, but something on hello page doesn't look correct.</span></span>

## <a name="overview"></a><span data-ttu-id="10148-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="10148-105">Overview</span></span>
<span data-ttu-id="10148-106">Alkalmazásproxy alkalmazások közzététele, amikor csak a legfelső szintű lapok érhetők el hello alkalmazáshoz való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="10148-106">When you publish an Application Proxy app, only pages under your root are accessible when accessing hello application.</span></span> <span data-ttu-id="10148-107">Hello lapján nem jelennek meg megfelelően, ha lehet, hogy hello legfelső szintű belső használt URL-cím hello alkalmazás hiányzik néhány lap erőforrást.</span><span class="sxs-lookup"><span data-stu-id="10148-107">If hello page isn’t displaying correctly, hello root internal URL used for hello application may be missing some page resources.</span></span> <span data-ttu-id="10148-108">tooresolve, győződjön meg arról, hogy a közzétett *összes* hello lap erőforrások hello az alkalmazás részeként.</span><span class="sxs-lookup"><span data-stu-id="10148-108">tooresolve, make sure you have published *all* hello resources for hello page as part of your application.</span></span>

<span data-ttu-id="10148-109">Ez a jelenség hello a hálózati követő megnyitásával ellenőrizheti (például a Fiddler, vagy az F12 eszközök Internet Explorer vagy Edge) hello lap és 404-es hibákat keres.</span><span class="sxs-lookup"><span data-stu-id="10148-109">You can verify this is hello issue by opening your network tracker (such as Fiddler, or F12 tools in Internet Explorer/Edge), loading hello page, and looking for 404 errors.</span></span> <span data-ttu-id="10148-110">Azt jelzi, hogy hello lapok, amelyek jelenleg nem található, és közzétett toobe lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="10148-110">That indicates hello pages that currently cannot be found and may still need toobe published.</span></span>

<span data-ttu-id="10148-111">Ebben az esetben, például azt feltételezik, miután közzétette költségek kérelmet egy belső URL-címe használatával <http://myapps/expenses>, de hello alkalmazása használja-e hello stíluslap <http://myapps/style.css>.</span><span class="sxs-lookup"><span data-stu-id="10148-111">As an example of this case, assume you have published an expenses application using an internal URL of <http://myapps/expenses>, but hello app uses hello stylesheet <http://myapps/style.css>.</span></span> <span data-ttu-id="10148-112">Ebben az esetben hello stíluslap nincs közzétéve az alkalmazásban, a 404-es hiba történt miközben a rendszer tooload style.css hello költségek app betöltése kivételt.</span><span class="sxs-lookup"><span data-stu-id="10148-112">In this case, hello stylesheet is not published in your application, so loading hello expenses app throw a 404 error while trying tooload style.css.</span></span> <span data-ttu-id="10148-113">Ebben a példában hello probléma szeretné feloldani egy belső URL-címével hello alkalmazás közzététele <http://myapp/> helyette.</span><span class="sxs-lookup"><span data-stu-id="10148-113">In this example, hello problem would be resolved by publishing hello application with an internal URL of <http://myapp/> instead.</span></span>

## <a name="problems-with-publishing-as-one-application"></a><span data-ttu-id="10148-114">Egy alkalmazás közzétételi kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="10148-114">Problems with publishing as one application</span></span>

<span data-ttu-id="10148-115">Ha még nincs lehetséges toopublish belül minden erőforrás hello ugyanahhoz az alkalmazáshoz, kell toopublish több alkalmazás és a hivatkozások között.</span><span class="sxs-lookup"><span data-stu-id="10148-115">If it is not possible toopublish all resources within hello same application, you need toopublish multiple applications and enable links between them.</span></span>

<span data-ttu-id="10148-116">toodo Igen, azt javasoljuk, hello [egyéni tartományok](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) megoldás.</span><span class="sxs-lookup"><span data-stu-id="10148-116">toodo so, we recommend using hello [custom domains](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) solution.</span></span> <span data-ttu-id="10148-117">Azonban az ehhez a megoldáshoz szükségesek, hogy Ön a tulajdonosa hello tanúsítványt a tartomány és az alkalmazások teljes tartományneveit (FQDN) használja.</span><span class="sxs-lookup"><span data-stu-id="10148-117">However, this solution requires that you own hello certificate for your domain and your applications use fully qualified domain names (FQDNs).</span></span> <span data-ttu-id="10148-118">Egyéb lehetőségek, lásd: hello [hivatkozások dokumentáció hibaelhárítása](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span><span class="sxs-lookup"><span data-stu-id="10148-118">For other options, see hello [troubleshoot broken links documentation](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span></span>

## <a name="next-steps"></a><span data-ttu-id="10148-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10148-119">Next steps</span></span>
[<span data-ttu-id="10148-120">Az Azure AD-alkalmazásproxy használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="10148-120">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
