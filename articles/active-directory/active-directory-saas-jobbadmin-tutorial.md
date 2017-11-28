---
title: "Oktatóanyag: Azure Active Directoryval integrált Jobbadmin |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jobbadmin között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c5208b0d-66a3-49ed-9aad-70d21f54aee0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 0796abd2934c0f94648b2c11e7fdf69304f835c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobbadmin"></a><span data-ttu-id="3ae7d-103">Oktatóanyag: Azure Active Directoryval integrált Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="3ae7d-103">Tutorial: Azure Active Directory integration with Jobbadmin</span></span>

<span data-ttu-id="3ae7d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jobbadmin az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3ae7d-104">In this tutorial, you learn how toointegrate Jobbadmin with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3ae7d-105">Jobbadmin integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-105">Integrating Jobbadmin with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3ae7d-106">Megadhatja a hozzáférés tooJobbadmin rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3ae7d-106">You can control in Azure AD who has access tooJobbadmin</span></span>
- <span data-ttu-id="3ae7d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJobbadmin (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3ae7d-107">You can enable your users tooautomatically get signed-on tooJobbadmin (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3ae7d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3ae7d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3ae7d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3ae7d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ae7d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3ae7d-110">Prerequisites</span></span>

<span data-ttu-id="3ae7d-111">az Azure AD integrálása Jobbadmin tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-111">tooconfigure Azure AD integration with Jobbadmin, you need hello following items:</span></span>

- <span data-ttu-id="3ae7d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3ae7d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3ae7d-113">Egy Jobbadmin egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3ae7d-113">A Jobbadmin single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3ae7d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3ae7d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3ae7d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3ae7d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3ae7d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3ae7d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3ae7d-118">Scenario description</span></span>
<span data-ttu-id="3ae7d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3ae7d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3ae7d-121">Hello gyűjteményből Jobbadmin hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3ae7d-121">Adding Jobbadmin from hello gallery</span></span>
2. <span data-ttu-id="3ae7d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3ae7d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobbadmin-from-hello-gallery"></a><span data-ttu-id="3ae7d-123">Hello gyűjteményből Jobbadmin hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3ae7d-123">Adding Jobbadmin from hello gallery</span></span>
<span data-ttu-id="3ae7d-124">tooconfigure hello integrációja Jobbadmin az Azure AD-be, meg kell tooadd Jobbadmin hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-124">tooconfigure hello integration of Jobbadmin into Azure AD, you need tooadd Jobbadmin from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3ae7d-125">**tooadd Jobbadmin hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3ae7d-125">**tooadd Jobbadmin from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae7d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3ae7d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3ae7d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3ae7d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3ae7d-133">Hello keresési mezőbe, írja be a **Jobbadmin**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-133">In hello search box, type **Jobbadmin**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_search.png)

5. <span data-ttu-id="3ae7d-135">A hello eredmények panelen válassza ki a **Jobbadmin**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-135">In hello results panel, select **Jobbadmin**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3ae7d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3ae7d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3ae7d-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="3ae7d-138">In this section, you configure and test Azure AD single sign-on with Jobbadmin based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3ae7d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Jobbadmin tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jobbadmin is tooa user in Azure AD.</span></span> <span data-ttu-id="3ae7d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Jobbadmin közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-140">In other words, a link relationship between an Azure AD user and hello related user in Jobbadmin needs toobe established.</span></span>

<span data-ttu-id="3ae7d-141">Jobbadmin, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-141">In Jobbadmin, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3ae7d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Jobbadmin-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-142">tooconfigure and test Azure AD single sign-on with Jobbadmin, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3ae7d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3ae7d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3ae7d-145">**[Jobbadmin tesztfelhasználó létrehozása](#creating-a-jobbadmin-test-user)**  -toohave egy megfelelője a Britta Simon a Jobbadmin, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-145">**[Creating a Jobbadmin test user](#creating-a-jobbadmin-test-user)** - toohave a counterpart of Britta Simon in Jobbadmin that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3ae7d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3ae7d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3ae7d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3ae7d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3ae7d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Jobbadmin alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jobbadmin application.</span></span>

<span data-ttu-id="3ae7d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Jobbadmin, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3ae7d-150">**tooconfigure Azure AD single sign-on with Jobbadmin, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae7d-151">Az Azure portál, a hello hello **Jobbadmin** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-151">In hello Azure portal, on hello **Jobbadmin** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3ae7d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_samlbase.png)

3. <span data-ttu-id="3ae7d-155">A hello **Jobbadmin tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-155">On hello **Jobbadmin Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_url.png)

    <span data-ttu-id="3ae7d-157">a.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-157">a.</span></span> <span data-ttu-id="3ae7d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span><span class="sxs-lookup"><span data-stu-id="3ae7d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span></span>

    <span data-ttu-id="3ae7d-159">b.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-159">b.</span></span> <span data-ttu-id="3ae7d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.jobnorge.no`</span><span class="sxs-lookup"><span data-stu-id="3ae7d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.jobnorge.no`</span></span>

    <span data-ttu-id="3ae7d-161">c.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-161">c.</span></span> <span data-ttu-id="3ae7d-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span><span class="sxs-lookup"><span data-stu-id="3ae7d-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3ae7d-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-163">These values are not real.</span></span> <span data-ttu-id="3ae7d-164">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-164">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3ae7d-165">Ügyfél [Jobbadmin ügyfél-támogatási csoport](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-165">Contact [Jobbadmin Client support team](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget these values.</span></span> 
 


4. <span data-ttu-id="3ae7d-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_certificate.png) 

5. <span data-ttu-id="3ae7d-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3ae7d-170">tooconfigure egyszeri bejelentkezést a **Jobbadmin** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Jobbadmin támogatási csoport](https://www.jobbnorge.no/om-oss/kontakt-oss).</span><span class="sxs-lookup"><span data-stu-id="3ae7d-170">tooconfigure single sign-on on **Jobbadmin** side, you need toosend hello downloaded **Metadata XML** too[Jobbadmin support team](https://www.jobbnorge.no/om-oss/kontakt-oss).</span></span> <span data-ttu-id="3ae7d-171">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-171">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3ae7d-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="3ae7d-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3ae7d-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3ae7d-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3ae7d-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3ae7d-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ae7d-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="3ae7d-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3ae7d-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="3ae7d-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae7d-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3ae7d-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3ae7d-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3ae7d-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3ae7d-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3ae7d-187">a.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-187">a.</span></span> <span data-ttu-id="3ae7d-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3ae7d-189">b.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-189">b.</span></span> <span data-ttu-id="3ae7d-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3ae7d-191">c.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-191">c.</span></span> <span data-ttu-id="3ae7d-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3ae7d-193">d.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-193">d.</span></span> <span data-ttu-id="3ae7d-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-194">Click **Create**.</span></span>
 
### <a name="creating-a-jobbadmin-test-user"></a><span data-ttu-id="3ae7d-195">Jobbadmin tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ae7d-195">Creating a Jobbadmin test user</span></span>

<span data-ttu-id="3ae7d-196">az Azure AD tooenable felhasználók toolog a tooJobbadmin, akkor ki kell építenie Jobbadmin be.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-196">tooenable Azure AD users toolog in tooJobbadmin, they must be provisioned into Jobbadmin.</span></span>
 
<span data-ttu-id="3ae7d-197">Lépjen kapcsolatba a [Jobbadmin támogatási csoport](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget hello felhasználók felvétele az oldalon.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-197">Please contact [Jobbadmin support team](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget hello users added on their side.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3ae7d-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3ae7d-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3ae7d-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooJobbadmin megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJobbadmin.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3ae7d-201">**tooassign Britta Simon tooJobbadmin, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="3ae7d-201">**tooassign Britta Simon tooJobbadmin, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae7d-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3ae7d-204">Hello alkalmazások listában válassza ki a **Jobbadmin**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-204">In hello applications list, select **Jobbadmin**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_app.png) 

3. <span data-ttu-id="3ae7d-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3ae7d-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-208">Click **Add** button.</span></span> <span data-ttu-id="3ae7d-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3ae7d-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3ae7d-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3ae7d-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3ae7d-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3ae7d-214">Testing single sign-on</span></span>

<span data-ttu-id="3ae7d-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3ae7d-216">Ha a hozzáférési Panel hello hello Jobbadmin csempe gombra kattint, Jobbadmin alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="3ae7d-216">When you click hello Jobbadmin tile in hello Access Panel, you should get login page of Jobbadmin application.</span></span>
<span data-ttu-id="3ae7d-217">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3ae7d-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3ae7d-218">További források</span><span class="sxs-lookup"><span data-stu-id="3ae7d-218">Additional resources</span></span>

* [<span data-ttu-id="3ae7d-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3ae7d-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3ae7d-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3ae7d-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_203.png

