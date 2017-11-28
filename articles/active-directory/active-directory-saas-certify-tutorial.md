---
title: "Oktatóanyag: Azure Active Directoryval integrált hitelesítés |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Certify között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b36e020-175a-4534-b341-85260739f889
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 000bef7b679a6f291b1f3cb42e10cb3ed424b25d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-certify"></a><span data-ttu-id="8c3bc-103">Oktatóanyag: Azure Active Directoryval integrált hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8c3bc-103">Tutorial: Azure Active Directory integration with Certify</span></span>

<span data-ttu-id="8c3bc-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate igazolja az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8c3bc-104">In this tutorial, you learn how toointegrate Certify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8c3bc-105">Certify integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-105">Integrating Certify with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8c3bc-106">Megadhatja a hozzáférés tooCertify rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8c3bc-106">You can control in Azure AD who has access tooCertify</span></span>
- <span data-ttu-id="8c3bc-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCertify (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8c3bc-107">You can enable your users tooautomatically get signed-on tooCertify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8c3bc-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8c3bc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8c3bc-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8c3bc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c3bc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c3bc-110">Prerequisites</span></span>

<span data-ttu-id="8c3bc-111">tooconfigure az Azure AD-integrációs Certify szüksége hello a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-111">tooconfigure Azure AD integration with Certify, you need hello following items:</span></span>

- <span data-ttu-id="8c3bc-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8c3bc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8c3bc-113">A hitelesítés egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8c3bc-113">A Certify single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8c3bc-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8c3bc-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8c3bc-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8c3bc-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8c3bc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8c3bc-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8c3bc-118">Scenario description</span></span>
<span data-ttu-id="8c3bc-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8c3bc-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8c3bc-121">Hitelesítés hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8c3bc-121">Adding Certify from hello gallery</span></span>
2. <span data-ttu-id="8c3bc-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8c3bc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-certify-from-hello-gallery"></a><span data-ttu-id="8c3bc-123">Hitelesítés hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8c3bc-123">Adding Certify from hello gallery</span></span>
<span data-ttu-id="8c3bc-124">tooconfigure hello integrációja hitelesítés az Azure AD-be, meg kell tooadd Certify hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-124">tooconfigure hello integration of Certify into Azure AD, you need tooadd Certify from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8c3bc-125">**tooadd Certify hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8c3bc-125">**tooadd Certify from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8c3bc-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8c3bc-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8c3bc-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8c3bc-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8c3bc-133">Hello keresési mezőbe, írja be a **Certify**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-133">In hello search box, type **Certify**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/tutorial_certify_search.png)

5. <span data-ttu-id="8c3bc-135">A hello eredmények panelen válassza ki a **Certify**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-135">In hello results panel, select **Certify**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/tutorial_certify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8c3bc-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8c3bc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8c3bc-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-138">In this section, you configure and test Azure AD single sign-on with Certify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8c3bc-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói, a Certify tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Certify is tooa user in Azure AD.</span></span> <span data-ttu-id="8c3bc-140">Ez azt jelenti hello kapcsolódó felhasználó a Certify és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-140">In other words, a link relationship between an Azure AD user and hello related user in Certify needs toobe established.</span></span>

<span data-ttu-id="8c3bc-141">A hitelesítés, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-141">In Certify, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8c3bc-142">tooconfigure és tesztelése az Azure AD-egyszeri bejelentkezés Certify szüksége toocomplete hello építőelemeket a következő:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-142">tooconfigure and test Azure AD single sign-on with Certify, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8c3bc-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8c3bc-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8c3bc-145">**[Certify tesztfelhasználó létrehozása](#creating-a-certify-test-user)**  -toohave egy megfelelője a Britta Simon a tanúsítása, hogy a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-145">**[Creating a Certify test user](#creating-a-certify-test-user)** - toohave a counterpart of Britta Simon in Certify that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8c3bc-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8c3bc-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8c3bc-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8c3bc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8c3bc-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Certify alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Certify application.</span></span>

<span data-ttu-id="8c3bc-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Certify hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8c3bc-150">**tooconfigure Azure AD single sign-on with Certify, perform hello following steps:**</span></span>

1. <span data-ttu-id="8c3bc-151">Az Azure portál, a hello hello **Certify** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-151">In hello Azure portal, on hello **Certify** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8c3bc-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_samlbase.png)

3. <span data-ttu-id="8c3bc-155">A hello **tanúsítására tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-155">On hello **Certify Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_url.png)

    <span data-ttu-id="8c3bc-157">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.certify.com`</span><span class="sxs-lookup"><span data-stu-id="8c3bc-157">In hello **Identifier** textbox, type hello URL: `https://www.certify.com`</span></span>

4. <span data-ttu-id="8c3bc-158">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-158">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_certificate.png) 

5. <span data-ttu-id="8c3bc-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8c3bc-162">A hello **tanúsítása Configuration** területén kattintson **konfigurálása tanúsítására** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-162">On hello **Certify Configuration** section, click **Configure Certify** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8c3bc-163">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8c3bc-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_configure.png) 

7. <span data-ttu-id="8c3bc-165">tooconfigure egyszeri bejelentkezést a **Certify** oldalon kell letöltött toosend hello **Certificate(Raw)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**túl[Certify támogatási csoport](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="8c3bc-165">tooconfigure single sign-on on **Certify** side, you need toosend hello downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Certify support team](mailto:support@certify.com).</span></span> <span data-ttu-id="8c3bc-166">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="8c3bc-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8c3bc-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8c3bc-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8c3bc-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8c3bc-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8c3bc-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c3bc-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="8c3bc-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8c3bc-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8c3bc-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8c3bc-174">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8c3bc-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8c3bc-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8c3bc-180">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8c3bc-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8c3bc-182">a.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-182">a.</span></span> <span data-ttu-id="8c3bc-183">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8c3bc-184">b.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-184">b.</span></span> <span data-ttu-id="8c3bc-185">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8c3bc-186">c.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-186">c.</span></span> <span data-ttu-id="8c3bc-187">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8c3bc-188">d.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-188">d.</span></span> <span data-ttu-id="8c3bc-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-189">Click **Create**.</span></span>
 
### <a name="creating-a-certify-test-user"></a><span data-ttu-id="8c3bc-190">Certify tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c3bc-190">Creating a Certify test user</span></span>

<span data-ttu-id="8c3bc-191">hello ebben a szakaszban célja toocreate Certify Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-191">hello objective of this section is toocreate a user called Britta Simon in Certify.</span></span> <span data-ttu-id="8c3bc-192">Igazolnia támogatja közvetlenül az időponthoz kötött kiépítés, alapértelmezett beállítás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-192">Certify supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="8c3bc-193">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-193">There is no action item for you in this section.</span></span> <span data-ttu-id="8c3bc-194">Új felhasználó létrejön egy kísérlet tooaccess hitelesítés során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-194">A new user will be created during an attempt tooaccess Certify if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="8c3bc-195">Ha egy felhasználó toocreate manuálisan kell, toocontact hello kell [Certify támogatási csoport](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="8c3bc-195">If you need toocreate an user manually, you need toocontact hello [Certify support team](mailto:support@certify.com).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8c3bc-196">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8c3bc-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8c3bc-197">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCertify megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCertify.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8c3bc-199">**tooassign Britta Simon tooCertify, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8c3bc-199">**tooassign Britta Simon tooCertify, perform hello following steps:**</span></span>

1. <span data-ttu-id="8c3bc-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8c3bc-202">Hello alkalmazások listában válassza ki a **Certify**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-202">In hello applications list, select **Certify**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_app.png) 

3. <span data-ttu-id="8c3bc-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8c3bc-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-206">Click **Add** button.</span></span> <span data-ttu-id="8c3bc-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8c3bc-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8c3bc-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8c3bc-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8c3bc-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8c3bc-212">Testing single sign-on</span></span>

<span data-ttu-id="8c3bc-213">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-213">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="8c3bc-214">Ha a hozzáférési Panel hello hello Certify csempe gombra kattint, automatikusan bejelentkezett tooyour Certify alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-214">When you click hello Certify tile in hello Access Panel, you should get automatically signed-on tooyour Certify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c3bc-215">További források</span><span class="sxs-lookup"><span data-stu-id="8c3bc-215">Additional resources</span></span>

* [<span data-ttu-id="8c3bc-216">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8c3bc-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8c3bc-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8c3bc-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-certify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-certify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-certify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-certify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-certify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-certify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-certify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-certify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-certify-tutorial/tutorial_general_203.png

