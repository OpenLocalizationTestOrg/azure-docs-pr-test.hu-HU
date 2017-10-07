---
title: "Oktatóanyag: Azure Active Directoryval integrált WORKS MOBILE |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és WORKS MOBILE között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 80192218a2e99a921834bb53e708d5e4fab413f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a><span data-ttu-id="716c8-103">Oktatóanyag: Azure Active Directoryval integrált WORKS MOBILE</span><span class="sxs-lookup"><span data-stu-id="716c8-103">Tutorial: Azure Active Directory integration with WORKS MOBILE</span></span>

<span data-ttu-id="716c8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan működik a toointegrate MOBILE az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="716c8-104">In this tutorial, you learn how toointegrate WORKS MOBILE with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="716c8-105">WORKS MOBILE integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="716c8-105">Integrating WORKS MOBILE with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="716c8-106">Megadhatja a hozzáférés tooWORKS MOBILE rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="716c8-106">You can control in Azure AD who has access tooWORKS MOBILE</span></span>
- <span data-ttu-id="716c8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWORKS MOBILE (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="716c8-107">You can enable your users tooautomatically get signed-on tooWORKS MOBILE (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="716c8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="716c8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="716c8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="716c8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="716c8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="716c8-110">Prerequisites</span></span>

<span data-ttu-id="716c8-111">tooconfigure az Azure AD integrálása WORKS MOBILE, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="716c8-111">tooconfigure Azure AD integration with WORKS MOBILE, you need hello following items:</span></span>

- <span data-ttu-id="716c8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="716c8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="716c8-113">Egy WORKS MOBILE egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="716c8-113">A WORKS MOBILE single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="716c8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="716c8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="716c8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="716c8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="716c8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="716c8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="716c8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="716c8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="716c8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="716c8-118">Scenario description</span></span>
<span data-ttu-id="716c8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="716c8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="716c8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="716c8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="716c8-121">Hello gyűjteményből WORKS MOBILE hozzáadása</span><span class="sxs-lookup"><span data-stu-id="716c8-121">Adding WORKS MOBILE from hello gallery</span></span>
2. <span data-ttu-id="716c8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="716c8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-works-mobile-from-hello-gallery"></a><span data-ttu-id="716c8-123">Hello gyűjteményből WORKS MOBILE hozzáadása</span><span class="sxs-lookup"><span data-stu-id="716c8-123">Adding WORKS MOBILE from hello gallery</span></span>
<span data-ttu-id="716c8-124">tooconfigure hello integrációs WORKS mobil az Azure AD-be, meg kell tooadd WORKS MOBILE hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="716c8-124">tooconfigure hello integration of WORKS MOBILE into Azure AD, you need tooadd WORKS MOBILE from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="716c8-125">**tooadd WORKS MOBILE hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="716c8-125">**tooadd WORKS MOBILE from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="716c8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="716c8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="716c8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="716c8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="716c8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="716c8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="716c8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="716c8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="716c8-133">Hello keresési mezőbe, írja be a **WORKS MOBILE**.</span><span class="sxs-lookup"><span data-stu-id="716c8-133">In hello search box, type **WORKS MOBILE**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_search.png)

5. <span data-ttu-id="716c8-135">A hello eredmények panelen válassza a **WORKS MOBILE**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="716c8-135">In hello results panel, select **WORKS MOBILE**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="716c8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="716c8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="716c8-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést és a "Britta Simon." nevű tesztfelhasználó alapján WORKS MOBILE</span><span class="sxs-lookup"><span data-stu-id="716c8-138">In this section, you configure and test Azure AD single sign-on with WORKS MOBILE based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="716c8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a WORKS MOBILE tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="716c8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in WORKS MOBILE is tooa user in Azure AD.</span></span> <span data-ttu-id="716c8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello WORKS MOBILE közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="716c8-140">In other words, a link relationship between an Azure AD user and hello related user in WORKS MOBILE needs toobe established.</span></span>

<span data-ttu-id="716c8-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a WORKS MOBILE.</span><span class="sxs-lookup"><span data-stu-id="716c8-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in WORKS MOBILE.</span></span>

<span data-ttu-id="716c8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés WORKS MOBILE-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="716c8-142">tooconfigure and test Azure AD single sign-on with WORKS MOBILE, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="716c8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="716c8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="716c8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="716c8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="716c8-145">**[WORKS MOBILE tesztfelhasználó létrehozása](#creating-a-works-mobile-test-user)**  -toohave egy megfelelője a Britta Simon a WORKS MOBILE, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="716c8-145">**[Creating a WORKS MOBILE test user](#creating-a-works-mobile-test-user)** - toohave a counterpart of Britta Simon in WORKS MOBILE that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="716c8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="716c8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="716c8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="716c8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="716c8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="716c8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="716c8-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az WORKS MOBILE alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="716c8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your WORKS MOBILE application.</span></span>

<span data-ttu-id="716c8-150">**tooconfigure az Azure AD egyszeri bejelentkezést és a WORKS MOBILE, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="716c8-150">**tooconfigure Azure AD single sign-on with WORKS MOBILE, perform hello following steps:**</span></span>

1. <span data-ttu-id="716c8-151">Az Azure portál, a hello hello **WORKS MOBILE** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="716c8-151">In hello Azure portal, on hello **WORKS MOBILE** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="716c8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="716c8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_samlbase.png)

3. <span data-ttu-id="716c8-155">A hello **WORKS MOBILE tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="716c8-155">On hello **WORKS MOBILE Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_url.png)

    <span data-ttu-id="716c8-157">a.</span><span class="sxs-lookup"><span data-stu-id="716c8-157">a.</span></span> <span data-ttu-id="716c8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.worksmobile.com/jp/myservice`</span><span class="sxs-lookup"><span data-stu-id="716c8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.worksmobile.com/jp/myservice`</span></span>

    <span data-ttu-id="716c8-159">b.</span><span class="sxs-lookup"><span data-stu-id="716c8-159">b.</span></span> <span data-ttu-id="716c8-160">A hello **azonosító** szövegmezőhöz hello érték típusa`worksmobile.com`</span><span class="sxs-lookup"><span data-stu-id="716c8-160">In hello **Identifier** textbox, type hello value as `worksmobile.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="716c8-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="716c8-161">This value is not real.</span></span> <span data-ttu-id="716c8-162">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="716c8-162">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="716c8-163">Ügyfél [WORKS MOBILESZKÖZ ügyfél-támogatási csoport](mailto:dl_ssoinfo@worksmobile.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="716c8-163">Contact [WORKS MOBILE Client support team](mailto:dl_ssoinfo@worksmobile.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="716c8-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="716c8-164">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_certificate.png) 

5. <span data-ttu-id="716c8-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="716c8-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="716c8-168">A hello **WORKS MOBILE konfigurációs** kattintson **WORKS MOBILALKALMAZÁS konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="716c8-168">On hello **WORKS MOBILE Configuration** section, click **Configure WORKS MOBILE** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="716c8-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="716c8-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_configure.png) 

7. <span data-ttu-id="716c8-171">az alkalmazáshoz konfigurált SSO tooget, forduljon a [WORKS MOBILE támogatási csoport](mailto:dl_ssoinfo@worksmobile.com) és adja meg a következő információk hello:</span><span class="sxs-lookup"><span data-stu-id="716c8-171">tooget SSO configured for your application, contact [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) and provide them with hello following information:</span></span> 

    <span data-ttu-id="716c8-172">• hello letöltött **tanúsítványfájl**</span><span class="sxs-lookup"><span data-stu-id="716c8-172">• hello downloaded **Certificate file**</span></span>

    <span data-ttu-id="716c8-173">• hello **SAML-alapú egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="716c8-173">• hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="716c8-174">• hello **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="716c8-174">• hello **SAML Entity ID**</span></span>

    <span data-ttu-id="716c8-175">• hello **Sign-Out URL-címe**</span><span class="sxs-lookup"><span data-stu-id="716c8-175">• hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="716c8-176">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="716c8-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="716c8-177">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="716c8-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="716c8-178">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="716c8-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="716c8-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="716c8-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="716c8-180">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="716c8-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="716c8-182">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="716c8-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="716c8-183">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="716c8-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="716c8-185">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="716c8-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="716c8-187">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="716c8-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="716c8-189">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="716c8-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="716c8-191">a.</span><span class="sxs-lookup"><span data-stu-id="716c8-191">a.</span></span> <span data-ttu-id="716c8-192">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="716c8-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="716c8-193">b.</span><span class="sxs-lookup"><span data-stu-id="716c8-193">b.</span></span> <span data-ttu-id="716c8-194">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="716c8-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="716c8-195">c.</span><span class="sxs-lookup"><span data-stu-id="716c8-195">c.</span></span> <span data-ttu-id="716c8-196">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="716c8-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="716c8-197">d.</span><span class="sxs-lookup"><span data-stu-id="716c8-197">d.</span></span> <span data-ttu-id="716c8-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="716c8-198">Click **Create**.</span></span>
 
### <a name="creating-a-works-mobile-test-user"></a><span data-ttu-id="716c8-199">WORKS MOBILE tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="716c8-199">Creating a WORKS MOBILE test user</span></span>

 <span data-ttu-id="716c8-200">Ebben a szakaszban egy felhasználó Britta Simon meghívta WORKS MOBILE hoz létre.</span><span class="sxs-lookup"><span data-stu-id="716c8-200">In this section, you create a user called Britta Simon in WORKS MOBILE.</span></span> <span data-ttu-id="716c8-201">Adjon együttműködve [WORKS MOBILE támogatási csoport](mailto:dl_ssoinfo@worksmobile.com) tooadd hello felhasználók az hello WORKS MOBILESZKÖZ-platformokon.</span><span class="sxs-lookup"><span data-stu-id="716c8-201">Please work with [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) tooadd hello users in hello WORKS MOBILE platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="716c8-202">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="716c8-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="716c8-203">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWORKS MOBILE megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="716c8-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWORKS MOBILE.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="716c8-205">**tooassign Britta Simon tooWORKS MOBILE, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="716c8-205">**tooassign Britta Simon tooWORKS MOBILE, perform hello following steps:**</span></span>

1. <span data-ttu-id="716c8-206">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="716c8-206">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="716c8-208">Hello alkalmazások listában válassza ki a **WORKS MOBILE**.</span><span class="sxs-lookup"><span data-stu-id="716c8-208">In hello applications list, select **WORKS MOBILE**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_app.png) 

3. <span data-ttu-id="716c8-210">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="716c8-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="716c8-212">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="716c8-212">Click **Add** button.</span></span> <span data-ttu-id="716c8-213">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="716c8-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="716c8-215">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="716c8-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="716c8-216">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="716c8-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="716c8-217">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="716c8-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="716c8-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="716c8-218">Testing single sign-on</span></span>

<span data-ttu-id="716c8-219">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="716c8-219">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="716c8-220">Hello WORKS MOBILE csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour WORKS MOBILE alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="716c8-220">When you click hello WORKS MOBILE tile in hello Access Panel, you should get automatically signed-on tooyour WORKS MOBILE application.</span></span>
<span data-ttu-id="716c8-221">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="716c8-221">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="716c8-222">További források</span><span class="sxs-lookup"><span data-stu-id="716c8-222">Additional resources</span></span>

* [<span data-ttu-id="716c8-223">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="716c8-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="716c8-224">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="716c8-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png

