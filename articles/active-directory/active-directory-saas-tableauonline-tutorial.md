---
title: "Oktatóanyag: Azure Active Directory-integráció a Tableau Online |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Tableau Online között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 590b2674270c340b4750c7b6feeaf4f0df4bf853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a><span data-ttu-id="8d78d-103">Oktatóanyag: Azure Active Directory-integráció a Tableau Online</span><span class="sxs-lookup"><span data-stu-id="8d78d-103">Tutorial: Azure Active Directory integration with Tableau Online</span></span>

<span data-ttu-id="8d78d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Tableau kapcsolódik az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8d78d-104">In this tutorial, you learn how toointegrate Tableau Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8d78d-105">Tableau Online integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8d78d-105">Integrating Tableau Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8d78d-106">Megadhatja a hozzáférés tooTableau Online rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8d78d-106">You can control in Azure AD who has access tooTableau Online</span></span>
- <span data-ttu-id="8d78d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTableau Online (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8d78d-107">You can enable your users tooautomatically get signed-on tooTableau Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8d78d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8d78d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8d78d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8d78d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d78d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8d78d-110">Prerequisites</span></span>

<span data-ttu-id="8d78d-111">tooconfigure az Azure AD Online szolgáltatással való integráció Tableau, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="8d78d-111">tooconfigure Azure AD integration with Tableau Online, you need hello following items:</span></span>

- <span data-ttu-id="8d78d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8d78d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8d78d-113">A Tableau Online egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8d78d-113">A Tableau Online single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8d78d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8d78d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8d78d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8d78d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8d78d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8d78d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8d78d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8d78d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8d78d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8d78d-118">Scenario description</span></span>
<span data-ttu-id="8d78d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8d78d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8d78d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8d78d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8d78d-121">Hello gyűjteményből Tableau Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8d78d-121">Adding Tableau Online from hello gallery</span></span>
2. <span data-ttu-id="8d78d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8d78d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-online-from-hello-gallery"></a><span data-ttu-id="8d78d-123">Hello gyűjteményből Tableau Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8d78d-123">Adding Tableau Online from hello gallery</span></span>
<span data-ttu-id="8d78d-124">tooconfigure hello integrációs Tableau online az Azure AD-be, meg kell tooadd Tableau Online hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8d78d-124">tooconfigure hello integration of Tableau Online into Azure AD, you need tooadd Tableau Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8d78d-125">**hello gyűjteményből Online Tableau tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8d78d-125">**tooadd Tableau Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d78d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8d78d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8d78d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8d78d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8d78d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8d78d-133">Hello keresési mezőbe, írja be a **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-133">In hello search box, type **Tableau Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. <span data-ttu-id="8d78d-135">A hello eredmények panelen válassza ki a **Tableau Online**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-135">In hello results panel, select **Tableau Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8d78d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8d78d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8d78d-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Tableau Online "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="8d78d-138">In this section, you configure and test Azure AD single sign-on with Tableau Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8d78d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Tableau Online tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8d78d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tableau Online is tooa user in Azure AD.</span></span> <span data-ttu-id="8d78d-140">Ez azt jelenti hello kapcsolódó felhasználó a Tableau Online és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8d78d-140">In other words, a link relationship between an Azure AD user and hello related user in Tableau Online needs toobe established.</span></span>

<span data-ttu-id="8d78d-141">A Tableau Online rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8d78d-141">In Tableau Online, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8d78d-142">tooconfigure és a Tableau Online az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8d78d-142">tooconfigure and test Azure AD single sign-on with Tableau Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8d78d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8d78d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8d78d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8d78d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8d78d-145">**[A Tableau Online tesztfelhasználó létrehozása](#creating-a-tableau-online-test-user)**  -toohave egy megfelelője a Britta Simon a Tableau Online csatolt toohello az Azure AD felhasználói ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8d78d-145">**[Creating a Tableau Online test user](#creating-a-tableau-online-test-user)** - toohave a counterpart of Britta Simon in Tableau Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8d78d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8d78d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8d78d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8d78d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8d78d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8d78d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8d78d-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez a Tableau Online alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d78d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tableau Online application.</span></span>

<span data-ttu-id="8d78d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Tableau Online, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8d78d-150">**tooconfigure Azure AD single sign-on with Tableau Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d78d-151">Az Azure portál, a hello hello **Tableau Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-151">In hello Azure portal, on hello **Tableau Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8d78d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8d78d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. <span data-ttu-id="8d78d-155">A hello **Tableau Online tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8d78d-155">On hello **Tableau Online Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    <span data-ttu-id="8d78d-157">a.</span><span class="sxs-lookup"><span data-stu-id="8d78d-157">a.</span></span> <span data-ttu-id="8d78d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://sso.online.tableau.com`</span><span class="sxs-lookup"><span data-stu-id="8d78d-158">In hello **Sign-on URL** textbox, type hello URL: `https://sso.online.tableau.com`</span></span>

    <span data-ttu-id="8d78d-159">b.</span><span class="sxs-lookup"><span data-stu-id="8d78d-159">b.</span></span> <span data-ttu-id="8d78d-160">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://sso.online.tableau.com/public/sp/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="8d78d-160">In hello **Identifier** textbox, type hello URL: `https://sso.online.tableau.com/public/sp/<instancename>`</span></span>

4. <span data-ttu-id="8d78d-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8d78d-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. <span data-ttu-id="8d78d-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8d78d-165">Egy másik böngészőablakban, bejelentkezés tooyour Tableau Online alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d78d-165">In a different browser window, sign-on tooyour Tableau Online application.</span></span> <span data-ttu-id="8d78d-166">Nyissa meg túl**beállítások** , majd **hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-166">Go too**Settings** and then **Authentication**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. <span data-ttu-id="8d78d-168">tooenable SAML a **hitelesítési típusok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8d78d-168">tooenable SAML, Under **Authentication Types** section.</span></span> <span data-ttu-id="8d78d-169">Ellenőrizze a hello **egyszeri bejelentkezéshez a SAML** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8d78d-169">Check hello **Single sign-on with SAML** checkbox.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. <span data-ttu-id="8d78d-171">Görgessen lefelé, amíg **importálási metaadatait tartalmazó fájl a Tableau Online** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8d78d-171">Scroll down until **Import metadata file into Tableau Online** section.</span></span>  <span data-ttu-id="8d78d-172">Kattintson a Tallózás gombra, és már letöltötte az Azure AD hello metaadatait tartalmazó fájl importálása.</span><span class="sxs-lookup"><span data-stu-id="8d78d-172">Click Browse and import hello metadata file you have downloaded from Azure AD.</span></span> <span data-ttu-id="8d78d-173">Kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-173">Then, click **Apply**.</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. <span data-ttu-id="8d78d-175">A hello **helyességi feltételek egyeznek** területen hello megfelelő identitásszolgáltató helyességi feltétel nevét beszúrása **e-mail cím**, **Utónév**, és **Vezetéknév** .</span><span class="sxs-lookup"><span data-stu-id="8d78d-175">In hello **Match assertions** section, insert hello corresponding Identity Provider assertion name for **email address**, **first name**, and **last name**.</span></span> <span data-ttu-id="8d78d-176">tooget ezt az információt az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="8d78d-176">tooget this information from Azure AD:</span></span> 
  
    <span data-ttu-id="8d78d-177">a.</span><span class="sxs-lookup"><span data-stu-id="8d78d-177">a.</span></span> <span data-ttu-id="8d78d-178">Az Azure-portálon hello, keresse fel a hello **Tableau Online** alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="8d78d-178">In hello Azure portal, go on hello **Tableau Online** application integration page.</span></span>
    
    <span data-ttu-id="8d78d-179">b.</span><span class="sxs-lookup"><span data-stu-id="8d78d-179">b.</span></span> <span data-ttu-id="8d78d-180">Hello attribútumok területen válassza ki a hello **"megtekintése és szerkesztése a más felhasználói attribútumok"** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8d78d-180">In hello attributes section, Select hello **"view and edit all other user attributes"** checkbox.</span></span> 
    
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    <span data-ttu-id="8d78d-182">c.</span><span class="sxs-lookup"><span data-stu-id="8d78d-182">c.</span></span> <span data-ttu-id="8d78d-183">Ezek az attribútumok hello névtér értéke másolja: givenname, az e-mailek és a Vezetéknév használatával hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8d78d-183">Copy hello namespace value for these attributes: givenname, email and surname by using hello following steps:</span></span>

   ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    <span data-ttu-id="8d78d-185">d.</span><span class="sxs-lookup"><span data-stu-id="8d78d-185">d.</span></span> <span data-ttu-id="8d78d-186">Kattintson a **user.givenname** érték</span><span class="sxs-lookup"><span data-stu-id="8d78d-186">Click **user.givenname** value</span></span> 
    
    <span data-ttu-id="8d78d-187">e.</span><span class="sxs-lookup"><span data-stu-id="8d78d-187">e.</span></span> <span data-ttu-id="8d78d-188">Hello érték másolása hello **névtér** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8d78d-188">Copy hello value from hello **namespace** textbox.</span></span>

   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    <span data-ttu-id="8d78d-190">f.</span><span class="sxs-lookup"><span data-stu-id="8d78d-190">f.</span></span> <span data-ttu-id="8d78d-191">hello e-mailek és a Vezetéknév toocopy hello namesapce értékeket kövesse az előző lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="8d78d-191">toocopy hello namesapce values for hello email and surname follow hello preceding steps.</span></span>

    <span data-ttu-id="8d78d-192">g.</span><span class="sxs-lookup"><span data-stu-id="8d78d-192">g.</span></span> <span data-ttu-id="8d78d-193">Váltás toohello Tableau Online alkalmazás, majd állítsa be a hello **Tableau Online attribútumok** szakasz az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8d78d-193">Switch toohello Tableau Online application, then set hello **Tableau Online Attributes** section as follows:</span></span>
     * <span data-ttu-id="8d78d-194">E-mailek: **mail** vagy **userprincipalname**</span><span class="sxs-lookup"><span data-stu-id="8d78d-194">Email: **mail** or **userprincipalname**</span></span>
     * <span data-ttu-id="8d78d-195">Utónév: **givenname**</span><span class="sxs-lookup"><span data-stu-id="8d78d-195">First name: **givenname**</span></span>
     * <span data-ttu-id="8d78d-196">Vezetéknév: **Vezetéknév**</span><span class="sxs-lookup"><span data-stu-id="8d78d-196">Last name: **surname**</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> <span data-ttu-id="8d78d-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8d78d-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8d78d-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8d78d-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8d78d-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8d78d-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8d78d-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d78d-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="8d78d-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8d78d-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8d78d-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8d78d-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d78d-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8d78d-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8d78d-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8d78d-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8d78d-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8d78d-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8d78d-213">a.</span><span class="sxs-lookup"><span data-stu-id="8d78d-213">a.</span></span> <span data-ttu-id="8d78d-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8d78d-215">b.</span><span class="sxs-lookup"><span data-stu-id="8d78d-215">b.</span></span> <span data-ttu-id="8d78d-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8d78d-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8d78d-217">c.</span><span class="sxs-lookup"><span data-stu-id="8d78d-217">c.</span></span> <span data-ttu-id="8d78d-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8d78d-219">d.</span><span class="sxs-lookup"><span data-stu-id="8d78d-219">d.</span></span> <span data-ttu-id="8d78d-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-220">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-online-test-user"></a><span data-ttu-id="8d78d-221">A Tableau Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d78d-221">Creating a Tableau Online test user</span></span>

<span data-ttu-id="8d78d-222">Ebben a szakaszban a Tableau Online Britta Simon nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8d78d-222">In this section, you create a user called Britta Simon in Tableau Online.</span></span>

1. <span data-ttu-id="8d78d-223">A **Tableau Online**, kattintson a **beállítások** , majd **hitelesítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8d78d-223">On **Tableau Online**, click **Settings** and then **Authentication** section.</span></span> <span data-ttu-id="8d78d-224">Görgessen lefelé, túl**felhasználók** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8d78d-224">Scroll down too**Select Users** section.</span></span> <span data-ttu-id="8d78d-225">Kattintson a **felhasználók hozzáadása az** , majd **adja meg az E-mail címek**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-225">Click **Add Users** and then **Enter Email Addresses**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. <span data-ttu-id="8d78d-227">Válassza ki **adja hozzá a felhasználók az egyszeri bejelentkezés (SSO) hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-227">Select **Add users for single sign-on (SSO) authentication**.</span></span> <span data-ttu-id="8d78d-228">A hello **E-mail címet adjon meg** szövegmező hozzáadásabritta.simon@contoso.com</span><span class="sxs-lookup"><span data-stu-id="8d78d-228">In hello **Enter Email Addresses** textbox add britta.simon@contoso.com</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. <span data-ttu-id="8d78d-230">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-230">Click **Create**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8d78d-231">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8d78d-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8d78d-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés Online hozzáférés tooTableau megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8d78d-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTableau Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8d78d-234">**tooassign Britta Simon tooTableau Online, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8d78d-234">**tooassign Britta Simon tooTableau Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d78d-235">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8d78d-237">Hello alkalmazások listában válassza ki a **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-237">In hello applications list, select **Tableau Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. <span data-ttu-id="8d78d-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8d78d-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8d78d-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8d78d-241">Click **Add** button.</span></span> <span data-ttu-id="8d78d-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8d78d-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8d78d-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8d78d-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8d78d-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8d78d-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8d78d-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8d78d-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8d78d-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8d78d-247">Testing single sign-on</span></span>

<span data-ttu-id="8d78d-248">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="8d78d-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="8d78d-249">Hello Tableau Online csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Tableau Online alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="8d78d-249">When you click hello Tableau Online tile in hello Access Panel, you should get automatically signed-on tooyour Tableau Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d78d-250">További források</span><span class="sxs-lookup"><span data-stu-id="8d78d-250">Additional resources</span></span>

* [<span data-ttu-id="8d78d-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8d78d-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8d78d-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8d78d-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

