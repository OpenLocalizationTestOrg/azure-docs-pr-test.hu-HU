---
title: "Oktatóanyag: Azure Active Directoryval integrált vászonra Lms |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a vászonra LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 8f4a09266a108e2c92326b0909dd0650b1c84d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a><span data-ttu-id="84ca3-103">Oktatóanyag: Azure Active Directoryval integrált vászonra LMS</span><span class="sxs-lookup"><span data-stu-id="84ca3-103">Tutorial: Azure Active Directory integration with Canvas LMS</span></span>

<span data-ttu-id="84ca3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate vászonra az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84ca3-104">In this tutorial, you learn how toointegrate Canvas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84ca3-105">Vászonra integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="84ca3-105">Integrating Canvas with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="84ca3-106">Megadhatja a hozzáférés tooCanvas rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="84ca3-106">You can control in Azure AD who has access tooCanvas</span></span>
- <span data-ttu-id="84ca3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCanvas (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="84ca3-107">You can enable your users tooautomatically get signed-on tooCanvas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84ca3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="84ca3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="84ca3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84ca3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84ca3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84ca3-110">Prerequisites</span></span>

<span data-ttu-id="84ca3-111">tooconfigure az Azure AD-integráció a vásznon, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="84ca3-111">tooconfigure Azure AD integration with Canvas, you need hello following items:</span></span>

- <span data-ttu-id="84ca3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84ca3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84ca3-113">A vászon egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="84ca3-113">A Canvas single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84ca3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="84ca3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84ca3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="84ca3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84ca3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84ca3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84ca3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84ca3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84ca3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84ca3-118">Scenario description</span></span>
<span data-ttu-id="84ca3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84ca3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84ca3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84ca3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84ca3-121">Hello gyűjteményből felvétele a vászonra</span><span class="sxs-lookup"><span data-stu-id="84ca3-121">Adding Canvas from hello gallery</span></span>
2. <span data-ttu-id="84ca3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84ca3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-canvas-from-hello-gallery"></a><span data-ttu-id="84ca3-123">Hello gyűjteményből felvétele a vászonra</span><span class="sxs-lookup"><span data-stu-id="84ca3-123">Adding Canvas from hello gallery</span></span>
<span data-ttu-id="84ca3-124">tooconfigure hello integráció a vásznon, az Azure AD-be, meg kell tooadd vásznon a hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="84ca3-124">tooconfigure hello integration of Canvas into Azure AD, you need tooadd Canvas from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="84ca3-125">**tooadd vásznon a hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="84ca3-125">**tooadd Canvas from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="84ca3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84ca3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="84ca3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="84ca3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="84ca3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="84ca3-133">Hello keresési mezőbe, írja be a **vászonra**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-133">In hello search box, type **Canvas**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. <span data-ttu-id="84ca3-135">A hello eredmények panelen válassza ki a **vászonra**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-135">In hello results panel, select **Canvas**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84ca3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84ca3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84ca3-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a vászon "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="84ca3-138">In this section, you configure and test Azure AD single sign-on with Canvas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="84ca3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a vásznon tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="84ca3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Canvas is tooa user in Azure AD.</span></span> <span data-ttu-id="84ca3-140">Ez azt jelenti a vásznon a hello kapcsolódó felhasználói és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="84ca3-140">In other words, a link relationship between an Azure AD user and hello related user in Canvas needs toobe established.</span></span>

<span data-ttu-id="84ca3-141">A vásznon, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="84ca3-141">In Canvas, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="84ca3-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a vásznon, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="84ca3-142">tooconfigure and test Azure AD single sign-on with Canvas, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="84ca3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="84ca3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="84ca3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84ca3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84ca3-145">**[Vászonra tesztfelhasználó létrehozása](#creating-a-canvas-test-user)**  -toohave egy megfelelője a Britta Simon a vásznon, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="84ca3-145">**[Creating a Canvas test user](#creating-a-canvas-test-user)** - toohave a counterpart of Britta Simon in Canvas that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="84ca3-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="84ca3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84ca3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="84ca3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84ca3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84ca3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84ca3-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a vászonra alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="84ca3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Canvas application.</span></span>

<span data-ttu-id="84ca3-150">**az Azure AD tooconfigure egyszeri bejelentkezést a vásznon, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="84ca3-150">**tooconfigure Azure AD single sign-on with Canvas, perform hello following steps:**</span></span>

1. <span data-ttu-id="84ca3-151">Az Azure portál, a hello hello **vászonra** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-151">In hello Azure portal, on hello **Canvas** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="84ca3-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="84ca3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. <span data-ttu-id="84ca3-155">A hello **vászonra tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84ca3-155">On hello **Canvas Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    <span data-ttu-id="84ca3-157">a.</span><span class="sxs-lookup"><span data-stu-id="84ca3-157">a.</span></span> <span data-ttu-id="84ca3-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.instructure.com`</span><span class="sxs-lookup"><span data-stu-id="84ca3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.instructure.com`</span></span>

    <span data-ttu-id="84ca3-159">b.</span><span class="sxs-lookup"><span data-stu-id="84ca3-159">b.</span></span> <span data-ttu-id="84ca3-160">A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<tenant-name>.instructure.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="84ca3-160">In hello **Identifier** textbox, type hello value using hello following pattern: `https://<tenant-name>.instructure.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84ca3-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="84ca3-161">These values are not real.</span></span> <span data-ttu-id="84ca3-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="84ca3-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="84ca3-163">Ügyfél [vászonra ügyfél-támogatási csoport](https://community.canvaslms.com/community/help) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="84ca3-163">Contact [Canvas Client support team](https://community.canvaslms.com/community/help) tooget these values.</span></span> 
 
4. <span data-ttu-id="84ca3-164">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="84ca3-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. <span data-ttu-id="84ca3-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="84ca3-168">A hello **vászonra konfigurációs** területén kattintson **konfigurálása vászonra** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="84ca3-168">On hello **Canvas Configuration** section, click **Configure Canvas** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="84ca3-169">Másolás hello **jelszó URL-cím módosítása, Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="84ca3-169">Copy hello **Change Password URL, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. <span data-ttu-id="84ca3-171">Egy másik webes böngészőablakban jelentkezzen tooyour vászonra vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="84ca3-171">In a different web browser window, log in tooyour Canvas company site as an administrator.</span></span>

8. <span data-ttu-id="84ca3-172">Nyissa meg túl**tanfolyamokat \> felügyelt fiókokat \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-172">Go too**Courses \> Managed Accounts \> Microsoft**.</span></span>
   
    <span data-ttu-id="84ca3-173">![Vászonra](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "vászonra")</span><span class="sxs-lookup"><span data-stu-id="84ca3-173">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

9. <span data-ttu-id="84ca3-174">Hello bal oldali hello navigációs ablaktábláján válassza ki **hitelesítési**, és kattintson a **új SAML-Config hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-174">In hello navigation pane on hello left, select **Authentication**, and then click **Add New SAML Config**.</span></span>
   
    <span data-ttu-id="84ca3-175">![Hitelesítési](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="84ca3-175">![Authentication](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")</span></span>

10. <span data-ttu-id="84ca3-176">Hello aktuális integráció lapján hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="84ca3-176">On hello Current Integration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="84ca3-177">![Aktuális integrációs](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuális integráció")</span><span class="sxs-lookup"><span data-stu-id="84ca3-177">![Current Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")</span></span>

    <span data-ttu-id="84ca3-178">a.</span><span class="sxs-lookup"><span data-stu-id="84ca3-178">a.</span></span> <span data-ttu-id="84ca3-179">A **IdP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84ca3-179">In **IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="84ca3-180">b.</span><span class="sxs-lookup"><span data-stu-id="84ca3-180">b.</span></span> <span data-ttu-id="84ca3-181">A **URL-cím napló** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84ca3-181">In **Log On URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal .</span></span>

    <span data-ttu-id="84ca3-182">c.</span><span class="sxs-lookup"><span data-stu-id="84ca3-182">c.</span></span> <span data-ttu-id="84ca3-183">A **napló kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84ca3-183">In **Log Out URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="84ca3-184">d.</span><span class="sxs-lookup"><span data-stu-id="84ca3-184">d.</span></span> <span data-ttu-id="84ca3-185">A **módosítás jelszó hivatkozásra** szövegmezőhöz Beillesztés hello értékének **jelszó URL-cím módosítása** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84ca3-185">In **Change Password Link** textbox, paste hello value of **Change Password URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="84ca3-186">e.</span><span class="sxs-lookup"><span data-stu-id="84ca3-186">e.</span></span> <span data-ttu-id="84ca3-187">A **tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** érték tanúsítvány, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84ca3-187">In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>      
        
    <span data-ttu-id="84ca3-188">f.</span><span class="sxs-lookup"><span data-stu-id="84ca3-188">f.</span></span> <span data-ttu-id="84ca3-189">A hello **bejelentkezési attribútum** listáról válassza ki **NameID**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-189">From hello **Login Attribute** list, select **NameID**.</span></span>

    <span data-ttu-id="84ca3-190">g.</span><span class="sxs-lookup"><span data-stu-id="84ca3-190">g.</span></span> <span data-ttu-id="84ca3-191">A hello **azonosító formátuma** listáról válassza ki **emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-191">From hello **Identifier Format** list, select **emailAddress**.</span></span>

    <span data-ttu-id="84ca3-192">h.</span><span class="sxs-lookup"><span data-stu-id="84ca3-192">h.</span></span> <span data-ttu-id="84ca3-193">Kattintson a **hitelesítési beállításainak mentése**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-193">Click **Save Authentication Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="84ca3-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="84ca3-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="84ca3-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="84ca3-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="84ca3-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84ca3-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84ca3-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84ca3-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="84ca3-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="84ca3-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="84ca3-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="84ca3-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="84ca3-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84ca3-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84ca3-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84ca3-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84ca3-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84ca3-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84ca3-209">a.</span><span class="sxs-lookup"><span data-stu-id="84ca3-209">a.</span></span> <span data-ttu-id="84ca3-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84ca3-211">b.</span><span class="sxs-lookup"><span data-stu-id="84ca3-211">b.</span></span> <span data-ttu-id="84ca3-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84ca3-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84ca3-213">c.</span><span class="sxs-lookup"><span data-stu-id="84ca3-213">c.</span></span> <span data-ttu-id="84ca3-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="84ca3-215">d.</span><span class="sxs-lookup"><span data-stu-id="84ca3-215">d.</span></span> <span data-ttu-id="84ca3-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-216">Click **Create**.</span></span>
 
### <a name="creating-a-canvas-test-user"></a><span data-ttu-id="84ca3-217">Vászonra tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84ca3-217">Creating a Canvas test user</span></span>

<span data-ttu-id="84ca3-218">az Azure AD tooenable felhasználók toolog tooCanvas, az ezeket ki kell építenie a vászonra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-218">tooenable Azure AD users toolog in tooCanvas, they must be provisioned into Canvas.</span></span>

<span data-ttu-id="84ca3-219">Esetén a vásznon a felhasználók átadása kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="84ca3-219">In case of Canvas, user provisioning is a manual task.</span></span>

<span data-ttu-id="84ca3-220">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="84ca3-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="84ca3-221">Jelentkezzen be tooyour **vászonra** bérlő.</span><span class="sxs-lookup"><span data-stu-id="84ca3-221">Log in tooyour **Canvas** tenant.</span></span>

2. <span data-ttu-id="84ca3-222">Nyissa meg túl**tanfolyamokat \> felügyelt fiókokat \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-222">Go too**Courses \> Managed Accounts \> Microsoft**.</span></span>
   
   <span data-ttu-id="84ca3-223">![Vászonra](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "vászonra")</span><span class="sxs-lookup"><span data-stu-id="84ca3-223">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

3. <span data-ttu-id="84ca3-224">Kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-224">Click **Users**.</span></span>
   
   <span data-ttu-id="84ca3-225">![Felhasználók](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="84ca3-225">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")</span></span>

4. <span data-ttu-id="84ca3-226">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-226">Click **Add New User**.</span></span>
   
   <span data-ttu-id="84ca3-227">![Felhasználók](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="84ca3-227">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")</span></span>

5. <span data-ttu-id="84ca3-228">Hello hozzáadása egy új felhasználó párbeszédpanel lap hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="84ca3-228">On hello Add a New User dialog page, perform hello following steps:</span></span>
   
   <span data-ttu-id="84ca3-229">![Felhasználó hozzáadása](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="84ca3-229">![Add User](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")</span></span>
   
   <span data-ttu-id="84ca3-230">a.</span><span class="sxs-lookup"><span data-stu-id="84ca3-230">a.</span></span> <span data-ttu-id="84ca3-231">A hello **teljes nevét** szövegmező, írja be például a felhasználó nevét hello **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-231">In hello **Full Name** textbox, enter hello name of user like **BrittaSimon**.</span></span>

   <span data-ttu-id="84ca3-232">b.</span><span class="sxs-lookup"><span data-stu-id="84ca3-232">b.</span></span> <span data-ttu-id="84ca3-233">A hello **E-mail** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="84ca3-233">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="84ca3-234">c.</span><span class="sxs-lookup"><span data-stu-id="84ca3-234">c.</span></span> <span data-ttu-id="84ca3-235">A hello **bejelentkezési** szövegmező, adja meg a hello felhasználó az Azure AD e-mail címét például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="84ca3-235">In hello **Login** textbox, enter hello user’s Azure AD email address like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="84ca3-236">d.</span><span class="sxs-lookup"><span data-stu-id="84ca3-236">d.</span></span> <span data-ttu-id="84ca3-237">Válassza ki **E-mail hello felhasználót a fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-237">Select **Email hello user about this account creation**.</span></span>

   <span data-ttu-id="84ca3-238">e.</span><span class="sxs-lookup"><span data-stu-id="84ca3-238">e.</span></span> <span data-ttu-id="84ca3-239">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-239">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="84ca3-240">Bármely más vászonra felhasználói fiók létrehozása eszközök vagy vászonra tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="84ca3-240">You can use any other Canvas user account creation tools or APIs provided by Canvas tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="84ca3-241">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="84ca3-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="84ca3-242">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCanvas megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="84ca3-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCanvas.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="84ca3-244">**tooassign Britta Simon tooCanvas, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="84ca3-244">**tooassign Britta Simon tooCanvas, perform hello following steps:**</span></span>

1. <span data-ttu-id="84ca3-245">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84ca3-247">Hello alkalmazások listában válassza ki a **vászonra**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-247">In hello applications list, select **Canvas**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. <span data-ttu-id="84ca3-249">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84ca3-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="84ca3-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84ca3-251">Click **Add** button.</span></span> <span data-ttu-id="84ca3-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84ca3-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="84ca3-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84ca3-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="84ca3-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84ca3-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84ca3-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84ca3-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84ca3-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84ca3-257">Testing single sign-on</span></span>

<span data-ttu-id="84ca3-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="84ca3-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="84ca3-259">Hello vászonra csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour vászonra alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="84ca3-259">When you click hello Canvas tile in hello Access Panel, you should get automatically signed-on tooyour Canvas application.</span></span>
<span data-ttu-id="84ca3-260">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84ca3-260">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84ca3-261">További források</span><span class="sxs-lookup"><span data-stu-id="84ca3-261">Additional resources</span></span>

* [<span data-ttu-id="84ca3-262">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="84ca3-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84ca3-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84ca3-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

