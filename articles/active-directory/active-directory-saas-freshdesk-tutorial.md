---
title: "Oktatóanyag: Azure Active Directoryval integrált FreshDesk |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FreshDesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 577a5eb6d9b1bc03030a2b47f63d375869c903bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a><span data-ttu-id="1a87d-103">Oktatóanyag: Azure Active Directoryval integrált FreshDesk</span><span class="sxs-lookup"><span data-stu-id="1a87d-103">Tutorial: Azure Active Directory integration with FreshDesk</span></span>

<span data-ttu-id="1a87d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FreshDesk az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1a87d-104">In this tutorial, you learn how toointegrate FreshDesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a87d-105">FreshDesk integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="1a87d-105">Integrating FreshDesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1a87d-106">Megadhatja a hozzáférés tooFreshDesk rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1a87d-106">You can control in Azure AD who has access tooFreshDesk</span></span>
- <span data-ttu-id="1a87d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFreshDesk (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1a87d-107">You can enable your users tooautomatically get signed-on tooFreshDesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1a87d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="1a87d-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="1a87d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a87d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a87d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1a87d-110">Prerequisites</span></span>

<span data-ttu-id="1a87d-111">az Azure AD integrálása FreshDesk tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1a87d-111">tooconfigure Azure AD integration with FreshDesk, you need hello following items:</span></span>

- <span data-ttu-id="1a87d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1a87d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a87d-113">Egy FreshDesk egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1a87d-113">A FreshDesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a87d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1a87d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a87d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="1a87d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a87d-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1a87d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="1a87d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a87d-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a87d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1a87d-118">Scenario description</span></span>
<span data-ttu-id="1a87d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1a87d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a87d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1a87d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a87d-121">Hello gyűjteményből FreshDesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a87d-121">Adding FreshDesk from hello gallery</span></span>
2. <span data-ttu-id="1a87d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a87d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshdesk-from-hello-gallery"></a><span data-ttu-id="1a87d-123">Hello gyűjteményből FreshDesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a87d-123">Adding FreshDesk from hello gallery</span></span>
<span data-ttu-id="1a87d-124">tooconfigure hello integrációja FreshDesk az Azure AD-be, meg kell tooadd FreshDesk hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="1a87d-124">tooconfigure hello integration of FreshDesk into Azure AD, you need tooadd FreshDesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1a87d-125">**tooadd FreshDesk hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1a87d-125">**tooadd FreshDesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a87d-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1a87d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1a87d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1a87d-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="1a87d-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1a87d-133">Hello keresési mezőbe, írja be a **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-133">In hello search box, type **FreshDesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. <span data-ttu-id="1a87d-135">A hello eredmények panelen válassza ki a **FreshDesk**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-135">In hello results panel, select **FreshDesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1a87d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a87d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1a87d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="1a87d-138">In this section, you configure and test Azure AD single sign-on with FreshDesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1a87d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FreshDesk tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1a87d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FreshDesk is tooa user in Azure AD.</span></span> <span data-ttu-id="1a87d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FreshDesk közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="1a87d-140">In other words, a link relationship between an Azure AD user and hello related user in FreshDesk needs toobe established.</span></span>

<span data-ttu-id="1a87d-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="1a87d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FreshDesk.</span></span>

<span data-ttu-id="1a87d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés FreshDesk-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1a87d-142">tooconfigure and test Azure AD single sign-on with FreshDesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1a87d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1a87d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1a87d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a87d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a87d-145">**[FreshDesk tesztfelhasználó létrehozása](#creating-a-freshdesk-test-user)**  -toohave Britta Simon FreshDesk, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="1a87d-145">**[Creating a FreshDesk test user](#creating-a-freshdesk-test-user)** - toohave a counterpart of Britta Simon in FreshDesk that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="1a87d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1a87d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1a87d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="1a87d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1a87d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a87d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1a87d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az FreshDesk alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1a87d-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FreshDesk application.</span></span>

<span data-ttu-id="1a87d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a FreshDesk, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1a87d-150">**tooconfigure Azure AD single sign-on with FreshDesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a87d-151">Hello Azure felügyeleti portálon, a hello **FreshDesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-151">In hello Azure Management portal, on hello **FreshDesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1a87d-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="1a87d-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. <span data-ttu-id="1a87d-155">A hello **FreshDesk tartomány és az URL-címek** területen adja meg a hello **bejelentkezési URL-cím** mint: `https://<tenant-name>.freshdesk.com` vagy bármely más érték Freshdesk javasolt.</span><span class="sxs-lookup"><span data-stu-id="1a87d-155">On hello **FreshDesk Domain and URLs** section, please enter hello **Sign-on URL** as: `https://<tenant-name>.freshdesk.com` or any other value Freshdesk has suggested.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > <span data-ttu-id="1a87d-157">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="1a87d-157">Please note that this is not the real value.</span></span> <span data-ttu-id="1a87d-158">Frissítse az értéket a tényleges bejelentkezési URL-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1a87d-158">You have to update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="1a87d-159">Ügyfél [FreshDesk ügyfél-támogatási csoport](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="1a87d-159">Contact [FreshDesk Client support team](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) to get this value.</span></span>  

4. <span data-ttu-id="1a87d-160">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítvány a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1a87d-160">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. <span data-ttu-id="1a87d-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1a87d-164">A hello **FreshDesk konfigurációs** területén kattintson **konfigurálása FreshDesk** tooopen konfigurálása bejelentkezés ablak.</span><span class="sxs-lookup"><span data-stu-id="1a87d-164">On hello **FreshDesk Configuration** section, click **Configure FreshDesk** tooopen Configure sign-on window.</span></span> <span data-ttu-id="1a87d-165">Hello SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím másolása hello **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="1a87d-165">Copy hello SAML Single Sign-On Service URL and Sign-Out URL from hello **Quick Reference** section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. <span data-ttu-id="1a87d-167">Egy másik webes böngészőablakban jelentkezzen be a Freshdesk vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1a87d-167">In a different web browser window, log into your Freshdesk company site as an administrator.</span></span>

8. <span data-ttu-id="1a87d-168">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-168">In hello menu on hello top, click **Admin**.</span></span>
   
   <span data-ttu-id="1a87d-169">![Felügyeleti](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="1a87d-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span></span>

9. <span data-ttu-id="1a87d-170">A hello **általános beállítások** lapra, majd **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-170">In hello **General Settings** tab, click **Security**.</span></span>
   
   <span data-ttu-id="1a87d-171">![Biztonsági](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="1a87d-171">![Security](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Security")</span></span>

10. <span data-ttu-id="1a87d-172">A hello **biztonsági** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1a87d-172">In hello **Security** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="1a87d-173">![Egyszeri bejelentkezés](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="1a87d-173">![Single Sign On](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Single Sign On")</span></span>
   
    <span data-ttu-id="1a87d-174">a.</span><span class="sxs-lookup"><span data-stu-id="1a87d-174">a.</span></span> <span data-ttu-id="1a87d-175">A **egyszeri bejelentkezés (SSO)**, jelölje be **a**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-175">For **Single Sign On (SSO)**, select **On**.</span></span>

    <span data-ttu-id="1a87d-176">b.</span><span class="sxs-lookup"><span data-stu-id="1a87d-176">b.</span></span> <span data-ttu-id="1a87d-177">Válassza ki **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-177">Select **SAML SSO**.</span></span>

    <span data-ttu-id="1a87d-178">c.</span><span class="sxs-lookup"><span data-stu-id="1a87d-178">c.</span></span> <span data-ttu-id="1a87d-179">Típus hello **SAML-alapú egyszeri bejelentkezési URL-címe** Azure portálról történő hello másolt **SAML bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1a87d-179">Type hello **SAML Single Sign-On Service URL** you copied from Azure portal into hello **SAML Login URL** textbox.</span></span>

    <span data-ttu-id="1a87d-180">d.</span><span class="sxs-lookup"><span data-stu-id="1a87d-180">d.</span></span> <span data-ttu-id="1a87d-181">Típus hello **Sign-Out URL-cím** Azure portálról történő hello másolt **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1a87d-181">Type hello **Sign-Out URL**  you copied from Azure portal into hello **Logout URL** textbox.</span></span>

    <span data-ttu-id="1a87d-182">e.</span><span class="sxs-lookup"><span data-stu-id="1a87d-182">e.</span></span> <span data-ttu-id="1a87d-183">Másolás hello **ujjlenyomat** Azure portálról letöltött hello tanúsítványból értékét, és illessze be hello **biztonsági tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1a87d-183">Copy hello **Thumbprint** value from hello downloaded certificate from Azure portal and paste it into hello **Security Certificate Fingerprint** textbox.</span></span>  
 
    >[!TIP]
    ><span data-ttu-id="1a87d-184">További részletekért lásd: [hogyan tooretrieve egy tanúsítvány-ujjlenyomat értékének](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="1a87d-184">For more details, see [How tooretrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span> 
    
    <span data-ttu-id="1a87d-185">f.</span><span class="sxs-lookup"><span data-stu-id="1a87d-185">f.</span></span> <span data-ttu-id="1a87d-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-186">Click **Save**.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1a87d-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a87d-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="1a87d-188">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="1a87d-188">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1a87d-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1a87d-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a87d-191">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-191">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1a87d-193">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="1a87d-193">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1a87d-195">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a87d-195">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1a87d-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1a87d-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1a87d-199">a.</span><span class="sxs-lookup"><span data-stu-id="1a87d-199">a.</span></span> <span data-ttu-id="1a87d-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a87d-201">b.</span><span class="sxs-lookup"><span data-stu-id="1a87d-201">b.</span></span> <span data-ttu-id="1a87d-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1a87d-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1a87d-203">c.</span><span class="sxs-lookup"><span data-stu-id="1a87d-203">c.</span></span> <span data-ttu-id="1a87d-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1a87d-205">d.</span><span class="sxs-lookup"><span data-stu-id="1a87d-205">d.</span></span> <span data-ttu-id="1a87d-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-206">Click **Create**.</span></span>
 
### <a name="creating-a-freshdesk-test-user"></a><span data-ttu-id="1a87d-207">FreshDesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a87d-207">Creating a FreshDesk test user</span></span>

<span data-ttu-id="1a87d-208">A sorrend tooenable az Azure AD felhasználók toolog FreshDesk be azok ki kell építenie FreshDesk be.</span><span class="sxs-lookup"><span data-stu-id="1a87d-208">In order tooenable Azure AD users toolog into FreshDesk, they must be provisioned into FreshDesk.</span></span>  
<span data-ttu-id="1a87d-209">FreshDesk hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1a87d-209">In hello case of FreshDesk, provisioning is a manual task.</span></span>

<span data-ttu-id="1a87d-210">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a87d-210">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a87d-211">Jelentkezzen be tooyour **Freshdesk** bérlő.</span><span class="sxs-lookup"><span data-stu-id="1a87d-211">Log in tooyour **Freshdesk** tenant.</span></span>
2. <span data-ttu-id="1a87d-212">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-212">In hello menu on hello top, click **Admin**.</span></span>
   
   <span data-ttu-id="1a87d-213">![Felügyeleti](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="1a87d-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span></span>

3. <span data-ttu-id="1a87d-214">A hello **általános beállítások** lapra, majd **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-214">In hello **General Settings** tab, click **Agents**.</span></span>
   
   <span data-ttu-id="1a87d-215">![Ügynökök](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "ügynökök")</span><span class="sxs-lookup"><span data-stu-id="1a87d-215">![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")</span></span>

4. <span data-ttu-id="1a87d-216">Kattintson a **új ügynök**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-216">Click **New Agent**.</span></span>
   
    <span data-ttu-id="1a87d-217">![Az új ügynök](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "új ügynök")</span><span class="sxs-lookup"><span data-stu-id="1a87d-217">![New Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "New Agent")</span></span>

5. <span data-ttu-id="1a87d-218">A következő párbeszédpanelen: hello ügynök adatait hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="1a87d-218">On hello Agent Information dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="1a87d-219">![Az ügynök adatait](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "ügynök adatait")</span><span class="sxs-lookup"><span data-stu-id="1a87d-219">![Agent Information](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent Information")</span></span>
   
   <span data-ttu-id="1a87d-220">a.</span><span class="sxs-lookup"><span data-stu-id="1a87d-220">a.</span></span> <span data-ttu-id="1a87d-221">A hello **teljes nevét** szövegmezőhöz hello Azure AD fióknevet, amelyet az tooprovision hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1a87d-221">In hello **Full Name** textbox, type hello name of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="1a87d-222">b.</span><span class="sxs-lookup"><span data-stu-id="1a87d-222">b.</span></span> <span data-ttu-id="1a87d-223">A hello **E-mail** szövegmezőhöz típus hello Azure AD e-mail cím az Azure AD hello fióknevet, amelyet tooprovision.</span><span class="sxs-lookup"><span data-stu-id="1a87d-223">In hello **Email** textbox, type hello Azure AD email address of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="1a87d-224">c.</span><span class="sxs-lookup"><span data-stu-id="1a87d-224">c.</span></span> <span data-ttu-id="1a87d-225">A hello **cím** szövegmezőhöz típus hello cím tooprovision kívánt hello Azure AD-fiók.</span><span class="sxs-lookup"><span data-stu-id="1a87d-225">In hello **Title** textbox, type hello title of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="1a87d-226">d.</span><span class="sxs-lookup"><span data-stu-id="1a87d-226">d.</span></span> <span data-ttu-id="1a87d-227">Válassza ki **ügynökök szerepkör**, és kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-227">Select **Agents role**, and then click **Assign**.</span></span>
       
   <span data-ttu-id="1a87d-228">e.</span><span class="sxs-lookup"><span data-stu-id="1a87d-228">e.</span></span> <span data-ttu-id="1a87d-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-229">Click **Save**.</span></span>     
   
    >[!NOTE]
    ><span data-ttu-id="1a87d-230">az Azure AD fióktulajdonos hello kap egy e-mailt, amely tartalmaz egy hivatkozást tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="1a87d-230">hello Azure AD account holder will get an email that includes a link tooconfirm hello account before it is activated.</span></span> 
    > 
    
    >[!NOTE]
    ><span data-ttu-id="1a87d-231">Bármely más Freshdesk felhasználói fiók létrehozása eszközök vagy Freshdesk tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="1a87d-231">You can use any other Freshdesk user account creation tools or APIs provided by Freshdesk tooprovision AAD user accounts.</span></span> <span data-ttu-id="1a87d-232">tooFreshDesk.</span><span class="sxs-lookup"><span data-stu-id="1a87d-232">tooFreshDesk.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1a87d-233">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1a87d-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1a87d-234">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooBox megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1a87d-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooBox.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1a87d-236">**tooassign Britta Simon tooFreshDesk, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="1a87d-236">**tooassign Britta Simon tooFreshDesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a87d-237">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-237">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1a87d-239">Hello alkalmazások listában válassza ki a **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-239">In hello applications list, select **FreshDesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. <span data-ttu-id="1a87d-241">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1a87d-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1a87d-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a87d-243">Click **Add** button.</span></span> <span data-ttu-id="1a87d-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a87d-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1a87d-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1a87d-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1a87d-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a87d-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a87d-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a87d-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1a87d-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1a87d-249">Testing single sign-on</span></span>

<span data-ttu-id="1a87d-250">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="1a87d-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1a87d-251">Ha a hozzáférési Panel hello hello FreshDesk csempe gombra kattint, bejelentkezési oldal tooget bejelentkezett tooyour FreshDesk alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="1a87d-251">When you click hello FreshDesk tile in hello Access Panel, you should get login page tooget signed-on tooyour FreshDesk application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a87d-252">További források</span><span class="sxs-lookup"><span data-stu-id="1a87d-252">Additional resources</span></span>

* [<span data-ttu-id="1a87d-253">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1a87d-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a87d-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1a87d-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

