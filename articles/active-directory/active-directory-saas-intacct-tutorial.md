---
title: "Oktatóanyag: Azure Active Directoryval integrált Intacct |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Intacct között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3500039615166c2f61fb408d85bb82dfaefba134
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a><span data-ttu-id="bbb80-103">Oktatóanyag: Azure Active Directoryval integrált Intacct</span><span class="sxs-lookup"><span data-stu-id="bbb80-103">Tutorial: Azure Active Directory integration with Intacct</span></span>

<span data-ttu-id="bbb80-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Intacct az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bbb80-104">In this tutorial, you learn how toointegrate Intacct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bbb80-105">Intacct integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bbb80-105">Integrating Intacct with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bbb80-106">Megadhatja a hozzáférés tooIntacct rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bbb80-106">You can control in Azure AD who has access tooIntacct</span></span>
- <span data-ttu-id="bbb80-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIntacct (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bbb80-107">You can enable your users tooautomatically get signed-on tooIntacct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bbb80-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bbb80-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bbb80-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bbb80-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbb80-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bbb80-110">Prerequisites</span></span>

<span data-ttu-id="bbb80-111">az Azure AD integrálása Intacct tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bbb80-111">tooconfigure Azure AD integration with Intacct, you need hello following items:</span></span>

- <span data-ttu-id="bbb80-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bbb80-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bbb80-113">Egy Intacct egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bbb80-113">An Intacct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bbb80-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bbb80-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bbb80-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bbb80-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bbb80-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bbb80-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bbb80-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bbb80-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bbb80-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bbb80-118">Scenario description</span></span>
<span data-ttu-id="bbb80-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bbb80-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bbb80-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bbb80-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bbb80-121">Hello gyűjteményből Intacct hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bbb80-121">Adding Intacct from hello gallery</span></span>
2. <span data-ttu-id="bbb80-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bbb80-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intacct-from-hello-gallery"></a><span data-ttu-id="bbb80-123">Hello gyűjteményből Intacct hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bbb80-123">Adding Intacct from hello gallery</span></span>
<span data-ttu-id="bbb80-124">tooconfigure hello integrációja Intacct az Azure AD-be, meg kell tooadd Intacct hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bbb80-124">tooconfigure hello integration of Intacct into Azure AD, you need tooadd Intacct from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bbb80-125">**tooadd Intacct hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bbb80-125">**tooadd Intacct from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bbb80-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bbb80-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bbb80-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bbb80-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bbb80-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bbb80-133">Hello keresési mezőbe, írja be a **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-133">In hello search box, type **Intacct**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. <span data-ttu-id="bbb80-135">A hello eredmények panelen válassza ki a **Intacct**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-135">In hello results panel, select **Intacct**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bbb80-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bbb80-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bbb80-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Intacct.</span><span class="sxs-lookup"><span data-stu-id="bbb80-138">In this section, you configure and test Azure AD single sign-on with Intacct based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bbb80-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Intacct tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bbb80-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Intacct is tooa user in Azure AD.</span></span> <span data-ttu-id="bbb80-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Intacct közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bbb80-140">In other words, a link relationship between an Azure AD user and hello related user in Intacct needs toobe established.</span></span>

<span data-ttu-id="bbb80-141">Intacct, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bbb80-141">In Intacct, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bbb80-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Intacct-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bbb80-142">tooconfigure and test Azure AD single sign-on with Intacct, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bbb80-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bbb80-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bbb80-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbb80-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bbb80-145">**[Egy Intacct tesztfelhasználó létrehozása](#creating-an-intacct-test-user)**  -toohave egy megfelelője a Britta Simon a Intacct, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bbb80-145">**[Creating an Intacct test user](#creating-an-intacct-test-user)** - toohave a counterpart of Britta Simon in Intacct that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bbb80-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bbb80-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bbb80-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bbb80-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bbb80-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bbb80-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bbb80-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Intacct alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bbb80-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Intacct application.</span></span>

<span data-ttu-id="bbb80-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Intacct, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bbb80-150">**tooconfigure Azure AD single sign-on with Intacct, perform hello following steps:**</span></span>

1. <span data-ttu-id="bbb80-151">Az Azure portál, a hello hello **Intacct** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-151">In hello Azure portal, on hello **Intacct** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bbb80-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bbb80-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. <span data-ttu-id="bbb80-155">A hello **Intacct tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bbb80-155">On hello **Intacct Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    <span data-ttu-id="bbb80-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="bbb80-157">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > <span data-ttu-id="bbb80-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="bbb80-158">This value is not real.</span></span> <span data-ttu-id="bbb80-159">Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="bbb80-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="bbb80-160">Ügyfél [Intacct támogatási csoport](https://us.intacct.com/support) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="bbb80-160">Contact [Intacct support team](https://us.intacct.com/support) tooget this value.</span></span>

4. <span data-ttu-id="bbb80-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bbb80-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. <span data-ttu-id="bbb80-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bbb80-165">A hello **Intacct konfigurációs** kattintson **konfigurálása Intacct** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bbb80-165">On hello **Intacct Configuration** section, click **Configure Intacct** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bbb80-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bbb80-166">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. <span data-ttu-id="bbb80-168">Egy másik webes böngészőablakban a bejelentkezés tooyour Intacct vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bbb80-168">In a different web browser window, sign in tooyour Intacct company site as an administrator.</span></span>

8. <span data-ttu-id="bbb80-169">Kattintson a hello **vállalati** fülre, majd **vállalati adatok**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-169">Click hello **Company** tab, and then click **Company Info**.</span></span>

    <span data-ttu-id="bbb80-170">![Vállalati](./media/active-directory-saas-intacct-tutorial/ic790037.png "vállalati")</span><span class="sxs-lookup"><span data-stu-id="bbb80-170">![Company](./media/active-directory-saas-intacct-tutorial/ic790037.png "Company")</span></span>

9. <span data-ttu-id="bbb80-171">Kattintson a hello **biztonsági** fülre, majd **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-171">Click hello **Security** tab, and then click **Edit**.</span></span>

    <span data-ttu-id="bbb80-172">![Biztonsági](./media/active-directory-saas-intacct-tutorial/ic790038.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="bbb80-172">![Security](./media/active-directory-saas-intacct-tutorial/ic790038.png "Security")</span></span>

10. <span data-ttu-id="bbb80-173">A hello **egyszeri bejelentkezés (SSO)** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bbb80-173">In hello **Single sign on (SSO)** section, perform hello following steps:</span></span>

    <span data-ttu-id="bbb80-174">![Egyszeri bejelentkezés](./media/active-directory-saas-intacct-tutorial/ic790039.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="bbb80-174">![Single sign on](./media/active-directory-saas-intacct-tutorial/ic790039.png "single sign on")</span></span>

    <span data-ttu-id="bbb80-175">a.</span><span class="sxs-lookup"><span data-stu-id="bbb80-175">a.</span></span> <span data-ttu-id="bbb80-176">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-176">Select **Enable single sign on**.</span></span>

    <span data-ttu-id="bbb80-177">b.</span><span class="sxs-lookup"><span data-stu-id="bbb80-177">b.</span></span> <span data-ttu-id="bbb80-178">Mint **identitás szolgáltatótípus**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-178">As **Identity provider type**, select **SAML 2.0**.</span></span>

    <span data-ttu-id="bbb80-179">c.</span><span class="sxs-lookup"><span data-stu-id="bbb80-179">c.</span></span> <span data-ttu-id="bbb80-180">A **kiállítójának URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bbb80-180">In **Issuer URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="bbb80-181">d.</span><span class="sxs-lookup"><span data-stu-id="bbb80-181">d.</span></span> <span data-ttu-id="bbb80-182">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bbb80-182">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bbb80-183">e.</span><span class="sxs-lookup"><span data-stu-id="bbb80-183">e.</span></span> <span data-ttu-id="bbb80-184">Nyissa meg a **base-64** kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="bbb80-184">Open your **base-64** encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** box.</span></span>
   
    <span data-ttu-id="bbb80-185">f.</span><span class="sxs-lookup"><span data-stu-id="bbb80-185">f.</span></span> <span data-ttu-id="bbb80-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="bbb80-187">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bbb80-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bbb80-188">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bbb80-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bbb80-189">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bbb80-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bbb80-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbb80-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="bbb80-191">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bbb80-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bbb80-193">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bbb80-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bbb80-194">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bbb80-196">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bbb80-198">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bbb80-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bbb80-200">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bbb80-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bbb80-202">a.</span><span class="sxs-lookup"><span data-stu-id="bbb80-202">a.</span></span> <span data-ttu-id="bbb80-203">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bbb80-204">b.</span><span class="sxs-lookup"><span data-stu-id="bbb80-204">b.</span></span> <span data-ttu-id="bbb80-205">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bbb80-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bbb80-206">c.</span><span class="sxs-lookup"><span data-stu-id="bbb80-206">c.</span></span> <span data-ttu-id="bbb80-207">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bbb80-208">d.</span><span class="sxs-lookup"><span data-stu-id="bbb80-208">d.</span></span> <span data-ttu-id="bbb80-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-209">Click **Create**.</span></span>
 
### <a name="creating-an-intacct-test-user"></a><span data-ttu-id="bbb80-210">Egy Intacct tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbb80-210">Creating an Intacct test user</span></span>

<span data-ttu-id="bbb80-211">tooset az Azure AD-felhasználókat bejelentkeznének tooIntacct, így azok ki kell építenie Intacct be.</span><span class="sxs-lookup"><span data-stu-id="bbb80-211">tooset up Azure AD users so they can sign in tooIntacct, they must be provisioned into Intacct.</span></span> <span data-ttu-id="bbb80-212">Intacct a kiépítés kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="bbb80-212">For Intacct, provisioning is a manual task.</span></span>

<span data-ttu-id="bbb80-213">**tooprovision felhasználói fiókok, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bbb80-213">**tooprovision user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="bbb80-214">Jelentkezzen be tooyour **Intacct** bérlő.</span><span class="sxs-lookup"><span data-stu-id="bbb80-214">Sign in tooyour **Intacct** tenant.</span></span>

2. <span data-ttu-id="bbb80-215">Kattintson a hello **vállalati** fülre, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-215">Click hello **Company** tab, and then click **Users**.</span></span>

    <span data-ttu-id="bbb80-216">![Felhasználók](./media/active-directory-saas-intacct-tutorial/ic790041.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="bbb80-216">![Users](./media/active-directory-saas-intacct-tutorial/ic790041.png "Users")</span></span>
3. <span data-ttu-id="bbb80-217">Kattintson a hello **Hozzáadás** fülre.</span><span class="sxs-lookup"><span data-stu-id="bbb80-217">Click hello **Add** tab.</span></span>

    <span data-ttu-id="bbb80-218">![Adja hozzá](./media/active-directory-saas-intacct-tutorial/ic790042.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="bbb80-218">![Add](./media/active-directory-saas-intacct-tutorial/ic790042.png "Add")</span></span>
4. <span data-ttu-id="bbb80-219">A hello **felhasználói adatok** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bbb80-219">In hello **User Information** section, perform hello following steps:</span></span>

    <span data-ttu-id="bbb80-220">![Felhasználói adatok](./media/active-directory-saas-intacct-tutorial/ic790043.png "felhasználói adatok")</span><span class="sxs-lookup"><span data-stu-id="bbb80-220">![User Information](./media/active-directory-saas-intacct-tutorial/ic790043.png "User Information")</span></span>

    <span data-ttu-id="bbb80-221">a.</span><span class="sxs-lookup"><span data-stu-id="bbb80-221">a.</span></span> <span data-ttu-id="bbb80-222">Adja meg a hello **Felhasználóazonosító**, hello **Vezetéknév**, **Utónév**, hello **E-mail cím**, hello **cím**, és hello **Phone** , az Azure AD-fiókot, hogy a kívánt tooprovision hello **felhasználói adatok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bbb80-222">Enter hello **User ID**, hello **Last name**, **First name**, hello **Email address**, hello **Title**, and hello **Phone** of an Azure AD account that you want tooprovision into hello **User Information** section.</span></span>

    <span data-ttu-id="bbb80-223">b.</span><span class="sxs-lookup"><span data-stu-id="bbb80-223">b.</span></span> <span data-ttu-id="bbb80-224">Jelölje be hello **rendszergazda jogosultságokkal** , az Azure AD-fiókot, amelyet az tooprovision.</span><span class="sxs-lookup"><span data-stu-id="bbb80-224">Select hello **Admin privileges** of an Azure AD account that you want tooprovision.</span></span>
   
    <span data-ttu-id="bbb80-225">c.</span><span class="sxs-lookup"><span data-stu-id="bbb80-225">c.</span></span> <span data-ttu-id="bbb80-226">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-226">Click **Save**.</span></span> <span data-ttu-id="bbb80-227">hello Azure AD fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="bbb80-227">hello Azure AD account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

>[!NOTE]
><span data-ttu-id="bbb80-228">Azure AD-felhasználói fiókok tooprovision, használhatja más Intacct felhasználói fiók létrehozása eszközök vagy Intacct által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="bbb80-228">tooprovision Azure AD user accounts, you can use other Intacct user account creation tools or APIs that are provided by Intacct.</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bbb80-229">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bbb80-229">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bbb80-230">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIntacct megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bbb80-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIntacct.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bbb80-232">**tooassign Britta Simon tooIntacct, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bbb80-232">**tooassign Britta Simon tooIntacct, perform hello following steps:**</span></span>

1. <span data-ttu-id="bbb80-233">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bbb80-235">Hello alkalmazások listában válassza ki a **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-235">In hello applications list, select **Intacct**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. <span data-ttu-id="bbb80-237">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bbb80-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bbb80-239">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bbb80-239">Click **Add** button.</span></span> <span data-ttu-id="bbb80-240">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bbb80-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bbb80-242">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bbb80-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bbb80-243">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bbb80-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bbb80-244">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bbb80-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bbb80-245">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bbb80-245">Testing single sign-on</span></span>

<span data-ttu-id="bbb80-246">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bbb80-246">In this section, you test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="bbb80-247">Ha a hozzáférési Panel hello hello Intacct csempe gombra kattint, akkor kell automatikusan megtörténik a tooyour Intacct alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bbb80-247">When you click hello Intacct tile in hello Access Panel, you should be automatically signed in tooyour Intacct application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bbb80-248">További források</span><span class="sxs-lookup"><span data-stu-id="bbb80-248">Additional resources</span></span>

* [<span data-ttu-id="bbb80-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bbb80-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bbb80-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bbb80-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

