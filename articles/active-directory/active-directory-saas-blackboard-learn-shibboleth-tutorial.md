---
title: "Oktatóanyag: Azure Active Directoryval integrált Antik további - Shibboleth |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a további Antik - Shibboleth között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e435cbb4-c0f0-400e-943c-5c923fa8ddf2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 40aa3ec5f42b93157af3c56daaadfa66203b21d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a><span data-ttu-id="52e9d-103">Oktatóanyag: Azure Active Directoryval integrált Antik további - Shibboleth</span><span class="sxs-lookup"><span data-stu-id="52e9d-103">Tutorial: Azure Active Directory integration with Blackboard Learn - Shibboleth</span></span>

<span data-ttu-id="52e9d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Antik további - Shibboleth az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="52e9d-104">In this tutorial, you learn how toointegrate Blackboard Learn - Shibboleth with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52e9d-105">További tudnivalók a Antik - Shibboleth az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="52e9d-105">Integrating Blackboard Learn - Shibboleth with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="52e9d-106">Megadhatja a hozzáférés tooBlackboard további - Shibboleth rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="52e9d-106">You can control in Azure AD who has access tooBlackboard Learn - Shibboleth</span></span>
- <span data-ttu-id="52e9d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBlackboard további - Shibboleth (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="52e9d-107">You can enable your users tooautomatically get signed-on tooBlackboard Learn - Shibboleth (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="52e9d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="52e9d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="52e9d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52e9d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52e9d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="52e9d-110">Prerequisites</span></span>

<span data-ttu-id="52e9d-111">a következő elemek hello kell tooconfigure Antik további - Shibboleth, az Azure AD integrálása:</span><span class="sxs-lookup"><span data-stu-id="52e9d-111">tooconfigure Azure AD integration with Blackboard Learn - Shibboleth, you need hello following items:</span></span>

- <span data-ttu-id="52e9d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="52e9d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="52e9d-113">A Antik további - Shibboleth egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="52e9d-113">A Blackboard Learn - Shibboleth single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="52e9d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="52e9d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="52e9d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="52e9d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="52e9d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="52e9d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="52e9d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52e9d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="52e9d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="52e9d-118">Scenario description</span></span>
<span data-ttu-id="52e9d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="52e9d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52e9d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="52e9d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="52e9d-121">Hozzáadás Antik további - Shibboleth hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="52e9d-121">Adding Blackboard Learn - Shibboleth from hello gallery</span></span>
2. <span data-ttu-id="52e9d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="52e9d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn---shibboleth-from-hello-gallery"></a><span data-ttu-id="52e9d-123">Hozzáadás Antik további - Shibboleth hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="52e9d-123">Adding Blackboard Learn - Shibboleth from hello gallery</span></span>
<span data-ttu-id="52e9d-124">tooconfigure hello integrációja Antik további - Shibboleth az Azure AD-be kell tooadd Antik további - Shibboleth hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="52e9d-124">tooconfigure hello integration of Blackboard Learn - Shibboleth into Azure AD, you need tooadd Blackboard Learn - Shibboleth from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="52e9d-125">**tooadd Antik további - Shibboleth hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="52e9d-125">**tooadd Blackboard Learn - Shibboleth from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="52e9d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="52e9d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="52e9d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="52e9d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="52e9d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="52e9d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="52e9d-133">Hello keresési mezőbe, írja be a **Antik további - Shibboleth**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-133">In hello search box, type **Blackboard Learn - Shibboleth**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_search.png)

5. <span data-ttu-id="52e9d-135">A hello eredmények panelen válassza ki a **Antik további - Shibboleth**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="52e9d-135">In hello results panel, select **Blackboard Learn - Shibboleth**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="52e9d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="52e9d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="52e9d-138">Ebben a szakaszban az Azure AD egyszeri bejelentkezést a Antik megtudhatja, tesztelése és konfigurálása – Shibboleth "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="52e9d-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn - Shibboleth based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="52e9d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow Antik megtudhatja, milyen hello megfelelőjére felhasználó - Shibboleth tooa felhasználói Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="52e9d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Blackboard Learn - Shibboleth is tooa user in Azure AD.</span></span> <span data-ttu-id="52e9d-140">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználói hello a Antik további - hivatkozás kapcsolatának Shibboleth kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="52e9d-140">In other words, a link relationship between an Azure AD user and hello related user in Blackboard Learn - Shibboleth needs toobe established.</span></span>

<span data-ttu-id="52e9d-141">Antik további - Shibboleth, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="52e9d-141">In Blackboard Learn - Shibboleth, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="52e9d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés teszthez Antik további - Shibboleth, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="52e9d-142">tooconfigure and test Azure AD single sign-on with Blackboard Learn - Shibboleth, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="52e9d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="52e9d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="52e9d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52e9d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52e9d-145">**[Egy Antik további - Shibboleth tesztfelhasználó létrehozása](#creating-a-blackboard-learn---shibboleth-test-user)**  - toohave egy megfelelője a Britta Simon Antik ismerje meg a - Shibboleth, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="52e9d-145">**[Creating a Blackboard Learn - Shibboleth test user](#creating-a-blackboard-learn---shibboleth-test-user)** - toohave a counterpart of Britta Simon in Blackboard Learn - Shibboleth that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="52e9d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="52e9d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52e9d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="52e9d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="52e9d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="52e9d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="52e9d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Antik további - Shibboleth alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="52e9d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Blackboard Learn - Shibboleth application.</span></span>

<span data-ttu-id="52e9d-150">**az Azure AD tooconfigure egyszeri bejelentkezést további Antik - Shibboleth, a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="52e9d-150">**tooconfigure Azure AD single sign-on with Blackboard Learn - Shibboleth, perform hello following steps:**</span></span>

1. <span data-ttu-id="52e9d-151">Az Azure portál, a hello hello **Antik további - Shibboleth** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-151">In hello Azure portal, on hello **Blackboard Learn - Shibboleth** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="52e9d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="52e9d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_samlbase.png)

3. <span data-ttu-id="52e9d-155">A hello **Antik további - Shibboleth tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="52e9d-155">On hello **Blackboard Learn - Shibboleth Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_url.png)

    <span data-ttu-id="52e9d-157">a.</span><span class="sxs-lookup"><span data-stu-id="52e9d-157">a.</span></span> <span data-ttu-id="52e9d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`</span><span class="sxs-lookup"><span data-stu-id="52e9d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`</span></span>

    <span data-ttu-id="52e9d-159">b.</span><span class="sxs-lookup"><span data-stu-id="52e9d-159">b.</span></span> <span data-ttu-id="52e9d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`</span><span class="sxs-lookup"><span data-stu-id="52e9d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`</span></span>

    <span data-ttu-id="52e9d-161">c.</span><span class="sxs-lookup"><span data-stu-id="52e9d-161">c.</span></span> <span data-ttu-id="52e9d-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`</span><span class="sxs-lookup"><span data-stu-id="52e9d-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="52e9d-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="52e9d-163">These values are not real.</span></span> <span data-ttu-id="52e9d-164">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="52e9d-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="52e9d-165">Ügyfél [Antik további - Shibboleth ügyfél-támogatási csoport](https://www.blackboard.com/forms/contact-us_form.aspx) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="52e9d-165">Contact [Blackboard Learn - Shibboleth Client support team](https://www.blackboard.com/forms/contact-us_form.aspx) tooget these values.</span></span> 

4. <span data-ttu-id="52e9d-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="52e9d-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_certificate.png) 

5. <span data-ttu-id="52e9d-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="52e9d-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="52e9d-170">A hello **Antik további - Shibboleth konfigurációs** kattintson **konfigurálása Antik további - Shibboleth** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="52e9d-170">On hello **Blackboard Learn - Shibboleth Configuration** section, click **Configure Blackboard Learn - Shibboleth** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="52e9d-171">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="52e9d-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_configure.png) 

7. <span data-ttu-id="52e9d-173">tooconfigure egyszeri bejelentkezést a **Antik további - Shibboleth** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím** túl[Antik további - Shibboleth támogatási csoport](https://www.blackboard.com/forms/contact-us_form.aspx).</span><span class="sxs-lookup"><span data-stu-id="52e9d-173">tooconfigure single sign-on on **Blackboard Learn - Shibboleth** side, you need toosend hello downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Blackboard Learn - Shibboleth support team](https://www.blackboard.com/forms/contact-us_form.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="52e9d-174">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="52e9d-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="52e9d-175">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="52e9d-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="52e9d-176">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="52e9d-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="52e9d-177">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="52e9d-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="52e9d-178">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="52e9d-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="52e9d-180">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="52e9d-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="52e9d-181">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="52e9d-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="52e9d-183">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="52e9d-185">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="52e9d-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="52e9d-187">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="52e9d-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="52e9d-189">a.</span><span class="sxs-lookup"><span data-stu-id="52e9d-189">a.</span></span> <span data-ttu-id="52e9d-190">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52e9d-191">b.</span><span class="sxs-lookup"><span data-stu-id="52e9d-191">b.</span></span> <span data-ttu-id="52e9d-192">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="52e9d-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="52e9d-193">c.</span><span class="sxs-lookup"><span data-stu-id="52e9d-193">c.</span></span> <span data-ttu-id="52e9d-194">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="52e9d-195">d.</span><span class="sxs-lookup"><span data-stu-id="52e9d-195">d.</span></span> <span data-ttu-id="52e9d-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="52e9d-196">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn---shibboleth-test-user"></a><span data-ttu-id="52e9d-197">Egy Antik további - Shibboleth tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="52e9d-197">Creating a Blackboard Learn - Shibboleth test user</span></span>

<span data-ttu-id="52e9d-198">Ebben a szakaszban egy felhasználó Britta Simon meghívta Antik további - Shibboleth hoz létre.</span><span class="sxs-lookup"><span data-stu-id="52e9d-198">In this section, you create a user called Britta Simon in Blackboard Learn - Shibboleth.</span></span> <span data-ttu-id="52e9d-199">Együttműködik a [Antik további - Shibboleth támogatási csoport](https://www.blackboard.com/forms/contact-us_form.aspx) tooadd hello felhasználók hello Antik további - Shibboleth platform.</span><span class="sxs-lookup"><span data-stu-id="52e9d-199">Work with your [Blackboard Learn - Shibboleth support team](https://www.blackboard.com/forms/contact-us_form.aspx) tooadd hello users in hello Blackboard Learn - Shibboleth platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="52e9d-200">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="52e9d-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="52e9d-201">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBlackboard további - Shibboleth megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="52e9d-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlackboard Learn - Shibboleth.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="52e9d-203">**tooassign Britta Simon tooBlackboard további - Shibboleth, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="52e9d-203">**tooassign Britta Simon tooBlackboard Learn - Shibboleth, perform hello following steps:**</span></span>

1. <span data-ttu-id="52e9d-204">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="52e9d-206">Hello alkalmazások listában válassza ki a **Antik további - Shibboleth**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-206">In hello applications list, select **Blackboard Learn - Shibboleth**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_app.png) 

3. <span data-ttu-id="52e9d-208">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="52e9d-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="52e9d-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="52e9d-210">Click **Add** button.</span></span> <span data-ttu-id="52e9d-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="52e9d-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="52e9d-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="52e9d-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="52e9d-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="52e9d-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="52e9d-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="52e9d-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="52e9d-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="52e9d-216">Testing single sign-on</span></span>

<span data-ttu-id="52e9d-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="52e9d-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="52e9d-218">Hello Antik további - Shibboleth csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Antik további - Shibboleth alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="52e9d-218">When you click hello Blackboard Learn - Shibboleth tile in hello Access Panel, you should get automatically signed-on tooyour Blackboard Learn - Shibboleth application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52e9d-219">További források</span><span class="sxs-lookup"><span data-stu-id="52e9d-219">Additional resources</span></span>

* [<span data-ttu-id="52e9d-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="52e9d-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52e9d-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="52e9d-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_203.png

