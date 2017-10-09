---
title: "aaaRegister alkalmazást a hello Azure AD v2.0-végponttól hello portál használatával |} Microsoft Docs"
description: "Hogyan tooregister egy alkalmazást a Microsoft bejelentkezés engedélyezése és használata a Microsoft-szolgáltatások hello v2.0-végponttól"
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
ms.openlocfilehash: c56c98906656062435516e820cb318a04c03149c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooregister-an-app-with-hello-v20-endpoint"></a><span data-ttu-id="d43f2-103">Hogyan tooregister hello v2.0-végponttól alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d43f2-103">How tooregister an app with hello v2.0 endpoint</span></span>
<span data-ttu-id="d43f2-104">egy alkalmazás, amely egyaránt msa-t, és az Azure AD fogadja toobuild bejelentkezés, először egy alkalmazást a Microsoft tooregister.</span><span class="sxs-lookup"><span data-stu-id="d43f2-104">toobuild an app that accepts both MSA & Azure AD sign-in, you'll first need tooregister an app with Microsoft.</span></span>  <span data-ttu-id="d43f2-105">Jelenleg nem képes toouse kell a meglévő alkalmazások, előfordulhat, hogy az Azure ad-vel vagy MSA - szüksége lesz egy újat márka toocreate.</span><span class="sxs-lookup"><span data-stu-id="d43f2-105">At this time, you won't be able toouse any existing apps you may have with Azure AD or MSA - you'll need toocreate a brand new one.</span></span>

> [!NOTE]
> <span data-ttu-id="d43f2-106">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="d43f2-106">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="d43f2-107">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d43f2-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="visit-hello-microsoft-app-registration-portal"></a><span data-ttu-id="d43f2-108">A Microsoft hello Microsoft app regisztrációs portál</span><span class="sxs-lookup"><span data-stu-id="d43f2-108">Visit hello Microsoft app registration portal</span></span>
<span data-ttu-id="d43f2-109">Első lépések először - lépjen a túl[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="d43f2-109">First things first - navigate too[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="d43f2-110">Ez az egy új app regisztrációs portált, ahol kezelheti a Microsoft-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="d43f2-110">This is a new app registration portal where you can manage your Microsoft apps.</span></span>

<span data-ttu-id="d43f2-111">Jelentkezzen be vagy egy személyes vagy a munkahelyi vagy iskolai Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d43f2-111">Sign in with either a personal or work or school Microsoft account.</span></span>  <span data-ttu-id="d43f2-112">Ha még nem rendelkezik, vagy egy új személyes fiók.</span><span class="sxs-lookup"><span data-stu-id="d43f2-112">If you don't have either, sign up for a new personal account.</span></span> <span data-ttu-id="d43f2-113">Lépjen tovább, nem tart sokáig - itt fogja várunk.</span><span class="sxs-lookup"><span data-stu-id="d43f2-113">Go ahead, it won't take long - we'll wait here.</span></span>

<span data-ttu-id="d43f2-114">Tenni?</span><span class="sxs-lookup"><span data-stu-id="d43f2-114">Done?</span></span> <span data-ttu-id="d43f2-115">Meg kell most megtekintik a Microsoft-alkalmazások listája olyan valószínűleg üres.</span><span class="sxs-lookup"><span data-stu-id="d43f2-115">You should now be looking at your list of Microsoft apps, which is probably empty.</span></span>  <span data-ttu-id="d43f2-116">Módosítsuk, amely.</span><span class="sxs-lookup"><span data-stu-id="d43f2-116">Let's change that.</span></span>

<span data-ttu-id="d43f2-117">Kattintson a **hozzáadhat egy alkalmazást**, és adjon neki egy nevet.</span><span class="sxs-lookup"><span data-stu-id="d43f2-117">Click **Add an app**, and give it a name.</span></span>  <span data-ttu-id="d43f2-118">hello portal rendeli az alkalmazás egy globálisan egyedi azonosítója, amelyet később a kódjában fog használni.</span><span class="sxs-lookup"><span data-stu-id="d43f2-118">hello portal will assign your app a globally unique  Application Id that you'll use later in your code.</span></span>  <span data-ttu-id="d43f2-119">Ha az alkalmazás olyan kiszolgálóoldali összetevőt tartalmaz, hogy kell-e hozzáférési jogkivonatok hívó API-k (gondolja, hogy: Office, az Azure vagy a saját webes API-t), érdemes toocreate egy **Alkalmazáskulcsot** itt is.</span><span class="sxs-lookup"><span data-stu-id="d43f2-119">If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want toocreate an **Application Secret** here as well.</span></span>

<span data-ttu-id="d43f2-120">Ezután adja hozzá a hello platformokat, amelyek az alkalmazás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d43f2-120">Next, add hello Platforms that your app will use.</span></span>

* <span data-ttu-id="d43f2-121">Webalapú alkalmazások esetén adja meg a **átirányítási URI-** ahol bejelentkezési üzenetek elküldhetők.</span><span class="sxs-lookup"><span data-stu-id="d43f2-121">For web based apps, provide a **Redirect URI** where sign-in messages can be sent.</span></span>
* <span data-ttu-id="d43f2-122">A mobile apps szolgáltatásban a másolja le hello alapértelmezett átirányítási URI-címe, automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="d43f2-122">For mobile apps, copy down hello default redirect uri automatically created for you.</span></span>

<span data-ttu-id="d43f2-123">Szükség esetén testre szabhatja, a bejelentkezési oldal a hello profilhoz című hello megjelenését és működését.</span><span class="sxs-lookup"><span data-stu-id="d43f2-123">Optionally, you can customize hello look and feel of your sign-in page in hello Profile section.</span></span>  <span data-ttu-id="d43f2-124">Győződjön meg arról, hogy tooclick **mentése** lépés előtt.</span><span class="sxs-lookup"><span data-stu-id="d43f2-124">Make sure tooclick **Save** before moving on.</span></span>

> [!NOTE]
> <span data-ttu-id="d43f2-125">Amikor hoz létre egy alkalmazást, amely [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello otthoni bérlői hello, hogy használni kívánt fiók toosign hello portált hello alkalmazás regisztrálva lesz.</span><span class="sxs-lookup"><span data-stu-id="d43f2-125">When you create an application using [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello application will be registered in hello home tenant of hello account that you use toosign into hello portal.</span></span>  <span data-ttu-id="d43f2-126">Ez azt jelenti, hogy nem regisztrálhatja az alkalmazás az Azure AD-bérlő személyes Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d43f2-126">This means that you can not register an application in your Azure AD tenant using a personal Microsoft account.</span></span>  <span data-ttu-id="d43f2-127">Ha explicit módon kívánja tooregister egy alkalmazás egy adott bérlőn, olyan fiókkal jelentkezzen be, hogy a bérlő eredetileg létrehozott.</span><span class="sxs-lookup"><span data-stu-id="d43f2-127">If you explicitly wish tooregister an application in a particular tenant, sign in with an account originally created in that tenant.</span></span>
> 
> 

## <a name="build-a-quick-start-app"></a><span data-ttu-id="d43f2-128">A gyors üzembe helyezés alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d43f2-128">Build a quick start app</span></span>
<span data-ttu-id="d43f2-129">Most, hogy a Microsoft app, befejezheti a v2.0 gyors üzembe helyezési oktatóprogramok valamelyikét.</span><span class="sxs-lookup"><span data-stu-id="d43f2-129">Now that you have a Microsoft app, you can complete one of our v2.0 quick start tutorials.</span></span>  <span data-ttu-id="d43f2-130">Az alábbiakban néhány javaslatot olvashat:</span><span class="sxs-lookup"><span data-stu-id="d43f2-130">Here are a few recommendations:</span></span>

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

