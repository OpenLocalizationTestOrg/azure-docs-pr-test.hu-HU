---
title: "Oktatóanyag: Azure Active Directoryval integrált Trakopolis |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Trakopolis között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 73d67c3e-4b4b-4d3b-aa58-6699ea1ccea3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 00f9b21c0a837d1d9fbbd9135367fae4b02db934
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trakopolis"></a><span data-ttu-id="5e915-103">Oktatóanyag: Azure Active Directoryval integrált Trakopolis</span><span class="sxs-lookup"><span data-stu-id="5e915-103">Tutorial: Azure Active Directory integration with Trakopolis</span></span>

<span data-ttu-id="5e915-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Trakopolis az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5e915-104">In this tutorial, you learn how toointegrate Trakopolis with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5e915-105">Trakopolis integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5e915-105">Integrating Trakopolis with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5e915-106">Megadhatja a hozzáférés tooTrakopolis rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5e915-106">You can control in Azure AD who has access tooTrakopolis</span></span>
- <span data-ttu-id="5e915-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTrakopolis (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5e915-107">You can enable your users tooautomatically get signed-on tooTrakopolis (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5e915-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5e915-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5e915-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5e915-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e915-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5e915-110">Prerequisites</span></span>

<span data-ttu-id="5e915-111">az Azure AD integrálása Trakopolis tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="5e915-111">tooconfigure Azure AD integration with Trakopolis, you need hello following items:</span></span>

- <span data-ttu-id="5e915-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5e915-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5e915-113">Egy Trakopolis egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5e915-113">A Trakopolis single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5e915-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5e915-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5e915-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5e915-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5e915-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5e915-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5e915-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e915-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5e915-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5e915-118">Scenario description</span></span>
<span data-ttu-id="5e915-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5e915-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5e915-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5e915-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5e915-121">Hello gyűjteményből Trakopolis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5e915-121">Adding Trakopolis from hello gallery</span></span>
2. <span data-ttu-id="5e915-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5e915-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trakopolis-from-hello-gallery"></a><span data-ttu-id="5e915-123">Hello gyűjteményből Trakopolis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5e915-123">Adding Trakopolis from hello gallery</span></span>
<span data-ttu-id="5e915-124">tooconfigure hello integrációja Trakopolis az Azure AD-be, meg kell tooadd Trakopolis hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5e915-124">tooconfigure hello integration of Trakopolis into Azure AD, you need tooadd Trakopolis from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5e915-125">**tooadd Trakopolis hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5e915-125">**tooadd Trakopolis from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e915-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5e915-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5e915-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5e915-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5e915-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5e915-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5e915-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5e915-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5e915-133">Hello keresési mezőbe, írja be a **Trakopolis**.</span><span class="sxs-lookup"><span data-stu-id="5e915-133">In hello search box, type **Trakopolis**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_search.png)

5. <span data-ttu-id="5e915-135">A hello eredmények panelen válassza ki a **Trakopolis**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5e915-135">In hello results panel, select **Trakopolis**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5e915-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5e915-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5e915-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Trakopolis.</span><span class="sxs-lookup"><span data-stu-id="5e915-138">In this section, you configure and test Azure AD single sign-on with Trakopolis based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5e915-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Trakopolis tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5e915-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Trakopolis is tooa user in Azure AD.</span></span> <span data-ttu-id="5e915-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Trakopolis közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5e915-140">In other words, a link relationship between an Azure AD user and hello related user in Trakopolis needs toobe established.</span></span>

<span data-ttu-id="5e915-141">Trakopolis, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5e915-141">In Trakopolis, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5e915-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Trakopolis-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5e915-142">tooconfigure and test Azure AD single sign-on with Trakopolis, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5e915-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5e915-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5e915-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e915-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5e915-145">**[Trakopolis tesztfelhasználó létrehozása](#creating-a-trakopolis-test-user)**  -toohave egy megfelelője a Britta Simon a Trakopolis, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5e915-145">**[Creating a Trakopolis test user](#creating-a-trakopolis-test-user)** - toohave a counterpart of Britta Simon in Trakopolis that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5e915-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5e915-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5e915-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5e915-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5e915-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e915-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5e915-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Trakopolis alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5e915-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Trakopolis application.</span></span>

<span data-ttu-id="5e915-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Trakopolis, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5e915-150">**tooconfigure Azure AD single sign-on with Trakopolis, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e915-151">Az Azure portál, a hello hello **Trakopolis** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5e915-151">In hello Azure portal, on hello **Trakopolis** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5e915-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5e915-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_samlbase.png)

3. <span data-ttu-id="5e915-155">A hello **Trakopolis tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5e915-155">On hello **Trakopolis Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_url.png)

    <span data-ttu-id="5e915-157">a.</span><span class="sxs-lookup"><span data-stu-id="5e915-157">a.</span></span> <span data-ttu-id="5e915-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.trakopolis.com/`</span><span class="sxs-lookup"><span data-stu-id="5e915-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.trakopolis.com/`</span></span>

    <span data-ttu-id="5e915-159">b.</span><span class="sxs-lookup"><span data-stu-id="5e915-159">b.</span></span> <span data-ttu-id="5e915-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.trakopolis.com`</span><span class="sxs-lookup"><span data-stu-id="5e915-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.trakopolis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5e915-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="5e915-161">These values are not real.</span></span> <span data-ttu-id="5e915-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="5e915-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5e915-163">Ügyfél [Trakopolis ügyfél-támogatási csoport](mailto:support@cantelematics.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5e915-163">Contact [Trakopolis Client support team](mailto:support@cantelematics.com) tooget these values.</span></span> 

4. <span data-ttu-id="5e915-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5e915-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_certificate.png) 

5. <span data-ttu-id="5e915-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e915-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakopolis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5e915-168">A hello **Trakopolis konfigurációs** kattintson **konfigurálása Trakopolis** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5e915-168">On hello **Trakopolis Configuration** section, click **Configure Trakopolis** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5e915-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5e915-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_configure.png) 

7. <span data-ttu-id="5e915-171">tooconfigure egyszeri bejelentkezést a **Trakopolis** oldalon kell letöltött toosend hello **metaadatainak XML-kódja, Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** túl[Trakopolis támogatási csoport](mailto:support@cantelematics.com).</span><span class="sxs-lookup"><span data-stu-id="5e915-171">tooconfigure single sign-on on **Trakopolis** side, you need toosend hello downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Trakopolis support team](mailto:support@cantelematics.com).</span></span> <span data-ttu-id="5e915-172">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="5e915-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5e915-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5e915-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5e915-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5e915-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5e915-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5e915-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5e915-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e915-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="5e915-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5e915-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5e915-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5e915-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e915-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5e915-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5e915-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5e915-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5e915-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e915-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5e915-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5e915-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5e915-188">a.</span><span class="sxs-lookup"><span data-stu-id="5e915-188">a.</span></span> <span data-ttu-id="5e915-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5e915-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5e915-190">b.</span><span class="sxs-lookup"><span data-stu-id="5e915-190">b.</span></span> <span data-ttu-id="5e915-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5e915-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5e915-192">c.</span><span class="sxs-lookup"><span data-stu-id="5e915-192">c.</span></span> <span data-ttu-id="5e915-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5e915-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5e915-194">d.</span><span class="sxs-lookup"><span data-stu-id="5e915-194">d.</span></span> <span data-ttu-id="5e915-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5e915-195">Click **Create**.</span></span>
 
### <a name="creating-a-trakopolis-test-user"></a><span data-ttu-id="5e915-196">Trakopolis tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e915-196">Creating a Trakopolis test user</span></span>

<span data-ttu-id="5e915-197">Ebben a szakaszban egy Trakopolis Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5e915-197">In this section, you create a user called Britta Simon in Trakopolis.</span></span> <span data-ttu-id="5e915-198">Együttműködve [Trakopolis támogatási csoport](mailto:support@cantelematics.com) felhasználót is hozzáadhat hello hello Trakopolis platform.</span><span class="sxs-lookup"><span data-stu-id="5e915-198">Work with [Trakopolis support team](mailto:support@cantelematics.com) to add hello users in hello Trakopolis platform.</span></span> <span data-ttu-id="5e915-199">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="5e915-199">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5e915-200">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5e915-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5e915-201">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTrakopolis megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5e915-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTrakopolis.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5e915-203">**tooassign Britta Simon tooTrakopolis, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5e915-203">**tooassign Britta Simon tooTrakopolis, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e915-204">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5e915-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5e915-206">Hello alkalmazások listában válassza ki a **Trakopolis**.</span><span class="sxs-lookup"><span data-stu-id="5e915-206">In hello applications list, select **Trakopolis**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_app.png) 

3. <span data-ttu-id="5e915-208">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5e915-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5e915-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e915-210">Click **Add** button.</span></span> <span data-ttu-id="5e915-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e915-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5e915-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5e915-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5e915-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e915-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5e915-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e915-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5e915-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5e915-216">Testing single sign-on</span></span>

<span data-ttu-id="5e915-217">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="5e915-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="5e915-218">Hello Trakopolis hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Trakopolis alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5e915-218">When you click hello Trakopolis tile in hello Access Panel, you should get automatically signed-on tooyour Trakopolis application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e915-219">További források</span><span class="sxs-lookup"><span data-stu-id="5e915-219">Additional resources</span></span>

* [<span data-ttu-id="5e915-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5e915-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5e915-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5e915-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_203.png

