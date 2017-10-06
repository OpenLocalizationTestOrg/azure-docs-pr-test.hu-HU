---
title: "Oktatóanyag: Azure Active Directoryval integrált UserEcho |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és UserEcho között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: efe4a94ed6e5d22d153565d4782850eac4dff37b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a><span data-ttu-id="01b3b-103">Oktatóanyag: Azure Active Directoryval integrált UserEcho</span><span class="sxs-lookup"><span data-stu-id="01b3b-103">Tutorial: Azure Active Directory integration with UserEcho</span></span>

<span data-ttu-id="01b3b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate UserEcho az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01b3b-104">In this tutorial, you learn how toointegrate UserEcho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01b3b-105">UserEcho integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="01b3b-105">Integrating UserEcho with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="01b3b-106">Megadhatja a hozzáférés tooUserEcho rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="01b3b-106">You can control in Azure AD who has access tooUserEcho</span></span>
- <span data-ttu-id="01b3b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooUserEcho (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="01b3b-107">You can enable your users tooautomatically get signed-on tooUserEcho (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01b3b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="01b3b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="01b3b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01b3b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01b3b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="01b3b-110">Prerequisites</span></span>

<span data-ttu-id="01b3b-111">az Azure AD integrálása UserEcho tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="01b3b-111">tooconfigure Azure AD integration with UserEcho, you need hello following items:</span></span>

- <span data-ttu-id="01b3b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="01b3b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01b3b-113">Egy UserEcho egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="01b3b-113">A UserEcho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01b3b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="01b3b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01b3b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="01b3b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01b3b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="01b3b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01b3b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01b3b-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01b3b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="01b3b-118">Scenario description</span></span>
<span data-ttu-id="01b3b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="01b3b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01b3b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="01b3b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01b3b-121">Hello gyűjteményből UserEcho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="01b3b-121">Adding UserEcho from hello gallery</span></span>
2. <span data-ttu-id="01b3b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="01b3b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-userecho-from-hello-gallery"></a><span data-ttu-id="01b3b-123">Hello gyűjteményből UserEcho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="01b3b-123">Adding UserEcho from hello gallery</span></span>
<span data-ttu-id="01b3b-124">tooconfigure hello integrációja UserEcho az Azure AD-be, meg kell tooadd UserEcho hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="01b3b-124">tooconfigure hello integration of UserEcho into Azure AD, you need tooadd UserEcho from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="01b3b-125">**tooadd UserEcho hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01b3b-125">**tooadd UserEcho from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="01b3b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01b3b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="01b3b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="01b3b-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="01b3b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="01b3b-133">Hello keresési mezőbe, írja be a **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-133">In hello search box, type **UserEcho**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. <span data-ttu-id="01b3b-135">A hello eredmények panelen válassza ki a **UserEcho**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-135">In hello results panel, select **UserEcho**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="01b3b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="01b3b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="01b3b-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UserEcho.</span><span class="sxs-lookup"><span data-stu-id="01b3b-138">In this section, you configure and test Azure AD single sign-on with UserEcho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="01b3b-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó UserEcho tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="01b3b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UserEcho is tooa user in Azure AD.</span></span> <span data-ttu-id="01b3b-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello UserEcho közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="01b3b-140">In other words, a link relationship between an Azure AD user and hello related user in UserEcho needs toobe established.</span></span>

<span data-ttu-id="01b3b-141">UserEcho, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="01b3b-141">In UserEcho, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="01b3b-142">tooconfigure és az Azure AD az egyszeri bejelentkezés UserEcho-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="01b3b-142">tooconfigure and test Azure AD single sign-on with UserEcho, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="01b3b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="01b3b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="01b3b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01b3b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01b3b-145">**[UserEcho tesztfelhasználó létrehozása](#creating-a-userecho-test-user)**  -toohave egy megfelelője a Britta Simon a UserEcho, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="01b3b-145">**[Creating a UserEcho test user](#creating-a-userecho-test-user)** - toohave a counterpart of Britta Simon in UserEcho that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="01b3b-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="01b3b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01b3b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="01b3b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="01b3b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="01b3b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="01b3b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az UserEcho alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="01b3b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UserEcho application.</span></span>

<span data-ttu-id="01b3b-150">**az Azure AD tooconfigure egyszeri bejelentkezést a UserEcho, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01b3b-150">**tooconfigure Azure AD single sign-on with UserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="01b3b-151">Az Azure portál, a hello hello **UserEcho** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-151">In hello Azure portal, on hello **UserEcho** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="01b3b-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="01b3b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. <span data-ttu-id="01b3b-155">A hello **UserEcho tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01b3b-155">On hello **UserEcho Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    <span data-ttu-id="01b3b-157">a.</span><span class="sxs-lookup"><span data-stu-id="01b3b-157">a.</span></span> <span data-ttu-id="01b3b-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.userecho.com/`</span><span class="sxs-lookup"><span data-stu-id="01b3b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.userecho.com/`</span></span>

    <span data-ttu-id="01b3b-159">b.</span><span class="sxs-lookup"><span data-stu-id="01b3b-159">b.</span></span> <span data-ttu-id="01b3b-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.userecho.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="01b3b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.userecho.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="01b3b-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="01b3b-161">These values are not real.</span></span> <span data-ttu-id="01b3b-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="01b3b-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="01b3b-163">Ügyfél [UserEcho ügyfél-támogatási csoport](https://feedback.userecho.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="01b3b-163">Contact [UserEcho Client support team](https://feedback.userecho.com/) tooget these values.</span></span> 

4. <span data-ttu-id="01b3b-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="01b3b-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. <span data-ttu-id="01b3b-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="01b3b-168">A hello **UserEcho konfigurációs** kattintson **konfigurálása UserEcho** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="01b3b-168">On hello **UserEcho Configuration** section, click **Configure UserEcho** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="01b3b-169">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="01b3b-169">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. <span data-ttu-id="01b3b-171">Egy másik böngészőablakban bejelentkezéskor tooyour UserEcho vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="01b3b-171">In another browser window, sign on tooyour UserEcho company site as an administrator.</span></span>

8. <span data-ttu-id="01b3b-172">Hello eszköztár hello felül a felhasználó nevét tooexpand hello menüre kattint, és kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-172">In hello toolbar on hello top, click your user name tooexpand hello menu, and then click **Setup**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. <span data-ttu-id="01b3b-174">Kattintson a **integrációja**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-174">Click **Integrations**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. <span data-ttu-id="01b3b-176">Kattintson a **webhely**, és kattintson a **egyszeri bejelentkezés (egy SAML2)**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-176">Click **Website**, and then click **Single sign-on (SAML2)**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. <span data-ttu-id="01b3b-178">A hello **egyszeri bejelentkezés (SAML)** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01b3b-178">On hello **Single sign-on (SAML)** page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    <span data-ttu-id="01b3b-180">a.</span><span class="sxs-lookup"><span data-stu-id="01b3b-180">a.</span></span> <span data-ttu-id="01b3b-181">Mint **SAML-kompatibilis**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-181">As **SAML-enabled**, select **Yes**.</span></span>
    
    <span data-ttu-id="01b3b-182">b.</span><span class="sxs-lookup"><span data-stu-id="01b3b-182">b.</span></span> <span data-ttu-id="01b3b-183">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **SAML SSO URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="01b3b-183">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **SAML SSO URL** textbox.</span></span>
    
    <span data-ttu-id="01b3b-184">c.</span><span class="sxs-lookup"><span data-stu-id="01b3b-184">c.</span></span> <span data-ttu-id="01b3b-185">Beillesztés **Sign-Out URL-cím**, amely akkor másolta, az Azure-portálon hello hello **távoli logoout URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="01b3b-185">Paste **Sign-Out URL**, which you have copied from hello Azure portal into hello **Remote logoout URL** textbox.</span></span>
    
    <span data-ttu-id="01b3b-186">d.</span><span class="sxs-lookup"><span data-stu-id="01b3b-186">d.</span></span> <span data-ttu-id="01b3b-187">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom, és illessze be hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="01b3b-187">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **X.509 Certificate** textbox.</span></span>
    
    <span data-ttu-id="01b3b-188">e.</span><span class="sxs-lookup"><span data-stu-id="01b3b-188">e.</span></span> <span data-ttu-id="01b3b-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="01b3b-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="01b3b-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="01b3b-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="01b3b-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="01b3b-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01b3b-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="01b3b-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="01b3b-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="01b3b-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="01b3b-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="01b3b-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="01b3b-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="01b3b-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01b3b-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01b3b-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01b3b-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01b3b-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01b3b-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01b3b-205">a.</span><span class="sxs-lookup"><span data-stu-id="01b3b-205">a.</span></span> <span data-ttu-id="01b3b-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01b3b-207">b.</span><span class="sxs-lookup"><span data-stu-id="01b3b-207">b.</span></span> <span data-ttu-id="01b3b-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01b3b-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01b3b-209">c.</span><span class="sxs-lookup"><span data-stu-id="01b3b-209">c.</span></span> <span data-ttu-id="01b3b-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="01b3b-211">d.</span><span class="sxs-lookup"><span data-stu-id="01b3b-211">d.</span></span> <span data-ttu-id="01b3b-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-212">Click **Create**.</span></span>
 
### <a name="creating-a-userecho-test-user"></a><span data-ttu-id="01b3b-213">UserEcho tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="01b3b-213">Creating a UserEcho test user</span></span>

<span data-ttu-id="01b3b-214">hello ebben a szakaszban célja toocreate UserEcho Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="01b3b-214">hello objective of this section is toocreate a user called Britta Simon in UserEcho.</span></span>

<span data-ttu-id="01b3b-215">**toocreate Britta Simon meghívta UserEcho, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01b3b-215">**toocreate a user called Britta Simon in UserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="01b3b-216">Bejelentkezés tooyour UserEcho vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="01b3b-216">Sign-on tooyour UserEcho company site as an administrator.</span></span>

2. <span data-ttu-id="01b3b-217">Hello eszköztár hello felül a felhasználó nevét tooexpand hello menüre kattint, és kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-217">In hello toolbar on hello top, click your user name tooexpand hello menu, and then click **Setup**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. <span data-ttu-id="01b3b-219">Kattintson a **felhasználók**, tooexpand hello **felhasználók** szakasz.</span><span class="sxs-lookup"><span data-stu-id="01b3b-219">Click **Users**, tooexpand hello **Users** section.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. <span data-ttu-id="01b3b-221">Kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-221">Click **Users**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. <span data-ttu-id="01b3b-223">Kattintson a **új felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-223">Click **Invite a new user**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. <span data-ttu-id="01b3b-225">A hello **új felhasználó meghívása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01b3b-225">On hello **Invite a new user** dialog, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    <span data-ttu-id="01b3b-227">a.</span><span class="sxs-lookup"><span data-stu-id="01b3b-227">a.</span></span> <span data-ttu-id="01b3b-228">A hello **neve** szövegmező, például Britta Simon hello felhasználói nevét.</span><span class="sxs-lookup"><span data-stu-id="01b3b-228">In hello **Name** textbox, type name of hello user like Britta Simon.</span></span>
    
    <span data-ttu-id="01b3b-229">b.</span><span class="sxs-lookup"><span data-stu-id="01b3b-229">b.</span></span>  <span data-ttu-id="01b3b-230">A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="01b3b-230">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="01b3b-231">c.</span><span class="sxs-lookup"><span data-stu-id="01b3b-231">c.</span></span> <span data-ttu-id="01b3b-232">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-232">Click **Invite**.</span></span>

<span data-ttu-id="01b3b-233">Meghívót küldött tooBritta, amely lehetővé teszi a saját toostart UserEcho használatával.</span><span class="sxs-lookup"><span data-stu-id="01b3b-233">An invitation is sent tooBritta, which enables her toostart using UserEcho.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="01b3b-234">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="01b3b-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="01b3b-235">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooUserEcho megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="01b3b-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUserEcho.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="01b3b-237">**tooassign Britta Simon tooUserEcho, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="01b3b-237">**tooassign Britta Simon tooUserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="01b3b-238">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="01b3b-240">Hello alkalmazások listában válassza ki a **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-240">In hello applications list, select **UserEcho**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. <span data-ttu-id="01b3b-242">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="01b3b-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="01b3b-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="01b3b-244">Click **Add** button.</span></span> <span data-ttu-id="01b3b-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01b3b-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="01b3b-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="01b3b-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="01b3b-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01b3b-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01b3b-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01b3b-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="01b3b-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="01b3b-250">Testing single sign-on</span></span>

<span data-ttu-id="01b3b-251">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="01b3b-251">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="01b3b-252">Ha a hozzáférési Panel hello hello UserEcho csempe gombra kattint, automatikusan bejelentkezett tooyour UserEcho alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="01b3b-252">When you click hello UserEcho tile in hello Access Panel, you should get automatically signed-on tooyour UserEcho application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01b3b-253">További források</span><span class="sxs-lookup"><span data-stu-id="01b3b-253">Additional resources</span></span>

* [<span data-ttu-id="01b3b-254">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="01b3b-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01b3b-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="01b3b-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

