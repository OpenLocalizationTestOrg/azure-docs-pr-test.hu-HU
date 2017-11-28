---
title: "Oktatóanyag: Azure Active Directoryval integrált BenSelect |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BenSelect között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffa17478-3ea1-4356-a289-545b5b9a4494
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c3705da337bf8f6e76de58cd21c5b047c8f5e12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benselect"></a><span data-ttu-id="7874e-103">Oktatóanyag: Azure Active Directoryval integrált BenSelect</span><span class="sxs-lookup"><span data-stu-id="7874e-103">Tutorial: Azure Active Directory integration with BenSelect</span></span>

<span data-ttu-id="7874e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BenSelect az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7874e-104">In this tutorial, you learn how toointegrate BenSelect with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7874e-105">BenSelect integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7874e-105">Integrating BenSelect with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7874e-106">Megadhatja a hozzáférés tooBenSelect rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7874e-106">You can control in Azure AD who has access tooBenSelect</span></span>
- <span data-ttu-id="7874e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBenSelect (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7874e-107">You can enable your users tooautomatically get signed-on tooBenSelect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7874e-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7874e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7874e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7874e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7874e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7874e-110">Prerequisites</span></span>

<span data-ttu-id="7874e-111">az Azure AD integrálása BenSelect tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7874e-111">tooconfigure Azure AD integration with BenSelect, you need hello following items:</span></span>

- <span data-ttu-id="7874e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7874e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7874e-113">Egy BenSelect egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7874e-113">A BenSelect single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7874e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7874e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7874e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7874e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7874e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7874e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7874e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7874e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7874e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7874e-118">Scenario description</span></span>
<span data-ttu-id="7874e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7874e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7874e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7874e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7874e-121">Hello gyűjteményből BenSelect hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7874e-121">Adding BenSelect from hello gallery</span></span>
2. <span data-ttu-id="7874e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7874e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benselect-from-hello-gallery"></a><span data-ttu-id="7874e-123">Hello gyűjteményből BenSelect hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7874e-123">Adding BenSelect from hello gallery</span></span>
<span data-ttu-id="7874e-124">tooconfigure hello integrációja BenSelect az Azure AD-be, meg kell tooadd BenSelect hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7874e-124">tooconfigure hello integration of BenSelect into Azure AD, you need tooadd BenSelect from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7874e-125">**tooadd BenSelect hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7874e-125">**tooadd BenSelect from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7874e-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7874e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7874e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7874e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7874e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7874e-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7874e-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7874e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7874e-133">Hello keresési mezőbe, írja be a **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="7874e-133">In hello search box, type **BenSelect**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_search.png)

5. <span data-ttu-id="7874e-135">A hello eredmények panelen válassza ki a **BenSelect**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7874e-135">In hello results panel, select **BenSelect**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7874e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7874e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7874e-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BenSelect</span><span class="sxs-lookup"><span data-stu-id="7874e-138">In this section, you configure and test Azure AD single sign-on with BenSelect based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7874e-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BenSelect tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7874e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BenSelect is tooa user in Azure AD.</span></span> <span data-ttu-id="7874e-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BenSelect közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7874e-140">In other words, a link relationship between an Azure AD user and hello related user in BenSelect needs toobe established.</span></span>

<span data-ttu-id="7874e-141">BenSelect, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7874e-141">In BenSelect, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7874e-142">tooconfigure és az Azure AD az egyszeri bejelentkezés BenSelect-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7874e-142">tooconfigure and test Azure AD single sign-on with BenSelect, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7874e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7874e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7874e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7874e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7874e-145">**[BenSelect tesztfelhasználó létrehozása](#creating-a-benselect-test-user)**  -toohave egy megfelelője a Britta Simon a BenSelect, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7874e-145">**[Creating a BenSelect test user](#creating-a-benselect-test-user)** - toohave a counterpart of Britta Simon in BenSelect that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7874e-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7874e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7874e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7874e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7874e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7874e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7874e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BenSelect alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7874e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BenSelect application.</span></span>

<span data-ttu-id="7874e-150">**az Azure AD tooconfigure egyszeri bejelentkezést a BenSelect, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7874e-150">**tooconfigure Azure AD single sign-on with BenSelect, perform hello following steps:**</span></span>

1. <span data-ttu-id="7874e-151">Az Azure portál, a hello hello **BenSelect** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7874e-151">In hello Azure portal, on hello **BenSelect** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7874e-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7874e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_samlbase.png)

3. <span data-ttu-id="7874e-155">A hello **BenSelect tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7874e-155">On hello **BenSelect Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_url.png)

    <span data-ttu-id="7874e-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="7874e-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7874e-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="7874e-158">This value is not real.</span></span> <span data-ttu-id="7874e-159">Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="7874e-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="7874e-160">Ügyfél [BenSelect támogatási csoport](mailto:support@selerix.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="7874e-160">Contact [BenSelect support team](mailto:support@selerix.com) tooget this value.</span></span>
 
4. <span data-ttu-id="7874e-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7874e-161">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_certificate.png) 

5. <span data-ttu-id="7874e-163">BenSelect alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="7874e-163">BenSelect application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="7874e-164">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7874e-164">Configure hello following claims for this application.</span></span> <span data-ttu-id="7874e-165">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="7874e-165">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="7874e-166">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="7874e-166">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

6. <span data-ttu-id="7874e-168">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="7874e-168">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="7874e-169">a.</span><span class="sxs-lookup"><span data-stu-id="7874e-169">a.</span></span> <span data-ttu-id="7874e-170">A hello **felhasználói azonosító** legördülő listában válassza ki **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="7874e-170">In hello **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="7874e-171">b.</span><span class="sxs-lookup"><span data-stu-id="7874e-171">b.</span></span> <span data-ttu-id="7874e-172">A hello **Mail** legördülő listában válassza ki **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="7874e-172">In hello **Mail** dropdown list, select **user.userprincipalname**.</span></span>

7. <span data-ttu-id="7874e-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7874e-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7874e-175">A hello **BenSelect konfigurációs** kattintson **konfigurálása BenSelect** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7874e-175">On hello **BenSelect Configuration** section, click **Configure BenSelect** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7874e-176">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7874e-176">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_configure.png) 

9. <span data-ttu-id="7874e-178">tooconfigure egyszeri bejelentkezést a **BenSelect** oldalon kell letöltött toosend hello **Certificate(Raw)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**túl[BenSelect támogatási csoport](mailto:support@selerix.com).</span><span class="sxs-lookup"><span data-stu-id="7874e-178">tooconfigure single sign-on on **BenSelect** side, you need toosend hello downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[BenSelect support team](mailto:support@selerix.com).</span></span>

   >[!NOTE]
   ><span data-ttu-id="7874e-179">Van szüksége, amely ezt az integrációt igényel (SHA1 nem támogatott) hello SHA-256 algoritmus toomention tooset hello SSO például app2101 hello megfelelő kiszolgálón stb.</span><span class="sxs-lookup"><span data-stu-id="7874e-179">You need toomention that this integration requires hello SHA256 algorithm (SHA1 is not supported) tooset hello SSO on hello appropriate server like app2101 etc.</span></span> 
   
> [!TIP]
> <span data-ttu-id="7874e-180">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7874e-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7874e-181">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7874e-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7874e-182">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7874e-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7874e-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7874e-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="7874e-184">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7874e-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7874e-186">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7874e-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7874e-187">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7874e-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7874e-189">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7874e-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7874e-191">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7874e-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7874e-193">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7874e-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7874e-195">a.</span><span class="sxs-lookup"><span data-stu-id="7874e-195">a.</span></span> <span data-ttu-id="7874e-196">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7874e-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7874e-197">b.</span><span class="sxs-lookup"><span data-stu-id="7874e-197">b.</span></span> <span data-ttu-id="7874e-198">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7874e-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7874e-199">c.</span><span class="sxs-lookup"><span data-stu-id="7874e-199">c.</span></span> <span data-ttu-id="7874e-200">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7874e-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7874e-201">d.</span><span class="sxs-lookup"><span data-stu-id="7874e-201">d.</span></span> <span data-ttu-id="7874e-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7874e-202">Click **Create**.</span></span>
 
### <a name="creating-a-benselect-test-user"></a><span data-ttu-id="7874e-203">BenSelect tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7874e-203">Creating a BenSelect test user</span></span>

<span data-ttu-id="7874e-204">hello ebben a szakaszban célja toocreate BenSelect Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7874e-204">hello objective of this section is toocreate a user called Britta Simon in BenSelect.</span></span> <span data-ttu-id="7874e-205">Együttműködve [BenSelect támogatási csoport](mailto:support@selerix.com) tooadd hello felhasználók hello BenSelect fiók.</span><span class="sxs-lookup"><span data-stu-id="7874e-205">Work with [BenSelect support team](mailto:support@selerix.com) tooadd hello users in hello BenSelect account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7874e-206">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7874e-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7874e-207">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBenSelect megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7874e-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBenSelect.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7874e-209">**tooassign Britta Simon tooBenSelect, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7874e-209">**tooassign Britta Simon tooBenSelect, perform hello following steps:**</span></span>

1. <span data-ttu-id="7874e-210">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7874e-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7874e-212">Hello alkalmazások listában válassza ki a **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="7874e-212">In hello applications list, select **BenSelect**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_app.png) 

3. <span data-ttu-id="7874e-214">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7874e-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7874e-216">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7874e-216">Click **Add** button.</span></span> <span data-ttu-id="7874e-217">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7874e-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7874e-219">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7874e-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7874e-220">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7874e-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7874e-221">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7874e-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7874e-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7874e-222">Testing single sign-on</span></span>

<span data-ttu-id="7874e-223">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="7874e-223">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="7874e-224">Ha a hozzáférési Panel hello hello BenSelect csempe gombra kattint, automatikusan bejelentkezett tooyour BenSelect alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7874e-224">When you click hello BenSelect tile in hello Access Panel, you should get automatically signed-on tooyour BenSelect application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7874e-225">További források</span><span class="sxs-lookup"><span data-stu-id="7874e-225">Additional resources</span></span>

* [<span data-ttu-id="7874e-226">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7874e-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7874e-227">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7874e-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png

