---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező személyek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és személyek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: d540c31867c92c4dc09db9c0833f8a8a7c02b371
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-people"></a><span data-ttu-id="4a940-103">Oktatóanyag: Azure Active Directory-integrációval rendelkező személyek</span><span class="sxs-lookup"><span data-stu-id="4a940-103">Tutorial: Azure Active Directory integration with People</span></span>

<span data-ttu-id="4a940-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate személyek az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4a940-104">In this tutorial, you learn how toointegrate People with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a940-105">Személyek integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4a940-105">Integrating People with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4a940-106">Megadhatja a hozzáférés tooPeople rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4a940-106">You can control in Azure AD who has access tooPeople</span></span>
- <span data-ttu-id="4a940-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPeople (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4a940-107">You can enable your users tooautomatically get signed-on tooPeople (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4a940-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4a940-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4a940-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a940-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a940-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4a940-110">Prerequisites</span></span>

<span data-ttu-id="4a940-111">tooconfigure az Azure AD-integrációs személyekkel, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4a940-111">tooconfigure Azure AD integration with People, you need hello following items:</span></span>

- <span data-ttu-id="4a940-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4a940-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a940-113">A felhasználók az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4a940-113">A People single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4a940-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4a940-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4a940-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4a940-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4a940-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4a940-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4a940-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a940-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a940-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4a940-118">Scenario description</span></span>
<span data-ttu-id="4a940-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4a940-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4a940-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4a940-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a940-121">Felhasználók hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4a940-121">Adding People from hello gallery</span></span>
2. <span data-ttu-id="4a940-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a940-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-people-from-hello-gallery"></a><span data-ttu-id="4a940-123">Felhasználók hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4a940-123">Adding People from hello gallery</span></span>
<span data-ttu-id="4a940-124">tooconfigure hello integrációs személyek, az Azure AD-be, meg kell tooadd személyek hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4a940-124">tooconfigure hello integration of People into Azure AD, you need tooadd People from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4a940-125">**tooadd személyek hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a940-125">**tooadd People from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a940-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a940-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4a940-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4a940-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4a940-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a940-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4a940-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4a940-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4a940-133">Hello keresési mezőbe, írja be a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="4a940-133">In hello search box, type **People**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/tutorial_people_search.png)

5. <span data-ttu-id="4a940-135">A hello eredmények panelen válassza ki a **személyek**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4a940-135">In hello results panel, select **People**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/tutorial_people_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4a940-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a940-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4a940-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="4a940-138">In this section, you configure and test Azure AD single sign-on with People based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4a940-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó személyek tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4a940-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in People is tooa user in Azure AD.</span></span> <span data-ttu-id="4a940-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello személyek közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4a940-140">In other words, a link relationship between an Azure AD user and hello related user in People needs toobe established.</span></span>

<span data-ttu-id="4a940-141">Személyek, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4a940-141">In People, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4a940-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése személyekkel, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4a940-142">tooconfigure and test Azure AD single sign-on with People, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4a940-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4a940-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4a940-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a940-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4a940-145">**[Személyek tesztfelhasználó létrehozása](#creating-a-people-test-user)**  -toohave Britta Simon a csatolt toohello az Azure AD felhasználói ábrázolása személynek egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="4a940-145">**[Creating a People test user](#creating-a-people-test-user)** - toohave a counterpart of Britta Simon in People that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4a940-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4a940-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a940-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4a940-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4a940-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a940-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4a940-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és személyek alkalmazásában egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="4a940-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your People application.</span></span>

<span data-ttu-id="4a940-150">**az Azure AD tooconfigure egyszeri bejelentkezést olyan személyekkel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a940-150">**tooconfigure Azure AD single sign-on with People, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a940-151">Az Azure portál, a hello hello **személyek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4a940-151">In hello Azure portal, on hello **People** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4a940-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4a940-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_samlbase.png)

3. <span data-ttu-id="4a940-155">A hello **személyek tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a940-155">On hello **People Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_url.png)

    <span data-ttu-id="4a940-157">a.</span><span class="sxs-lookup"><span data-stu-id="4a940-157">a.</span></span> <span data-ttu-id="4a940-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.peoplehr.com/`</span><span class="sxs-lookup"><span data-stu-id="4a940-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.peoplehr.com/`</span></span>

    <span data-ttu-id="4a940-159">b.</span><span class="sxs-lookup"><span data-stu-id="4a940-159">b.</span></span> <span data-ttu-id="4a940-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.peoplehr.com`</span><span class="sxs-lookup"><span data-stu-id="4a940-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.peoplehr.com`</span></span>

    <span data-ttu-id="4a940-161">c.</span><span class="sxs-lookup"><span data-stu-id="4a940-161">c.</span></span> <span data-ttu-id="4a940-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span><span class="sxs-lookup"><span data-stu-id="4a940-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4a940-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4a940-163">These values are not real.</span></span> <span data-ttu-id="4a940-164">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="4a940-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="4a940-165">Ügyfél [személyek ügyfél-támogatási csoport](mailto:customerservices@peoplehr.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4a940-165">Contact [People Client support team](mailto:customerservices@peoplehr.com) tooget these values.</span></span>

5. <span data-ttu-id="4a940-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4a940-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_certificate.png) 

6. <span data-ttu-id="4a940-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a940-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="4a940-170">tooget az alkalmazáshoz konfigurált SSO, toosign tooyour személyek bérlői rendszergazdaként kell.</span><span class="sxs-lookup"><span data-stu-id="4a940-170">tooget SSO configured for your application, you need toosign-on tooyour People tenant as an administrator.</span></span>
   
8. <span data-ttu-id="4a940-171">Hello hello bal oldali menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4a940-171">In hello menu on hello left side, click **Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_001.png)

9. <span data-ttu-id="4a940-173">Kattintson a **vállalati**.</span><span class="sxs-lookup"><span data-stu-id="4a940-173">Click **Company**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_002.png)

10. <span data-ttu-id="4a940-175">A hello **feltöltés "Egyszeri bejelentkezés" SAML metaadatok fájl**, kattintson a **Tallózás** tooupload hello letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="4a940-175">On hello **Upload 'Single Sign On' SAML meta-data file**, click **Browse** tooupload hello downloaded metadata file.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)

> [!TIP]
> <span data-ttu-id="4a940-177">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4a940-177">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4a940-178">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4a940-178">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4a940-179">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4a940-179">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4a940-180">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a940-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="4a940-181">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4a940-181">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4a940-183">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4a940-183">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a940-184">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a940-184">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4a940-186">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4a940-186">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4a940-188">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a940-188">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4a940-190">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a940-190">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4a940-192">a.</span><span class="sxs-lookup"><span data-stu-id="4a940-192">a.</span></span> <span data-ttu-id="4a940-193">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a940-193">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4a940-194">b.</span><span class="sxs-lookup"><span data-stu-id="4a940-194">b.</span></span> <span data-ttu-id="4a940-195">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4a940-195">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4a940-196">c.</span><span class="sxs-lookup"><span data-stu-id="4a940-196">c.</span></span> <span data-ttu-id="4a940-197">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4a940-197">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4a940-198">d.</span><span class="sxs-lookup"><span data-stu-id="4a940-198">d.</span></span> <span data-ttu-id="4a940-199">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4a940-199">Click **Create**.</span></span>
 
### <a name="creating-a-people-test-user"></a><span data-ttu-id="4a940-200">Személyek tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a940-200">Creating a People test user</span></span>

<span data-ttu-id="4a940-201">Ebben a szakaszban egy Britta Simon személyek nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4a940-201">In this section, you create a user called Britta Simon in People.</span></span> <span data-ttu-id="4a940-202">Együttműködve [személyek ügyfél-támogatási csoport](mailto:customerservices@peoplehr.com) felhasználót is hozzáadhat hello hello személyek platform.</span><span class="sxs-lookup"><span data-stu-id="4a940-202">Work with [People Client support team](mailto:customerservices@peoplehr.com) to add hello users in hello People platform.</span></span> <span data-ttu-id="4a940-203">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="4a940-203">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4a940-204">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4a940-204">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4a940-205">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPeople megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4a940-205">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPeople.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4a940-207">**tooassign Britta Simon tooPeople, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4a940-207">**tooassign Britta Simon tooPeople, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a940-208">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a940-208">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4a940-210">Hello alkalmazások listában válassza ki a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="4a940-210">In hello applications list, select **People**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_app.png) 

3. <span data-ttu-id="4a940-212">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4a940-212">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4a940-214">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a940-214">Click **Add** button.</span></span> <span data-ttu-id="4a940-215">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a940-215">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4a940-217">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4a940-217">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4a940-218">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a940-218">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4a940-219">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a940-219">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4a940-220">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4a940-220">Testing single sign-on</span></span>

<span data-ttu-id="4a940-221">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="4a940-221">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="4a940-222">Hello személyek hozzáférési Panel hello csempére kattintva kapja meg automatikusan bejelentkezett tooyour személyek alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4a940-222">When you click hello People tile in hello Access Panel, you should get automatically signed-on tooyour People application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a940-223">További források</span><span class="sxs-lookup"><span data-stu-id="4a940-223">Additional resources</span></span>

* [<span data-ttu-id="4a940-224">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4a940-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a940-225">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4a940-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-people-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png

