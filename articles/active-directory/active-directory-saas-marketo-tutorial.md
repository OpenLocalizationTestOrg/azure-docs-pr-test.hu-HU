---
title: "Oktatóanyag: Azure Active Directoryval integrált Marketo |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Marketo között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 87f88cde4f027f99a83c1ab3b318247bb4d658ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a><span data-ttu-id="74c81-103">Oktatóanyag: Azure Active Directoryval integrált Marketo</span><span class="sxs-lookup"><span data-stu-id="74c81-103">Tutorial: Azure Active Directory integration with Marketo</span></span>

<span data-ttu-id="74c81-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Marketo az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="74c81-104">In this tutorial, you learn how toointegrate Marketo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74c81-105">A Marketo integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="74c81-105">Integrating Marketo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="74c81-106">Megadhatja a hozzáférés tooMarketo rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="74c81-106">You can control in Azure AD who has access tooMarketo</span></span>
- <span data-ttu-id="74c81-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMarketo (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="74c81-107">You can enable your users tooautomatically get signed-on tooMarketo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="74c81-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="74c81-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="74c81-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74c81-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74c81-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="74c81-110">Prerequisites</span></span>

<span data-ttu-id="74c81-111">az Azure AD integrálása Marketo tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="74c81-111">tooconfigure Azure AD integration with Marketo, you need hello following items:</span></span>

- <span data-ttu-id="74c81-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="74c81-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74c81-113">A Marketo egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="74c81-113">A Marketo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74c81-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="74c81-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74c81-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="74c81-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74c81-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="74c81-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74c81-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74c81-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74c81-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="74c81-118">Scenario description</span></span>
<span data-ttu-id="74c81-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="74c81-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74c81-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="74c81-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74c81-121">A Marketo hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="74c81-121">Adding Marketo from hello gallery</span></span>
2. <span data-ttu-id="74c81-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="74c81-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-marketo-from-hello-gallery"></a><span data-ttu-id="74c81-123">A Marketo hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="74c81-123">Adding Marketo from hello gallery</span></span>
<span data-ttu-id="74c81-124">tooconfigure hello integrációja Marketo az Azure AD-be, meg kell tooadd Marketo hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="74c81-124">tooconfigure hello integration of Marketo into Azure AD, you need tooadd Marketo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="74c81-125">**tooadd Marketo hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="74c81-125">**tooadd Marketo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="74c81-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="74c81-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="74c81-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="74c81-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="74c81-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="74c81-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="74c81-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="74c81-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="74c81-133">Hello keresési mezőbe, írja be a **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="74c81-133">In hello search box, type **Marketo**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. <span data-ttu-id="74c81-135">A hello eredmények panelen válassza ki a **Marketo**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="74c81-135">In hello results panel, select **Marketo**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="74c81-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="74c81-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="74c81-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Marketo "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="74c81-138">In this section, you configure and test Azure AD single sign-on with Marketo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="74c81-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Marketo tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="74c81-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Marketo is tooa user in Azure AD.</span></span> <span data-ttu-id="74c81-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Marketo közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="74c81-140">In other words, a link relationship between an Azure AD user and hello related user in Marketo needs toobe established.</span></span>

<span data-ttu-id="74c81-141">A Marketo, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="74c81-141">In Marketo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="74c81-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Marketo-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="74c81-142">tooconfigure and test Azure AD single sign-on with Marketo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="74c81-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="74c81-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="74c81-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74c81-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74c81-145">**[A Marketo tesztfelhasználó létrehozása](#creating-a-marketo-test-user)**  -toohave egy megfelelője a Britta Simon a Marketo, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="74c81-145">**[Creating a Marketo test user](#creating-a-marketo-test-user)** - toohave a counterpart of Britta Simon in Marketo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="74c81-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="74c81-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74c81-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="74c81-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="74c81-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="74c81-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="74c81-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Marketo-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="74c81-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Marketo application.</span></span>

<span data-ttu-id="74c81-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Marketo, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="74c81-150">**tooconfigure Azure AD single sign-on with Marketo, perform hello following steps:**</span></span>

1. <span data-ttu-id="74c81-151">Az Azure portál, a hello hello **Marketo** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="74c81-151">In hello Azure portal, on hello **Marketo** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="74c81-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="74c81-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. <span data-ttu-id="74c81-155">A hello **Marketo-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="74c81-155">On hello **Marketo Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    <span data-ttu-id="74c81-157">a.</span><span class="sxs-lookup"><span data-stu-id="74c81-157">a.</span></span> <span data-ttu-id="74c81-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://saml.marketo.com/sp`</span><span class="sxs-lookup"><span data-stu-id="74c81-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://saml.marketo.com/sp`</span></span>

    <span data-ttu-id="74c81-159">b.</span><span class="sxs-lookup"><span data-stu-id="74c81-159">b.</span></span> <span data-ttu-id="74c81-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.marketo.com/saml/assertion/\<munchkinid\>`</span><span class="sxs-lookup"><span data-stu-id="74c81-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://login.marketo.com/saml/assertion/\<munchkinid\>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="74c81-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="74c81-161">These values are not real.</span></span> <span data-ttu-id="74c81-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="74c81-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="74c81-163">Ügyfél [Marketo támogatási csoport](http://investors.marketo.com/contactus.cfm) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="74c81-163">Contact [Marketo support team](http://investors.marketo.com/contactus.cfm) tooget these values.</span></span>
 
4. <span data-ttu-id="74c81-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="74c81-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. <span data-ttu-id="74c81-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="74c81-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="74c81-168">A hello **Marketo konfigurációs** területén kattintson **konfigurálása Marketo** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="74c81-168">On hello **Marketo Configuration** section, click **Configure Marketo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="74c81-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="74c81-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. <span data-ttu-id="74c81-171">tooget Munchkin azonosítója az alkalmazás, jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo, és hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="74c81-171">tooget Munchkin Id of your application, log in tooMarketo using admin credentials and perform following actions:</span></span>
   
    <span data-ttu-id="74c81-172">a.</span><span class="sxs-lookup"><span data-stu-id="74c81-172">a.</span></span> <span data-ttu-id="74c81-173">Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="74c81-173">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="74c81-174">b.</span><span class="sxs-lookup"><span data-stu-id="74c81-174">b.</span></span> <span data-ttu-id="74c81-175">Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.</span><span class="sxs-lookup"><span data-stu-id="74c81-175">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="74c81-177">c.</span><span class="sxs-lookup"><span data-stu-id="74c81-177">c.</span></span> <span data-ttu-id="74c81-178">Keresse meg a toohello integrációs menüben, majd kattintson a hello **Munchkin hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="74c81-178">Navigate toohello Integration menu and click hello **Munchkin link**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    <span data-ttu-id="74c81-180">d.</span><span class="sxs-lookup"><span data-stu-id="74c81-180">d.</span></span> <span data-ttu-id="74c81-181">Hello hello képernyőn megjelenő Munchkin azonosító másolja, majd fejezze be a válasz URL-CÍMEN az Azure AD hello beállítása varázsló.</span><span class="sxs-lookup"><span data-stu-id="74c81-181">Copy hello Munchkin Id shown on hello screen and complete your Reply URL in hello Azure AD configuration wizard.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. <span data-ttu-id="74c81-183">tooconfigure hello SSO hello alkalmazásban, hajtsa végre a következő lépések hello:</span><span class="sxs-lookup"><span data-stu-id="74c81-183">tooconfigure hello SSO in hello application, follow hello below steps:</span></span>
   
    <span data-ttu-id="74c81-184">a.</span><span class="sxs-lookup"><span data-stu-id="74c81-184">a.</span></span> <span data-ttu-id="74c81-185">Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="74c81-185">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="74c81-186">b.</span><span class="sxs-lookup"><span data-stu-id="74c81-186">b.</span></span> <span data-ttu-id="74c81-187">Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.</span><span class="sxs-lookup"><span data-stu-id="74c81-187">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="74c81-189">c.</span><span class="sxs-lookup"><span data-stu-id="74c81-189">c.</span></span> <span data-ttu-id="74c81-190">Toohello integrációs menüben keresse meg és kattintson **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="74c81-190">Navigate toohello Integration menu and click **Single Sign On**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    <span data-ttu-id="74c81-192">d.</span><span class="sxs-lookup"><span data-stu-id="74c81-192">d.</span></span> <span data-ttu-id="74c81-193">tooenable hello SAML-beállításokat, kattintson a **szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="74c81-193">tooenable hello SAML Settings, click **Edit** button.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    <span data-ttu-id="74c81-195">e.</span><span class="sxs-lookup"><span data-stu-id="74c81-195">e.</span></span> <span data-ttu-id="74c81-196">**Engedélyezett** egyszeri bejelentkezés beállításait.</span><span class="sxs-lookup"><span data-stu-id="74c81-196">**Enabled** Single Sign-On settings.</span></span>
   
    <span data-ttu-id="74c81-197">f.</span><span class="sxs-lookup"><span data-stu-id="74c81-197">f.</span></span> <span data-ttu-id="74c81-198">Beillesztés hello **SAML Entitásazonosító**, a hello **kibocsátó azonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="74c81-198">Paste hello **SAML Entity ID**, in hello **Issuer ID** textbox.</span></span>
   
    <span data-ttu-id="74c81-199">g.</span><span class="sxs-lookup"><span data-stu-id="74c81-199">g.</span></span> <span data-ttu-id="74c81-200">A hello **Entitásazonosító** szövegmezőhöz hello az URL-címet adjon meg `http://saml.marketo.com/sp`.</span><span class="sxs-lookup"><span data-stu-id="74c81-200">In hello **Entity ID** textbox, enter hello URL as `http://saml.marketo.com/sp`.</span></span>
   
    <span data-ttu-id="74c81-201">h.</span><span class="sxs-lookup"><span data-stu-id="74c81-201">h.</span></span> <span data-ttu-id="74c81-202">Jelölje be hello felhasználói azonosító hely szerint **névazonosítója elem**.</span><span class="sxs-lookup"><span data-stu-id="74c81-202">Select hello User ID Location as **Name Identifier element**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > <span data-ttu-id="74c81-204">Ha a felhasználói azonosító nem egyszerű felhasználónév értéke hello attribútum lapon hello értékének módosítása.</span><span class="sxs-lookup"><span data-stu-id="74c81-204">If your User Identifier is not UPN value then change hello value in hello Attribute tab.</span></span>
   
    <span data-ttu-id="74c81-205">i.</span><span class="sxs-lookup"><span data-stu-id="74c81-205">i.</span></span> <span data-ttu-id="74c81-206">Töltse fel a hello tanúsítványt, amely már letöltötte az Azure AD konfigurálása varázsló.</span><span class="sxs-lookup"><span data-stu-id="74c81-206">Upload hello certificate, which you have downloaded from Azure AD configuration wizard.</span></span> <span data-ttu-id="74c81-207">**Mentés** hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="74c81-207">**Save** hello settings.</span></span>
   
    <span data-ttu-id="74c81-208">j.</span><span class="sxs-lookup"><span data-stu-id="74c81-208">j.</span></span> <span data-ttu-id="74c81-209">Hello átirányítási hibalapok beállításainak szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="74c81-209">Edit hello Redirect Pages settings.</span></span>
   
    <span data-ttu-id="74c81-210">k.</span><span class="sxs-lookup"><span data-stu-id="74c81-210">k.</span></span> <span data-ttu-id="74c81-211">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="74c81-211">Paste hello **SAML Single Sign-On Service URL** in hello **Login URL** textbox.</span></span>
   
    <span data-ttu-id="74c81-212">l.</span><span class="sxs-lookup"><span data-stu-id="74c81-212">l.</span></span> <span data-ttu-id="74c81-213">Beillesztés hello **Sign-Out URL-cím** a hello **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="74c81-213">Paste hello **Sign-Out URL** in hello **Logout URL** textbox.</span></span>
   
    <span data-ttu-id="74c81-214">m.</span><span class="sxs-lookup"><span data-stu-id="74c81-214">m.</span></span> <span data-ttu-id="74c81-215">A hello **hiba URL-cím**, másolása a **Marketo-példány URL-címe** kattintson **mentése** toosave beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="74c81-215">In hello **Error URL**, copy your **Marketo instance URL** and click **Save** button toosave settings.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. <span data-ttu-id="74c81-217">tooenable hello egyszeri Bejelentkezést a felhasználók számára, a következő műveletek teljes hello:</span><span class="sxs-lookup"><span data-stu-id="74c81-217">tooenable hello SSO for users, complete hello following actions:</span></span>
   
    <span data-ttu-id="74c81-218">a.</span><span class="sxs-lookup"><span data-stu-id="74c81-218">a.</span></span> <span data-ttu-id="74c81-219">Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="74c81-219">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="74c81-220">b.</span><span class="sxs-lookup"><span data-stu-id="74c81-220">b.</span></span> <span data-ttu-id="74c81-221">Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.</span><span class="sxs-lookup"><span data-stu-id="74c81-221">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="74c81-223">c.</span><span class="sxs-lookup"><span data-stu-id="74c81-223">c.</span></span> <span data-ttu-id="74c81-224">Keresse meg a toohello **biztonsági** menüre, majd **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="74c81-224">Navigate toohello **Security** menu and click **Login Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    <span data-ttu-id="74c81-226">d.</span><span class="sxs-lookup"><span data-stu-id="74c81-226">d.</span></span> <span data-ttu-id="74c81-227">Hello ellenőrizze **szükséges SSO** beállítás és **mentése** hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="74c81-227">Check hello **Require SSO** option and **Save** hello settings.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> <span data-ttu-id="74c81-229">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="74c81-229">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="74c81-230">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="74c81-230">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="74c81-231">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74c81-231">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="74c81-232">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="74c81-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="74c81-233">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="74c81-233">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="74c81-235">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="74c81-235">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="74c81-236">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="74c81-236">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="74c81-238">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="74c81-238">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="74c81-240">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74c81-240">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="74c81-242">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="74c81-242">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="74c81-244">a.</span><span class="sxs-lookup"><span data-stu-id="74c81-244">a.</span></span> <span data-ttu-id="74c81-245">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74c81-245">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74c81-246">b.</span><span class="sxs-lookup"><span data-stu-id="74c81-246">b.</span></span> <span data-ttu-id="74c81-247">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="74c81-247">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="74c81-248">c.</span><span class="sxs-lookup"><span data-stu-id="74c81-248">c.</span></span> <span data-ttu-id="74c81-249">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="74c81-249">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="74c81-250">d.</span><span class="sxs-lookup"><span data-stu-id="74c81-250">d.</span></span> <span data-ttu-id="74c81-251">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="74c81-251">Click **Create**.</span></span>
 
### <a name="creating-a-marketo-test-user"></a><span data-ttu-id="74c81-252">A Marketo tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="74c81-252">Creating a Marketo test user</span></span>

<span data-ttu-id="74c81-253">Ebben a szakaszban a Marketo Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="74c81-253">In this section, you create a user called Britta Simon in Marketo.</span></span> <span data-ttu-id="74c81-254">Kövesse ezeket lépéseket toocreate Marketo platform felhasználójának.</span><span class="sxs-lookup"><span data-stu-id="74c81-254">follow these steps toocreate a user in Marketo platform.</span></span>

1. <span data-ttu-id="74c81-255">Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="74c81-255">Log in tooMarketo app using admin credentials.</span></span>

2. <span data-ttu-id="74c81-256">Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.</span><span class="sxs-lookup"><span data-stu-id="74c81-256">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. <span data-ttu-id="74c81-258">Keresse meg a toohello **biztonsági** menüre, majd **felhasználók és szerepkörök**</span><span class="sxs-lookup"><span data-stu-id="74c81-258">Navigate toohello **Security** menu and click **Users & Roles**</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. <span data-ttu-id="74c81-260">Kattintson a hello **hívhat meg új felhasználói** hello felhasználók lapon hivatkozás</span><span class="sxs-lookup"><span data-stu-id="74c81-260">Click hello **Invite New User** link on hello Users tab</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. <span data-ttu-id="74c81-262">A hello hívhat meg új felhasználó varázsló kitöltés hello következő információ</span><span class="sxs-lookup"><span data-stu-id="74c81-262">In hello Invite New User wizard fill hello following information</span></span>
   
    <span data-ttu-id="74c81-263">a.</span><span class="sxs-lookup"><span data-stu-id="74c81-263">a.</span></span> <span data-ttu-id="74c81-264">Adja meg a hello felhasználói **E-mail** hello szövegmezőjének cím</span><span class="sxs-lookup"><span data-stu-id="74c81-264">Enter hello user **Email** address in hello textbox</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    <span data-ttu-id="74c81-266">b.</span><span class="sxs-lookup"><span data-stu-id="74c81-266">b.</span></span> <span data-ttu-id="74c81-267">Adja meg a hello **Keresztnév** hello szövegmezőben</span><span class="sxs-lookup"><span data-stu-id="74c81-267">Enter hello **First Name** in hello textbox</span></span>
   
    <span data-ttu-id="74c81-268">c.</span><span class="sxs-lookup"><span data-stu-id="74c81-268">c.</span></span> <span data-ttu-id="74c81-269">Adja meg a hello **Vezetéknév** hello szövegmezőben</span><span class="sxs-lookup"><span data-stu-id="74c81-269">Enter hello **Last Name**  in hello textbox</span></span>
   
    <span data-ttu-id="74c81-270">d.</span><span class="sxs-lookup"><span data-stu-id="74c81-270">d.</span></span> <span data-ttu-id="74c81-271">Kattintson a **Tovább** gombra</span><span class="sxs-lookup"><span data-stu-id="74c81-271">Click **Next**</span></span>

6. <span data-ttu-id="74c81-272">A hello **engedélyek** lapra, jelölje be hello **userRoles** kattintson **tovább**</span><span class="sxs-lookup"><span data-stu-id="74c81-272">In hello **Permissions** tab, select hello **userRoles** and click **Next**</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. <span data-ttu-id="74c81-274">Kattintson a hello **küldése** toosend hello felhasználói meghívó gomb</span><span class="sxs-lookup"><span data-stu-id="74c81-274">Click hello **Send** button toosend hello user invitation</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. <span data-ttu-id="74c81-276">Felhasználó hello e-mail értesítést kap, és rendelkezik tooclick hello hivatkozásra, és hello jelszó tooactivate hello fiók módosítása.</span><span class="sxs-lookup"><span data-stu-id="74c81-276">User receives hello email notification and has tooclick hello link and change hello password tooactivate hello account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="74c81-277">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="74c81-277">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="74c81-278">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMarketo megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="74c81-278">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMarketo.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="74c81-280">**tooassign Britta Simon tooMarketo, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="74c81-280">**tooassign Britta Simon tooMarketo, perform hello following steps:**</span></span>

1. <span data-ttu-id="74c81-281">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="74c81-281">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="74c81-283">Hello alkalmazások listában válassza ki a **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="74c81-283">In hello applications list, select **Marketo**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. <span data-ttu-id="74c81-285">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="74c81-285">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="74c81-287">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="74c81-287">Click **Add** button.</span></span> <span data-ttu-id="74c81-288">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74c81-288">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="74c81-290">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="74c81-290">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="74c81-291">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74c81-291">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74c81-292">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74c81-292">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="74c81-293">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="74c81-293">Testing single sign-on</span></span>

<span data-ttu-id="74c81-294">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="74c81-294">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="74c81-295">Ha a hozzáférési Panel hello hello Marketo csempe gombra kattint, automatikusan bejelentkezett tooyour Marketo alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="74c81-295">When you click hello Marketo tile in hello Access Panel, you should get automatically signed-on tooyour Marketo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74c81-296">További források</span><span class="sxs-lookup"><span data-stu-id="74c81-296">Additional resources</span></span>

* [<span data-ttu-id="74c81-297">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="74c81-297">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74c81-298">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="74c81-298">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

