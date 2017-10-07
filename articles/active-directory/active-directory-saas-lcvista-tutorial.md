---
title: "Oktatóanyag: Azure Active Directoryval integrált LCVista |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LCVista között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 5a4c7eb7d54b8b68c8241a97b9e516a3f6e55c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a><span data-ttu-id="8baa8-103">Oktatóanyag: Azure Active Directoryval integrált LCVista</span><span class="sxs-lookup"><span data-stu-id="8baa8-103">Tutorial: Azure Active Directory integration with LCVista</span></span>

<span data-ttu-id="8baa8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LCVista az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8baa8-104">In this tutorial, you learn how toointegrate LCVista with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8baa8-105">LCVista integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8baa8-105">Integrating LCVista with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8baa8-106">Megadhatja a hozzáférés tooLCVista rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8baa8-106">You can control in Azure AD who has access tooLCVista</span></span>
- <span data-ttu-id="8baa8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLCVista (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8baa8-107">You can enable your users tooautomatically get signed-on tooLCVista (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8baa8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8baa8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8baa8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8baa8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8baa8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8baa8-110">Prerequisites</span></span>

<span data-ttu-id="8baa8-111">az Azure AD integrálása LCVista tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8baa8-111">tooconfigure Azure AD integration with LCVista, you need hello following items:</span></span>

- <span data-ttu-id="8baa8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8baa8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8baa8-113">Egy LCVista egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="8baa8-113">A LCVista single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8baa8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8baa8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8baa8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8baa8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8baa8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8baa8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8baa8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8baa8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8baa8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8baa8-118">Scenario description</span></span>
<span data-ttu-id="8baa8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8baa8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8baa8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8baa8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8baa8-121">Hello gyűjteményből LCVista hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8baa8-121">Adding LCVista from hello gallery</span></span>
2. <span data-ttu-id="8baa8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8baa8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lcvista-from-hello-gallery"></a><span data-ttu-id="8baa8-123">Hello gyűjteményből LCVista hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8baa8-123">Adding LCVista from hello gallery</span></span>
<span data-ttu-id="8baa8-124">tooconfigure hello integrációja LCVista az Azure AD-be, meg kell tooadd LCVista hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8baa8-124">tooconfigure hello integration of LCVista into Azure AD, you need tooadd LCVista from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8baa8-125">**tooadd LCVista hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8baa8-125">**tooadd LCVista from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8baa8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8baa8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8baa8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8baa8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8baa8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8baa8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8baa8-133">Hello keresési mezőbe, írja be a **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-133">In hello search box, type **LCVista**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_search.png)

5. <span data-ttu-id="8baa8-135">A hello eredmények panelen válassza ki a **LCVista**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8baa8-135">In hello results panel, select **LCVista**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8baa8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8baa8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8baa8-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján LCVista</span><span class="sxs-lookup"><span data-stu-id="8baa8-138">In this section, you configure and test Azure AD single sign-on with LCVista based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8baa8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LCVista tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8baa8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LCVista is tooa user in Azure AD.</span></span> <span data-ttu-id="8baa8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LCVista közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8baa8-140">In other words, a link relationship between an Azure AD user and hello related user in LCVista needs toobe established.</span></span>

<span data-ttu-id="8baa8-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a LCVista.</span><span class="sxs-lookup"><span data-stu-id="8baa8-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LCVista.</span></span>

<span data-ttu-id="8baa8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés LCVista-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8baa8-142">tooconfigure and test Azure AD single sign-on with LCVista, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8baa8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8baa8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8baa8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8baa8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8baa8-145">**[LCVista tesztfelhasználó létrehozása](#creating-a-lcvista-test-user)**  -toohave egy megfelelője a Britta Simon a LCVista, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8baa8-145">**[Creating a LCVista test user](#creating-a-lcvista-test-user)** - toohave a counterpart of Britta Simon in LCVista that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8baa8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8baa8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8baa8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8baa8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8baa8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8baa8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8baa8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az LCVista alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8baa8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LCVista application.</span></span>

<span data-ttu-id="8baa8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a LCVista, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8baa8-150">**tooconfigure Azure AD single sign-on with LCVista, perform hello following steps:**</span></span>

1. <span data-ttu-id="8baa8-151">Az Azure portál, a hello hello **LCVista** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-151">In hello Azure portal, on hello **LCVista** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8baa8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8baa8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_samlbase.png)

3. <span data-ttu-id="8baa8-155">A hello **LCVista tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8baa8-155">On hello **LCVista Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_url.png)

    <span data-ttu-id="8baa8-157">a.</span><span class="sxs-lookup"><span data-stu-id="8baa8-157">a.</span></span> <span data-ttu-id="8baa8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.lcvista.com/rainier/login`</span><span class="sxs-lookup"><span data-stu-id="8baa8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.lcvista.com/rainier/login`</span></span>

    <span data-ttu-id="8baa8-159">b.</span><span class="sxs-lookup"><span data-stu-id="8baa8-159">b.</span></span> <span data-ttu-id="8baa8-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.lcvista.com`</span><span class="sxs-lookup"><span data-stu-id="8baa8-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.lcvista.com`</span></span> 
     
    > [!NOTE] 
    > <span data-ttu-id="8baa8-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="8baa8-161">These values are not hello real.</span></span> <span data-ttu-id="8baa8-162">Frissítse a azonosító és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="8baa8-162">Update these values with hello actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="8baa8-163">Ügyfél [LCVista ügyfél-támogatási csoport](https://lcvista.com/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8baa8-163">Contact [LCVista Client support team](https://lcvista.com/contact) tooget these values.</span></span> 

4. <span data-ttu-id="8baa8-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8baa8-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_certificate.png) 

5. <span data-ttu-id="8baa8-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8baa8-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="8baa8-168">A hello **LCVista konfigurációs** kattintson **konfigurálása LCVista** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8baa8-168">On hello **LCVista Configuration** section, click **Configure LCVista** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8baa8-169">Másolás hello **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8baa8-169">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_configure.png) 

7.  <span data-ttu-id="8baa8-171">Bejelentkezés tooyour LCVista alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8baa8-171">Sign on tooyour LCVista application as an administrator.</span></span>

8. <span data-ttu-id="8baa8-172">A hello **SAML-Config** területen ellenőrizze hello **SAML engedélyezése bejelentkezési** és írja be a hello adatait, ahogyan az alábbi kép.</span><span class="sxs-lookup"><span data-stu-id="8baa8-172">In hello **SAML Config** section, check hello **Enable SAML login** and enter hello details as mentioned in below image.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_config.png)

    <span data-ttu-id="8baa8-174">a.</span><span class="sxs-lookup"><span data-stu-id="8baa8-174">a.</span></span> <span data-ttu-id="8baa8-175">Beillesztés hello **kiállítójának URL-címe** amelyhez Azure ad-hello másolta **Entitásazonosító** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8baa8-175">Paste hello **Issuer URL** which you have copied from Azure AD in hello **Entity ID** section.</span></span> 

    <span data-ttu-id="8baa8-176">b.</span><span class="sxs-lookup"><span data-stu-id="8baa8-176">b.</span></span> <span data-ttu-id="8baa8-177">Beillesztés hello **egyszeri bejelentkezési URL-címe** amelyhez Azure ad-hello másolta **URL-cím** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8baa8-177">Paste hello **Single Sign-On Service URL** which you have copied from Azure AD in hello **URL** section.</span></span>

    <span data-ttu-id="8baa8-178">c.</span><span class="sxs-lookup"><span data-stu-id="8baa8-178">c.</span></span> <span data-ttu-id="8baa8-179">A metaadatok (XML), amely az Azure-portálról letöltött, hello értéket másol **x.509** és beillesztheti hello **x509 tanúsítvány** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8baa8-179">From Metadata (XML) which you have downloaded from Azure portal, copy hello value **X509Certificate** and paste it in hello **x509 Certificate** section.</span></span>

    <span data-ttu-id="8baa8-180">d.</span><span class="sxs-lookup"><span data-stu-id="8baa8-180">d.</span></span> <span data-ttu-id="8baa8-181">A hello **Keresztnév attribútum** szövegmezőhöz Beillesztés hello érték `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="8baa8-181">In hello **First name attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>

    <span data-ttu-id="8baa8-182">e.</span><span class="sxs-lookup"><span data-stu-id="8baa8-182">e.</span></span> <span data-ttu-id="8baa8-183">A hello **utolsó name attribútum** szövegmezőhöz Beillesztés hello érték `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="8baa8-183">In hello **Last name attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>

    <span data-ttu-id="8baa8-184">f.</span><span class="sxs-lookup"><span data-stu-id="8baa8-184">f.</span></span> <span data-ttu-id="8baa8-185">A hello **E-mail attribútum** szövegmezőhöz Beillesztés hello érték `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="8baa8-185">In hello **Email attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="8baa8-186">g.</span><span class="sxs-lookup"><span data-stu-id="8baa8-186">g.</span></span> <span data-ttu-id="8baa8-187">A hello **felhasználónév attribútum** szövegmezőhöz Beillesztés hello érték `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="8baa8-187">In hello **Username attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>

    <span data-ttu-id="8baa8-188">e.</span><span class="sxs-lookup"><span data-stu-id="8baa8-188">e.</span></span> <span data-ttu-id="8baa8-189">Kattintson a **mentése** toosave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="8baa8-189">Click **Save** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="8baa8-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8baa8-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8baa8-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8baa8-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8baa8-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8baa8-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8baa8-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8baa8-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="8baa8-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8baa8-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8baa8-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8baa8-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8baa8-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8baa8-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lcvista-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8baa8-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lcvista-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8baa8-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8baa8-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lcvista-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8baa8-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8baa8-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lcvista-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8baa8-205">a.</span><span class="sxs-lookup"><span data-stu-id="8baa8-205">a.</span></span> <span data-ttu-id="8baa8-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8baa8-207">b.</span><span class="sxs-lookup"><span data-stu-id="8baa8-207">b.</span></span> <span data-ttu-id="8baa8-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8baa8-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8baa8-209">c.</span><span class="sxs-lookup"><span data-stu-id="8baa8-209">c.</span></span> <span data-ttu-id="8baa8-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8baa8-211">d.</span><span class="sxs-lookup"><span data-stu-id="8baa8-211">d.</span></span> <span data-ttu-id="8baa8-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8baa8-212">Click **Create**.</span></span>
 
### <a name="creating-a-lcvista-test-user"></a><span data-ttu-id="8baa8-213">LCVista tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8baa8-213">Creating a LCVista test user</span></span>

<span data-ttu-id="8baa8-214">Ebben a szakaszban egy LCVista Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8baa8-214">In this section, you create a user called Britta Simon in LCVista.</span></span> <span data-ttu-id="8baa8-215">Toocontact kell [LCVista ügyfél-támogatási csoport](https://lcvista.com/contact) tooadd hello felhasználók hello LCVista alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8baa8-215">You need toocontact [LCVista Client support team](https://lcvista.com/contact) tooadd hello users in hello LCVista application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8baa8-216">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8baa8-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8baa8-217">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLCVista megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8baa8-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLCVista.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8baa8-219">**tooassign Britta Simon tooLCVista, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8baa8-219">**tooassign Britta Simon tooLCVista, perform hello following steps:**</span></span>

1. <span data-ttu-id="8baa8-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8baa8-222">Hello alkalmazások listában válassza ki a **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-222">In hello applications list, select **LCVista**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_app.png) 

3. <span data-ttu-id="8baa8-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8baa8-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8baa8-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8baa8-226">Click **Add** button.</span></span> <span data-ttu-id="8baa8-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8baa8-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8baa8-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8baa8-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8baa8-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8baa8-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8baa8-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8baa8-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8baa8-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8baa8-232">Testing single sign-on</span></span>

<span data-ttu-id="8baa8-233">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8baa8-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="8baa8-234">Kattintson a hello LCVista csempe hello hozzáférési panelre, akkor nem lesz átirányítva tooOrganization bejelentkezési lapon.</span><span class="sxs-lookup"><span data-stu-id="8baa8-234">Click hello LCVista tile in hello Access Panel, you will be redirected tooOrganization sign on page.</span></span> <span data-ttu-id="8baa8-235">Sikeres bejelentkezés után a bejelentkezett tooyour LCVista alkalmazás lesz.</span><span class="sxs-lookup"><span data-stu-id="8baa8-235">After successful login, you will be signed-on tooyour LCVista application.</span></span> <span data-ttu-id="8baa8-236">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="8baa8-236">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8baa8-237">További források</span><span class="sxs-lookup"><span data-stu-id="8baa8-237">Additional resources</span></span>

* [<span data-ttu-id="8baa8-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8baa8-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8baa8-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8baa8-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_203.png

