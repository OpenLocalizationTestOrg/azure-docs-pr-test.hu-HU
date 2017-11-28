---
title: "Oktatóanyag: Azure Active Directoryval integrált AppDynamics |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és AppDynamics között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9b63afec73d7442e6ac1ce34b511beea6f43ffe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a><span data-ttu-id="d74aa-103">Oktatóanyag: Azure Active Directoryval integrált AppDynamics</span><span class="sxs-lookup"><span data-stu-id="d74aa-103">Tutorial: Azure Active Directory integration with AppDynamics</span></span>

<span data-ttu-id="d74aa-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate AppDynamics az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d74aa-104">In this tutorial, you learn how toointegrate AppDynamics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d74aa-105">AppDynamics integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d74aa-105">Integrating AppDynamics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d74aa-106">Megadhatja a hozzáférés tooAppDynamics rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d74aa-106">You can control in Azure AD who has access tooAppDynamics</span></span>
- <span data-ttu-id="d74aa-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAppDynamics (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d74aa-107">You can enable your users tooautomatically get signed-on tooAppDynamics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d74aa-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d74aa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d74aa-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d74aa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d74aa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d74aa-110">Prerequisites</span></span>

<span data-ttu-id="d74aa-111">az Azure AD integrálása AppDynamics tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d74aa-111">tooconfigure Azure AD integration with AppDynamics, you need hello following items:</span></span>

- <span data-ttu-id="d74aa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d74aa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d74aa-113">Egy AppDynamics egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d74aa-113">An AppDynamics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d74aa-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d74aa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d74aa-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d74aa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d74aa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d74aa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d74aa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d74aa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d74aa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d74aa-118">Scenario description</span></span>
<span data-ttu-id="d74aa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d74aa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d74aa-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d74aa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d74aa-121">Hello gyűjteményből AppDynamics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d74aa-121">Adding AppDynamics from hello gallery</span></span>
2. <span data-ttu-id="d74aa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d74aa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appdynamics-from-hello-gallery"></a><span data-ttu-id="d74aa-123">Hello gyűjteményből AppDynamics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d74aa-123">Adding AppDynamics from hello gallery</span></span>
<span data-ttu-id="d74aa-124">tooconfigure hello integrációja AppDynamics az Azure AD-be, meg kell tooadd AppDynamics hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d74aa-124">tooconfigure hello integration of AppDynamics into Azure AD, you need tooadd AppDynamics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d74aa-125">**tooadd AppDynamics hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d74aa-125">**tooadd AppDynamics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d74aa-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d74aa-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d74aa-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d74aa-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d74aa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d74aa-133">Hello keresési mezőbe, írja be a **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-133">In hello search box, type **AppDynamics**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_search.png)

5. <span data-ttu-id="d74aa-135">A hello eredmények panelen válassza ki a **AppDynamics**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-135">In hello results panel, select **AppDynamics**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d74aa-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d74aa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d74aa-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AppDynamics</span><span class="sxs-lookup"><span data-stu-id="d74aa-138">In this section, you configure and test Azure AD single sign-on with AppDynamics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d74aa-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó AppDynamics tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d74aa-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AppDynamics is tooa user in Azure AD.</span></span> <span data-ttu-id="d74aa-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello AppDynamics közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d74aa-140">In other words, a link relationship between an Azure AD user and hello related user in AppDynamics needs toobe established.</span></span>

<span data-ttu-id="d74aa-141">AppDynamics, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d74aa-141">In AppDynamics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d74aa-142">tooconfigure és az Azure AD az egyszeri bejelentkezés AppDynamics-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d74aa-142">tooconfigure and test Azure AD single sign-on with AppDynamics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d74aa-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d74aa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d74aa-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d74aa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d74aa-145">**[Egy AppDynamics tesztfelhasználó létrehozása](#creating-an-appdynamics-test-user)**  -toohave egy megfelelője a Britta Simon a AppDynamics, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d74aa-145">**[Creating an AppDynamics test user](#creating-an-appdynamics-test-user)** - toohave a counterpart of Britta Simon in AppDynamics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d74aa-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d74aa-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d74aa-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d74aa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d74aa-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d74aa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d74aa-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az AppDynamics alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d74aa-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AppDynamics application.</span></span>

<span data-ttu-id="d74aa-150">**az Azure AD tooconfigure egyszeri bejelentkezést a AppDynamics, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d74aa-150">**tooconfigure Azure AD single sign-on with AppDynamics, perform hello following steps:**</span></span>

1. <span data-ttu-id="d74aa-151">Az Azure portál, a hello hello **AppDynamics** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-151">In hello Azure portal, on hello **AppDynamics** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d74aa-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d74aa-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. <span data-ttu-id="d74aa-155">A hello **AppDynamics tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d74aa-155">On hello **AppDynamics Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_url.png)

    <span data-ttu-id="d74aa-157">a.</span><span class="sxs-lookup"><span data-stu-id="d74aa-157">a.</span></span> <span data-ttu-id="d74aa-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.saas.appdynamics.com`</span><span class="sxs-lookup"><span data-stu-id="d74aa-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.saas.appdynamics.com`</span></span>

    <span data-ttu-id="d74aa-159">b.</span><span class="sxs-lookup"><span data-stu-id="d74aa-159">b.</span></span> <span data-ttu-id="d74aa-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.saas.appdynamics.com/controller`</span><span class="sxs-lookup"><span data-stu-id="d74aa-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.saas.appdynamics.com/controller`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d74aa-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d74aa-161">These values are not real.</span></span> <span data-ttu-id="d74aa-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="d74aa-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d74aa-163">Ügyfél [AppDynamics ügyfél-támogatási csoport](https://www.appdynamics.com/support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d74aa-163">Contact [AppDynamics Client support team](https://www.appdynamics.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="d74aa-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d74aa-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_certificate.png) 

5. <span data-ttu-id="d74aa-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d74aa-168">A hello **AppDynamics konfigurációs** kattintson **konfigurálása AppDynamics** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d74aa-168">On hello **AppDynamics Configuration** section, click **Configure AppDynamics** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d74aa-169">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d74aa-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_configure.png) 

7. <span data-ttu-id="d74aa-171">Egy másik webes böngészőablakban jelentkezzen tooyour AppDynamics vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d74aa-171">In a different web browser window, log in tooyour AppDynamics company site as an administrator.</span></span>

8. <span data-ttu-id="d74aa-172">Hello hello felső eszköztárán kattintson **beállítások**, és kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-172">In hello toolbar on hello top, click **Settings**, and then click **Administration**.</span></span>
   
    <span data-ttu-id="d74aa-173">![Felügyeleti](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="d74aa-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span></span>

9. <span data-ttu-id="d74aa-174">Kattintson a hello **hitelesítési szolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="d74aa-174">Click hello **Authentication Provider** tab.</span></span>
   
    <span data-ttu-id="d74aa-175">![Hitelesítésszolgáltató](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "hitelesítési szolgáltató")</span><span class="sxs-lookup"><span data-stu-id="d74aa-175">![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "Authentication Provider")</span></span>

10. <span data-ttu-id="d74aa-176">A hello **hitelesítési szolgáltató** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d74aa-176">In hello **Authentication Provider** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d74aa-177">![SAML-alapú konfigurációs](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="d74aa-177">![SAML Configuration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML Configuration")</span></span>   

    <span data-ttu-id="d74aa-178">a.</span><span class="sxs-lookup"><span data-stu-id="d74aa-178">a.</span></span> <span data-ttu-id="d74aa-179">Mint **hitelesítési szolgáltató**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-179">As **Authentication Provider**, select **SAML**.</span></span>

    <span data-ttu-id="d74aa-180">b.</span><span class="sxs-lookup"><span data-stu-id="d74aa-180">b.</span></span> <span data-ttu-id="d74aa-181">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d74aa-181">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d74aa-182">c.</span><span class="sxs-lookup"><span data-stu-id="d74aa-182">c.</span></span> <span data-ttu-id="d74aa-183">A hello **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d74aa-183">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="d74aa-184">d.</span><span class="sxs-lookup"><span data-stu-id="d74aa-184">d.</span></span> <span data-ttu-id="d74aa-185">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="d74aa-185">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** textbox</span></span>

    <span data-ttu-id="d74aa-186">e.</span><span class="sxs-lookup"><span data-stu-id="d74aa-186">e.</span></span> <span data-ttu-id="d74aa-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-187">Click **Save**.</span></span>

     <span data-ttu-id="d74aa-188">![Mentés](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "mentése")</span><span class="sxs-lookup"><span data-stu-id="d74aa-188">![Save](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "Save")</span></span>

> [!TIP]
> <span data-ttu-id="d74aa-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d74aa-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d74aa-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d74aa-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d74aa-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d74aa-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d74aa-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d74aa-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="d74aa-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d74aa-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d74aa-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d74aa-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d74aa-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d74aa-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d74aa-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d74aa-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d74aa-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d74aa-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d74aa-204">a.</span><span class="sxs-lookup"><span data-stu-id="d74aa-204">a.</span></span> <span data-ttu-id="d74aa-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d74aa-206">b.</span><span class="sxs-lookup"><span data-stu-id="d74aa-206">b.</span></span> <span data-ttu-id="d74aa-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d74aa-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d74aa-208">c.</span><span class="sxs-lookup"><span data-stu-id="d74aa-208">c.</span></span> <span data-ttu-id="d74aa-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d74aa-210">d.</span><span class="sxs-lookup"><span data-stu-id="d74aa-210">d.</span></span> <span data-ttu-id="d74aa-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-211">Click **Create**.</span></span>
 
### <a name="creating-an-appdynamics-test-user"></a><span data-ttu-id="d74aa-212">Egy AppDynamics tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d74aa-212">Creating an AppDynamics test user</span></span>

<span data-ttu-id="d74aa-213">az Azure AD tooenable felhasználók toolog a tooAppDynamics, akkor ki kell építenie AppDynamics be.</span><span class="sxs-lookup"><span data-stu-id="d74aa-213">tooenable Azure AD users toolog in tooAppDynamics, they must be provisioned into AppDynamics.</span></span> <span data-ttu-id="d74aa-214">AppDynamics hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d74aa-214">In hello case of AppDynamics, provisioning is a manual task.</span></span>

<span data-ttu-id="d74aa-215">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d74aa-215">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="d74aa-216">Jelentkezzen be tooyour AppDynamics vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d74aa-216">Log in tooyour AppDynamics company site as an administrator.</span></span>

2. <span data-ttu-id="d74aa-217">Nyissa meg túl**felhasználók**, és kattintson a  **+**  tooopen hello **felhasználó létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d74aa-217">Go too**Users**, and then click **+** tooopen hello **Create User** dialog.</span></span>
   
    <span data-ttu-id="d74aa-218">![Felhasználók](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="d74aa-218">![Users](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "Users")</span></span>

3. <span data-ttu-id="d74aa-219">A hello **felhasználó létrehozása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d74aa-219">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d74aa-220">![Hozzon létre felhasználói](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="d74aa-220">![Create User](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "Create User")</span></span>
   
    <span data-ttu-id="d74aa-221">a.</span><span class="sxs-lookup"><span data-stu-id="d74aa-221">a.</span></span> <span data-ttu-id="d74aa-222">Típus hello **felhasználónév**, **neve**, **E-mail**, **új jelszó**, **ismételje meg az új jelszó** az érvényes AAD tooprovision hello a kívánt fiók kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="d74aa-222">Type hello **Username**, **Name**, **Email**, **New Password**, **Repeat New Password** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="d74aa-223">b.</span><span class="sxs-lookup"><span data-stu-id="d74aa-223">b.</span></span> <span data-ttu-id="d74aa-224">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-224">Click **Save**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="d74aa-225">Bármely más AppDynamics felhasználói fiók létrehozása eszközök vagy AppDynamics tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="d74aa-225">You can use any other AppDynamics user account creation tools or APIs provided by AppDynamics tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d74aa-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d74aa-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d74aa-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAppDynamics megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d74aa-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAppDynamics.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d74aa-229">**tooassign Britta Simon tooAppDynamics, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d74aa-229">**tooassign Britta Simon tooAppDynamics, perform hello following steps:**</span></span>

1. <span data-ttu-id="d74aa-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d74aa-232">Hello alkalmazások listában válassza ki a **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-232">In hello applications list, select **AppDynamics**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_app.png) 

3. <span data-ttu-id="d74aa-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d74aa-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d74aa-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d74aa-236">Click **Add** button.</span></span> <span data-ttu-id="d74aa-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d74aa-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d74aa-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d74aa-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d74aa-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d74aa-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d74aa-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d74aa-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d74aa-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d74aa-242">Testing single sign-on</span></span>

<span data-ttu-id="d74aa-243">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="d74aa-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d74aa-244">Hello AppDynamics hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour AppDynamics alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d74aa-244">When you click hello AppDynamics tile in hello Access Panel, you should get automatically signed-on tooyour AppDynamics application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d74aa-245">További források</span><span class="sxs-lookup"><span data-stu-id="d74aa-245">Additional resources</span></span>

* [<span data-ttu-id="d74aa-246">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d74aa-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d74aa-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d74aa-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_203.png

