---
title: "Az Azure Active Directory B2C: Amazon konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés az Azure Active Directory B2C által védett alkalmazások Amazon-fiókkal rendelkező felhasználók számára."
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
ms.openlocfilehash: dcc97e1b7f6287bd7692c52bf068950065a26572
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a><span data-ttu-id="b0b3e-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés adhat Amazon-fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="b0b3e-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Amazon accounts</span></span>
## <a name="create-an-amazon-application"></a><span data-ttu-id="b0b3e-104">Az Amazon-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0b3e-104">Create an Amazon application</span></span>
<span data-ttu-id="b0b3e-105">Az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként Amazon használatához szüksége Amazon alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-105">To use Amazon as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create an Amazon application and supply it with the right parameters.</span></span> <span data-ttu-id="b0b3e-106">Ehhez az Amazon-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-106">You need an Amazon account to do this.</span></span> <span data-ttu-id="b0b3e-107">Ha még nincs fiókja, beszerezheti a [http://www.amazon.com/](http://www.amazon.com/).</span><span class="sxs-lookup"><span data-stu-id="b0b3e-107">If you don’t have one, you can get it at [http://www.amazon.com/](http://www.amazon.com/).</span></span>

1. <span data-ttu-id="b0b3e-108">Lépjen a [Amazon fejlesztői központ](https://login.amazon.com/) és jelentkezzen be az Amazon fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-108">Go to the [Amazon Developer Center](https://login.amazon.com/) and sign in with your Amazon account credentials.</span></span>
2. <span data-ttu-id="b0b3e-109">Ha még nem tette meg, kattintson a **regisztráció**, hajtsa végre a fejlesztői regisztrációs lépéseket, és fogadja el a szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-109">If you have not already done so, click **Sign Up**, follow the developer registration steps, and accept the policy.</span></span>
3. <span data-ttu-id="b0b3e-110">Kattintson a **új alkalmazás regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-110">Click **Register new application**.</span></span>
   
    ![Egy új alkalmazást az Amazon webhelyen regisztrálása](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. <span data-ttu-id="b0b3e-112">Adja meg az alkalmazással kapcsolatos információk (**neve**, **leírás**, és **adatvédelmi nyilatkozat URL-címe**), és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-112">Provide application information (**Name**, **Description**, and **Privacy Notice URL**) and click **Save**.</span></span>
   
    ![Amazon egy új alkalmazás regisztrálásához alkalmazással kapcsolatos adatok megadása](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. <span data-ttu-id="b0b3e-114">Az a **Webbeállításainak** területen értékeinek másolására **ügyfél-azonosító** és **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-114">In the **Web Settings** section, copy the values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="b0b3e-115">(Kattintson a **megjelenítése titkos** gombra kattintva megjelenik ez.) Mindkettő konfigurálásához Amazon-bérlőben identitás-szolgáltatóként van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-115">(You need to click the **Show Secret** button to see this.) You need both of them to configure Amazon as an identity provider in your tenant.</span></span> <span data-ttu-id="b0b3e-116">Kattintson a **szerkesztése** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-116">Click **Edit** at the bottom of the section.</span></span> <span data-ttu-id="b0b3e-117">**Titkos Ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-117">**Client Secret** is an important security credential.</span></span>
   
    ![Olyan ügyfél-Azonosítót és titkos Ügyfélkulcs az Amazon: új alkalmazás](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. <span data-ttu-id="b0b3e-119">Adja meg `https://login.microsoftonline.com` a a **engedélyezett JavaScript források** mező és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a a **visszatérési URL-címek engedélyezett** mező.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-119">Enter `https://login.microsoftonline.com` in the **Allowed JavaScript Origins** field and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Allowed Return URLs** field.</span></span> <span data-ttu-id="b0b3e-120">Cserélje le **{tenant}** a bérlő nevű (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="b0b3e-120">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="b0b3e-121">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-121">Click **Save**.</span></span> <span data-ttu-id="b0b3e-122">A **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-122">The **{tenant}** value is case-sensitive.</span></span>
   
    ![JavaScript források és visszatérési URL-címek megadása az új alkalmazást az Amazon:](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="b0b3e-124">A bérlő az identitás-szolgáltatóként Amazon konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0b3e-124">Configure Amazon as an identity provider in your tenant</span></span>
1. <span data-ttu-id="b0b3e-125">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-125">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="b0b3e-126">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-126">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="b0b3e-127">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-127">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="b0b3e-128">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-128">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="b0b3e-129">Írja be például a "Amzn".</span><span class="sxs-lookup"><span data-stu-id="b0b3e-129">For example, enter "Amzn".</span></span>
5. <span data-ttu-id="b0b3e-130">Kattintson a **identitás szolgáltatótípus**, jelölje be **Amazon**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-130">Click **Identity provider type**, select **Amazon**, and click **OK**.</span></span>
6. <span data-ttu-id="b0b3e-131">Kattintson a **az identitásszolgáltató beállítása** , és adja meg az ügyfél-azonosító és a titkos ügyfélkulcs az Amazon-alkalmazás, amelyet korábban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-131">Click **Set up this identity provider** and enter the client ID and client secret of the Amazon application that you created earlier.</span></span>
7. <span data-ttu-id="b0b3e-132">Kattintson a **OK** majd **létrehozása** az Amazon-konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b0b3e-132">Click **OK** and then click **Create** to save your Amazon configuration.</span></span>

