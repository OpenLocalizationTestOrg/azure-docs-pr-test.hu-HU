---
title: "Oktatóanyag: Azure Active Directoryval integrált Lynda.com |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Lynda.com között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6c92789-8b64-4049-bac9-8cb928398433
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: fb8d7824e5121da79e9248393b0cbcb0efaffec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lyndacom"></a><span data-ttu-id="aed00-103">Oktatóanyag: Azure Active Directoryval integrált Lynda.com</span><span class="sxs-lookup"><span data-stu-id="aed00-103">Tutorial: Azure Active Directory integration with Lynda.com</span></span>

<span data-ttu-id="aed00-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Lynda.com az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="aed00-104">In this tutorial, you learn how toointegrate Lynda.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aed00-105">Lynda.com integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="aed00-105">Integrating Lynda.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="aed00-106">Megadhatja a hozzáférés tooLynda.com rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="aed00-106">You can control in Azure AD who has access tooLynda.com</span></span>
- <span data-ttu-id="aed00-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLynda.com (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="aed00-107">You can enable your users tooautomatically get signed-on tooLynda.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="aed00-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="aed00-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="aed00-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aed00-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aed00-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aed00-110">Prerequisites</span></span>

<span data-ttu-id="aed00-111">az Azure AD integrálása Lynda.com tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="aed00-111">tooconfigure Azure AD integration with Lynda.com, you need hello following items:</span></span>

- <span data-ttu-id="aed00-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="aed00-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aed00-113">Egy Lynda.com egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="aed00-113">A Lynda.com single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aed00-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="aed00-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aed00-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="aed00-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aed00-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="aed00-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aed00-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aed00-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aed00-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="aed00-118">Scenario description</span></span>
<span data-ttu-id="aed00-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="aed00-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aed00-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="aed00-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aed00-121">Hello gyűjteményből Lynda.com hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aed00-121">Adding Lynda.com from hello gallery</span></span>
2. <span data-ttu-id="aed00-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aed00-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lyndacom-from-hello-gallery"></a><span data-ttu-id="aed00-123">Hello gyűjteményből Lynda.com hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aed00-123">Adding Lynda.com from hello gallery</span></span>
<span data-ttu-id="aed00-124">tooconfigure hello integrációja Lynda.com az Azure AD-be, meg kell tooadd Lynda.com hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="aed00-124">tooconfigure hello integration of Lynda.com into Azure AD, you need tooadd Lynda.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="aed00-125">**tooadd Lynda.com hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="aed00-125">**tooadd Lynda.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="aed00-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="aed00-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="aed00-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="aed00-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="aed00-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="aed00-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="aed00-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="aed00-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="aed00-133">Hello keresési mezőbe, írja be a **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="aed00-133">In hello search box, type **Lynda.com**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_search.png)

5. <span data-ttu-id="aed00-135">A hello eredmények panelen válassza ki a **Lynda.com**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="aed00-135">In hello results panel, select **Lynda.com**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="aed00-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aed00-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="aed00-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Lynda.com</span><span class="sxs-lookup"><span data-stu-id="aed00-138">In this section, you configure and test Azure AD single sign-on with Lynda.com based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="aed00-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Lynda.com tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="aed00-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lynda.com is tooa user in Azure AD.</span></span> <span data-ttu-id="aed00-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Lynda.com közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="aed00-140">In other words, a link relationship between an Azure AD user and hello related user in Lynda.com needs toobe established.</span></span>

<span data-ttu-id="aed00-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="aed00-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Lynda.com.</span></span>

<span data-ttu-id="aed00-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Lynda.com-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="aed00-142">tooconfigure and test Azure AD single sign-on with Lynda.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="aed00-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="aed00-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="aed00-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aed00-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aed00-145">**[Lynda.com tesztfelhasználó létrehozása](#creating-a-lyndacom-test-user)**  -toohave egy megfelelője a Britta Simon a Lynda.com, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="aed00-145">**[Creating a Lynda.com test user](#creating-a-lyndacom-test-user)** - toohave a counterpart of Britta Simon in Lynda.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="aed00-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="aed00-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aed00-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="aed00-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="aed00-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aed00-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="aed00-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Lynda.com alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="aed00-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lynda.com application.</span></span>

<span data-ttu-id="aed00-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Lynda.com, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="aed00-150">**tooconfigure Azure AD single sign-on with Lynda.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="aed00-151">Az Azure portál, a hello hello **Lynda.com** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="aed00-151">In hello Azure portal, on hello **Lynda.com** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="aed00-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="aed00-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_samlbase.png)

3. <span data-ttu-id="aed00-155">A hello **Lynda.com tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="aed00-155">On hello **Lynda.com Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_url.png)

    <span data-ttu-id="aed00-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span><span class="sxs-lookup"><span data-stu-id="aed00-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="aed00-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="aed00-158">This value is not real.</span></span> <span data-ttu-id="aed00-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="aed00-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="aed00-160">Ügyfél [Lynda.com ügyfél-támogatási csoport](https://www.linkedin.com/help/lynda/ask) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="aed00-160">Contact [Lynda.com Client support team](https://www.linkedin.com/help/lynda/ask) tooget these values.</span></span> 
 
4. <span data-ttu-id="aed00-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="aed00-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_certificate.png) 

5. <span data-ttu-id="aed00-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="aed00-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lynda-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="aed00-165">tooconfigure egyszeri bejelentkezést a **Lynda.com** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** [Lynda.com támogatási](https://www.linkedin.com/help/lynda/ask).</span><span class="sxs-lookup"><span data-stu-id="aed00-165">tooconfigure single sign-on on **Lynda.com** side, you need toosend hello downloaded **Metadata XML** [Lynda.com support](https://www.linkedin.com/help/lynda/ask).</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="aed00-166">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="aed00-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="aed00-167">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="aed00-167">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="aed00-169">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="aed00-169">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="aed00-170">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="aed00-170">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lynda-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="aed00-172">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="aed00-172">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lynda-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="aed00-174">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aed00-174">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lynda-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="aed00-176">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="aed00-176">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lynda-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="aed00-178">a.</span><span class="sxs-lookup"><span data-stu-id="aed00-178">a.</span></span> <span data-ttu-id="aed00-179">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aed00-179">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aed00-180">b.</span><span class="sxs-lookup"><span data-stu-id="aed00-180">b.</span></span> <span data-ttu-id="aed00-181">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="aed00-181">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="aed00-182">c.</span><span class="sxs-lookup"><span data-stu-id="aed00-182">c.</span></span> <span data-ttu-id="aed00-183">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="aed00-183">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="aed00-184">d.</span><span class="sxs-lookup"><span data-stu-id="aed00-184">d.</span></span> <span data-ttu-id="aed00-185">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="aed00-185">Click **Create**.</span></span>
 
### <a name="creating-a-lyndacom-test-user"></a><span data-ttu-id="aed00-186">Lynda.com tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="aed00-186">Creating a Lynda.com test user</span></span>

<span data-ttu-id="aed00-187">Nincs a akkor tooconfigure felhasználók átadásához tooLynda.com művelet elem.</span><span class="sxs-lookup"><span data-stu-id="aed00-187">There is no action item for you tooconfigure user provisioning tooLynda.com.</span></span>  
<span data-ttu-id="aed00-188">Ha egy hozzárendelt felhasználó megpróbál a hozzáférési panelen hello tooLynda.com toolog, Lynda.com ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="aed00-188">When an assigned user tries toolog in tooLynda.com using hello access panel, Lynda.com checks whether hello user exists.</span></span>  

<span data-ttu-id="aed00-189">Nincs még nincs felhasználói fiók érhető el, ha a Lynda.com automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="aed00-189">If there is no user account available yet, it is automatically created by Lynda.com.</span></span>

>[!NOTE]
><span data-ttu-id="aed00-190">Bármely más Lynda.com felhasználói fiók létrehozása eszközök vagy Lynda.com tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="aed00-190">You can use any other Lynda.com user account creation tools or APIs provided by Lynda.com tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="aed00-191">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="aed00-191">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="aed00-192">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLynda.com megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="aed00-192">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLynda.com.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="aed00-194">**tooassign Britta Simon tooLynda.com, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="aed00-194">**tooassign Britta Simon tooLynda.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="aed00-195">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="aed00-195">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="aed00-197">Hello alkalmazások listában válassza ki a **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="aed00-197">In hello applications list, select **Lynda.com**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_app.png) 

3. <span data-ttu-id="aed00-199">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="aed00-199">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="aed00-201">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="aed00-201">Click **Add** button.</span></span> <span data-ttu-id="aed00-202">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aed00-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="aed00-204">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="aed00-204">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="aed00-205">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aed00-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aed00-206">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aed00-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="aed00-207">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="aed00-207">Testing single sign-on</span></span>

<span data-ttu-id="aed00-208">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="aed00-208">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="aed00-209">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="aed00-209">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="aed00-210">További források</span><span class="sxs-lookup"><span data-stu-id="aed00-210">Additional resources</span></span>

* [<span data-ttu-id="aed00-211">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="aed00-211">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aed00-212">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="aed00-212">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_203.png

