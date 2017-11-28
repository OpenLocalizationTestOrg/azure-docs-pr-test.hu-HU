---
title: "Oktatóanyag: Azure Active Directoryval integrált InsideView |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és InsideView között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 979c0c24f3a18a193616061b8c2e78292233a56d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a><span data-ttu-id="1c74a-103">Oktatóanyag: Azure Active Directoryval integrált InsideView</span><span class="sxs-lookup"><span data-stu-id="1c74a-103">Tutorial: Azure Active Directory integration with InsideView</span></span>

<span data-ttu-id="1c74a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate InsideView az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1c74a-104">In this tutorial, you learn how toointegrate InsideView with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1c74a-105">InsideView integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="1c74a-105">Integrating InsideView with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1c74a-106">Megadhatja a hozzáférés tooInsideView rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1c74a-106">You can control in Azure AD who has access tooInsideView</span></span>
- <span data-ttu-id="1c74a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooInsideView (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1c74a-107">You can enable your users tooautomatically get signed-on tooInsideView (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1c74a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1c74a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1c74a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1c74a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c74a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1c74a-110">Prerequisites</span></span>

<span data-ttu-id="1c74a-111">az Azure AD integrálása InsideView tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1c74a-111">tooconfigure Azure AD integration with InsideView, you need hello following items:</span></span>

- <span data-ttu-id="1c74a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1c74a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1c74a-113">Egy InsideView egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1c74a-113">A InsideView single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1c74a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1c74a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1c74a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="1c74a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1c74a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1c74a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1c74a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1c74a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1c74a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1c74a-118">Scenario description</span></span>
<span data-ttu-id="1c74a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1c74a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1c74a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1c74a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1c74a-121">Hello gyűjteményből InsideView hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1c74a-121">Adding InsideView from hello gallery</span></span>
2. <span data-ttu-id="1c74a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1c74a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insideview-from-hello-gallery"></a><span data-ttu-id="1c74a-123">Hello gyűjteményből InsideView hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1c74a-123">Adding InsideView from hello gallery</span></span>
<span data-ttu-id="1c74a-124">tooconfigure hello integrációja InsideView tooAzure AD a, meg kell tooadd InsideView hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="1c74a-124">tooconfigure hello integration of InsideView in tooAzure AD, you need tooadd InsideView from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1c74a-125">**tooadd InsideView hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1c74a-125">**tooadd InsideView from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c74a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1c74a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1c74a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1c74a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="1c74a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1c74a-133">Hello keresési mezőbe, írja be a **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-133">In hello search box, type **InsideView**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. <span data-ttu-id="1c74a-135">A hello eredmények panelen válassza ki a **InsideView**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-135">In hello results panel, select **InsideView**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1c74a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1c74a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1c74a-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján InsideView</span><span class="sxs-lookup"><span data-stu-id="1c74a-138">In this section, you configure and test Azure AD single sign-on with InsideView based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1c74a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó InsideView tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1c74a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in InsideView is tooa user in Azure AD.</span></span> <span data-ttu-id="1c74a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello InsideView közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="1c74a-140">In other words, a link relationship between an Azure AD user and hello related user in InsideView needs toobe established.</span></span>

<span data-ttu-id="1c74a-141">InsideView, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1c74a-141">In InsideView, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1c74a-142">tooconfigure és az Azure AD az egyszeri bejelentkezés InsideView-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1c74a-142">tooconfigure and test Azure AD single sign-on with InsideView, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1c74a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1c74a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1c74a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c74a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1c74a-145">**[InsideView tesztfelhasználó létrehozása](#creating-a-insideview-test-user)**  -toohave egy megfelelője a Britta Simon a InsideView, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="1c74a-145">**[Creating a InsideView test user](#creating-a-insideview-test-user)** - toohave a counterpart of Britta Simon in InsideView that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1c74a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1c74a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1c74a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="1c74a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1c74a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c74a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1c74a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az InsideView alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1c74a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your InsideView application.</span></span>

<span data-ttu-id="1c74a-150">**az Azure AD tooconfigure egyszeri bejelentkezést a InsideView, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1c74a-150">**tooconfigure Azure AD single sign-on with InsideView, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c74a-151">Az Azure portál, a hello hello **InsideView** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-151">In hello Azure portal, on hello **InsideView** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1c74a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1c74a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. <span data-ttu-id="1c74a-155">A hello **InsideView tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1c74a-155">On hello **InsideView Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    <span data-ttu-id="1c74a-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://my.insideview.com/iv/<STS Name>/login.iv`</span><span class="sxs-lookup"><span data-stu-id="1c74a-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://my.insideview.com/iv/<STS Name>/login.iv`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1c74a-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="1c74a-158">This value is not real.</span></span> <span data-ttu-id="1c74a-159">Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="1c74a-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="1c74a-160">Ügyfél [InsideView támogatási csoport ](mailto:support@insideview.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="1c74a-160">Contact [InsideView support team ](mailto:support@insideview.com) tooget this value.</span></span>
 
4. <span data-ttu-id="1c74a-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1c74a-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. <span data-ttu-id="1c74a-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1c74a-165">A hello **InsideView konfigurációs** kattintson **konfigurálása InsideView** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1c74a-165">On hello **InsideView Configuration** section, click **Configure InsideView** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1c74a-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1c74a-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. <span data-ttu-id="1c74a-168">Egy másik webes böngészőablakban jelentkezzen tooyour InsideView vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1c74a-168">In a different web browser window, log in tooyour InsideView company site as an administrator.</span></span>

8. <span data-ttu-id="1c74a-169">Hello hello felső eszköztárán kattintson **Admin**, **SingleSignOn beállítások**, és kattintson a **hozzáadása SAML**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-169">In hello toolbar on hello top, click **Admin**, **SingleSignOn Settings**, and then click **Add SAML**.</span></span>
   
   <span data-ttu-id="1c74a-170">![SAML-alapú egyszeri bejelentkezési beállítások](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML-alapú egyszeri bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="1c74a-170">![SAML Single Sign On Settings](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML Single Sign On Settings")</span></span>

9. <span data-ttu-id="1c74a-171">A hello **hozzáadása egy új SAML** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1c74a-171">In hello **Add a New SAML** section, perform hello following steps:</span></span>

    <span data-ttu-id="1c74a-172">![Adja hozzá egy új SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "egy új SAML hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="1c74a-172">![Add a New SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "Add a New SAML")</span></span>
   
    <span data-ttu-id="1c74a-173">a.</span><span class="sxs-lookup"><span data-stu-id="1c74a-173">a.</span></span> <span data-ttu-id="1c74a-174">A hello **STS neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="1c74a-174">In hello **STS Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="1c74a-175">b.</span><span class="sxs-lookup"><span data-stu-id="1c74a-175">b.</span></span> <span data-ttu-id="1c74a-176">A **SamlP/WS-Fed kéretlen végpont** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1c74a-176">In **SamlP/WS-Fed Unsolicited EndPoint** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="1c74a-177">c.</span><span class="sxs-lookup"><span data-stu-id="1c74a-177">c.</span></span> <span data-ttu-id="1c74a-178">Nyissa meg a base-64 kódolású tanúsítványt, amely már letöltötte az Azure portál, másolása hello a vágólapra tartalma, és majd illessze be azt toohello **STS tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1c74a-178">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **STS Certificate** textbox.</span></span>

    <span data-ttu-id="1c74a-179">d.</span><span class="sxs-lookup"><span data-stu-id="1c74a-179">d.</span></span> <span data-ttu-id="1c74a-180">A hello **Crm felhasználói azonosító leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="1c74a-180">In hello **Crm User Id Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
        
    <span data-ttu-id="1c74a-181">e.</span><span class="sxs-lookup"><span data-stu-id="1c74a-181">e.</span></span> <span data-ttu-id="1c74a-182">A hello **Crm E-mail leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="1c74a-182">In hello **Crm Email Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="1c74a-183">f.</span><span class="sxs-lookup"><span data-stu-id="1c74a-183">f.</span></span> <span data-ttu-id="1c74a-184">A hello **Crm első neve Mapping** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="1c74a-184">In hello **Crm First Name Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="1c74a-185">g.</span><span class="sxs-lookup"><span data-stu-id="1c74a-185">g.</span></span> <span data-ttu-id="1c74a-186">A hello **Crm Vezetéknév leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="1c74a-186">In hello **Crm lastName Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>  

    <span data-ttu-id="1c74a-187">h.</span><span class="sxs-lookup"><span data-stu-id="1c74a-187">h.</span></span> <span data-ttu-id="1c74a-188">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1c74a-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="1c74a-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1c74a-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="1c74a-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1c74a-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1c74a-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1c74a-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c74a-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="1c74a-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="1c74a-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1c74a-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1c74a-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c74a-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1c74a-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1c74a-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1c74a-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1c74a-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1c74a-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1c74a-204">a.</span><span class="sxs-lookup"><span data-stu-id="1c74a-204">a.</span></span> <span data-ttu-id="1c74a-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1c74a-206">b.</span><span class="sxs-lookup"><span data-stu-id="1c74a-206">b.</span></span> <span data-ttu-id="1c74a-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1c74a-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1c74a-208">c.</span><span class="sxs-lookup"><span data-stu-id="1c74a-208">c.</span></span> <span data-ttu-id="1c74a-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1c74a-210">d.</span><span class="sxs-lookup"><span data-stu-id="1c74a-210">d.</span></span> <span data-ttu-id="1c74a-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-211">Click **Create**.</span></span>
 
### <a name="creating-a-insideview-test-user"></a><span data-ttu-id="1c74a-212">InsideView tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c74a-212">Creating a InsideView test user</span></span>

<span data-ttu-id="1c74a-213">az Azure AD tooenable felhasználók toolog tooInsideView, az ezeket ki kell építenie a tooInsideView.</span><span class="sxs-lookup"><span data-stu-id="1c74a-213">tooenable Azure AD users toolog in tooInsideView, they must be provisioned in tooInsideView.</span></span> <span data-ttu-id="1c74a-214">InsideView hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1c74a-214">In hello case of InsideView, provisioning is a manual task.</span></span>

<span data-ttu-id="1c74a-215">tooget felhasználók vagy a névjegyek létrehozása InsideView, lépjen kapcsolatba a [InsideView támogatási csoport](mailto:support@insideview.com).</span><span class="sxs-lookup"><span data-stu-id="1c74a-215">tooget users or contacts created in InsideView, Contact [InsideView support team](mailto:support@insideview.com).</span></span>

>[!NOTE]
><span data-ttu-id="1c74a-216">Bármely más InsideView felhasználói fiók létrehozása eszközök vagy InsideView tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="1c74a-216">You can use any other InsideView user account creation tools or APIs provided by InsideView tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1c74a-217">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1c74a-217">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1c74a-218">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooInsideView megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1c74a-218">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInsideView.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1c74a-220">**tooassign Britta Simon tooInsideView, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="1c74a-220">**tooassign Britta Simon tooInsideView, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c74a-221">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-221">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1c74a-223">Hello alkalmazások listában válassza ki a **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-223">In hello applications list, select **InsideView**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. <span data-ttu-id="1c74a-225">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1c74a-225">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1c74a-227">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1c74a-227">Click **Add** button.</span></span> <span data-ttu-id="1c74a-228">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1c74a-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1c74a-230">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1c74a-230">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1c74a-231">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1c74a-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1c74a-232">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1c74a-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1c74a-233">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1c74a-233">Testing single sign-on</span></span>

<span data-ttu-id="1c74a-234">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="1c74a-234">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1c74a-235">Ha a hozzáférési Panel hello hello InsideView csempe gombra kattint, automatikusan bejelentkezett tooyour InsideView alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="1c74a-235">When you click hello InsideView tile in hello Access Panel, you should get automatically signed-on tooyour InsideView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c74a-236">További források</span><span class="sxs-lookup"><span data-stu-id="1c74a-236">Additional resources</span></span>

* [<span data-ttu-id="1c74a-237">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1c74a-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1c74a-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1c74a-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

