---
title: "Az alkalmazás regisztrálása az Azure AD v2.0-végpontra a portál használatával |} Microsoft Docs"
description: "Alkalmazás regisztrálása a Microsoft bejelentkezés engedélyezése és használata a Microsoft-szolgáltatásokat a v2.0-végpontra segítségével"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: e6202aa8665c906382666fe08a561421e50e0a8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a><span data-ttu-id="f30a1-103">Alkalmazás regisztrálása a v2.0-végponttal</span><span class="sxs-lookup"><span data-stu-id="f30a1-103">How to register an app with the v2.0 endpoint</span></span>
<span data-ttu-id="f30a1-104">Msa-t és az Azure AD is elfogadó alkalmazás létrehozásához bejelentkezés, először regisztrálja az alkalmazást a Microsoft számára.</span><span class="sxs-lookup"><span data-stu-id="f30a1-104">To build an app that accepts both MSA & Azure AD sign-in, you'll first need to register an app with Microsoft.</span></span>  <span data-ttu-id="f30a1-105">Most, akkor használhatja a meglévő alkalmazásokat, előfordulhat, hogy az Azure ad-vel vagy MSA - szüksége lesz egy teljesen új létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f30a1-105">At this time, you won't be able to use any existing apps you may have with Azure AD or MSA - you'll need to create a brand new one.</span></span>

> [!NOTE]
> <span data-ttu-id="f30a1-106">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="f30a1-106">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="f30a1-107">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="f30a1-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a><span data-ttu-id="f30a1-108">Látogasson el a Microsoft app-regisztrálási portál</span><span class="sxs-lookup"><span data-stu-id="f30a1-108">Visit the Microsoft app registration portal</span></span>
<span data-ttu-id="f30a1-109">Első lépések először - navigáljon [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="f30a1-109">First things first - navigate to [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="f30a1-110">Ez az egy új app regisztrációs portált, ahol kezelheti a Microsoft-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f30a1-110">This is a new app registration portal where you can manage your Microsoft apps.</span></span>

<span data-ttu-id="f30a1-111">Jelentkezzen be vagy egy személyes vagy a munkahelyi vagy iskolai Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="f30a1-111">Sign in with either a personal or work or school Microsoft account.</span></span>  <span data-ttu-id="f30a1-112">Ha még nem rendelkezik, vagy egy új személyes fiók.</span><span class="sxs-lookup"><span data-stu-id="f30a1-112">If you don't have either, sign up for a new personal account.</span></span> <span data-ttu-id="f30a1-113">Lépjen tovább, nem tart sokáig - itt fogja várunk.</span><span class="sxs-lookup"><span data-stu-id="f30a1-113">Go ahead, it won't take long - we'll wait here.</span></span>

<span data-ttu-id="f30a1-114">Tenni?</span><span class="sxs-lookup"><span data-stu-id="f30a1-114">Done?</span></span> <span data-ttu-id="f30a1-115">Meg kell most megtekintik a Microsoft-alkalmazások listája olyan valószínűleg üres.</span><span class="sxs-lookup"><span data-stu-id="f30a1-115">You should now be looking at your list of Microsoft apps, which is probably empty.</span></span>  <span data-ttu-id="f30a1-116">Módosítsuk, amely.</span><span class="sxs-lookup"><span data-stu-id="f30a1-116">Let's change that.</span></span>

<span data-ttu-id="f30a1-117">Kattintson a **hozzáadhat egy alkalmazást**, és adjon neki egy nevet.</span><span class="sxs-lookup"><span data-stu-id="f30a1-117">Click **Add an app**, and give it a name.</span></span>  <span data-ttu-id="f30a1-118">A portál rendeli az alkalmazás egy globálisan egyedi azonosítója, amelyet később a kódjában fog használni.</span><span class="sxs-lookup"><span data-stu-id="f30a1-118">The portal will assign your app a globally unique  Application Id that you'll use later in your code.</span></span>  <span data-ttu-id="f30a1-119">Ha az alkalmazás olyan kiszolgálóoldali összetevőt tartalmaz, hogy kell-e hozzáférési jogkivonatok hívó API-k (gondolja, hogy: Office, az Azure vagy a saját webes API-t), érdemes létrehozni egy **Alkalmazáskulcsot** itt is.</span><span class="sxs-lookup"><span data-stu-id="f30a1-119">If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want to create an **Application Secret** here as well.</span></span>

<span data-ttu-id="f30a1-120">Ezután adja hozzá a platformokat, amelyek az alkalmazás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f30a1-120">Next, add the Platforms that your app will use.</span></span>

* <span data-ttu-id="f30a1-121">Webalapú alkalmazások esetén adja meg a **átirányítási URI-** ahol bejelentkezési üzenetek elküldhetők.</span><span class="sxs-lookup"><span data-stu-id="f30a1-121">For web based apps, provide a **Redirect URI** where sign-in messages can be sent.</span></span>
* <span data-ttu-id="f30a1-122">Mobilalkalmazások másolja le az automatikusan létrehozott, alapértelmezett átirányítási uri.</span><span class="sxs-lookup"><span data-stu-id="f30a1-122">For mobile apps, copy down the default redirect uri automatically created for you.</span></span>

<span data-ttu-id="f30a1-123">Másik lehetőségként testre szabhatja a bejelentkezési oldal a profil szakaszban megjelenését és működését.</span><span class="sxs-lookup"><span data-stu-id="f30a1-123">Optionally, you can customize the look and feel of your sign-in page in the Profile section.</span></span>  <span data-ttu-id="f30a1-124">Győződjön meg arról, hogy kattintson **mentése** lépés előtt.</span><span class="sxs-lookup"><span data-stu-id="f30a1-124">Make sure to click **Save** before moving on.</span></span>

> [!NOTE]
> <span data-ttu-id="f30a1-125">Amikor hoz létre egy alkalmazást, amely [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), az alkalmazás regisztrálva lesz, a home bérlői fiók használatával jelentkezzen be a portálra.</span><span class="sxs-lookup"><span data-stu-id="f30a1-125">When you create an application using [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), the application will be registered in the home tenant of the account that you use to sign into the portal.</span></span>  <span data-ttu-id="f30a1-126">Ez azt jelenti, hogy nem regisztrálhatja az alkalmazás az Azure AD-bérlő személyes Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="f30a1-126">This means that you can not register an application in your Azure AD tenant using a personal Microsoft account.</span></span>  <span data-ttu-id="f30a1-127">Ha explicit módon szeretne egy alkalmazást regisztrálni kell az egy adott bérlő, olyan fiókkal jelentkezzen be, hogy a bérlő eredetileg létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f30a1-127">If you explicitly wish to register an application in a particular tenant, sign in with an account originally created in that tenant.</span></span>
> 
> 

## <a name="build-a-quick-start-app"></a><span data-ttu-id="f30a1-128">A gyors üzembe helyezés alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f30a1-128">Build a quick start app</span></span>
<span data-ttu-id="f30a1-129">Most, hogy a Microsoft app, befejezheti a v2.0 gyors üzembe helyezési oktatóprogramok valamelyikét.</span><span class="sxs-lookup"><span data-stu-id="f30a1-129">Now that you have a Microsoft app, you can complete one of our v2.0 quick start tutorials.</span></span>  <span data-ttu-id="f30a1-130">Az alábbiakban néhány javaslatot olvashat:</span><span class="sxs-lookup"><span data-stu-id="f30a1-130">Here are a few recommendations:</span></span>

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

