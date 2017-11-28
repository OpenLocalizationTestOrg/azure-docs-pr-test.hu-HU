---
title: "Oktatóanyag: Azure Active Directory-integráció kis javításait |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és kis fejlesztései között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 33213fe4b61f5005cf78bee2c05b2b1e5e71ae8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a><span data-ttu-id="453d0-103">Oktatóanyag: Azure Active Directoryval integrált kis fejlesztései</span><span class="sxs-lookup"><span data-stu-id="453d0-103">Tutorial: Azure Active Directory integration with Small Improvements</span></span>

<span data-ttu-id="453d0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate az Azure Active Directory (Azure AD) kis fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="453d0-104">In this tutorial, you learn how toointegrate Small Improvements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="453d0-105">Kis fejlesztései integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="453d0-105">Integrating Small Improvements with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="453d0-106">Megadhatja a hozzáférés tooSmall fejlesztései rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="453d0-106">You can control in Azure AD who has access tooSmall Improvements</span></span>
- <span data-ttu-id="453d0-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSmall fejlesztései (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="453d0-107">You can enable your users tooautomatically get signed-on tooSmall Improvements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="453d0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="453d0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="453d0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="453d0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="453d0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="453d0-110">Prerequisites</span></span>

<span data-ttu-id="453d0-111">tooconfigure az Azure AD integrálása kisméretű fejlesztései, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="453d0-111">tooconfigure Azure AD integration with Small Improvements, you need hello following items:</span></span>

- <span data-ttu-id="453d0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="453d0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="453d0-113">Egy kis fejlesztései az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="453d0-113">A Small Improvements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="453d0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="453d0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="453d0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="453d0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="453d0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="453d0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="453d0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="453d0-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="453d0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="453d0-118">Scenario description</span></span>
<span data-ttu-id="453d0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="453d0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="453d0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="453d0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="453d0-121">Kis fejlesztései hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="453d0-121">Adding Small Improvements from hello gallery</span></span>
2. <span data-ttu-id="453d0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="453d0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-small-improvements-from-hello-gallery"></a><span data-ttu-id="453d0-123">Kis fejlesztései hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="453d0-123">Adding Small Improvements from hello gallery</span></span>
<span data-ttu-id="453d0-124">tooconfigure hello integrálása kisméretű fejlesztései az Azure AD-be, meg kell tooadd kis fejlesztései hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="453d0-124">tooconfigure hello integration of Small Improvements into Azure AD, you need tooadd Small Improvements from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="453d0-125">**tooadd kis fejlesztései hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="453d0-125">**tooadd Small Improvements from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="453d0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="453d0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="453d0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="453d0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="453d0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="453d0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="453d0-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="453d0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="453d0-133">Hello keresési mezőbe, írja be a **kis fejlesztései**.</span><span class="sxs-lookup"><span data-stu-id="453d0-133">In hello search box, type **Small Improvements**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_search.png)

5. <span data-ttu-id="453d0-135">A hello eredmények panelen válassza ki a **kis fejlesztései**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="453d0-135">In hello results panel, select **Small Improvements**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="453d0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="453d0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="453d0-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján kis fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="453d0-138">In this section, you configure and test Azure AD single sign-on with Small Improvements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="453d0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó kis fejlesztései tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="453d0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Small Improvements is tooa user in Azure AD.</span></span> <span data-ttu-id="453d0-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello kis fejlesztései közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="453d0-140">In other words, a link relationship between an Azure AD user and hello related user in Small Improvements needs toobe established.</span></span>

<span data-ttu-id="453d0-141">Kis fejlesztései, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="453d0-141">In Small Improvements, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="453d0-142">tooconfigure és kis javításait az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="453d0-142">tooconfigure and test Azure AD single sign-on with Small Improvements, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="453d0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="453d0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="453d0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="453d0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="453d0-145">**[Kis fejlesztései tesztfelhasználó létrehozása](#creating-a-small-improvements-test-user)**  -toohave Britta Simon a csatolt toohello az Azure AD felhasználói ábrázolása kis fejlesztésekkel egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="453d0-145">**[Creating a Small Improvements test user](#creating-a-small-improvements-test-user)** - toohave a counterpart of Britta Simon in Small Improvements that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="453d0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="453d0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="453d0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="453d0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="453d0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="453d0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="453d0-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és kis fejlesztései alkalmazásában egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="453d0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Small Improvements application.</span></span>

<span data-ttu-id="453d0-150">**az Azure AD tooconfigure egyszeri bejelentkezés kis javításait, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="453d0-150">**tooconfigure Azure AD single sign-on with Small Improvements, perform hello following steps:**</span></span>

1. <span data-ttu-id="453d0-151">Az Azure portál, a hello hello **kis fejlesztései** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="453d0-151">In hello Azure portal, on hello **Small Improvements** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="453d0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="453d0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_samlbase.png)

3. <span data-ttu-id="453d0-155">A hello **kis fejlesztései tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="453d0-155">On hello **Small Improvements Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_url.png)

    <span data-ttu-id="453d0-157">a.</span><span class="sxs-lookup"><span data-stu-id="453d0-157">a.</span></span> <span data-ttu-id="453d0-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.small-improvements.com`</span><span class="sxs-lookup"><span data-stu-id="453d0-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.small-improvements.com`</span></span>

    <span data-ttu-id="453d0-159">b.</span><span class="sxs-lookup"><span data-stu-id="453d0-159">b.</span></span> <span data-ttu-id="453d0-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.small-improvements.com`</span><span class="sxs-lookup"><span data-stu-id="453d0-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.small-improvements.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="453d0-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="453d0-161">These values are not real.</span></span> <span data-ttu-id="453d0-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="453d0-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="453d0-163">Ügyfél [kis fejlesztései ügyfél-támogatási csoport](mailto:support@small-improvements.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="453d0-163">Contact [Small Improvements Client support team](mailto:support@small-improvements.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="453d0-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="453d0-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_certificate.png) 

5. <span data-ttu-id="453d0-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="453d0-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="453d0-168">A hello **kis fejlesztései konfigurációs** területén kattintson **kis fejlesztései konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="453d0-168">On hello **Small Improvements Configuration** section, click **Configure Small Improvements** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="453d0-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="453d0-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_configure.png) 

7. <span data-ttu-id="453d0-171">Egy másik böngészőablakban bejelentkezéskor tooyour kis fejlesztései vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="453d0-171">In another browser window, sign on tooyour Small Improvements company site as an administrator.</span></span>

8. <span data-ttu-id="453d0-172">A hello fő irányítópult-oldalon, kattintson az **felügyeleti** hello bal oldali gomb.</span><span class="sxs-lookup"><span data-stu-id="453d0-172">From hello main dashboard page, click **Administration** button on hello left.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

9. <span data-ttu-id="453d0-174">Kattintson a hello **SAML SSO** gombot **integrációja** szakasz.</span><span class="sxs-lookup"><span data-stu-id="453d0-174">Click hello **SAML SSO** button from **Integrations** section.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

10. <span data-ttu-id="453d0-176">Hello egyszeri bejelentkezés beállítása lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="453d0-176">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    <span data-ttu-id="453d0-178">a.</span><span class="sxs-lookup"><span data-stu-id="453d0-178">a.</span></span> <span data-ttu-id="453d0-179">A hello **HTTP-végpont** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="453d0-179">In hello **HTTP Endpoint** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="453d0-180">b.</span><span class="sxs-lookup"><span data-stu-id="453d0-180">b.</span></span> <span data-ttu-id="453d0-181">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom, és illessze be hello **x509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="453d0-181">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="453d0-182">c.</span><span class="sxs-lookup"><span data-stu-id="453d0-182">c.</span></span> <span data-ttu-id="453d0-183">Ha a toohave egyszeri bejelentkezés és bejelentkezés hitelesítési lehetőséget a felhasználók, majd ellenőrizze a hello **bejelentkezési azonosító és jelszó hozzáférésének engedélyezése túl** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="453d0-183">If you wish toohave SSO and Login form authentication option available for users, then check hello **Enable access via login/password too** option.</span></span>  

    <span data-ttu-id="453d0-184">d.</span><span class="sxs-lookup"><span data-stu-id="453d0-184">d.</span></span> <span data-ttu-id="453d0-185">Adja meg a hello megfelelő értéket a tooName hello Egyszeri bejelentkezési gomb hello **SAML Rákérdezés** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="453d0-185">Enter hello appropriate value tooName hello SSO Login button in hello **SAML Prompt** textbox.</span></span>  

    <span data-ttu-id="453d0-186">e.</span><span class="sxs-lookup"><span data-stu-id="453d0-186">e.</span></span> <span data-ttu-id="453d0-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="453d0-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="453d0-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="453d0-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="453d0-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="453d0-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="453d0-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="453d0-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="453d0-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="453d0-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="453d0-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="453d0-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="453d0-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="453d0-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="453d0-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="453d0-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="453d0-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="453d0-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="453d0-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="453d0-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="453d0-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="453d0-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="453d0-203">a.</span><span class="sxs-lookup"><span data-stu-id="453d0-203">a.</span></span> <span data-ttu-id="453d0-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="453d0-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="453d0-205">b.</span><span class="sxs-lookup"><span data-stu-id="453d0-205">b.</span></span> <span data-ttu-id="453d0-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="453d0-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="453d0-207">c.</span><span class="sxs-lookup"><span data-stu-id="453d0-207">c.</span></span> <span data-ttu-id="453d0-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="453d0-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="453d0-209">d.</span><span class="sxs-lookup"><span data-stu-id="453d0-209">d.</span></span> <span data-ttu-id="453d0-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="453d0-210">Click **Create**.</span></span>
 
### <a name="creating-a-small-improvements-test-user"></a><span data-ttu-id="453d0-211">Kis fejlesztései tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="453d0-211">Creating a Small Improvements test user</span></span>

<span data-ttu-id="453d0-212">az Azure AD tooenable felhasználók toolog tooSmall fejlesztései, a azok ki kell építenie a kis fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="453d0-212">tooenable Azure AD users toolog in tooSmall Improvements, they must be provisioned into Small Improvements.</span></span> <span data-ttu-id="453d0-213">Kis fejlesztései hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="453d0-213">In hello case of Small Improvements, provisioning is a manual task.</span></span>

<span data-ttu-id="453d0-214">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="453d0-214">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="453d0-215">Bejelentkezés tooyour kis fejlesztései vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="453d0-215">Sign-on tooyour Small Improvements company site as an administrator.</span></span>

2. <span data-ttu-id="453d0-216">Hello kezdőlapról lépjen a bal oldali hello toohello menüben, kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="453d0-216">From hello Home page, go toohello menu on hello left, click **Administration**.</span></span>

3. <span data-ttu-id="453d0-217">Hello kattintson **felhasználói Directory** gomb felhasználókezelés szakaszából.</span><span class="sxs-lookup"><span data-stu-id="453d0-217">Click hello **User Directory** button from User Management section.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

4. <span data-ttu-id="453d0-219">Kattintson a **felhasználók hozzáadása az**.</span><span class="sxs-lookup"><span data-stu-id="453d0-219">Click **Add users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

5. <span data-ttu-id="453d0-221">A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="453d0-221">On hello **Add Users** dialog, perform hello following steps:</span></span> 

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_12.png)
    
    <span data-ttu-id="453d0-223">a.</span><span class="sxs-lookup"><span data-stu-id="453d0-223">a.</span></span> <span data-ttu-id="453d0-224">Adja meg a hello **Utónév** például a felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="453d0-224">Enter hello **first name** of user like **Britta**.</span></span>

    <span data-ttu-id="453d0-225">b.</span><span class="sxs-lookup"><span data-stu-id="453d0-225">b.</span></span> <span data-ttu-id="453d0-226">Adja meg a hello **Vezetéknév** például a felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="453d0-226">Enter hello **Last name** of user like **Simon**.</span></span>

    <span data-ttu-id="453d0-227">c.</span><span class="sxs-lookup"><span data-stu-id="453d0-227">c.</span></span> <span data-ttu-id="453d0-228">Adja meg a hello **E-mail** például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="453d0-228">Enter hello **Email** of user like **brittasimon@contoso.com**.</span></span> 

    <span data-ttu-id="453d0-229">d.</span><span class="sxs-lookup"><span data-stu-id="453d0-229">d.</span></span> <span data-ttu-id="453d0-230">Másik lehetőségként tooenter személyes üdvözlőüzenetére hello **értesítő e-mail küldése** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="453d0-230">You can also choose tooenter hello personal message in hello **Send notification email** box.</span></span> <span data-ttu-id="453d0-231">Ha nem kíván toosend hello értesítési, majd törölje ezt a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="453d0-231">If you do not wish toosend hello notification, then uncheck this checkbox.</span></span>

    <span data-ttu-id="453d0-232">e.</span><span class="sxs-lookup"><span data-stu-id="453d0-232">e.</span></span> <span data-ttu-id="453d0-233">Kattintson a **létrehozása a felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="453d0-233">Click **Create Users**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="453d0-234">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="453d0-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="453d0-235">Ebben a szakaszban engedélyezése Azure egyszeri bejelentkezés Britta Simon toouse tooSmall fejlesztései hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="453d0-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSmall Improvements.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="453d0-237">**tooassign Britta Simon tooSmall fejlesztései, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="453d0-237">**tooassign Britta Simon tooSmall Improvements, perform hello following steps:**</span></span>

1. <span data-ttu-id="453d0-238">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="453d0-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="453d0-240">Hello alkalmazások listában válassza ki a **kis fejlesztései**.</span><span class="sxs-lookup"><span data-stu-id="453d0-240">In hello applications list, select **Small Improvements**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_app.png) 

3. <span data-ttu-id="453d0-242">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="453d0-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="453d0-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="453d0-244">Click **Add** button.</span></span> <span data-ttu-id="453d0-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="453d0-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="453d0-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="453d0-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="453d0-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="453d0-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="453d0-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="453d0-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="453d0-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="453d0-250">Testing single sign-on</span></span>

<span data-ttu-id="453d0-251">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="453d0-251">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="453d0-252">Hello kis fejlesztései hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour kis fejlesztései alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="453d0-252">When you click hello Small Improvements tile in hello Access Panel, you should get automatically signed-on tooyour Small Improvements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="453d0-253">További források</span><span class="sxs-lookup"><span data-stu-id="453d0-253">Additional resources</span></span>

* [<span data-ttu-id="453d0-254">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="453d0-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="453d0-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="453d0-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_203.png

