---
title: "Oktatóanyag: Munkahelyi Learning Azure Active Directoryval integrált |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahelyi tanulási között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: fa09d585d57932a95cadba9a66029765d7df3694
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a><span data-ttu-id="4928f-103">Oktatóanyag: Munkahelyi Learning Azure Active Directory integrációja</span><span class="sxs-lookup"><span data-stu-id="4928f-103">Tutorial: Azure Active Directory integration with Learning at Work</span></span>

<span data-ttu-id="4928f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate tanulási munkahelyi Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4928f-104">In this tutorial, you learn how toointegrate Learning at Work with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4928f-105">Learning munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4928f-105">Integrating Learning at Work with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4928f-106">Megadhatja a munkahelyi hozzáférés tooLearning rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4928f-106">You can control in Azure AD who has access tooLearning at Work</span></span>
- <span data-ttu-id="4928f-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLearning munkahelyi (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="4928f-107">You can enable your users tooautomatically get signed-on tooLearning at Work (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4928f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4928f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4928f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4928f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4928f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4928f-110">Prerequisites</span></span>

<span data-ttu-id="4928f-111">tooconfigure munkahelyi Learning Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4928f-111">tooconfigure Azure AD integration with Learning at Work, you need hello following items:</span></span>

- <span data-ttu-id="4928f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4928f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4928f-113">A Learning munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="4928f-113">A Learning at Work single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4928f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4928f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4928f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4928f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4928f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4928f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4928f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4928f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4928f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4928f-118">Scenario description</span></span>
<span data-ttu-id="4928f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4928f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4928f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4928f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4928f-121">Learning hozzáadása munkahelyi hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4928f-121">Adding Learning at Work from hello gallery</span></span>
2. <span data-ttu-id="4928f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4928f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-at-work-from-hello-gallery"></a><span data-ttu-id="4928f-123">Learning hozzáadása munkahelyi hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4928f-123">Adding Learning at Work from hello gallery</span></span>
<span data-ttu-id="4928f-124">tooconfigure hello integráció az Azure AD-be munkahelyi tanulás kell tooadd tanulási munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4928f-124">tooconfigure hello integration of Learning at Work into Azure AD, you need tooadd Learning at Work from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4928f-125">**tooadd tanulási munkahelyi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4928f-125">**tooadd Learning at Work from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4928f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4928f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4928f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4928f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4928f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4928f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4928f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4928f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4928f-133">Hello keresési mezőbe, írja be a **munkahelyi tanulási**.</span><span class="sxs-lookup"><span data-stu-id="4928f-133">In hello search box, type **Learning at Work**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_search.png)

5. <span data-ttu-id="4928f-135">A hello eredmények panelen válassza a **munkahelyi tanulási**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4928f-135">In hello results panel, select **Learning at Work**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4928f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4928f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4928f-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján munkahelyi Learning segítségével</span><span class="sxs-lookup"><span data-stu-id="4928f-138">In this section, you configure and test Azure AD single sign-on with Learning at Work based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4928f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a munkahelyi tanulási tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4928f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Learning at Work is tooa user in Azure AD.</span></span> <span data-ttu-id="4928f-140">Ez azt jelenti hello kapcsolódó felhasználó a munkahelyi tanulási és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4928f-140">In other words, a link relationship between an Azure AD user and hello related user in Learning at Work needs toobe established.</span></span>

<span data-ttu-id="4928f-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** munkahelyi szeretnének.</span><span class="sxs-lookup"><span data-stu-id="4928f-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Learning at Work.</span></span>

<span data-ttu-id="4928f-142">tooconfigure és munkahelyi Learning segítségével az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4928f-142">tooconfigure and test Azure AD single sign-on with Learning at Work, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4928f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4928f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4928f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4928f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4928f-145">**[A tanulás következő munkahelyi tesztfelhasználó létrehozása](#creating-a-learning-at-work-test-user)**  -toohave egy felhasználó csatolt toohello az Azure AD ábrázolása munkahelyi szeretnének Britta Simon megfelelője.</span><span class="sxs-lookup"><span data-stu-id="4928f-145">**[Creating a Learning at Work test user](#creating-a-learning-at-work-test-user)** - toohave a counterpart of Britta Simon in Learning at Work that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4928f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4928f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4928f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4928f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4928f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4928f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4928f-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a tanulás következő munkahelyi alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="4928f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Learning at Work application.</span></span>

<span data-ttu-id="4928f-150">**az Azure AD tooconfigure egyszeri bejelentkezés munkahelyi Learning segítségével hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4928f-150">**tooconfigure Azure AD single sign-on with Learning at Work, perform hello following steps:**</span></span>

1. <span data-ttu-id="4928f-151">Az Azure portál, a hello hello **munkahelyi tanulási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4928f-151">In hello Azure portal, on hello **Learning at Work** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4928f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4928f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

3. <span data-ttu-id="4928f-155">A hello **munkahelyi tartomány és az URL-címek tanulási** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4928f-155">On hello **Learning at Work Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_url.png)

    <span data-ttu-id="4928f-157">a.</span><span class="sxs-lookup"><span data-stu-id="4928f-157">a.</span></span> <span data-ttu-id="4928f-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span><span class="sxs-lookup"><span data-stu-id="4928f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span></span>

    <span data-ttu-id="4928f-159">b.</span><span class="sxs-lookup"><span data-stu-id="4928f-159">b.</span></span> <span data-ttu-id="4928f-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span><span class="sxs-lookup"><span data-stu-id="4928f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4928f-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="4928f-161">These values are not hello real.</span></span> <span data-ttu-id="4928f-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="4928f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4928f-163">Ügyfél [munkahelyi ügyfél-támogatási csapatával: tanulási](https://www.learninga-z.com/site/contact/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4928f-163">Contact [Learning at Work Client support team](https://www.learninga-z.com/site/contact/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="4928f-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4928f-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

5. <span data-ttu-id="4928f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4928f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4928f-168">A hello **munkahelyi konfigurálás tanulási** területén kattintson **konfigurálása tanulási munkahelyi** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4928f-168">On hello **Learning at Work Configuration** section, click **Configure Learning at Work** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4928f-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4928f-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

7. <span data-ttu-id="4928f-171">tooconfigure egyszeri bejelentkezést a **munkahelyi tanulási** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **SAML Entitásazonosító**, **SAML-alapú egyszeri bejelentkezést. URL-címe**, és **Sign-Out URL-cím** túl[munkahelyi terméktámogatási tanulási](https://www.learninga-z.com/site/contact/support).</span><span class="sxs-lookup"><span data-stu-id="4928f-171">tooconfigure single sign-on on **Learning at Work** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL** too[Learning at Work support](https://www.learninga-z.com/site/contact/support).</span></span>

> [!TIP]
> <span data-ttu-id="4928f-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4928f-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4928f-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4928f-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4928f-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4928f-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4928f-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4928f-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="4928f-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4928f-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4928f-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4928f-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4928f-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4928f-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4928f-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4928f-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4928f-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4928f-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4928f-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4928f-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4928f-187">a.</span><span class="sxs-lookup"><span data-stu-id="4928f-187">a.</span></span> <span data-ttu-id="4928f-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4928f-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4928f-189">b.</span><span class="sxs-lookup"><span data-stu-id="4928f-189">b.</span></span> <span data-ttu-id="4928f-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4928f-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4928f-191">c.</span><span class="sxs-lookup"><span data-stu-id="4928f-191">c.</span></span> <span data-ttu-id="4928f-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4928f-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4928f-193">d.</span><span class="sxs-lookup"><span data-stu-id="4928f-193">d.</span></span> <span data-ttu-id="4928f-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4928f-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-at-work-test-user"></a><span data-ttu-id="4928f-195">A tanulás következő munkahelyi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4928f-195">Creating a Learning at Work test user</span></span>

<span data-ttu-id="4928f-196">Ebben a szakaszban a felhasználó munkahelyi tanulási Britta Simon meghívta hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4928f-196">In this section, you create a user called Britta Simon in Learning at Work.</span></span> <span data-ttu-id="4928f-197">Együttműködve [munkahelyi terméktámogatási tanulási](https://www.learninga-z.com/site/contact/support) tooadd hello felhasználók tanulási hello: munkahelyi platform.</span><span class="sxs-lookup"><span data-stu-id="4928f-197">Work with [Learning at Work support](https://www.learninga-z.com/site/contact/support) tooadd hello users in hello Learning at Work platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4928f-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4928f-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4928f-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés a munkahelyi hozzáférés tooLearning megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4928f-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearning at Work.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4928f-201">**tooassign Britta Simon tooLearning munkahelyi, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4928f-201">**tooassign Britta Simon tooLearning at Work, perform hello following steps:**</span></span>

1. <span data-ttu-id="4928f-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4928f-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4928f-204">Hello alkalmazások listában válassza ki a **munkahelyi tanulási**.</span><span class="sxs-lookup"><span data-stu-id="4928f-204">In hello applications list, select **Learning at Work**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_app.png) 

3. <span data-ttu-id="4928f-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4928f-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4928f-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4928f-208">Click **Add** button.</span></span> <span data-ttu-id="4928f-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4928f-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4928f-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4928f-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4928f-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4928f-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4928f-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4928f-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4928f-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4928f-214">Testing single sign-on</span></span>

<span data-ttu-id="4928f-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4928f-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4928f-216">Hello kattintva, a munkahelyi csempe tanulási hello hozzáférési Panel, automatikusan bejelentkezett tooyour szerezheti be a munkahelyi alkalmazás tanulási.</span><span class="sxs-lookup"><span data-stu-id="4928f-216">When you click hello Learning at Work tile in hello Access Panel, you should get automatically signed-on tooyour Learning at Work application.</span></span>
<span data-ttu-id="4928f-217">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4928f-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4928f-218">További források</span><span class="sxs-lookup"><span data-stu-id="4928f-218">Additional resources</span></span>

* [<span data-ttu-id="4928f-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4928f-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4928f-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4928f-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png

