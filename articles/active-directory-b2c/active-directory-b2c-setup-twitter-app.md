---
title: "Az Azure Active Directory B2C: Twitter konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés az Azure Active Directory B2C által védett alkalmazások Twitter fiókkal rendelkező felhasználók számára."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 82a001dd53cdddcf3b360090f3250af593c96fbb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a><span data-ttu-id="c33e3-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés adhat Twitter-fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="c33e3-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Twitter accounts</span></span>

> [!NOTE]
> <span data-ttu-id="c33e3-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="c33e3-104">This feature is in preview.</span></span>
> 

## <a name="create-a-twitter-application"></a><span data-ttu-id="c33e3-105">Twitter-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c33e3-105">Create a Twitter application</span></span>
<span data-ttu-id="c33e3-106">Az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként Twitter használatához szüksége Twitter-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="c33e3-106">To use Twitter as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Twitter application and supply it with the right parameters.</span></span> <span data-ttu-id="c33e3-107">Ehhez Twitter developer-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="c33e3-107">You need a Twitter developer account to do this.</span></span> <span data-ttu-id="c33e3-108">Ha még nincs fiókja, beszerezheti a [https://dev.twitter.com/](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="c33e3-108">If you don’t have one, you can get it at [https://dev.twitter.com/](https://dev.twitter.com/).</span></span>

1. <span data-ttu-id="c33e3-109">Lépjen a [fejlesztői webhely Twitter](https://dev.twitter.com/) és jelentkezzen be a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c33e3-109">Go to the [Twitter developer's website](https://dev.twitter.com/) and sign in with your credentials.</span></span>
2. <span data-ttu-id="c33e3-110">Kattintson a **alkalmazásaimat** alatt **eszközök & támogatási** , majd **új alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-110">Click **My apps** under **Tools & Support** and then click **Create New App**.</span></span> 
3. <span data-ttu-id="c33e3-111">Ebben a formátumban adja meg az értéket a **neve**, **leírás**, és **webhely**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-111">In the form, provide a value for the **Name**, **Description**, and **Website**.</span></span>
4. <span data-ttu-id="c33e3-112">Az a **visszahívási URL-cím**, adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="c33e3-112">For the **Callback URL**, enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span></span> <span data-ttu-id="c33e3-113">Győződjön meg arról, hogy **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="c33e3-113">Make sure to replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
5. <span data-ttu-id="c33e3-114">A jelölőnégyzet bejelölésével vállalja, hogy a **fejlesztői megállapodás** kattintson **az Twitter-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-114">Check the box to agree to the **Developer Agreement** and click **Create your Twitter application**.</span></span>
6. <span data-ttu-id="c33e3-115">Az alkalmazás létrehozása után kattintson **kulcsok és a hozzáférési jogkivonatok**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-115">Once the app is created, click **Keys and Access Tokens**.</span></span>
7. <span data-ttu-id="c33e3-116">Másolja a értékének **kulcsa** és **felhasználói titok**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-116">Copy the value of **Consumer Key** and **Consumer Secret**.</span></span> <span data-ttu-id="c33e3-117">Mindkettő konfigurálásához Twitter-bérlőben identitás-szolgáltatóként kell.</span><span class="sxs-lookup"><span data-stu-id="c33e3-117">You will need both of them to configure Twitter as an identity provider in your tenant.</span></span>

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="c33e3-118">Twitter-bérlőben identitás-szolgáltatóként konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c33e3-118">Configure Twitter as an identity provider in your tenant</span></span>
1. <span data-ttu-id="c33e3-119">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="c33e3-119">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="c33e3-120">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-120">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="c33e3-121">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="c33e3-121">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="c33e3-122">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="c33e3-122">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="c33e3-123">Adja meg például "Twitter".</span><span class="sxs-lookup"><span data-stu-id="c33e3-123">For example, enter "Twitter".</span></span>
5. <span data-ttu-id="c33e3-124">Kattintson a **identitás szolgáltatótípus**, jelölje be **Twitter**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-124">Click **Identity provider type**, select **Twitter**, and click **OK**.</span></span>
6. <span data-ttu-id="c33e3-125">Kattintson a **az identitásszolgáltató beállítása** , és írja be a Twitteren **kulcsa** a a **ügyfél-azonosító** és a Twitter **felhasználói titok** a a **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="c33e3-125">Click **Set up this identity provider** and enter the Twitter **Consumer Key** for the **Client id** and the Twitter **Consumer Secret** for the **Client secret**.</span></span>
7. <span data-ttu-id="c33e3-126">Kattintson a **OK**, és kattintson a **létrehozása** a Twitter-konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c33e3-126">Click **OK**, and then click **Create** to save your Twitter configuration.</span></span>

