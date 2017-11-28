---
title: "Oktatóanyag: Azure Active Directory-integráció SCC életciklusával |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SCC életciklus között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 63623c5bd2d951612040f0121615a406bf83e890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="cf144-103">Oktatóanyag: Azure Active Directoryval integrált SCC életciklusa</span><span class="sxs-lookup"><span data-stu-id="cf144-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>

<span data-ttu-id="cf144-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SCC életciklusa az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cf144-104">In this tutorial, you learn how toointegrate SCC LifeCycle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cf144-105">SCC életciklus integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="cf144-105">Integrating SCC LifeCycle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cf144-106">Megadhatja a hozzáférés tooSCC életciklus rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cf144-106">You can control in Azure AD who has access tooSCC LifeCycle</span></span>
- <span data-ttu-id="cf144-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSCC életciklusának (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="cf144-107">You can enable your users tooautomatically get signed-on tooSCC LifeCycle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cf144-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cf144-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cf144-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cf144-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf144-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cf144-110">Prerequisites</span></span>

<span data-ttu-id="cf144-111">tooconfigure SCC életciklus integrálása az Azure AD, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="cf144-111">tooconfigure Azure AD integration with SCC LifeCycle, you need hello following items:</span></span>

- <span data-ttu-id="cf144-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cf144-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cf144-113">Egy SCC életciklusa az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="cf144-113">An SCC LifeCycle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cf144-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="cf144-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cf144-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="cf144-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cf144-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf144-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cf144-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cf144-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cf144-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cf144-118">Scenario description</span></span>
<span data-ttu-id="cf144-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cf144-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cf144-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cf144-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cf144-121">Hello gyűjteményből SCC életciklus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cf144-121">Adding SCC LifeCycle from hello gallery</span></span>
2. <span data-ttu-id="cf144-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cf144-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scc-lifecycle-from-hello-gallery"></a><span data-ttu-id="cf144-123">Hello gyűjteményből SCC életciklus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cf144-123">Adding SCC LifeCycle from hello gallery</span></span>
<span data-ttu-id="cf144-124">tooconfigure hello integrációs SCC életciklus, az Azure AD-be, meg kell tooadd SCC életciklus hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="cf144-124">tooconfigure hello integration of SCC LifeCycle into Azure AD, you need tooadd SCC LifeCycle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cf144-125">**tooadd SCC életciklus hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cf144-125">**tooadd SCC LifeCycle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf144-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cf144-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cf144-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cf144-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cf144-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cf144-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cf144-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="cf144-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cf144-133">Hello keresési mezőbe, írja be a **SCC életciklus**.</span><span class="sxs-lookup"><span data-stu-id="cf144-133">In hello search box, type **SCC LifeCycle**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_search.png)

5. <span data-ttu-id="cf144-135">A hello eredmények panelen válassza a **SCC életciklus**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="cf144-135">In hello results panel, select **SCC LifeCycle**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cf144-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cf144-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="cf144-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés SCC életciklusával "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="cf144-138">In this section, you configure and test Azure AD single sign-on with SCC LifeCycle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cf144-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói SCC életciklusának tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="cf144-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SCC LifeCycle is tooa user in Azure AD.</span></span> <span data-ttu-id="cf144-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello SCC életciklusának közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="cf144-140">In other words, a link relationship between an Azure AD user and hello related user in SCC LifeCycle needs toobe established.</span></span>

<span data-ttu-id="cf144-141">SCC életciklusát, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="cf144-141">In SCC LifeCycle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cf144-142">tooconfigure és az Azure AD az egyszeri bejelentkezés SCC életciklus-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="cf144-142">tooconfigure and test Azure AD single sign-on with SCC LifeCycle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cf144-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cf144-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cf144-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf144-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cf144-145">**[Egy SCC életciklus tesztfelhasználó létrehozása](#creating-an-scc-lifecycle-test-user)**  -toohave egy megfelelője a Britta Simon SCC életciklusának, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="cf144-145">**[Creating an SCC LifeCycle test user](#creating-an-scc-lifecycle-test-user)** - toohave a counterpart of Britta Simon in SCC LifeCycle that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cf144-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cf144-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cf144-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="cf144-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cf144-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cf144-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cf144-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az SCC életciklus-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cf144-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SCC LifeCycle application.</span></span>

<span data-ttu-id="cf144-150">**az Azure AD tooconfigure egyszeri bejelentkezés SCC életciklusával, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cf144-150">**tooconfigure Azure AD single sign-on with SCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf144-151">Az Azure portál, a hello hello **SCC életciklus** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cf144-151">In hello Azure portal, on hello **SCC LifeCycle** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cf144-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cf144-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_samlbase.png)

3. <span data-ttu-id="cf144-155">A hello **SCC életciklus tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cf144-155">On hello **SCC LifeCycle Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_url.png)

    <span data-ttu-id="cf144-157">a.</span><span class="sxs-lookup"><span data-stu-id="cf144-157">a.</span></span> <span data-ttu-id="cf144-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub-domain>.scc.com/ic7/welcome/customer/PICTtest.aspx`</span><span class="sxs-lookup"><span data-stu-id="cf144-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub-domain>.scc.com/ic7/welcome/customer/PICTtest.aspx`</span></span>

    <span data-ttu-id="cf144-159">b.</span><span class="sxs-lookup"><span data-stu-id="cf144-159">b.</span></span> <span data-ttu-id="cf144-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="cf144-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://bs1.scc.com/<entity>`|
    | `https://lifecycle.scc.com/<entity>`|
    
    > [!NOTE] 
    > <span data-ttu-id="cf144-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="cf144-161">These values are not real.</span></span> <span data-ttu-id="cf144-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="cf144-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cf144-163">Ügyfél [SCC életciklus ügyfél-támogatási csoport](mailto:lifecycle.support@scc.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cf144-163">Contact [SCC LifeCycle Client support team](mailto:lifecycle.support@scc.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="cf144-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cf144-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_certificate.png) 

5. <span data-ttu-id="cf144-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf144-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cf144-168">tooconfigure egyszeri bejelentkezést a **SCC életciklus** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[SCC életciklus támogatási csoport](mailto:lifecycle.support@scc.com).</span><span class="sxs-lookup"><span data-stu-id="cf144-168">tooconfigure single sign-on on **SCC LifeCycle** side, you need toosend hello downloaded **Metadata XML** too[SCC LifeCycle support team](mailto:lifecycle.support@scc.com).</span></span> <span data-ttu-id="cf144-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="cf144-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

     >[!NOTE]
   ><span data-ttu-id="cf144-170">Egyszeri bejelentkezés hello SCC életciklus támogatási csoport által engedélyezett toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cf144-170">Single sign-on has toobe enabled by hello SCC LifeCycle support team.</span></span>
   > 
   > 

> [!TIP]
> <span data-ttu-id="cf144-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="cf144-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cf144-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="cf144-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cf144-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cf144-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cf144-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf144-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="cf144-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="cf144-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cf144-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="cf144-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf144-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cf144-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cf144-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cf144-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cf144-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf144-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cf144-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cf144-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cf144-186">a.</span><span class="sxs-lookup"><span data-stu-id="cf144-186">a.</span></span> <span data-ttu-id="cf144-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cf144-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cf144-188">b.</span><span class="sxs-lookup"><span data-stu-id="cf144-188">b.</span></span> <span data-ttu-id="cf144-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cf144-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cf144-190">c.</span><span class="sxs-lookup"><span data-stu-id="cf144-190">c.</span></span> <span data-ttu-id="cf144-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cf144-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cf144-192">d.</span><span class="sxs-lookup"><span data-stu-id="cf144-192">d.</span></span> <span data-ttu-id="cf144-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cf144-193">Click **Create**.</span></span>
 
### <a name="creating-an-scc-lifecycle-test-user"></a><span data-ttu-id="cf144-194">Egy SCC életciklus tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf144-194">Creating an SCC LifeCycle test user</span></span>

<span data-ttu-id="cf144-195">A sorrend tooenable az Azure AD felhasználók toolog SCC életciklus be azok kell üzembe SCC életciklus be.</span><span class="sxs-lookup"><span data-stu-id="cf144-195">In order tooenable Azure AD users toolog into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="cf144-196">Nincs művelet elem a akkor tooconfigure felhasználók átadásához tooSCC életciklusát.</span><span class="sxs-lookup"><span data-stu-id="cf144-196">There is no action item for you tooconfigure user provisioning tooSCC LifeCycle.</span></span>

<span data-ttu-id="cf144-197">Amikor egy hozzárendelt felhasználó záma toolog SCC életciklus be, SCC életciklus fiók automatikusan létrejön, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="cf144-197">When an assigned user tries toolog into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

> [!NOTE]
    > <span data-ttu-id="cf144-198">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="cf144-198">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cf144-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cf144-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cf144-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSCC életciklus megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="cf144-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSCC LifeCycle.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cf144-202">**tooassign Britta Simon tooSCC életciklusát, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cf144-202">**tooassign Britta Simon tooSCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf144-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="cf144-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications.**</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cf144-205">Hello alkalmazások listában válassza ki a **SCC életciklus**.</span><span class="sxs-lookup"><span data-stu-id="cf144-205">In hello applications list, select **SCC LifeCycle**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_app.png) 

3. <span data-ttu-id="cf144-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cf144-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cf144-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf144-209">Click **Add** button.</span></span> <span data-ttu-id="cf144-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf144-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cf144-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cf144-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cf144-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf144-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cf144-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf144-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cf144-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cf144-215">Testing single sign-on</span></span>

<span data-ttu-id="cf144-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="cf144-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cf144-217">Hello SCC életciklus hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooSCC életciklus alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cf144-217">When you click hello SCC LifeCycle tile in hello Access Panel, you should get automatically signed-on tooSCC LifeCycle application.</span></span>
<span data-ttu-id="cf144-218">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cf144-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf144-219">További források</span><span class="sxs-lookup"><span data-stu-id="cf144-219">Additional resources</span></span>

* [<span data-ttu-id="cf144-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="cf144-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cf144-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cf144-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_203.png

