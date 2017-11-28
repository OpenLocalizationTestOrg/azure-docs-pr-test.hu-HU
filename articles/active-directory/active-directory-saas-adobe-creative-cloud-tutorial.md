---
title: "Oktatóanyag: Azure Active Directory-integráció a Adobe kreatív felhőalapú |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Adobe kreatív felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5e66255e9785465974a23cd3ef79c24e28c0250f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a><span data-ttu-id="91e81-103">Oktatóanyag: Azure Active Directoryval integrált Adobe kreatív felhő</span><span class="sxs-lookup"><span data-stu-id="91e81-103">Tutorial: Azure Active Directory integration with Adobe Creative Cloud</span></span>

<span data-ttu-id="91e81-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Adobe Creative felhőalapú Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="91e81-104">In this tutorial, you learn how toointegrate Adobe Creative Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="91e81-105">Az Adobe kreatív felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="91e81-105">Integrating Adobe Creative Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="91e81-106">Megadhatja a hozzáférés tooAdobe kreatív felhő rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="91e81-106">You can control in Azure AD who has access tooAdobe Creative Cloud</span></span>
- <span data-ttu-id="91e81-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAdobe kreatív felhő (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="91e81-107">You can enable your users tooautomatically get signed-on tooAdobe Creative Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="91e81-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="91e81-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="91e81-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="91e81-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91e81-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="91e81-110">Prerequisites</span></span>

<span data-ttu-id="91e81-111">tooconfigure Adobe kreatív felhőalapú Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="91e81-111">tooconfigure Azure AD integration with Adobe Creative Cloud, you need hello following items:</span></span>

- <span data-ttu-id="91e81-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="91e81-112">An Azure AD subscription</span></span>
- <span data-ttu-id="91e81-113">Egy Adobe kreatív felhő egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="91e81-113">A Adobe Creative Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="91e81-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="91e81-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="91e81-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="91e81-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="91e81-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="91e81-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="91e81-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="91e81-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="91e81-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="91e81-118">Scenario description</span></span>
<span data-ttu-id="91e81-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="91e81-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="91e81-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="91e81-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="91e81-121">Hello gyűjteményből Adobe kreatív felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91e81-121">Adding Adobe Creative Cloud from hello gallery</span></span>
2. <span data-ttu-id="91e81-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="91e81-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-creative-cloud-from-hello-gallery"></a><span data-ttu-id="91e81-123">Hello gyűjteményből Adobe kreatív felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91e81-123">Adding Adobe Creative Cloud from hello gallery</span></span>
<span data-ttu-id="91e81-124">tooconfigure hello integrációs Adobe kreatív felhőalapú, az Azure AD-be, meg kell tooadd Adobe kreatív felhő hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="91e81-124">tooconfigure hello integration of Adobe Creative Cloud into Azure AD, you need tooadd Adobe Creative Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="91e81-125">**tooadd Adobe kreatív felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="91e81-125">**tooadd Adobe Creative Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e81-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="91e81-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="91e81-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="91e81-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="91e81-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="91e81-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="91e81-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="91e81-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="91e81-133">Hello keresési mezőbe, írja be a **Adobe kreatív felhő**.</span><span class="sxs-lookup"><span data-stu-id="91e81-133">In hello search box, type **Adobe Creative Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. <span data-ttu-id="91e81-135">A hello eredmények panelen válassza a **Adobe kreatív felhő**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="91e81-135">In hello results panel, select **Adobe Creative Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="91e81-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="91e81-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="91e81-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Adobe kreatív felhő.</span><span class="sxs-lookup"><span data-stu-id="91e81-138">In this section, you configure and test Azure AD single sign-on with Adobe Creative Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="91e81-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Adobe kreatív felhőben tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="91e81-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adobe Creative Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="91e81-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Adobe kreatív felhőben közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="91e81-140">In other words, a link relationship between an Azure AD user and hello related user in Adobe Creative Cloud needs toobe established.</span></span>

<span data-ttu-id="91e81-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Adobe kreatív felhőben.</span><span class="sxs-lookup"><span data-stu-id="91e81-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Adobe Creative Cloud.</span></span>

<span data-ttu-id="91e81-142">tooconfigure és a Adobe kreatív felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="91e81-142">tooconfigure and test Azure AD single sign-on with Adobe Creative Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="91e81-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="91e81-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="91e81-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91e81-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="91e81-145">**[Az Adobe kreatív felhő tesztfelhasználó létrehozása](#creating-an-adobe-creative-cloud-test-user)**  -toohave Britta Simon Adobe kreatív felhőbe, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="91e81-145">**[Creating an Adobe Creative Cloud test user](#creating-an-adobe-creative-cloud-test-user)** - toohave a counterpart of Britta Simon in Adobe Creative Cloud that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="91e81-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="91e81-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="91e81-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="91e81-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="91e81-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="91e81-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="91e81-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Adobe kreatív felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="91e81-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Adobe Creative Cloud application.</span></span>

<span data-ttu-id="91e81-150">**a Adobe kreatív felhőalapú, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="91e81-150">**tooconfigure Azure AD single sign-on with Adobe Creative Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e81-151">Hello Azure felügyeleti portálon, a hello **Adobe kreatív felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="91e81-151">In hello Azure Management portal, on hello **Adobe Creative Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="91e81-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="91e81-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. <span data-ttu-id="91e81-155">A hello **Adobe kreatív felhőalapú tartományt és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="91e81-155">On hello **Adobe Creative Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    <span data-ttu-id="91e81-157">a.</span><span class="sxs-lookup"><span data-stu-id="91e81-157">a.</span></span> <span data-ttu-id="91e81-158">A hello **azonosító** szövegmezőhöz hello érték típusa:`https://www.okta.com/saml2/service-provider/<token>`</span><span class="sxs-lookup"><span data-stu-id="91e81-158">In hello **Identifier** textbox, type hello value as: `https://www.okta.com/saml2/service-provider/<token>`</span></span>

    <span data-ttu-id="91e81-159">b.</span><span class="sxs-lookup"><span data-stu-id="91e81-159">b.</span></span> <span data-ttu-id="91e81-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.okta.com/auth/saml20/accauthlinktest`</span><span class="sxs-lookup"><span data-stu-id="91e81-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.okta.com/auth/saml20/accauthlinktest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="91e81-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="91e81-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="91e81-162">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="91e81-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="91e81-163">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="91e81-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="91e81-164">Ha egy felhasználó toocreate manuálisan kell, kell toocontact hello Adobe kreatív felhő támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="91e81-164">If you need toocreate an user manually, you need toocontact hello Adobe Creative Cloud support team.</span></span>

4. <span data-ttu-id="91e81-165">A hello **Adobe kreatív felhőalapú tartományt és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="91e81-165">On hello **Adobe Creative Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    <span data-ttu-id="91e81-167">a.</span><span class="sxs-lookup"><span data-stu-id="91e81-167">a.</span></span> <span data-ttu-id="91e81-168">Kattintson a hello **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="91e81-168">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="91e81-169">b.</span><span class="sxs-lookup"><span data-stu-id="91e81-169">b.</span></span> <span data-ttu-id="91e81-170">A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://adobe.com`</span><span class="sxs-lookup"><span data-stu-id="91e81-170">In hello **Sign-on URL** textbox, type hello value as: `https://adobe.com`</span></span>

5. <span data-ttu-id="91e81-171">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="91e81-171">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. <span data-ttu-id="91e81-173">A hello **Adobe egy Felhőkonfigurációk kreatív** kattintson **Adobe kreatív felhő konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="91e81-173">On hello **Adobe Creative Cloud Configuration** section, click **Configure Adobe Creative Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="91e81-174">Másolja a hello **SAML entitásazonosító** és **SAML SSO URL-címe** rövid összefoglaló szakaszából.</span><span class="sxs-lookup"><span data-stu-id="91e81-174">Please copy hello **SAML Entity Id** and **SAML SSO Service URL** from Quick Reference section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. <span data-ttu-id="91e81-176">Egy másik webes böngészőablakban, bejelentkezés tooyour Adobe kreatív felhő Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="91e81-176">In a different web browser window, sign-on tooyour Adobe Creative Cloud tenant as an administrator.</span></span>

8.  <span data-ttu-id="91e81-177">Nyissa meg túl**identitás** a hello bal oldali navigációs panelen, majd kattintson a tartomány.</span><span class="sxs-lookup"><span data-stu-id="91e81-177">Go too**Identity** on hello left navigation pane and click your domain.</span></span> <span data-ttu-id="91e81-178">Hajtsa végre a következő lépéseket hello **egyszeri bejelentkezési a konfiguráció szükséges** szakasz.</span><span class="sxs-lookup"><span data-stu-id="91e81-178">Then perform hello following steps on **Single Sign On Configuration Required** section.</span></span>

    <span data-ttu-id="91e81-179">![Beállítások](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="91e81-179">![Settings](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Settings")</span></span>

9. <span data-ttu-id="91e81-180">Kattintson a **Tallózás** tooupload hello letöltött tanúsítvány az Azure AD túl**IDP tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="91e81-180">Click **Browse** tooupload hello downloaded certificate from Azure AD too**IDP Certificate**.</span></span>

10. <span data-ttu-id="91e81-181">A hello **IDP kibocsátó** szövegmező, írja be a hello értéket **SAML entitásazonosító** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="91e81-181">In hello **IDP issuer** textbox, put hello value of **SAML Entity Id** which you copied from **Configure sign-on** section in Azure portal.</span></span>

11. <span data-ttu-id="91e81-182">A hello **IDP bejelentkezési URL-cím** szövegmező, írja be a hello értéket **SAML SSO URL-címe** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="91e81-182">In hello **IDP Login URL** textbox, put hello value of **SAML SSO Service URL** which you copied from **Configure sign-on** section in Azure portal.</span></span>

12. <span data-ttu-id="91e81-183">Válassza ki **HTTP - átirányítási** , **IDP kötés**.</span><span class="sxs-lookup"><span data-stu-id="91e81-183">Select **HTTP - Redirect** as **IDP Binding**.</span></span>

13. <span data-ttu-id="91e81-184">Válassza ki **E-mail cím** , **felhasználói bejelentkezési beállítás**.</span><span class="sxs-lookup"><span data-stu-id="91e81-184">Select **Email Address** as **User Login Setting**.</span></span>
 
14. <span data-ttu-id="91e81-185">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="91e81-185">Click **Save** button.</span></span>

15. <span data-ttu-id="91e81-186">hello irányítópult mostantól jelent hello XML **"Metaadatok letöltése"** fájlt.</span><span class="sxs-lookup"><span data-stu-id="91e81-186">hello dashboard will now present hello XML **"Download Metadata"** file.</span></span> <span data-ttu-id="91e81-187">Adobe EntityDescriptor és URL-címe AssertionConsumerService tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="91e81-187">It contains Adobe’s EntityDescriptor URL and AssertionConsumerService URL.</span></span> <span data-ttu-id="91e81-188">Nyissa meg a hello fájlt, és konfigurálja őket hello Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91e81-188">Please open hello file and configure them in hello Azure AD application.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    <span data-ttu-id="91e81-191">a.</span><span class="sxs-lookup"><span data-stu-id="91e81-191">a.</span></span> <span data-ttu-id="91e81-192">Használjon hello EntityDescriptor érték Adobe meg a megadott **azonosító** a hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="91e81-192">Use hello EntityDescriptor value Adobe provided you for **Identifier** on hello **Configure App Settings** dialog.</span></span>

    <span data-ttu-id="91e81-193">b.</span><span class="sxs-lookup"><span data-stu-id="91e81-193">b.</span></span> <span data-ttu-id="91e81-194">Használjon hello AssertionConsumerService érték Adobe meg a megadott **válasz URL-CÍMEN** a hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="91e81-194">Use hello AssertionConsumerService value Adobe provided you for **Reply URL** on hello **Configure App Settings** dialog.</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="91e81-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="91e81-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="91e81-196">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="91e81-196">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="91e81-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="91e81-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e81-199">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="91e81-199">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="91e81-201">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="91e81-201">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="91e81-203">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="91e81-203">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="91e81-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="91e81-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="91e81-207">a.</span><span class="sxs-lookup"><span data-stu-id="91e81-207">a.</span></span> <span data-ttu-id="91e81-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="91e81-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="91e81-209">b.</span><span class="sxs-lookup"><span data-stu-id="91e81-209">b.</span></span> <span data-ttu-id="91e81-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="91e81-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="91e81-211">c.</span><span class="sxs-lookup"><span data-stu-id="91e81-211">c.</span></span> <span data-ttu-id="91e81-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="91e81-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="91e81-213">d.</span><span class="sxs-lookup"><span data-stu-id="91e81-213">d.</span></span> <span data-ttu-id="91e81-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="91e81-214">Click **Create**.</span></span> 

### <a name="creating-an-adobe-creative-cloud-test-user"></a><span data-ttu-id="91e81-215">Az Adobe kreatív felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="91e81-215">Creating an Adobe Creative Cloud test user</span></span>

<span data-ttu-id="91e81-216">A sorrend tooenable az Azure AD felhasználók toolog Adobe kreatív felhőbe azok ki kell építenie Adobe kreatív felhőbe.</span><span class="sxs-lookup"><span data-stu-id="91e81-216">In order tooenable Azure AD users toolog into Adobe Creative Cloud, they must be provisioned into Adobe Creative Cloud.</span></span>  
<span data-ttu-id="91e81-217">Az Adobe kreatív felhő hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="91e81-217">In hello case of Adobe Creative Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="91e81-218">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="91e81-218">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e81-219">Jelentkezzen be tooyour Adobe kreatív felhő vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="91e81-219">Log in tooyour Adobe Creative Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="91e81-220">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="91e81-220">Click **People**.</span></span>

    <span data-ttu-id="91e81-221">![Személyek](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="91e81-221">![People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="91e81-222">Kattintson a **felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="91e81-222">Click **Invite User**.</span></span>

    <span data-ttu-id="91e81-223">![Meghívott felhasználóknak](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "meghívott felhasználóknak")</span><span class="sxs-lookup"><span data-stu-id="91e81-223">![Invite Users](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="91e81-224">A hello **meghívása személyek** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="91e81-224">On hello **Invite People** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="91e81-225">![Felkérése](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="91e81-225">![Invite People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="91e81-226">a.</span><span class="sxs-lookup"><span data-stu-id="91e81-226">a.</span></span> <span data-ttu-id="91e81-227">A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="91e81-227">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>
    
    <span data-ttu-id="91e81-228">b.</span><span class="sxs-lookup"><span data-stu-id="91e81-228">b.</span></span> <span data-ttu-id="91e81-229">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="91e81-229">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="91e81-230">hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="91e81-230">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="91e81-231">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="91e81-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="91e81-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezést az access tooAdobe kreatív felhő megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="91e81-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooAdobe Creative Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="91e81-234">**tooassign Britta Simon tooAdobe kreatív felhő, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="91e81-234">**tooassign Britta Simon tooAdobe Creative Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e81-235">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="91e81-235">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="91e81-237">Hello alkalmazások listában válassza ki a **Adobe kreatív felhő**.</span><span class="sxs-lookup"><span data-stu-id="91e81-237">In hello applications list, select **Adobe Creative Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. <span data-ttu-id="91e81-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="91e81-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="91e81-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="91e81-241">Click **Add** button.</span></span> <span data-ttu-id="91e81-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="91e81-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="91e81-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="91e81-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="91e81-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="91e81-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="91e81-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="91e81-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="91e81-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="91e81-247">Testing single sign-on</span></span>

<span data-ttu-id="91e81-248">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="91e81-248">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="91e81-249">Hello Adobe kreatív felhő csempe a hozzáférési Panel hello gombra kapja meg automatikusan bejelentkezett tooyour Adobe kreatív felhőalapú alkalmazásnál.</span><span class="sxs-lookup"><span data-stu-id="91e81-249">When you click hello Adobe Creative Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Adobe Creative Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="91e81-250">További források</span><span class="sxs-lookup"><span data-stu-id="91e81-250">Additional resources</span></span>

* [<span data-ttu-id="91e81-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="91e81-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="91e81-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="91e81-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png