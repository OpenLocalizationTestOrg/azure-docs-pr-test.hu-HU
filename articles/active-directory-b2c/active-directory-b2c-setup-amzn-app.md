---
title: "Az Azure Active Directory B2C: Amazon konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers az Azure Active Directory B2C által védett alkalmazások Amazon-fiókkal rendelkező."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 60d7c4b76d9d3e86ed535765329abed07f1e5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-amazon-accounts"></a><span data-ttu-id="fd322-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Amazon-fiókkal rendelkező adja meg.</span><span class="sxs-lookup"><span data-stu-id="fd322-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Amazon accounts</span></span>
## <a name="create-an-amazon-application"></a><span data-ttu-id="fd322-104">Az Amazon-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd322-104">Create an Amazon application</span></span>
<span data-ttu-id="fd322-105">toouse Amazon identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate Amazon kérelmet kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="fd322-105">toouse Amazon as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate an Amazon application and supply it with hello right parameters.</span></span> <span data-ttu-id="fd322-106">Az Amazon-fiókkal toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="fd322-106">You need an Amazon account toodo this.</span></span> <span data-ttu-id="fd322-107">Ha még nincs fiókja, beszerezheti a [http://www.amazon.com/](http://www.amazon.com/).</span><span class="sxs-lookup"><span data-stu-id="fd322-107">If you don’t have one, you can get it at [http://www.amazon.com/](http://www.amazon.com/).</span></span>

1. <span data-ttu-id="fd322-108">Nyissa meg toohello [Amazon fejlesztői központ](https://login.amazon.com/) és jelentkezzen be az Amazon fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="fd322-108">Go toohello [Amazon Developer Center](https://login.amazon.com/) and sign in with your Amazon account credentials.</span></span>
2. <span data-ttu-id="fd322-109">Ha még nem tette meg, kattintson a **regisztráció**, hello fejlesztői regisztrációs lépésekkel, és fogadja el a hello házirend.</span><span class="sxs-lookup"><span data-stu-id="fd322-109">If you have not already done so, click **Sign Up**, follow hello developer registration steps, and accept hello policy.</span></span>
3. <span data-ttu-id="fd322-110">Kattintson a **új alkalmazás regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="fd322-110">Click **Register new application**.</span></span>
   
    ![Egy új alkalmazás hello Amazon webhelyen regisztrálása](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. <span data-ttu-id="fd322-112">Adja meg az alkalmazással kapcsolatos információk (**neve**, **leírás**, és **adatvédelmi nyilatkozat URL-címe**), és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="fd322-112">Provide application information (**Name**, **Description**, and **Privacy Notice URL**) and click **Save**.</span></span>
   
    ![Amazon egy új alkalmazás regisztrálásához alkalmazással kapcsolatos adatok megadása](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. <span data-ttu-id="fd322-114">A hello **Webbeállításainak** részben, a Másolás hello értékének **ügyfél-azonosító** és **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="fd322-114">In hello **Web Settings** section, copy hello values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="fd322-115">(Tooclick hello kell **megjelenítése titkos** toosee ez gombra.) Mindkettő kell tooconfigure Amazon-bérlőben identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="fd322-115">(You need tooclick hello **Show Secret** button toosee this.) You need both of them tooconfigure Amazon as an identity provider in your tenant.</span></span> <span data-ttu-id="fd322-116">Kattintson a **szerkesztése** hello szakasz hello alján.</span><span class="sxs-lookup"><span data-stu-id="fd322-116">Click **Edit** at hello bottom of hello section.</span></span> <span data-ttu-id="fd322-117">**Titkos Ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="fd322-117">**Client Secret** is an important security credential.</span></span>
   
    ![Olyan ügyfél-Azonosítót és titkos Ügyfélkulcs az Amazon: új alkalmazás](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. <span data-ttu-id="fd322-119">Adja meg `https://login.microsoftonline.com` a hello **engedélyezett JavaScript források** mező és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **visszatérési URL-címek engedélyezett** mező.</span><span class="sxs-lookup"><span data-stu-id="fd322-119">Enter `https://login.microsoftonline.com` in hello **Allowed JavaScript Origins** field and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Allowed Return URLs** field.</span></span> <span data-ttu-id="fd322-120">Cserélje le **{tenant}** a bérlő nevű (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="fd322-120">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="fd322-121">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fd322-121">Click **Save**.</span></span> <span data-ttu-id="fd322-122">Hello **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="fd322-122">hello **{tenant}** value is case-sensitive.</span></span>
   
    ![JavaScript források és visszatérési URL-címek megadása az új alkalmazást az Amazon:](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="fd322-124">A bérlő az identitás-szolgáltatóként Amazon konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd322-124">Configure Amazon as an identity provider in your tenant</span></span>
1. <span data-ttu-id="fd322-125">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fd322-125">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="fd322-126">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="fd322-126">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="fd322-127">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="fd322-127">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="fd322-128">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="fd322-128">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="fd322-129">Írja be például a "Amzn".</span><span class="sxs-lookup"><span data-stu-id="fd322-129">For example, enter "Amzn".</span></span>
5. <span data-ttu-id="fd322-130">Kattintson a **identitás szolgáltatótípus**, jelölje be **Amazon**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd322-130">Click **Identity provider type**, select **Amazon**, and click **OK**.</span></span>
6. <span data-ttu-id="fd322-131">Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello azonosító és az ügyfél titkos ügyfélkulcs az Amazon alkalmazás korábban létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="fd322-131">Click **Set up this identity provider** and enter hello client ID and client secret of hello Amazon application that you created earlier.</span></span>
7. <span data-ttu-id="fd322-132">Kattintson a **OK** majd **létrehozása** toosave az Amazon konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="fd322-132">Click **OK** and then click **Create** toosave your Amazon configuration.</span></span>

