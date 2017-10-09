---
title: "Oktatóanyag: Azure Active Directoryval integrált Projectplace |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Projectplace között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 95d109052096161f995ff26a18f8d64f0c4a3dc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a><span data-ttu-id="a57d1-103">Oktatóanyag: Azure Active Directoryval integrált Projectplace</span><span class="sxs-lookup"><span data-stu-id="a57d1-103">Tutorial: Azure Active Directory integration with Projectplace</span></span>

<span data-ttu-id="a57d1-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Projectplace az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a57d1-104">In this tutorial, you learn how toointegrate Projectplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a57d1-105">Projectplace integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a57d1-105">Integrating Projectplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a57d1-106">Megadhatja a hozzáférés tooProjectplace rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a57d1-106">You can control in Azure AD who has access tooProjectplace</span></span>
- <span data-ttu-id="a57d1-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooProjectplace (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a57d1-107">You can enable your users tooautomatically get signed-on tooProjectplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a57d1-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a57d1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a57d1-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a57d1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a57d1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a57d1-110">Prerequisites</span></span>

<span data-ttu-id="a57d1-111">az Azure AD-integráció a Projectplace tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a57d1-111">tooconfigure Azure AD integration with Projectplace, you need hello following items:</span></span>

- <span data-ttu-id="a57d1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a57d1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a57d1-113">A Projectplace egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a57d1-113">A Projectplace single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a57d1-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a57d1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a57d1-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a57d1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a57d1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a57d1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a57d1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a57d1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a57d1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a57d1-118">Scenario description</span></span>
<span data-ttu-id="a57d1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a57d1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a57d1-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a57d1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a57d1-121">Hello gyűjteményből Projectplace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a57d1-121">Adding Projectplace from hello gallery</span></span>
2. <span data-ttu-id="a57d1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a57d1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-projectplace-from-hello-gallery"></a><span data-ttu-id="a57d1-123">Hello gyűjteményből Projectplace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a57d1-123">Adding Projectplace from hello gallery</span></span>
<span data-ttu-id="a57d1-124">tooconfigure hello integrációja Projectplace az Azure AD-be, meg kell tooadd Projectplace hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a57d1-124">tooconfigure hello integration of Projectplace into Azure AD, you need tooadd Projectplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a57d1-125">**tooadd Projectplace hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a57d1-125">**tooadd Projectplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a57d1-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a57d1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a57d1-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a57d1-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a57d1-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a57d1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a57d1-133">Hello keresési mezőbe, írja be a **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-133">In hello search box, type **Projectplace**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_search.png)

5. <span data-ttu-id="a57d1-135">A hello eredmények panelen válassza ki a **Projectplace**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a57d1-135">In hello results panel, select **Projectplace**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a57d1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a57d1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a57d1-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Projectplace.</span><span class="sxs-lookup"><span data-stu-id="a57d1-138">In this section, you configure and test Azure AD single sign-on with Projectplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a57d1-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Projectplace tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a57d1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Projectplace is tooa user in Azure AD.</span></span> <span data-ttu-id="a57d1-140">Ez azt jelenti hello kapcsolódó felhasználó a Projectplace és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a57d1-140">In other words, a link relationship between an Azure AD user and hello related user in Projectplace needs toobe established.</span></span>

<span data-ttu-id="a57d1-141">A Projectplace, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a57d1-141">In Projectplace, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a57d1-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Projectplace-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a57d1-142">tooconfigure and test Azure AD single sign-on with Projectplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a57d1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a57d1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a57d1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a57d1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a57d1-145">**[A Projectplace tesztfelhasználó létrehozása](#creating-a-projectplace-test-user)**  -toohave egy megfelelője a Britta Simon a Projectplace, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a57d1-145">**[Creating a Projectplace test user](#creating-a-projectplace-test-user)** - toohave a counterpart of Britta Simon in Projectplace that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a57d1-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a57d1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a57d1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a57d1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a57d1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a57d1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a57d1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Projectplace-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a57d1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Projectplace application.</span></span>

<span data-ttu-id="a57d1-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Projectplace, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a57d1-150">**tooconfigure Azure AD single sign-on with Projectplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="a57d1-151">Az Azure portál, a hello hello **Projectplace** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-151">In hello Azure portal, on hello **Projectplace** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a57d1-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a57d1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_samlbase.png)

3. <span data-ttu-id="a57d1-155">A hello **Projectplace-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a57d1-155">On hello **Projectplace Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_url.png)

    <span data-ttu-id="a57d1-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.projectplace.com`</span><span class="sxs-lookup"><span data-stu-id="a57d1-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.projectplace.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a57d1-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="a57d1-158">This value is not real.</span></span> <span data-ttu-id="a57d1-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a57d1-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a57d1-160">Ügyfél [Projectplace ügyfél-támogatási csoport](https://success.planview.com/Projectplace/Support) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="a57d1-160">Contact [Projectplace Client support team](https://success.planview.com/Projectplace/Support) tooget this value.</span></span> 
 
4. <span data-ttu-id="a57d1-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a57d1-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_certificate.png) 

5. <span data-ttu-id="a57d1-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a57d1-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a57d1-165">tooconfigure egyszeri bejelentkezést a **Projectplace** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Projectplace támogatási csoport](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="a57d1-165">tooconfigure single sign-on on **Projectplace** side, you need toosend hello downloaded **Metadata XML** too[Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="a57d1-166">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="a57d1-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="a57d1-167">hello egyszeri bejelentkezés konfigurációs rendelkezik hello által végzett toobe [Projectplace támogatási csoport](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="a57d1-167">hello single sign-on configuration has toobe performed by hello [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="a57d1-168">Értesítést kap, amint hello konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a57d1-168">You will get a notification as soon as hello configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="a57d1-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a57d1-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a57d1-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a57d1-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a57d1-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a57d1-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a57d1-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a57d1-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="a57d1-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a57d1-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a57d1-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a57d1-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a57d1-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a57d1-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a57d1-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a57d1-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a57d1-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a57d1-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a57d1-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a57d1-184">a.</span><span class="sxs-lookup"><span data-stu-id="a57d1-184">a.</span></span> <span data-ttu-id="a57d1-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a57d1-186">b.</span><span class="sxs-lookup"><span data-stu-id="a57d1-186">b.</span></span> <span data-ttu-id="a57d1-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a57d1-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a57d1-188">c.</span><span class="sxs-lookup"><span data-stu-id="a57d1-188">c.</span></span> <span data-ttu-id="a57d1-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a57d1-190">d.</span><span class="sxs-lookup"><span data-stu-id="a57d1-190">d.</span></span> <span data-ttu-id="a57d1-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a57d1-191">Click **Create**.</span></span>
 
### <a name="creating-a-projectplace-test-user"></a><span data-ttu-id="a57d1-192">A Projectplace tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a57d1-192">Creating a Projectplace test user</span></span>

<span data-ttu-id="a57d1-193">A sorrend tooenable az Azure AD felhasználók toolog a Projectplace azok ki kell építenie a Projectplace.</span><span class="sxs-lookup"><span data-stu-id="a57d1-193">In order tooenable Azure AD users toolog into Projectplace, they must be provisioned into Projectplace.</span></span> <span data-ttu-id="a57d1-194">A Projectplace hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="a57d1-194">In hello case of Projectplace, provisioning is a manual task.</span></span>

<span data-ttu-id="a57d1-195">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a57d1-195">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="a57d1-196">Jelentkezzen be tooyour **Projectplace** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a57d1-196">Log in tooyour **Projectplace** company site as an administrator.</span></span>

2. <span data-ttu-id="a57d1-197">Nyissa meg túl**személyek**, és kattintson a **tagok**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-197">Go too**People**, and then click **Members**.</span></span>
   
    <span data-ttu-id="a57d1-198">![Személyek](./media/active-directory-saas-projectplace-tutorial/ic790228.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="a57d1-198">![People](./media/active-directory-saas-projectplace-tutorial/ic790228.png "People")</span></span>

3. <span data-ttu-id="a57d1-199">Kattintson a **felvenni abban az esetben**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-199">Click **Add Member**.</span></span>
   
    <span data-ttu-id="a57d1-200">![Tagok hozzáadása](./media/active-directory-saas-projectplace-tutorial/ic790232.png "tagok hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a57d1-200">![Add Members](./media/active-directory-saas-projectplace-tutorial/ic790232.png "Add Members")</span></span>

4. <span data-ttu-id="a57d1-201">A hello **tag hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a57d1-201">In hello **Add Member** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="a57d1-202">![Új tagok](./media/active-directory-saas-projectplace-tutorial/ic790233.png "új tagok számára")</span><span class="sxs-lookup"><span data-stu-id="a57d1-202">![New Members](./media/active-directory-saas-projectplace-tutorial/ic790233.png "New Members")</span></span>
   
    <span data-ttu-id="a57d1-203">a.</span><span class="sxs-lookup"><span data-stu-id="a57d1-203">a.</span></span> <span data-ttu-id="a57d1-204">A hello **új tagok** szövegmezőhöz típusú hello e-mail cím tooprovision hello a kívánt fiók érvényes AAD kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="a57d1-204">In hello **New Members** textbox, type hello email address of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="a57d1-205">b.</span><span class="sxs-lookup"><span data-stu-id="a57d1-205">b.</span></span> <span data-ttu-id="a57d1-206">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-206">Click **Send**.</span></span>

   <span data-ttu-id="a57d1-207">Az e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik toohello Azure Active Directory fióktulajdonos küld.</span><span class="sxs-lookup"><span data-stu-id="a57d1-207">An email including a link tooconfirm hello account before it becomes active is sent toohello Azure Active Directory account holder.</span></span>

>[!NOTE]
><span data-ttu-id="a57d1-208">Bármely más Projectplace felhasználói fiók létrehozása eszközök vagy Projectplace tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="a57d1-208">You can use any other Projectplace user account creation tools or APIs provided by Projectplace tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a57d1-209">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a57d1-209">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a57d1-210">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooProjectplace megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a57d1-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooProjectplace.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a57d1-212">**tooassign Britta Simon tooProjectplace, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a57d1-212">**tooassign Britta Simon tooProjectplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="a57d1-213">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a57d1-215">Hello alkalmazások listában válassza ki a **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-215">In hello applications list, select **Projectplace**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_app.png) 

3. <span data-ttu-id="a57d1-217">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a57d1-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a57d1-219">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a57d1-219">Click **Add** button.</span></span> <span data-ttu-id="a57d1-220">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a57d1-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a57d1-222">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a57d1-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a57d1-223">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a57d1-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a57d1-224">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a57d1-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a57d1-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a57d1-225">Testing single sign-on</span></span>

<span data-ttu-id="a57d1-226">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a57d1-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a57d1-227">Ha a hozzáférési Panel hello hello Projectplace csempe gombra kattint, automatikusan bejelentkezett tooyour Projectplace alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="a57d1-227">When you click hello Projectplace tile in hello Access Panel, you should get automatically signed-on tooyour Projectplace application.</span></span>
<span data-ttu-id="a57d1-228">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a57d1-228">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a57d1-229">További források</span><span class="sxs-lookup"><span data-stu-id="a57d1-229">Additional resources</span></span>

* [<span data-ttu-id="a57d1-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a57d1-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a57d1-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a57d1-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_203.png

