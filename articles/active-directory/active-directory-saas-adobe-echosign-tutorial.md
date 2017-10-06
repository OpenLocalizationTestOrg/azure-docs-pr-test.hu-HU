---
title: "Oktatóanyag: Azure Active Directory-integráció Adobe bejelentkezési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Adobe bejelentkezési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b4b07907f30a0890003554a02a76d968400b43ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a><span data-ttu-id="71e1d-103">Oktatóanyag: Azure Active Directory-integráció Adobe bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="71e1d-103">Tutorial: Azure Active Directory integration with Adobe Sign</span></span>

<span data-ttu-id="71e1d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Adobe jelentkezzen az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="71e1d-104">In this tutorial, you learn how toointegrate Adobe Sign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="71e1d-105">Az Adobe bejelentkezési integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="71e1d-105">Integrating Adobe Sign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="71e1d-106">Megadhatja a hozzáférés tooAdobe bejelentkezési rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="71e1d-106">You can control in Azure AD who has access tooAdobe Sign</span></span>
- <span data-ttu-id="71e1d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAdobe (egyszeri bejelentkezés) bejelentkezési és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="71e1d-107">You can enable your users tooautomatically get signed-on tooAdobe Sign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="71e1d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="71e1d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="71e1d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="71e1d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71e1d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="71e1d-110">Prerequisites</span></span>

<span data-ttu-id="71e1d-111">az Azure AD-integrációs Adobe bejelentkezési tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="71e1d-111">tooconfigure Azure AD integration with Adobe Sign, you need hello following items:</span></span>

- <span data-ttu-id="71e1d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="71e1d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="71e1d-113">Az Adobe bejelentkezési egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="71e1d-113">An Adobe Sign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="71e1d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="71e1d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="71e1d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="71e1d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="71e1d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="71e1d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="71e1d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="71e1d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="71e1d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="71e1d-118">Scenario description</span></span>
<span data-ttu-id="71e1d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="71e1d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="71e1d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="71e1d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="71e1d-121">Hello gyűjteményből Adobe bejelentkezési hozzáadása</span><span class="sxs-lookup"><span data-stu-id="71e1d-121">Adding Adobe Sign from hello gallery</span></span>
2. <span data-ttu-id="71e1d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="71e1d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-sign-from-hello-gallery"></a><span data-ttu-id="71e1d-123">Hello gyűjteményből Adobe bejelentkezési hozzáadása</span><span class="sxs-lookup"><span data-stu-id="71e1d-123">Adding Adobe Sign from hello gallery</span></span>
<span data-ttu-id="71e1d-124">tooconfigure hello integrációja Adobe bejelentkezés az Azure AD-be, meg kell tooadd Adobe bejelentkezési hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="71e1d-124">tooconfigure hello integration of Adobe Sign into Azure AD, you need tooadd Adobe Sign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="71e1d-125">**tooadd Adobe bejelentkezési hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="71e1d-125">**tooadd Adobe Sign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="71e1d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="71e1d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="71e1d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="71e1d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="71e1d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="71e1d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="71e1d-133">Hello keresési mezőbe, írja be a **Adobe bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-133">In hello search box, type **Adobe Sign**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. <span data-ttu-id="71e1d-135">A hello eredmények panelen válassza ki a **Adobe bejelentkezési**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="71e1d-135">In hello results panel, select **Adobe Sign**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="71e1d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="71e1d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="71e1d-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Adobe bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="71e1d-138">In this section, you configure and test Azure AD single sign-on with Adobe Sign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="71e1d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Adobe bejelentkezési tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="71e1d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adobe Sign is tooa user in Azure AD.</span></span> <span data-ttu-id="71e1d-140">Ez azt jelenti hello kapcsolódó felhasználói az Adobe bejelentkezési és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="71e1d-140">In other words, a link relationship between an Azure AD user and hello related user in Adobe Sign needs toobe established.</span></span>

<span data-ttu-id="71e1d-141">Az Adobe bejelentkezési rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="71e1d-141">In Adobe Sign, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="71e1d-142">tooconfigure és Adobe bejelentkezési az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="71e1d-142">tooconfigure and test Azure AD single sign-on with Adobe Sign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="71e1d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="71e1d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="71e1d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="71e1d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="71e1d-145">**[Az Adobe bejelentkezési tesztfelhasználó létrehozása](#creating-an-adobe-sign-test-user)**  -toohave egy megfelelője a Britta Simon az Adobe bejelentkezési, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="71e1d-145">**[Creating an Adobe Sign test user](#creating-an-adobe-sign-test-user)** - toohave a counterpart of Britta Simon in Adobe Sign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="71e1d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="71e1d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="71e1d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="71e1d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="71e1d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="71e1d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="71e1d-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Adobe bejelentkezési alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="71e1d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Adobe Sign application.</span></span>

<span data-ttu-id="71e1d-150">**az Azure AD tooconfigure egyszeri bejelentkezés Adobe bejelentkezési, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="71e1d-150">**tooconfigure Azure AD single sign-on with Adobe Sign, perform hello following steps:**</span></span>

1. <span data-ttu-id="71e1d-151">Az Azure portál, a hello hello **Adobe bejelentkezési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-151">In hello Azure portal, on hello **Adobe Sign** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="71e1d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="71e1d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. <span data-ttu-id="71e1d-155">A hello **Adobe bejelentkezési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="71e1d-155">On hello **Adobe Sign Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    <span data-ttu-id="71e1d-157">a.</span><span class="sxs-lookup"><span data-stu-id="71e1d-157">a.</span></span> <span data-ttu-id="71e1d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.echosign.com/`</span><span class="sxs-lookup"><span data-stu-id="71e1d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.echosign.com/`</span></span>

    <span data-ttu-id="71e1d-159">b.</span><span class="sxs-lookup"><span data-stu-id="71e1d-159">b.</span></span> <span data-ttu-id="71e1d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.echosign.com`</span><span class="sxs-lookup"><span data-stu-id="71e1d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.echosign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="71e1d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="71e1d-161">These values are not real.</span></span> <span data-ttu-id="71e1d-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="71e1d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="71e1d-163">Ügyfél [Adobe bejelentkezési ügyfél-támogatási csoport](https://helpx.adobe.com/in/contact/support.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="71e1d-163">Contact [Adobe Sign Client support team](https://helpx.adobe.com/in/contact/support.html) tooget these values.</span></span> 
 
4. <span data-ttu-id="71e1d-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="71e1d-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. <span data-ttu-id="71e1d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="71e1d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="71e1d-168">A hello **Adobe bejelentkezési konfigurációs** kattintson **konfigurálása Adobe bejelentkezési** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="71e1d-168">On hello **Adobe Sign Configuration** section, click **Configure Adobe Sign** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="71e1d-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="71e1d-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. <span data-ttu-id="71e1d-171">Egy másik webes böngészőablakban jelentkezzen tooyour Adobe bejelentkezési vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="71e1d-171">In a different web browser window, log in tooyour Adobe Sign company site as an administrator.</span></span>

8. <span data-ttu-id="71e1d-172">Hello hello felső menüben kattintson a **fiók**, és a bal oldalon hello hello navigációs ablakában kattintson az **SAML beállítások** alatt **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-172">In hello menu on hello top, click **Account**, and then, in hello navigation pane on hello left side, click **SAML Settings** under **Account Settings**.</span></span>
   
   <span data-ttu-id="71e1d-173">![Fiók](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "fiók")</span><span class="sxs-lookup"><span data-stu-id="71e1d-173">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")</span></span>

9. <span data-ttu-id="71e1d-174">Hello SAML beállítások szakaszban, a hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="71e1d-174">In hello SAML Settings section, perform hello following steps:</span></span>
   
   <span data-ttu-id="71e1d-175">![SAML-alapú beállítások](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="71e1d-175">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML Settings")</span></span>
   
   <span data-ttu-id="71e1d-176">a.</span><span class="sxs-lookup"><span data-stu-id="71e1d-176">a.</span></span> <span data-ttu-id="71e1d-177">Mint **SAML mód**, jelölje be **SAML kötelező**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-177">As **SAML Mode**, select **SAML Mandatory**.</span></span>
   
   <span data-ttu-id="71e1d-178">b.</span><span class="sxs-lookup"><span data-stu-id="71e1d-178">b.</span></span> <span data-ttu-id="71e1d-179">Válassza ki **engedélyezése EchoSign Fiókrendszergazdák toolog a EchoSign hitelesítő adataival**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-179">Select **Allow EchoSign Account Administrators toolog in using their EchoSign Credentials**.</span></span>
   
   <span data-ttu-id="71e1d-180">c.</span><span class="sxs-lookup"><span data-stu-id="71e1d-180">c.</span></span> <span data-ttu-id="71e1d-181">Mint **felhasználó létrehozása**, jelölje be **keresztül SAML hitelesített felhasználók automatikus hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-181">As **User Creation**, select **Automatically add users authenticated through SAML**.</span></span>

10. <span data-ttu-id="71e1d-182">Helyezze át, amely, a lépések végrehajtása a következő hello:</span><span class="sxs-lookup"><span data-stu-id="71e1d-182">Move on, performing hello following steps:</span></span>

       <span data-ttu-id="71e1d-183">![SAML-alapú beállítások](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="71e1d-183">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML Settings")</span></span>

    <span data-ttu-id="71e1d-184">a.</span><span class="sxs-lookup"><span data-stu-id="71e1d-184">a.</span></span> <span data-ttu-id="71e1d-185">Beillesztés **SAML Entitásazonosító**, amely Azure-portálon való hello másolta **IdP Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="71e1d-185">Paste **SAML Entity ID**, which you have copied from Azure portal into hello **IdP Entity ID** textbox.</span></span>
    
    <span data-ttu-id="71e1d-186">b.</span><span class="sxs-lookup"><span data-stu-id="71e1d-186">b.</span></span> <span data-ttu-id="71e1d-187">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon való hello másolta **IdP bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="71e1d-187">Paste **SAML Single Sign-On Service URL**, which you have copied from Azure portal into hello **IdP Login URL** textbox.</span></span>
   
    <span data-ttu-id="71e1d-188">c.</span><span class="sxs-lookup"><span data-stu-id="71e1d-188">c.</span></span> <span data-ttu-id="71e1d-189">Beillesztés **Sign-Out URL-cím**, amely Azure-portálon való hello másolta **IdP kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="71e1d-189">Paste **Sign-Out URL**, which you have copied from Azure portal into hello **IdP Logout URL** textbox.</span></span>

    <span data-ttu-id="71e1d-190">d.</span><span class="sxs-lookup"><span data-stu-id="71e1d-190">d.</span></span> <span data-ttu-id="71e1d-191">Nyissa meg a letöltött **Certificate(Base64)** fájlt a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **IdP tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="71e1d-191">Open your downloaded **Certificate(Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Certificate** textbox</span></span>

    <span data-ttu-id="71e1d-192">e.</span><span class="sxs-lookup"><span data-stu-id="71e1d-192">e.</span></span> <span data-ttu-id="71e1d-193">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-193">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="71e1d-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="71e1d-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="71e1d-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="71e1d-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="71e1d-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="71e1d-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="71e1d-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="71e1d-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="71e1d-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="71e1d-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="71e1d-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="71e1d-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="71e1d-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="71e1d-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="71e1d-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="71e1d-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="71e1d-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="71e1d-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="71e1d-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="71e1d-209">a.</span><span class="sxs-lookup"><span data-stu-id="71e1d-209">a.</span></span> <span data-ttu-id="71e1d-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="71e1d-211">b.</span><span class="sxs-lookup"><span data-stu-id="71e1d-211">b.</span></span> <span data-ttu-id="71e1d-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="71e1d-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="71e1d-213">c.</span><span class="sxs-lookup"><span data-stu-id="71e1d-213">c.</span></span> <span data-ttu-id="71e1d-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="71e1d-215">d.</span><span class="sxs-lookup"><span data-stu-id="71e1d-215">d.</span></span> <span data-ttu-id="71e1d-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="71e1d-216">Click **Create**.</span></span>
 
### <a name="creating-an-adobe-sign-test-user"></a><span data-ttu-id="71e1d-217">Az Adobe bejelentkezési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="71e1d-217">Creating an Adobe Sign test user</span></span>

<span data-ttu-id="71e1d-218">az Azure AD tooenable felhasználók toolog a bejelentkezési tooAdobe, akkor ki kell építenie Adobe jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="71e1d-218">tooenable Azure AD users toolog in tooAdobe Sign, they must be provisioned into Adobe Sign.</span></span> <span data-ttu-id="71e1d-219">Az Adobe bejelentkezési hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="71e1d-219">In hello case of Adobe Sign, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="71e1d-220">Bármely más Adobe bejelentkezési felhasználói fiók létrehozása eszközök vagy Adobe bejelentkezési tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="71e1d-220">You can use any other Adobe Sign user account creation tools or APIs provided by Adobe Sign tooprovision AAD user accounts.</span></span> 

<span data-ttu-id="71e1d-221">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="71e1d-221">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="71e1d-222">Jelentkezzen be tooyour **Adobe bejelentkezési** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="71e1d-222">Log in tooyour **Adobe Sign** company site as administrator.</span></span>

2. <span data-ttu-id="71e1d-223">Hello hello felső menüben kattintson a **fiók**, és a bal oldalon hello hello navigációs ablakában kattintson az **felhasználók és csoportok**, és kattintson a **hozzon létre egy új felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-223">In hello menu on hello top, click **Account**, and then, in hello navigation pane on hello left side, click **Users & Groups**, and then, click **Create a new user**.</span></span>
   
   <span data-ttu-id="71e1d-224">![Fiók](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "fiók")</span><span class="sxs-lookup"><span data-stu-id="71e1d-224">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")</span></span>
   
3. <span data-ttu-id="71e1d-225">A hello **új felhasználó létrehozása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="71e1d-225">In hello **Create New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="71e1d-226">![Hozzon létre felhasználói](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="71e1d-226">![Create User](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "Create User")</span></span>
   
   <span data-ttu-id="71e1d-227">a.</span><span class="sxs-lookup"><span data-stu-id="71e1d-227">a.</span></span> <span data-ttu-id="71e1d-228">Típus hello **E-mail cím**, **Utónév**, és **Vezetéknév** a egy érvényes AAD-fiókba, a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="71e1d-228">Type hello **Email Address**, **First Name**, and **Last Name** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
   <span data-ttu-id="71e1d-229">b.</span><span class="sxs-lookup"><span data-stu-id="71e1d-229">b.</span></span> <span data-ttu-id="71e1d-230">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-230">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="71e1d-231">hello Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="71e1d-231">hello Azure Active Directory account holder receives an email that includes a link tooconfirm hello account before it becomes active.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="71e1d-232">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="71e1d-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="71e1d-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAdobe bejelentkezési megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="71e1d-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAdobe Sign.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="71e1d-235">**tooassign Britta Simon tooAdobe bejelentkezési, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="71e1d-235">**tooassign Britta Simon tooAdobe Sign, perform hello following steps:**</span></span>

1. <span data-ttu-id="71e1d-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="71e1d-238">Hello alkalmazások listában válassza ki a **Adobe bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-238">In hello applications list, select **Adobe Sign**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. <span data-ttu-id="71e1d-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="71e1d-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="71e1d-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="71e1d-242">Click **Add** button.</span></span> <span data-ttu-id="71e1d-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="71e1d-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="71e1d-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="71e1d-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="71e1d-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="71e1d-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="71e1d-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="71e1d-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="71e1d-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="71e1d-248">Testing single sign-on</span></span>

<span data-ttu-id="71e1d-249">Hello Adobe bejelentkezési hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Adobe bejelentkezési alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="71e1d-249">When you click hello Adobe Sign tile in hello Access Panel, you should get automatically signed-on tooyour Adobe Sign application.</span></span>
<span data-ttu-id="71e1d-250">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="71e1d-250">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71e1d-251">További források</span><span class="sxs-lookup"><span data-stu-id="71e1d-251">Additional resources</span></span>

* [<span data-ttu-id="71e1d-252">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="71e1d-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="71e1d-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="71e1d-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

