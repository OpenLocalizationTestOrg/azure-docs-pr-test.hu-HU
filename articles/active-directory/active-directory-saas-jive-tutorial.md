---
title: "Oktatóanyag: Azure Active Directoryval integrált Jive |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jive között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: f22bf78a55e8a4a9ea2f0020ef2f535be88b6302
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a><span data-ttu-id="f87a3-103">Oktatóanyag: Azure Active Directoryval integrált Jive</span><span class="sxs-lookup"><span data-stu-id="f87a3-103">Tutorial: Azure Active Directory integration with Jive</span></span>

<span data-ttu-id="f87a3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jive az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f87a3-104">In this tutorial, you learn how toointegrate Jive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f87a3-105">Jive integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f87a3-105">Integrating Jive with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f87a3-106">Megadhatja a hozzáférés tooJive rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f87a3-106">You can control in Azure AD who has access tooJive</span></span>
- <span data-ttu-id="f87a3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJive (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f87a3-107">You can enable your users tooautomatically get signed-on tooJive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f87a3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f87a3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f87a3-109">Ha szeretne tooknow az Azure AD SaaS integrálásáról további információt, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f87a3-109">If you want tooknow more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f87a3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f87a3-110">Prerequisites</span></span>

<span data-ttu-id="f87a3-111">az Azure AD integrálása Jive tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f87a3-111">tooconfigure Azure AD integration with Jive, you need hello following items:</span></span>

- <span data-ttu-id="f87a3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f87a3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f87a3-113">Egy Jive egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f87a3-113">A Jive single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f87a3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f87a3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f87a3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f87a3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f87a3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f87a3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f87a3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f87a3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f87a3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f87a3-118">Scenario description</span></span>
<span data-ttu-id="f87a3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f87a3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f87a3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f87a3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f87a3-121">Hello gyűjteményből Jive hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f87a3-121">Adding Jive from hello gallery</span></span>
2. <span data-ttu-id="f87a3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f87a3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jive-from-hello-gallery"></a><span data-ttu-id="f87a3-123">Hello gyűjteményből Jive hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f87a3-123">Adding Jive from hello gallery</span></span>
<span data-ttu-id="f87a3-124">tooconfigure hello integrálása az Azure AD-be Jive, meg kell tooadd Jive hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f87a3-124">tooconfigure hello integration of Jive into Azure AD, you need tooadd Jive from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f87a3-125">**tooadd Jive hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f87a3-125">**tooadd Jive from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f87a3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f87a3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f87a3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f87a3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f87a3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f87a3-133">Hello keresési mezőbe, írja be a **Jive**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-133">In hello search box, type **Jive**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/tutorial_jive_search.png)

5. <span data-ttu-id="f87a3-135">A hello eredmények panelen válassza ki a **Jive**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-135">In hello results panel, select **Jive**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f87a3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f87a3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f87a3-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jive</span><span class="sxs-lookup"><span data-stu-id="f87a3-138">In this section, you configure and test Azure AD single sign-on with Jive based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f87a3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Jive tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f87a3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jive is tooa user in Azure AD.</span></span> <span data-ttu-id="f87a3-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Jive közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f87a3-140">In other words, a link relationship between an Azure AD user and hello related user in Jive needs toobe established.</span></span>

<span data-ttu-id="f87a3-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Jive.</span><span class="sxs-lookup"><span data-stu-id="f87a3-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Jive.</span></span>

<span data-ttu-id="f87a3-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Jive-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f87a3-142">tooconfigure and test Azure AD single sign-on with Jive, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f87a3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f87a3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f87a3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f87a3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f87a3-145">**[Jive tesztfelhasználó létrehozása](#creating-a-jive-test-user)**  -toohave egy megfelelője a Britta Simon a Jive, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f87a3-145">**[Creating a Jive test user](#creating-a-jive-test-user)** - toohave a counterpart of Britta Simon in Jive that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f87a3-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f87a3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f87a3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f87a3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f87a3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f87a3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f87a3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Jive alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f87a3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jive application.</span></span>

<span data-ttu-id="f87a3-150">**Jive, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f87a3-150">**tooconfigure Azure AD single sign-on with Jive, perform hello following steps:**</span></span>

1. <span data-ttu-id="f87a3-151">Az Azure portál, a hello hello **Jive** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-151">In hello Azure portal, on hello **Jive** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f87a3-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f87a3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_samlbase.png)

3. <span data-ttu-id="f87a3-155">A hello **Jive tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f87a3-155">On hello **Jive Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_url.png)

    <span data-ttu-id="f87a3-157">a.</span><span class="sxs-lookup"><span data-stu-id="f87a3-157">a.</span></span> <span data-ttu-id="f87a3-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instance name>.jivecustom.com`</span><span class="sxs-lookup"><span data-stu-id="f87a3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instance name>.jivecustom.com`</span></span>

    <span data-ttu-id="f87a3-159">b.</span><span class="sxs-lookup"><span data-stu-id="f87a3-159">b.</span></span> <span data-ttu-id="f87a3-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instance name>.jiveon.com`</span><span class="sxs-lookup"><span data-stu-id="f87a3-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instance name>.jiveon.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f87a3-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="f87a3-161">These values are not hello real.</span></span> <span data-ttu-id="f87a3-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f87a3-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="f87a3-163">Ügyfél [Jive ügyfél-támogatási csoport](https://www.jivesoftware.com/services-support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f87a3-163">Contact [Jive Client support team](https://www.jivesoftware.com/services-support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f87a3-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f87a3-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_certificate.png) 

5. <span data-ttu-id="f87a3-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f87a3-168">tooconfigure egyszeri bejelentkezést a **Jive** oldal, a bejelentkezés tooyour Jive Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="f87a3-168">tooconfigure single sign-on on **Jive** side, sign-on tooyour Jive tenant as an administrator.</span></span>

7. <span data-ttu-id="f87a3-169">Hello hello felső menüben kattintson a "**Saml**."</span><span class="sxs-lookup"><span data-stu-id="f87a3-169">In hello menu on hello top, Click "**Saml**."</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    <span data-ttu-id="f87a3-171">a.</span><span class="sxs-lookup"><span data-stu-id="f87a3-171">a.</span></span> <span data-ttu-id="f87a3-172">Válassza ki **engedélyezve** alatt hello **általános** fülre.</span><span class="sxs-lookup"><span data-stu-id="f87a3-172">Select **Enabled** under hello **General** tab.</span></span>   
    <span data-ttu-id="f87a3-173">b.</span><span class="sxs-lookup"><span data-stu-id="f87a3-173">b.</span></span> <span data-ttu-id="f87a3-174">Kattintson a hello "**összes saml-beállításainak mentése**" gombra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-174">Click hello "**Save all saml settings**" button.</span></span>

8. <span data-ttu-id="f87a3-175">Keresse meg a toohello "**Idp metaadatok**" lapon.</span><span class="sxs-lookup"><span data-stu-id="f87a3-175">Navigate toohello "**Idp Metadata**" tab.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)
   
    <span data-ttu-id="f87a3-177">a.</span><span class="sxs-lookup"><span data-stu-id="f87a3-177">a.</span></span> <span data-ttu-id="f87a3-178">Másolja a letöltött hello metaadatok XML-fájl hello tartalmat, és illessze be hello **Identity Provider (IDP) metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f87a3-178">Copy hello content of hello downloaded metadata XML file, and then paste it into hello **Identity Provider (IDP) Metadata** textbox.</span></span>
    
    <span data-ttu-id="f87a3-179">b.</span><span class="sxs-lookup"><span data-stu-id="f87a3-179">b.</span></span> <span data-ttu-id="f87a3-180">Kattintson a hello "**összes saml-beállításainak mentése**" gombra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-180">Click hello "**Save all saml settings**" button.</span></span> 

9. <span data-ttu-id="f87a3-181">Nyissa meg toohello "**attribútum Felhasználóleképezés**" lapon.</span><span class="sxs-lookup"><span data-stu-id="f87a3-181">Go toohello "**User Attribute Mapping**" tab.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)
   
    <span data-ttu-id="f87a3-183">a.</span><span class="sxs-lookup"><span data-stu-id="f87a3-183">a.</span></span> <span data-ttu-id="f87a3-184">A hello **E-mail** szövegmező, másolás és beillesztés hello attribútum neve **mail** érték.</span><span class="sxs-lookup"><span data-stu-id="f87a3-184">In hello **Email** textbox, copy and paste hello attribute name of **mail** value.</span></span>
   
    <span data-ttu-id="f87a3-185">b.</span><span class="sxs-lookup"><span data-stu-id="f87a3-185">b.</span></span> <span data-ttu-id="f87a3-186">A hello **Utónév** szövegmező, másolás és beillesztés hello attribútum neve **givenname** érték.</span><span class="sxs-lookup"><span data-stu-id="f87a3-186">In hello **First Name** textbox, copy and paste hello attribute name of **givenname** value.</span></span>
   
    <span data-ttu-id="f87a3-187">c.</span><span class="sxs-lookup"><span data-stu-id="f87a3-187">c.</span></span> <span data-ttu-id="f87a3-188">A hello **Vezetéknév** szövegmező, másolás és beillesztés hello attribútum neve **vezetékneve** érték.</span><span class="sxs-lookup"><span data-stu-id="f87a3-188">In hello **Last Name** textbox, copy and paste hello attribute name of **surname** value.</span></span>

> [!TIP]
> <span data-ttu-id="f87a3-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f87a3-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f87a3-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f87a3-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f87a3-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f87a3-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f87a3-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f87a3-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="f87a3-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f87a3-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f87a3-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f87a3-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f87a3-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f87a3-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f87a3-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f87a3-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f87a3-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f87a3-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f87a3-204">a.</span><span class="sxs-lookup"><span data-stu-id="f87a3-204">a.</span></span> <span data-ttu-id="f87a3-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f87a3-206">b.</span><span class="sxs-lookup"><span data-stu-id="f87a3-206">b.</span></span> <span data-ttu-id="f87a3-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f87a3-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f87a3-208">c.</span><span class="sxs-lookup"><span data-stu-id="f87a3-208">c.</span></span> <span data-ttu-id="f87a3-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f87a3-210">d.</span><span class="sxs-lookup"><span data-stu-id="f87a3-210">d.</span></span> <span data-ttu-id="f87a3-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-211">Click **Create**.</span></span>
 
### <a name="creating-a-jive-test-user"></a><span data-ttu-id="f87a3-212">Jive tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f87a3-212">Creating a Jive test user</span></span>

<span data-ttu-id="f87a3-213">Együttműködve [Jive ügyfél-támogatási csoport](https://www.jivesoftware.com/services-support/) tooadd hello felhasználók hello Jive platform.</span><span class="sxs-lookup"><span data-stu-id="f87a3-213">Work with [Jive Client support team](https://www.jivesoftware.com/services-support/) tooadd hello users in hello Jive platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f87a3-214">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f87a3-214">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f87a3-215">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooJive megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f87a3-215">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJive.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f87a3-217">**tooassign Britta Simon tooJive, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f87a3-217">**tooassign Britta Simon tooJive, perform hello following steps:**</span></span>

1. <span data-ttu-id="f87a3-218">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-218">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f87a3-220">Hello alkalmazások listában válassza ki a **Jive**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-220">In hello applications list, select **Jive**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_app.png) 

3. <span data-ttu-id="f87a3-222">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f87a3-222">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f87a3-224">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f87a3-224">Click **Add** button.</span></span> <span data-ttu-id="f87a3-225">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f87a3-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f87a3-227">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f87a3-227">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f87a3-228">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f87a3-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f87a3-229">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f87a3-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f87a3-230">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f87a3-230">Testing single sign-on</span></span>

<span data-ttu-id="f87a3-231">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f87a3-231">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f87a3-232">Ha a hozzáférési Panel hello hello Jive csempe gombra kattint, automatikusan bejelentkezett tooyour Jive alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f87a3-232">When you click hello Jive tile in hello Access Panel, you should get automatically signed-on tooyour Jive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f87a3-233">További források</span><span class="sxs-lookup"><span data-stu-id="f87a3-233">Additional resources</span></span>

* [<span data-ttu-id="f87a3-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f87a3-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f87a3-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f87a3-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="f87a3-236">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f87a3-236">Configure User Provisioning</span></span>](active-directory-saas-jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jive-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png

