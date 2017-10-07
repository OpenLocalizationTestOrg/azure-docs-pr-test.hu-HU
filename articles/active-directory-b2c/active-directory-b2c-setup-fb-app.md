---
title: "Az Azure Active Directory B2C: Facebook konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers az Azure Active Directory B2C által védett alkalmazások Facebook-fiókkal rendelkező."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 4c3b79c7248bd1e789bf33fc467abb27d0170edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-facebook-accounts"></a><span data-ttu-id="d9c21-103">Az Azure Active Directory B2C: Tooconsumers regisztráció és bejelentkezés Facebook-fiókkal rendelkező adja meg.</span><span class="sxs-lookup"><span data-stu-id="d9c21-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Facebook accounts</span></span>
## <a name="create-a-facebook-application"></a><span data-ttu-id="d9c21-104">Facebook-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9c21-104">Create a Facebook application</span></span>
<span data-ttu-id="d9c21-105">toouse Facebook identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, kell toocreate Facebook-alkalmazás, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="d9c21-105">toouse Facebook as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Facebook application and supply it with hello right parameters.</span></span> <span data-ttu-id="d9c21-106">A Facebook-fiókkal toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="d9c21-106">You need a Facebook account toodo this.</span></span> <span data-ttu-id="d9c21-107">Ha még nincs fiókja, beszerezheti a [https://www.facebook.com/](https://www.facebook.com/).</span><span class="sxs-lookup"><span data-stu-id="d9c21-107">If you don’t have one, you can get it at [https://www.facebook.com/](https://www.facebook.com/).</span></span>

1. <span data-ttu-id="d9c21-108">Nyissa meg toohello [Facebook-fejlesztőknek](https://developers.facebook.com/) webhelyet, és jelentkezzen be a Facebook-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="d9c21-108">Go toohello [Facebook for developers](https://developers.facebook.com/) website and sign in with your Facebook account credentials.</span></span>
2. <span data-ttu-id="d9c21-109">Ha még nem tette meg, Facebook-fejlesztőként kell tooregister.</span><span class="sxs-lookup"><span data-stu-id="d9c21-109">If you have not already done so, you need tooregister as a Facebook developer.</span></span> <span data-ttu-id="d9c21-110">toodo, kattintson **regisztrálása** (a hello jobb felső sarkában hello oldalon), fogadja el a Facebook a házirendeket, és hello regisztráció végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d9c21-110">toodo this, click **Register** (on hello upper-right corner of hello page), accept Facebook's policies, and complete hello registration steps.</span></span>
3. <span data-ttu-id="d9c21-111">Kattintson a **saját alkalmazások** majd **fel egy új alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-111">Click **My Apps** and then click **Add a New App**.</span></span> 
4. <span data-ttu-id="d9c21-112">Hello formában adja meg a **megjelenített név** és egy érvényes **kapcsolattartó E-mail**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-112">In hello form, provide a **Display Name** and a valid **Contact Email**.</span></span>
5. <span data-ttu-id="d9c21-113">Kattintson a **App ID Azonosítójának létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-113">Click **Create App ID**.</span></span> <span data-ttu-id="d9c21-114">Ez tooaccept Facebook platform házirendek megkövetelik, és az online biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="d9c21-114">This may require you tooaccept Facebook platform policies and complete an online security check.</span></span>
6. <span data-ttu-id="d9c21-115">Hello bal oldali oszlopban, kattintson a **beállítások** majd **alapvető** Ha nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="d9c21-115">In hello left column, click **Settings** and then select **Basic** if not selected already.</span></span>
7. <span data-ttu-id="d9c21-116">Válassza ki a **kategória**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-116">Select a **Category**.</span></span> 
8. <span data-ttu-id="d9c21-117">Kattintson a **+ Hozzáadás Platform** válassza **webhely**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-117">Click **+ Add Platform** and select **Website**.</span></span>
   
    ![Facebook - beállítások](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - beállítások - webhely](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. <span data-ttu-id="d9c21-120">Adja meg `https://login.microsoftonline.com/` a hello **webhely URL-címe** mezőben, majd kattintson **módosítások mentése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="d9c21-120">Enter `https://login.microsoftonline.com/` in hello **Site URL** field and then click **Save Changes** at hello bottom of hello page.</span></span>
   
    ![Facebook - webhely URL-címe](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. <span data-ttu-id="d9c21-122">Másolja a hello értékének **Alkalmazásazonosító**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-122">Copy hello value of **App ID**.</span></span> <span data-ttu-id="d9c21-123">Kattintson a **megjelenítése** , és másolja a hello értékének **alkalmazás titkos kulcs**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-123">Click **Show** and copy hello value of **App Secret**.</span></span> <span data-ttu-id="d9c21-124">Szüksége lesz mindkettő tooconfigure Facebook-bérlőben identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="d9c21-124">You will need both of them tooconfigure Facebook as an identity provider in your tenant.</span></span> <span data-ttu-id="d9c21-125">**Alkalmazás titkos kulcs** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="d9c21-125">**App Secret** is an important security credential.</span></span>
   
    ![Facebook - Alkalmazásazonosító & alkalmazás titkos kulcs](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. <span data-ttu-id="d9c21-127">Kattintson a **+ Hozzáadás termék** a bal oldali navigációs hello és majd hello **Set Up** gombra kattint, a **Facebook bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-127">Click **+ Add Product** on hello left navigation and then hello **Set Up** button for **Facebook Login**.</span></span>
   
    ![Facebook - Facebook-bejelentkezés](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. <span data-ttu-id="d9c21-129">Kattintson a **beállítások** hello jobb nav alatt a **Facebook-bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="d9c21-129">Click **Settings** on hello right nav under **Facebook Login**</span></span>

    ![Facebook - Facebook bejelentkezési beállítások](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. <span data-ttu-id="d9c21-131">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **érvényes OAuth-alapú átirányítási URI-azonosítók** hello mezőjét **OAuth ügyfélbeállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d9c21-131">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Valid OAuth redirect URIs** field in hello **Client OAuth Settings** section.</span></span> <span data-ttu-id="d9c21-132">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="d9c21-132">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="d9c21-133">Kattintson a **módosítások mentése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="d9c21-133">Click **Save Changes** at hello bottom of hello page.</span></span>
    
    ![Facebook - OAuth átirányítási URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. <span data-ttu-id="d9c21-135">toomake a Facebook-alkalmazás az Azure AD B2C tárolóhálózatnak kell toomake nyilvánosan elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="d9c21-135">toomake your Facebook application usable by Azure AD B2C, you need toomake it publicly available.</span></span> <span data-ttu-id="d9c21-136">Ehhez kattintson **App felülvizsgálati** a bal oldali navigációs hello és indításának hello kapcsoló hello oldal hello tetején túl**Igen** elemre kattintva **megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-136">You can do this by clicking **App Review** on hello left navigation and by turning hello switch at hello top of hello page too**YES** and clicking **Confirm**.</span></span>
    
    ![Facebook - alkalmazást nyilvános](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="d9c21-138">Konfigurálja a Facebook-bérlőben identitás-szolgáltatóként</span><span class="sxs-lookup"><span data-stu-id="d9c21-138">Configure Facebook as an identity provider in your tenant</span></span>
1. <span data-ttu-id="d9c21-139">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d9c21-139">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="d9c21-140">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-140">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="d9c21-141">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="d9c21-141">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="d9c21-142">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d9c21-142">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="d9c21-143">Írja be például a "Facebook".</span><span class="sxs-lookup"><span data-stu-id="d9c21-143">For example, enter "Facebook".</span></span>
5. <span data-ttu-id="d9c21-144">Kattintson a **identitás szolgáltatótípus**, jelölje be **Facebook**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9c21-144">Click **Identity provider type**, select **Facebook**, and click **OK**.</span></span>
6. <span data-ttu-id="d9c21-145">Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello app ID és az alkalmazás titkos kulcs (a korábban létrehozott Facebook-alkalmazás hello) a hello **ügyfél-azonosító** és **ügyfélkulcs**mezők kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="d9c21-145">Click **Set up this identity provider** and enter hello app ID and app secret (of hello Facebook application that you created earlier) in hello **Client ID** and **Client secret** fields respectively.</span></span>
7. <span data-ttu-id="d9c21-146">Kattintson a **OK**, és kattintson a **létrehozása** toosave a Facebook-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="d9c21-146">Click **OK**, and then click **Create** toosave your Facebook configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="d9c21-147">Hozzáadása egy **identitásszolgáltató** tooyour bérlő nem módosítja a meglévő szabályzatokat.</span><span class="sxs-lookup"><span data-stu-id="d9c21-147">Adding an **Identity provider** tooyour tenant does not modify your existing policies.</span></span> <span data-ttu-id="d9c21-148">Ne felejtse el tooupdate a házirendek újonnan létrehozott hello identitásszolgáltató-ot.</span><span class="sxs-lookup"><span data-stu-id="d9c21-148">Remember tooupdate your policies by including hello identity provider you just created.</span></span>
>