---
title: "Az Azure Active Directory B2C: Twitter konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Twitter-fiókok az Azure Active Directory B2C által védett alkalmazások."
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
ms.openlocfilehash: 275c5c73fd5e8e5075e77fee942cbc1b5e1586cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-twitter-accounts"></a><span data-ttu-id="92204-103">Az Azure Active Directory B2C: Adja meg a regisztráció és bejelentkezés tooconsumers, a Twitter-fiókokkal</span><span class="sxs-lookup"><span data-stu-id="92204-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Twitter accounts</span></span>

> [!NOTE]
> <span data-ttu-id="92204-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="92204-104">This feature is in preview.</span></span>
> 

## <a name="create-a-twitter-application"></a><span data-ttu-id="92204-105">Twitter-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="92204-105">Create a Twitter application</span></span>
<span data-ttu-id="92204-106">az Azure Active Directory (Azure AD) B2C-ben identitás-szolgáltatóként toouse Twitter, toocreate egy Twitter-alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="92204-106">toouse Twitter as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Twitter application and supply it with hello right parameters.</span></span> <span data-ttu-id="92204-107">Egy Twitter fejlesztői fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="92204-107">You need a Twitter developer account toodo this.</span></span> <span data-ttu-id="92204-108">Ha még nincs fiókja, beszerezheti a [https://dev.twitter.com/](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="92204-108">If you don’t have one, you can get it at [https://dev.twitter.com/](https://dev.twitter.com/).</span></span>

1. <span data-ttu-id="92204-109">Nyissa meg toohello [fejlesztői webhely Twitter](https://dev.twitter.com/) és jelentkezzen be a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="92204-109">Go toohello [Twitter developer's website](https://dev.twitter.com/) and sign in with your credentials.</span></span>
2. <span data-ttu-id="92204-110">Kattintson a **alkalmazásaimat** alatt **eszközök & támogatási** , majd **új alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="92204-110">Click **My apps** under **Tools & Support** and then click **Create New App**.</span></span> 
3. <span data-ttu-id="92204-111">Hello képernyőn adjon meg egy értéket hello **neve**, **leírás**, és **webhely**.</span><span class="sxs-lookup"><span data-stu-id="92204-111">In hello form, provide a value for hello **Name**, **Description**, and **Website**.</span></span>
4. <span data-ttu-id="92204-112">A hello **visszahívási URL-cím**, adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="92204-112">For hello **Callback URL**, enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span></span> <span data-ttu-id="92204-113">Győződjön meg arról, hogy tooreplace **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="92204-113">Make sure tooreplace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
5. <span data-ttu-id="92204-114">Ellenőrizze hello mezőben tooagree toohello **fejlesztői megállapodás** kattintson **az Twitter-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="92204-114">Check hello box tooagree toohello **Developer Agreement** and click **Create your Twitter application**.</span></span>
6. <span data-ttu-id="92204-115">Hello alkalmazás létrehozása után kattintson **kulcsok és a hozzáférési jogkivonatok**.</span><span class="sxs-lookup"><span data-stu-id="92204-115">Once hello app is created, click **Keys and Access Tokens**.</span></span>
7. <span data-ttu-id="92204-116">Másolja a hello értékének **kulcsa** és **felhasználói titok**.</span><span class="sxs-lookup"><span data-stu-id="92204-116">Copy hello value of **Consumer Key** and **Consumer Secret**.</span></span> <span data-ttu-id="92204-117">Szüksége lesz mindkettő tooconfigure Twitter-bérlőben identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="92204-117">You will need both of them tooconfigure Twitter as an identity provider in your tenant.</span></span>

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="92204-118">Twitter-bérlőben identitás-szolgáltatóként konfigurálása</span><span class="sxs-lookup"><span data-stu-id="92204-118">Configure Twitter as an identity provider in your tenant</span></span>
1. <span data-ttu-id="92204-119">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="92204-119">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="92204-120">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="92204-120">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="92204-121">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="92204-121">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="92204-122">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="92204-122">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="92204-123">Adja meg például "Twitter".</span><span class="sxs-lookup"><span data-stu-id="92204-123">For example, enter "Twitter".</span></span>
5. <span data-ttu-id="92204-124">Kattintson a **identitás szolgáltatótípus**, jelölje be **Twitter**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="92204-124">Click **Identity provider type**, select **Twitter**, and click **OK**.</span></span>
6. <span data-ttu-id="92204-125">Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello Twitter **kulcsa** a hello **ügyfél-azonosító** és a hello Twitter **felhasználói titok**a hello **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="92204-125">Click **Set up this identity provider** and enter hello Twitter **Consumer Key** for hello **Client id** and hello Twitter **Consumer Secret** for hello **Client secret**.</span></span>
7. <span data-ttu-id="92204-126">Kattintson a **OK**, és kattintson a **létrehozása** toosave a Twitter-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="92204-126">Click **OK**, and then click **Create** toosave your Twitter configuration.</span></span>

