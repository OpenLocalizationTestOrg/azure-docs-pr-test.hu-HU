---
title: "Oktatóanyag: Azure Active Directoryval integrált 8 x 8 virtuális Office |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és 8 x 8 virtuális Office között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: df5c5de77285cd3912b68cc3b1e3eee274aa951c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a><span data-ttu-id="c1606-103">Oktatóanyag: Azure Active Directoryval integrált 8 x 8 virtuális Office</span><span class="sxs-lookup"><span data-stu-id="c1606-103">Tutorial: Azure Active Directory integration with 8x8 Virtual Office</span></span>

<span data-ttu-id="c1606-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 8 x 8 virtuális Office, az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c1606-104">In this tutorial, you learn how toointegrate 8x8 Virtual Office with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1606-105">8 x 8 integrálása virtuális Office és az Azure AD segítségével a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c1606-105">Integrating 8x8 Virtual Office with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c1606-106">Szabályozhatja az Azure AD hozzáférési too8x8 rendelkező virtuális Office</span><span class="sxs-lookup"><span data-stu-id="c1606-106">You can control in Azure AD who has access too8x8 Virtual Office</span></span>
- <span data-ttu-id="c1606-107">Engedélyezheti a felhasználóknak tooautomatically bejelentkezett too8x8 virtuális Office (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c1606-107">You can enable your users tooautomatically get signed-on too8x8 Virtual Office (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c1606-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c1606-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c1606-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1606-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1606-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1606-110">Prerequisites</span></span>

<span data-ttu-id="c1606-111">az Azure AD integrálása 8 x 8 virtuális Office tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="c1606-111">tooconfigure Azure AD integration with 8x8 Virtual Office, you need hello following items:</span></span>

- <span data-ttu-id="c1606-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c1606-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1606-113">Egy 8 x 8 virtuális Office egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c1606-113">A 8x8 Virtual Office single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1606-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c1606-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1606-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c1606-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1606-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c1606-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1606-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1606-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1606-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c1606-118">Scenario description</span></span>
<span data-ttu-id="c1606-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c1606-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1606-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c1606-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1606-121">8 x 8 virtuális Office hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c1606-121">Adding 8x8 Virtual Office from hello gallery</span></span>
2. <span data-ttu-id="c1606-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c1606-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-8x8-virtual-office-from-hello-gallery"></a><span data-ttu-id="c1606-123">8 x 8 virtuális Office hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c1606-123">Adding 8x8 Virtual Office from hello gallery</span></span>
<span data-ttu-id="c1606-124">tooconfigure hello integrációs 8 x 8 virtuális Office, az Azure AD-be, meg kell tooadd 8 x 8 virtuális Office hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c1606-124">tooconfigure hello integration of 8x8 Virtual Office into Azure AD, you need tooadd 8x8 Virtual Office from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c1606-125">**tooadd 8 x 8 virtuális Office hello gyűjteményből, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1606-125">**tooadd 8x8 Virtual Office from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1606-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c1606-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c1606-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c1606-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c1606-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c1606-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c1606-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c1606-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c1606-133">Hello keresési mezőbe, írja be a **8 x 8 virtuális Office**.</span><span class="sxs-lookup"><span data-stu-id="c1606-133">In hello search box, type **8x8 Virtual Office**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. <span data-ttu-id="c1606-135">A hello eredmények panelen válassza ki a **8 x 8 virtuális Office**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c1606-135">In hello results panel, select **8x8 Virtual Office**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c1606-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c1606-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c1606-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a 8 x 8 virtuális Office "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="c1606-138">In this section, you configure and test Azure AD single sign-on with 8x8 Virtual Office based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c1606-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói 8 x 8 virtuális Office tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c1606-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 8x8 Virtual Office is tooa user in Azure AD.</span></span> <span data-ttu-id="c1606-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello 8 x 8 hivatkozás kapcsolatát virtuális Office kell létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="c1606-140">In other words, a link relationship between an Azure AD user and hello related user in 8x8 Virtual Office needs toobe established.</span></span>

<span data-ttu-id="c1606-141">8 x 8 virtuális Office, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c1606-141">In 8x8 Virtual Office, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c1606-142">tooconfigure és az Azure AD az egyszeri bejelentkezés 8 x 8 virtuális Office-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c1606-142">tooconfigure and test Azure AD single sign-on with 8x8 Virtual Office, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c1606-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c1606-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c1606-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1606-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1606-145">**[8 x 8 virtuális Office tesztfelhasználó létrehozása](#creating-a-8x8-virtual-office-test-user)**  -toohave egy megfelelője a Britta Simon 8 x 8 virtuális irodában, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c1606-145">**[Creating a 8x8 Virtual Office test user](#creating-a-8x8-virtual-office-test-user)** - toohave a counterpart of Britta Simon in 8x8 Virtual Office that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c1606-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c1606-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1606-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c1606-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c1606-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1606-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c1606-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és az 8 x 8 virtuális Office-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c1606-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 8x8 Virtual Office application.</span></span>

<span data-ttu-id="c1606-150">**8 x 8 virtuális Office, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c1606-150">**tooconfigure Azure AD single sign-on with 8x8 Virtual Office, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1606-151">Az Azure portál, a hello hello **8 x 8 virtuális Office** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c1606-151">In hello Azure portal, on hello **8x8 Virtual Office** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c1606-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c1606-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. <span data-ttu-id="c1606-155">A hello **8 x 8 virtuális Office tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c1606-155">On hello **8x8 Virtual Office Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    <span data-ttu-id="c1606-157">a.</span><span class="sxs-lookup"><span data-stu-id="c1606-157">a.</span></span> <span data-ttu-id="c1606-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="c1606-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="c1606-159">| `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |</span><span class="sxs-lookup"><span data-stu-id="c1606-159">| `https://sso.8x8.com/<companyname>` |
 | `https://www.8x8.com/<companyname>` |
 | `https://sso.8x8pilot.com/<companyname>` |</span></span>

    <span data-ttu-id="c1606-160">b.</span><span class="sxs-lookup"><span data-stu-id="c1606-160">b.</span></span> <span data-ttu-id="c1606-161">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="c1606-161">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="c1606-162">| `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|</span><span class="sxs-lookup"><span data-stu-id="c1606-162">| `https://<subdomain>.8x8.com/saml2` |
 | `https://<subdomain>.8x8pilot.com/saml2`|</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c1606-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c1606-163">These values are not real.</span></span> <span data-ttu-id="c1606-164">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="c1606-164">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="c1606-165">Ügyfél [8 x 8 virtuális Office támogatási csoport](https://www.8x8.com/about-us/contact-us) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c1606-165">Contact [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us) tooget these values.</span></span>
 


4. <span data-ttu-id="c1606-166">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c1606-166">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. <span data-ttu-id="c1606-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1606-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c1606-170">A hello **8 x 8 virtuális Office konfigurációs** kattintson **konfigurálása 8 x 8 virtuális Office** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c1606-170">On hello **8x8 Virtual Office Configuration** section, click **Configure 8x8 Virtual Office** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c1606-171">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c1606-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. <span data-ttu-id="c1606-173">Bejelentkezés tooyour 8 x 8 virtuális Office Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="c1606-173">Sign-on tooyour 8x8 Virtual Office tenant as an administrator.</span></span>

8. <span data-ttu-id="c1606-174">Válassza ki **virtuális Office fiók Mgr** alkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="c1606-174">Select **Virtual Office Account Mgr** on Application Panel.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. <span data-ttu-id="c1606-176">Válassza ki **üzleti** toomanage fiókot, és kattintson a **bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1606-176">Select **Business** account toomanage and click **Sign In** button.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. <span data-ttu-id="c1606-178">Kattintson a **fiókok** hello menülistában lapján.</span><span class="sxs-lookup"><span data-stu-id="c1606-178">Click **Accounts** tab in hello menu list.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. <span data-ttu-id="c1606-180">Kattintson a **egyszeri bejelentkezés** hello fiókok közül.</span><span class="sxs-lookup"><span data-stu-id="c1606-180">Click **Single Sign On** in hello list of Accounts.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. <span data-ttu-id="c1606-182">Válassza ki **egyszeri bejelentkezés** hitelesítési módszert, és kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="c1606-182">Select **Single Sign On** under Authentication method and click **SAML**.</span></span>
    
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. <span data-ttu-id="c1606-184">Másolás **SAML SSO URL-cím**, **egyetlen Sign Out szolgáltatás URL-címe** és **kiállítójának URL-címe** az Azure AD túl**bejelentkezési az URL-cím**, **bejelentkezési kijelentkezési URL-cím**  és **kiállítójának URL-címe** 8 x 8 virtuális Office.</span><span class="sxs-lookup"><span data-stu-id="c1606-184">Copy **SAML SSO URL**, **Single Sing Out Service URL** and **Issuer URL** from Azure AD too**Sign In URL**, **Sign Out URL** and **Issuer URL** in 8x8 Virtual Office.</span></span> 
    
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. <span data-ttu-id="c1606-186">Kattintson a **böngésző** tooupload hello tanúsítványt, amely az Azure AD letöltött gombra, és kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1606-186">Click **Browser** button tooupload hello certificate which you downloaded from Azure AD, and click hello **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="c1606-187">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c1606-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c1606-188">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c1606-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c1606-189">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1606-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c1606-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1606-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="c1606-191">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c1606-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c1606-193">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c1606-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1606-194">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c1606-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c1606-196">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c1606-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c1606-198">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1606-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c1606-200">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c1606-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c1606-202">a.</span><span class="sxs-lookup"><span data-stu-id="c1606-202">a.</span></span> <span data-ttu-id="c1606-203">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1606-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c1606-204">b.</span><span class="sxs-lookup"><span data-stu-id="c1606-204">b.</span></span> <span data-ttu-id="c1606-205">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c1606-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c1606-206">c.</span><span class="sxs-lookup"><span data-stu-id="c1606-206">c.</span></span> <span data-ttu-id="c1606-207">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c1606-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c1606-208">d.</span><span class="sxs-lookup"><span data-stu-id="c1606-208">d.</span></span> <span data-ttu-id="c1606-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c1606-209">Click **Create**.</span></span>
 
### <a name="creating-a-8x8-virtual-office-test-user"></a><span data-ttu-id="c1606-210">8 x 8 virtuális Office tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1606-210">Creating a 8x8 Virtual Office test user</span></span>

<span data-ttu-id="c1606-211">hello ebben a szakaszban célja toocreate 8 x 8 virtuális Office Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="c1606-211">hello objective of this section is toocreate a user called Britta Simon in 8x8 Virtual Office.</span></span> <span data-ttu-id="c1606-212">8 x 8 virtuális Office támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="c1606-212">8x8 Virtual Office supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="c1606-213">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="c1606-213">There is no action item for you in this section.</span></span> <span data-ttu-id="c1606-214">Egy új felhasználó létrehozása során egy kísérlet tooaccess 8 x 8 virtuális Office Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="c1606-214">A new user is created during an attempt tooaccess 8x8 Virtual Office if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="c1606-215">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [8 x 8 virtuális Office támogatási csoport](https://www.8x8.com/about-us/contact-us).</span><span class="sxs-lookup"><span data-stu-id="c1606-215">If you need toocreate a user manually, you need toocontact hello [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c1606-216">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c1606-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c1606-217">Ebben a szakaszban engedélyezze Britta Simon megadásával Azure egyszeri bejelentkezés toouse too8x8 virtuális Office eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c1606-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too8x8 Virtual Office.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c1606-219">**tooassign Britta Simon too8x8 virtuális Office, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c1606-219">**tooassign Britta Simon too8x8 Virtual Office, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1606-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c1606-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c1606-222">Hello alkalmazások listában válassza ki a **8 x 8 virtuális Office**.</span><span class="sxs-lookup"><span data-stu-id="c1606-222">In hello applications list, select **8x8 Virtual Office**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. <span data-ttu-id="c1606-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c1606-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c1606-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1606-226">Click **Add** button.</span></span> <span data-ttu-id="c1606-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1606-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c1606-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c1606-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c1606-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1606-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1606-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1606-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c1606-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c1606-232">Testing single sign-on</span></span>

<span data-ttu-id="c1606-233">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c1606-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c1606-234">Ha a hozzáférési Panel hello hello 8 x 8 virtuális Office csempe gombra kattint, automatikusan bejelentkezett tooyour 8 x 8 virtuális Office-alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c1606-234">When you click hello 8x8 Virtual Office tile in hello Access Panel, you should get automatically signed-on tooyour 8x8 Virtual Office application.</span></span>
<span data-ttu-id="c1606-235">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="c1606-235">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1606-236">További források</span><span class="sxs-lookup"><span data-stu-id="c1606-236">Additional resources</span></span>

* [<span data-ttu-id="c1606-237">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c1606-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1606-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c1606-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

