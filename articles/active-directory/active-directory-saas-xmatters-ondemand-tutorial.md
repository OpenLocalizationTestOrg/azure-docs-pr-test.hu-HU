---
title: "Oktatóanyag: Azure Active Directoryval integrált xMatters OnDemand |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és xMatters OnDemand között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7cc8f9f0d8cefc8a60b9514027437ced50c66242
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a><span data-ttu-id="eee8d-103">Oktatóanyag: Azure Active Directoryval integrált xMatters kötegmérete</span><span class="sxs-lookup"><span data-stu-id="eee8d-103">Tutorial: Azure Active Directory integration with xMatters OnDemand</span></span>

<span data-ttu-id="eee8d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate xMatters OnDemand az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eee8d-104">In this tutorial, you learn how toointegrate xMatters OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eee8d-105">XMatters OnDemand integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="eee8d-105">Integrating xMatters OnDemand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="eee8d-106">Megadhatja a hozzáférés tooxMatters OnDemand rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="eee8d-106">You can control in Azure AD who has access tooxMatters OnDemand</span></span>
- <span data-ttu-id="eee8d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooxMatters OnDemand (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="eee8d-107">You can enable your users tooautomatically get signed-on tooxMatters OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eee8d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="eee8d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="eee8d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eee8d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eee8d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eee8d-110">Prerequisites</span></span>

<span data-ttu-id="eee8d-111">tooconfigure xMatters OnDemand az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="eee8d-111">tooconfigure Azure AD integration with xMatters OnDemand, you need hello following items:</span></span>

- <span data-ttu-id="eee8d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="eee8d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eee8d-113">Egy OnDemand egyszeri bejelentkezés xMatters előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="eee8d-113">A xMatters OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eee8d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="eee8d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eee8d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="eee8d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eee8d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="eee8d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eee8d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eee8d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eee8d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="eee8d-118">Scenario description</span></span>
<span data-ttu-id="eee8d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="eee8d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eee8d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="eee8d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eee8d-121">Hello gyűjteményből xMatters OnDemand hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eee8d-121">Adding xMatters OnDemand from hello gallery</span></span>
2. <span data-ttu-id="eee8d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="eee8d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-xmatters-ondemand-from-hello-gallery"></a><span data-ttu-id="eee8d-123">Hello gyűjteményből xMatters OnDemand hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eee8d-123">Adding xMatters OnDemand from hello gallery</span></span>
<span data-ttu-id="eee8d-124">tooconfigure hello integrációja xMatters OnDemand az Azure AD-be, meg kell tooadd xMatters OnDemand hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="eee8d-124">tooconfigure hello integration of xMatters OnDemand into Azure AD, you need tooadd xMatters OnDemand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="eee8d-125">**tooadd xMatters OnDemand hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="eee8d-125">**tooadd xMatters OnDemand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="eee8d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eee8d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="eee8d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="eee8d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="eee8d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="eee8d-133">Hello keresési mezőbe, írja be a **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-133">In hello search box, type **xMatters OnDemand**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. <span data-ttu-id="eee8d-135">A hello eredmények panelen válassza ki a **xMatters OnDemand**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-135">In hello results panel, select **xMatters OnDemand**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eee8d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="eee8d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eee8d-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a xMatters OnDemand "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="eee8d-138">In this section, you configure and test Azure AD single sign-on with xMatters OnDemand based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="eee8d-139">Az egyszeri bejelentkezés toowork, az Azure AD kell tooknow milyen hello xMatters OnDemand megfelelőjére felhasználó tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="eee8d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in xMatters OnDemand is tooa user in Azure AD.</span></span> <span data-ttu-id="eee8d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello xMatters a hivatkozás kapcsolatának OnDemand kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="eee8d-140">In other words, a link relationship between an Azure AD user and hello related user in xMatters OnDemand needs toobe established.</span></span>

<span data-ttu-id="eee8d-141">XMatters OnDemand, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="eee8d-141">In xMatters OnDemand, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="eee8d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés xMatters OnDemand-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="eee8d-142">tooconfigure and test Azure AD single sign-on with xMatters OnDemand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="eee8d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="eee8d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="eee8d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eee8d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eee8d-145">**[XMatters OnDemand tesztfelhasználó létrehozása](#creating-a-xmatters-ondemand-test-user)**  -toohave Britta Simon egy partner, a xMatters OnDemand, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="eee8d-145">**[Creating a xMatters OnDemand test user](#creating-a-xmatters-ondemand-test-user)** - toohave a counterpart of Britta Simon in xMatters OnDemand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="eee8d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="eee8d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eee8d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="eee8d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eee8d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eee8d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eee8d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a xMatters OnDemand-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="eee8d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your xMatters OnDemand application.</span></span>

<span data-ttu-id="eee8d-150">**tooconfigure az Azure AD egyszeri bejelentkezést a xMatters OnDemand, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="eee8d-150">**tooconfigure Azure AD single sign-on with xMatters OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="eee8d-151">Az Azure portál, a hello hello **xMatters OnDemand** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-151">In hello Azure portal, on hello **xMatters OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="eee8d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="eee8d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. <span data-ttu-id="eee8d-155">A hello **xMatters OnDemand-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="eee8d-155">On hello **xMatters OnDemand Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    <span data-ttu-id="eee8d-157">a.</span><span class="sxs-lookup"><span data-stu-id="eee8d-157">a.</span></span> <span data-ttu-id="eee8d-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="eee8d-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    <span data-ttu-id="eee8d-159">b.</span><span class="sxs-lookup"><span data-stu-id="eee8d-159">b.</span></span> <span data-ttu-id="eee8d-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="eee8d-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > <span data-ttu-id="eee8d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="eee8d-161">These values are not real.</span></span> <span data-ttu-id="eee8d-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="eee8d-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="eee8d-163">Ügyfél [xMatters OnDemand támogatási csoport](https://www.xmatters.com/company/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="eee8d-163">Contact [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="eee8d-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** hello fájlt helyben, majd mentse **c:\\XMatters OnDemand.cer**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file locally as **c:\\XMatters OnDemand.cer**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="eee8d-166">Tooforward hello tanúsítvány toohello kell [xMatters OnDemand támogatási csoport](https://www.xmatters.com/company/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="eee8d-166">You need tooforward hello certificate toohello [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/).</span></span> <span data-ttu-id="eee8d-167">hello tanúsítványt kell toobe hello xMatters támogatási csoport által feltöltött hello egyszeri bejelentkezés konfigurációs is véglegesítése előtt.</span><span class="sxs-lookup"><span data-stu-id="eee8d-167">hello certificate needs toobe uploaded by hello xMatters support team before you can finalize hello single sign-on configuration.</span></span> 

5. <span data-ttu-id="eee8d-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eee8d-170">A hello **xMatters OnDemand konfigurációs** kattintson **xMatters OnDemand konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="eee8d-170">On hello **xMatters OnDemand Configuration** section, click **Configure xMatters OnDemand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="eee8d-171">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="eee8d-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. <span data-ttu-id="eee8d-173">Egy másik webes böngészőablakban jelentkezzen be tooyour XMatters OnDemand vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="eee8d-173">In a different web browser window, log in tooyour XMatters OnDemand company site as an administrator.</span></span>

8. <span data-ttu-id="eee8d-174">Hello hello felső eszköztárán kattintson **Admin**, és kattintson a **vállalat adatait** hello bal oldali navigációs sávján hello.</span><span class="sxs-lookup"><span data-stu-id="eee8d-174">In hello toolbar on hello top, click **Admin**, and then click **Company Details** in hello navigation bar on hello left side.</span></span>
   
    <span data-ttu-id="eee8d-175">![Felügyeleti](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="eee8d-175">![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")</span></span>

9. <span data-ttu-id="eee8d-176">A hello **SAML-alapú konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="eee8d-176">On hello **SAML Configuration** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="eee8d-177">![SAML-alapú konfigurációs](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="eee8d-177">![SAML configuration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")</span></span>
   
    <span data-ttu-id="eee8d-178">a.</span><span class="sxs-lookup"><span data-stu-id="eee8d-178">a.</span></span> <span data-ttu-id="eee8d-179">Válassza ki **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-179">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="eee8d-180">b.</span><span class="sxs-lookup"><span data-stu-id="eee8d-180">b.</span></span> <span data-ttu-id="eee8d-181">Beillesztés **SAML Entitásazonosító**, amelyek akkor másolta, az Azure-portálon hello hello **identitás Szolgáltatóazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="eee8d-181">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **Identity Provider ID** textbox.</span></span>
   
    <span data-ttu-id="eee8d-182">c.</span><span class="sxs-lookup"><span data-stu-id="eee8d-182">c.</span></span> <span data-ttu-id="eee8d-183">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="eee8d-183">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Single Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="eee8d-184">d.</span><span class="sxs-lookup"><span data-stu-id="eee8d-184">d.</span></span> <span data-ttu-id="eee8d-185">Beillesztés **Sign-Out URL-cím**, amely akkor másolta, az Azure-portálon hello hello **egyetlen kijelentkezési URL-címet** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="eee8d-185">Paste **Sign-Out URL**, which you have copied from hello Azure portal into hello **Single Logout URL** textbox.</span></span>
   
    <span data-ttu-id="eee8d-186">e.</span><span class="sxs-lookup"><span data-stu-id="eee8d-186">e.</span></span> <span data-ttu-id="eee8d-187">A hello vállalati részleteit megjelenítő oldalra, hello lap tetején kattintson **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-187">On hello Company Details page, at hello top, click **Save Changes**.</span></span>
    
    <span data-ttu-id="eee8d-188">![A vállalati részletek](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "vállalati részletei")</span><span class="sxs-lookup"><span data-stu-id="eee8d-188">![Company details](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")</span></span>

> [!TIP]
> <span data-ttu-id="eee8d-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="eee8d-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="eee8d-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="eee8d-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="eee8d-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eee8d-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eee8d-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eee8d-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="eee8d-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="eee8d-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="eee8d-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="eee8d-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="eee8d-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eee8d-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eee8d-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eee8d-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eee8d-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="eee8d-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eee8d-204">a.</span><span class="sxs-lookup"><span data-stu-id="eee8d-204">a.</span></span> <span data-ttu-id="eee8d-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eee8d-206">b.</span><span class="sxs-lookup"><span data-stu-id="eee8d-206">b.</span></span> <span data-ttu-id="eee8d-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eee8d-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eee8d-208">c.</span><span class="sxs-lookup"><span data-stu-id="eee8d-208">c.</span></span> <span data-ttu-id="eee8d-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="eee8d-210">d.</span><span class="sxs-lookup"><span data-stu-id="eee8d-210">d.</span></span> <span data-ttu-id="eee8d-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-211">Click **Create**.</span></span>
 
### <a name="creating-a-xmatters-ondemand-test-user"></a><span data-ttu-id="eee8d-212">XMatters OnDemand tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eee8d-212">Creating a xMatters OnDemand test user</span></span>

<span data-ttu-id="eee8d-213">A sorrend tooenable az Azure AD felhasználók toolog a tooXMatters OnDemand azok kell üzembe XMatters OnDemand be.</span><span class="sxs-lookup"><span data-stu-id="eee8d-213">In order tooenable Azure AD users toolog in tooXMatters OnDemand, they must be provisioned into XMatters OnDemand.</span></span> <span data-ttu-id="eee8d-214">XMatters OnDemand hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="eee8d-214">In hello case of XMatters OnDemand, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="eee8d-215">tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="eee8d-215">tooprovision a user accounts, perform hello following steps:</span></span>
1. <span data-ttu-id="eee8d-216">Jelentkezzen be tooyour **XMatters OnDemand** bérlő.</span><span class="sxs-lookup"><span data-stu-id="eee8d-216">Log in tooyour **XMatters OnDemand** tenant.</span></span>

2.  <span data-ttu-id="eee8d-217">Kattintson a **felhasználók** lapra, majd kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-217">Click **Users** tab. and then click **Add User**.</span></span>
  
    <span data-ttu-id="eee8d-218">![Felhasználók](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="eee8d-218">![Users](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")</span></span>

3. <span data-ttu-id="eee8d-219">A hello **hozzáadni egy felhasználót** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="eee8d-219">In hello **Add a User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="eee8d-220">![Felhasználó hozzáadása](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="eee8d-220">![Add a User](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")</span></span>

    <span data-ttu-id="eee8d-221">a.</span><span class="sxs-lookup"><span data-stu-id="eee8d-221">a.</span></span> <span data-ttu-id="eee8d-222">Válassza ki **aktív**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-222">Select **Active**.</span></span>

    <span data-ttu-id="eee8d-223">b.</span><span class="sxs-lookup"><span data-stu-id="eee8d-223">b.</span></span> <span data-ttu-id="eee8d-224">A hello **Felhasználóazonosító** szövegmező, például a felhasználó hello felhasználói azonosítója Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="eee8d-224">In hello **User ID** textbox, type hello user id of user like Brittasimon@contoso.com.</span></span>
   
    <span data-ttu-id="eee8d-225">c.</span><span class="sxs-lookup"><span data-stu-id="eee8d-225">c.</span></span> <span data-ttu-id="eee8d-226">A hello **Keresztnév** szövegmezőhöz hello felhasználó például Britta első nevét.</span><span class="sxs-lookup"><span data-stu-id="eee8d-226">In hello **First Name** textbox, type first name of hello user like Britta.</span></span>

    <span data-ttu-id="eee8d-227">d.</span><span class="sxs-lookup"><span data-stu-id="eee8d-227">d.</span></span> <span data-ttu-id="eee8d-228">A hello **Vezetéknév** szövegmezőhöz hello felhasználó például Simon utolsó típusnév.</span><span class="sxs-lookup"><span data-stu-id="eee8d-228">In hello **Last Name** textbox, type last name of hello user like Simon.</span></span>
    
    <span data-ttu-id="eee8d-229">e.</span><span class="sxs-lookup"><span data-stu-id="eee8d-229">e.</span></span> <span data-ttu-id="eee8d-230">A hello **hely** szövegmezőhöz Enter hello érvényes hely egy érvényes Azure AD-fiókot szeretne tooprovision.</span><span class="sxs-lookup"><span data-stu-id="eee8d-230">In hello **Site** textbox, Enter hello valid site of a valid Azure AD account you want tooprovision.</span></span>
    
    <span data-ttu-id="eee8d-231">f.</span><span class="sxs-lookup"><span data-stu-id="eee8d-231">f.</span></span> <span data-ttu-id="eee8d-232">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-232">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="eee8d-233">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="eee8d-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="eee8d-234">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooxMatters OnDemand megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="eee8d-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooxMatters OnDemand.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="eee8d-236">**tooassign Britta Simon tooxMatters OnDemand, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="eee8d-236">**tooassign Britta Simon tooxMatters OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="eee8d-237">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="eee8d-239">Hello alkalmazások listában válassza ki a **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-239">In hello applications list, select **xMatters OnDemand**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. <span data-ttu-id="eee8d-241">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="eee8d-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="eee8d-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="eee8d-243">Click **Add** button.</span></span> <span data-ttu-id="eee8d-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eee8d-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="eee8d-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="eee8d-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="eee8d-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eee8d-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eee8d-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eee8d-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eee8d-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="eee8d-249">Testing single sign-on</span></span>

<span data-ttu-id="eee8d-250">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="eee8d-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="eee8d-251">Hello xMatters OnDemand csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour xMatters OnDemand alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="eee8d-251">When you click hello xMatters OnDemand tile in hello Access Panel, you should get automatically signed-on tooyour xMatters OnDemand application.</span></span>
<span data-ttu-id="eee8d-252">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="eee8d-252">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eee8d-253">További források</span><span class="sxs-lookup"><span data-stu-id="eee8d-253">Additional resources</span></span>

* [<span data-ttu-id="eee8d-254">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="eee8d-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eee8d-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="eee8d-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

