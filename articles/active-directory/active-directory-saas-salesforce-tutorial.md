---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1d848518ee30910e051cdc4746c599219f3b5a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a><span data-ttu-id="97cdc-103">Oktatóanyag: Azure Active Directoryval integrált Salesforce</span><span class="sxs-lookup"><span data-stu-id="97cdc-103">Tutorial: Azure Active Directory integration with Salesforce</span></span>

<span data-ttu-id="97cdc-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Salesforce az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="97cdc-104">In this tutorial, you learn how toointegrate Salesforce with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="97cdc-105">Salesforce integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="97cdc-105">Integrating Salesforce with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="97cdc-106">Megadhatja a hozzáférés tooSalesforce rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="97cdc-106">You can control in Azure AD who has access tooSalesforce</span></span>
- <span data-ttu-id="97cdc-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSalesforce (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="97cdc-107">You can enable your users tooautomatically get signed-on tooSalesforce (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="97cdc-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="97cdc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="97cdc-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97cdc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97cdc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="97cdc-110">Prerequisites</span></span>

<span data-ttu-id="97cdc-111">az Azure AD-integráció a Salesforce tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="97cdc-111">tooconfigure Azure AD integration with Salesforce, you need hello following items:</span></span>

- <span data-ttu-id="97cdc-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="97cdc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="97cdc-113">A Salesforce egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="97cdc-113">A Salesforce single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="97cdc-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="97cdc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="97cdc-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="97cdc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97cdc-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="97cdc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="97cdc-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97cdc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97cdc-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="97cdc-118">Scenario description</span></span>
<span data-ttu-id="97cdc-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="97cdc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="97cdc-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="97cdc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97cdc-121">Salesforce hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="97cdc-121">Adding Salesforce from hello gallery</span></span>
2. <span data-ttu-id="97cdc-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="97cdc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-from-hello-gallery"></a><span data-ttu-id="97cdc-123">Salesforce hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="97cdc-123">Adding Salesforce from hello gallery</span></span>
<span data-ttu-id="97cdc-124">tooconfigure hello integrációja Salesforce az Azure AD-be, meg kell tooadd Salesforce hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="97cdc-124">tooconfigure hello integration of Salesforce into Azure AD, you need tooadd Salesforce from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="97cdc-125">**tooadd Salesforce hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="97cdc-125">**tooadd Salesforce from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="97cdc-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97cdc-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="97cdc-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="97cdc-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="97cdc-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="97cdc-133">Hello keresési mezőbe, írja be a **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-133">In hello search box, type **Salesforce**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. <span data-ttu-id="97cdc-135">A hello eredmények panelen válassza ki a **Salesforce**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-135">In hello results panel, select **Salesforce**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="97cdc-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="97cdc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="97cdc-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Salesforce "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="97cdc-138">In this section, you configure and test Azure AD single sign-on with Salesforce based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="97cdc-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Salesforce-ban az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="97cdc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Salesforce is tooa user in Azure AD.</span></span> <span data-ttu-id="97cdc-140">Ez azt jelenti hello kapcsolódó felhasználó a Salesforce-ban és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="97cdc-140">In other words, a link relationship between an Azure AD user and hello related user in Salesforce needs toobe established.</span></span>

<span data-ttu-id="97cdc-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="97cdc-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Salesforce.</span></span>

<span data-ttu-id="97cdc-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Salesforce-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="97cdc-142">tooconfigure and test Azure AD single sign-on with Salesforce, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="97cdc-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="97cdc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="97cdc-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97cdc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97cdc-145">**[A Salesforce tesztfelhasználó létrehozása](#creating-a-salesforce-test-user)**  -toohave egy megfelelője a Britta Simon a Salesforce, amely a felhasználó ábrázolása csatolt toohello az Azure AD-ban.</span><span class="sxs-lookup"><span data-stu-id="97cdc-145">**[Creating a Salesforce test user](#creating-a-salesforce-test-user)** - toohave a counterpart of Britta Simon in Salesforce that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="97cdc-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="97cdc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97cdc-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="97cdc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="97cdc-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="97cdc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="97cdc-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Salesforce alkalmazást az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="97cdc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Salesforce application.</span></span>

<span data-ttu-id="97cdc-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Salesforce, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="97cdc-150">**tooconfigure Azure AD single sign-on with Salesforce, perform hello following steps:**</span></span>

1. <span data-ttu-id="97cdc-151">Az Azure portál, a hello hello **Salesforce** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-151">In hello Azure portal, on hello **Salesforce** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="97cdc-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="97cdc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. <span data-ttu-id="97cdc-155">A hello **Salesforce-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="97cdc-155">On hello **Salesforce Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    <span data-ttu-id="97cdc-157">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:</span><span class="sxs-lookup"><span data-stu-id="97cdc-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern:</span></span> 
   * <span data-ttu-id="97cdc-158">Vállalati fiók:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="97cdc-158">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
   * <span data-ttu-id="97cdc-159">Fejlesztői fiók:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="97cdc-159">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="97cdc-160">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="97cdc-160">These values are not hello real.</span></span> <span data-ttu-id="97cdc-161">Frissítheti ezeket az értékeket hello tényleges bejelentkezési URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="97cdc-161">Update these values with hello actual Sign-on URL.</span></span> <span data-ttu-id="97cdc-162">Ügyfél [Salesforce ügyfél-támogatási csoport](https://help.salesforce.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="97cdc-162">Contact [Salesforce Client support team](https://help.salesforce.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="97cdc-163">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="97cdc-163">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. <span data-ttu-id="97cdc-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="97cdc-167">A hello **Salesforce konfigurációs** kattintson **konfigurálása Salesforce** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="97cdc-167">On hello **Salesforce Configuration** section, click **Configure Salesforce** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="97cdc-168">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="97cdc-168">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

    <span data-ttu-id="97cdc-169">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="97cdc-169">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span></span>
7.  <span data-ttu-id="97cdc-170">Új lap megnyitása a böngészőben, és jelentkezzen be a Salesforce-rendszergazdai fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="97cdc-170">Open a new tab in your browser and log in tooyour Salesforce administrator account.</span></span>

8.  <span data-ttu-id="97cdc-171">A hello **rendszergazda** navigációs ablaktábláján kattintson **biztonsági vezérlők** tooexpand hello kapcsolatos szakasz.</span><span class="sxs-lookup"><span data-stu-id="97cdc-171">Under hello **Administrator** navigation pane, click **Security Controls** tooexpand hello related section.</span></span> <span data-ttu-id="97cdc-172">Kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-172">Then click **Single Sign-On Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  <span data-ttu-id="97cdc-174">A hello **egyszeri bejelentkezési beállítások** hello kattintson **szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-174">On hello **Single Sign-On Settings** page, click hello **Edit** button.</span></span>
    <span data-ttu-id="97cdc-175">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span><span class="sxs-lookup"><span data-stu-id="97cdc-175">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span></span>

      > [!NOTE]
      > <span data-ttu-id="97cdc-176">Ha nem tooenable egyszeri bejelentkezés beállításait a Salesforce-fiókhoz, szükség lehet a toocontact [Salesforce ügyfél-támogatási csoport](https://help.salesforce.com/support).</span><span class="sxs-lookup"><span data-stu-id="97cdc-176">If you are unable tooenable Single Sign-On settings for your Salesforce account, you may need toocontact [Salesforce Client support team](https://help.salesforce.com/support).</span></span> 

10. <span data-ttu-id="97cdc-177">Válassza ki **SAML engedélyezett**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-177">Select **SAML Enabled**, and then click **Save**.</span></span>

      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. <span data-ttu-id="97cdc-179">tooconfigure a SAML-alapú egyszeri bejelentkezés beállításait, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-179">tooconfigure your SAML single sign-on settings, click **New**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. <span data-ttu-id="97cdc-181">A hello **SAML-alapú egyszeri bejelentkezési beállítás szerkesztése** lapján ellenőrizze a következő konfigurációk hello:</span><span class="sxs-lookup"><span data-stu-id="97cdc-181">On hello **SAML Single Sign-On Setting Edit** page, make hello following configurations:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    <span data-ttu-id="97cdc-183">a.</span><span class="sxs-lookup"><span data-stu-id="97cdc-183">a.</span></span> <span data-ttu-id="97cdc-184">A hello **neve** mezőben adjon meg egy felhasználóbarát nevet ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="97cdc-184">For hello **Name** field, type in a friendly name for this configuration.</span></span> <span data-ttu-id="97cdc-185">Értéket biztosító **neve** automatikusan feltölti a hello **API-név** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="97cdc-185">Providing a value for **Name** automatically populate hello **API Name** textbox.</span></span>

    <span data-ttu-id="97cdc-186">b.</span><span class="sxs-lookup"><span data-stu-id="97cdc-186">b.</span></span> <span data-ttu-id="97cdc-187">Beillesztés **SMAL Entitásazonosító** hello érték **kibocsátó** mezőjét a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="97cdc-187">Paste **SMAL Entity ID** value into hello **Issuer** field in Salesforce.</span></span>

    <span data-ttu-id="97cdc-188">c.</span><span class="sxs-lookup"><span data-stu-id="97cdc-188">c.</span></span> <span data-ttu-id="97cdc-189">A hello **entitásazonosító szövegmező**, adja meg a Salesforce tartomány nevét a következő mintát hello használata:</span><span class="sxs-lookup"><span data-stu-id="97cdc-189">In hello **Entity Id textbox**, type your Salesforce domain name using hello following pattern:</span></span>
      
      * <span data-ttu-id="97cdc-190">Vállalati fiók:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="97cdc-190">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
      * <span data-ttu-id="97cdc-191">Fejlesztői fiók:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="97cdc-191">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>
      
    <span data-ttu-id="97cdc-192">d.</span><span class="sxs-lookup"><span data-stu-id="97cdc-192">d.</span></span> <span data-ttu-id="97cdc-193">Kattintson a **Tallózás** vagy **Choose File** tooopen hello **Choose File tooUpload** párbeszédpanelen válassza ki a Salesforce-tanúsítványt, és kattintson **nyissa meg a**tooupload hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="97cdc-193">Click **Browse** or **Choose File** tooopen hello **Choose File tooUpload** dialog, select your Salesforce certificate, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="97cdc-194">e.</span><span class="sxs-lookup"><span data-stu-id="97cdc-194">e.</span></span> <span data-ttu-id="97cdc-195">A **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz salesforce.com felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-195">For **SAML Identity Type**, select **Assertion contains User's salesforce.com username**.</span></span>

    <span data-ttu-id="97cdc-196">f.</span><span class="sxs-lookup"><span data-stu-id="97cdc-196">f.</span></span> <span data-ttu-id="97cdc-197">A **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**</span><span class="sxs-lookup"><span data-stu-id="97cdc-197">For **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**</span></span>

    <span data-ttu-id="97cdc-198">g.</span><span class="sxs-lookup"><span data-stu-id="97cdc-198">g.</span></span> <span data-ttu-id="97cdc-199">Beillesztés **egyszeri bejelentkezési URL-címe** történő hello **Identity Provider bejelentkezési URL-cím** mezőjét a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="97cdc-199">Paste **Single Sign-On Service URL** into hello **Identity Provider Login URL** field in Salesforce.</span></span>
    
    <span data-ttu-id="97cdc-200">h.</span><span class="sxs-lookup"><span data-stu-id="97cdc-200">h.</span></span> <span data-ttu-id="97cdc-201">A **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP-átirányítás**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-201">For **Service Provider Initiated Request Binding**, select **HTTP Redirect**.</span></span>
    
    <span data-ttu-id="97cdc-202">i.</span><span class="sxs-lookup"><span data-stu-id="97cdc-202">i.</span></span> <span data-ttu-id="97cdc-203">Végezetül kattintson **mentése** tooapply a SAML-alapú egyszeri bejelentkezés beállításait.</span><span class="sxs-lookup"><span data-stu-id="97cdc-203">Finally, click **Save** tooapply your SAML single sign-on settings.</span></span>

13. <span data-ttu-id="97cdc-204">A Salesforce hello bal oldali navigációs panelén kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-204">On hello left navigation pane in Salesforce, click **Domain Management** tooexpand hello related section, and then click **My Domain**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. <span data-ttu-id="97cdc-206">Görgessen lefelé toohello **hitelesítési** szakaszt, és kattintson a hello **szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-206">Scroll down toohello **Authentication Configuration** section, and click hello **Edit** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. <span data-ttu-id="97cdc-208">A hello **hitelesítési szolgáltatás** szakaszt, válassza ki a SAML SSO konfigurációs hello rövid nevét, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-208">In hello **Authentication Service** section, select hello friendly name of your SAML SSO configuration, and then click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > <span data-ttu-id="97cdc-210">Ha egynél több hitelesítési szolgáltatás be van jelölve, a felhasználók mely hitelesítési szolgáltatás például felszólító tooselect toosign be az egyszeri bejelentkezés tooyour Salesforce környezet elindítása közben.</span><span class="sxs-lookup"><span data-stu-id="97cdc-210">If more than one authentication service is selected, users are prompted tooselect which authentication service they like toosign in with while initiating single sign-on tooyour Salesforce environment.</span></span> <span data-ttu-id="97cdc-211">Ha nem szeretné, hogy azt toohappen, akkor meg kell **hagyja bejelölve minden hitelesítési szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-211">If you don’t want it toohappen, then you should **leave all other authentication services unchecked**.</span></span>
<CE>    
> [!TIP]
> <span data-ttu-id="97cdc-212">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="97cdc-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="97cdc-213">Ez az alkalmazás hozzáadása a hello után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="97cdc-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="97cdc-214">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="97cdc-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="97cdc-215">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="97cdc-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="97cdc-216">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="97cdc-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="97cdc-218">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="97cdc-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="97cdc-219">A bal oldali navigációs ablaktáblája hello hello **Azure-portálon**, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-219">On hello left navigation pane in hello **Azure portal**, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97cdc-221">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-221">toodisplay hello list of users, Go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97cdc-223">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97cdc-223">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97cdc-225">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="97cdc-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="97cdc-227">a.</span><span class="sxs-lookup"><span data-stu-id="97cdc-227">a.</span></span> <span data-ttu-id="97cdc-228">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="97cdc-229">b.</span><span class="sxs-lookup"><span data-stu-id="97cdc-229">b.</span></span> <span data-ttu-id="97cdc-230">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97cdc-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97cdc-231">c.</span><span class="sxs-lookup"><span data-stu-id="97cdc-231">c.</span></span> <span data-ttu-id="97cdc-232">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="97cdc-233">d.</span><span class="sxs-lookup"><span data-stu-id="97cdc-233">d.</span></span> <span data-ttu-id="97cdc-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-234">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-test-user"></a><span data-ttu-id="97cdc-235">A Salesforce tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="97cdc-235">Creating a Salesforce test user</span></span>

<span data-ttu-id="97cdc-236">Ebben a szakaszban egy Britta Simon nevű felhasználó létrehozása a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="97cdc-236">In this section, a user called Britta Simon is created in Salesforce.</span></span> <span data-ttu-id="97cdc-237">Salesforce támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="97cdc-237">Salesforce supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="97cdc-238">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="97cdc-238">There is no action item for you in this section.</span></span> <span data-ttu-id="97cdc-239">Ha a felhasználó nem létezik a Salesforce-ban, egy új tooaccess Salesforce tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="97cdc-239">If a user doesn't already exist in Salesforce, a new one is created when you attempt tooaccess Salesforce.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="97cdc-240">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="97cdc-240">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="97cdc-241">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSalesforce megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="97cdc-241">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSalesforce.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="97cdc-243">**tooassign Britta Simon tooSalesforce, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="97cdc-243">**tooassign Britta Simon tooSalesforce, perform hello following steps:**</span></span>

1. <span data-ttu-id="97cdc-244">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-244">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="97cdc-246">Hello alkalmazások listában válassza ki a **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-246">In hello applications list, select **Salesforce**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. <span data-ttu-id="97cdc-248">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-248">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="97cdc-250">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="97cdc-250">Click **Add** button.</span></span> <span data-ttu-id="97cdc-251">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97cdc-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="97cdc-253">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="97cdc-253">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="97cdc-254">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97cdc-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97cdc-255">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97cdc-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="97cdc-256">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="97cdc-256">Testing single sign-on</span></span>

<span data-ttu-id="97cdc-257">tootest a egyszeri bejelentkezés beállításokat, nyissa meg hello hozzáférési panelre a [https://myapps.microsoft.com](https://myapps.microsoft.com/), majd bejelentkezés a hello teszt fiókba, és kattintson **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="97cdc-257">tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), then sign into hello test account, and click **Salesforce**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97cdc-258">További források</span><span class="sxs-lookup"><span data-stu-id="97cdc-258">Additional resources</span></span>

* [<span data-ttu-id="97cdc-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="97cdc-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97cdc-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="97cdc-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="97cdc-261">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="97cdc-261">Configure User Provisioning</span></span>](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

