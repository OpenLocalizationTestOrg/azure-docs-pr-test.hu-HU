---
title: "Oktatóanyag: Azure Active Directory-integráció a 10 000 ft tervek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és 10 000 ft-csomagok."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 9aa6fd079da4f931d4dd7277407a07e56091d7c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a><span data-ttu-id="cd14a-103">Oktatóanyag: Azure Active Directory-integráció a 10 000 ft tervek</span><span class="sxs-lookup"><span data-stu-id="cd14a-103">Tutorial: Azure Active Directory integration with 10,000ft Plans</span></span>

<span data-ttu-id="cd14a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 10 000 ft tervek az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cd14a-104">In this tutorial, you learn how toointegrate 10,000ft Plans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd14a-105">10 000 ft tervek integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="cd14a-105">Integrating 10,000ft Plans with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cd14a-106">Megadhatja a hozzáférés too10, 000ft tervek rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cd14a-106">You can control in Azure AD who has access too10,000ft Plans</span></span>
- <span data-ttu-id="cd14a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett too10, 000ft terveket (egyszeri bejelentkezés) az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="cd14a-107">You can enable your users tooautomatically get signed-on too10,000ft Plans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd14a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cd14a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cd14a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd14a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd14a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd14a-110">Prerequisites</span></span>

<span data-ttu-id="cd14a-111">az Azure AD integrálása a 10 000 ft tervek tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="cd14a-111">tooconfigure Azure AD integration with 10,000ft Plans, you need hello following items:</span></span>

- <span data-ttu-id="cd14a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cd14a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd14a-113">A 10 000 ft tervek egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="cd14a-113">A 10,000ft Plans single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd14a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="cd14a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd14a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="cd14a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd14a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cd14a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd14a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverziós ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd14a-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd14a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cd14a-118">Scenario description</span></span>
<span data-ttu-id="cd14a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cd14a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd14a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cd14a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd14a-121">10 000 ft hozzáadása tervek hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="cd14a-121">Adding 10,000ft Plans from hello gallery</span></span>
2. <span data-ttu-id="cd14a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cd14a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-10000ft-plans-from-hello-gallery"></a><span data-ttu-id="cd14a-123">10 000 ft hozzáadása tervek hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="cd14a-123">Adding 10,000ft Plans from hello gallery</span></span>
<span data-ttu-id="cd14a-124">tooconfigure hello integrációs 10 000 ft tervek az Azure AD-be, meg kell tooadd 10 000 ft tervek hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="cd14a-124">tooconfigure hello integration of 10,000ft Plans into Azure AD, you need tooadd 10,000ft Plans from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cd14a-125">**10 000 ft tooadd tervek hello gyűjteményből, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="cd14a-125">**tooadd 10,000ft Plans from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd14a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cd14a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd14a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cd14a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cd14a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="cd14a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cd14a-133">Hello keresési mezőbe, írja be a **10 000 ft tervek**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-133">In hello search box, type **10,000ft Plans**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. <span data-ttu-id="cd14a-135">A hello eredmények panelen válassza a **10 000 ft tervek**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="cd14a-135">In hello results panel, select **10,000ft Plans**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd14a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cd14a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cd14a-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a 10 000 ft "Britta Simon." nevű tesztfelhasználó alapján tervek</span><span class="sxs-lookup"><span data-stu-id="cd14a-138">In this section, you configure and test Azure AD single sign-on with 10,000ft Plans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cd14a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a 10 000 ft csomagokban tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="cd14a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 10,000ft Plans is tooa user in Azure AD.</span></span> <span data-ttu-id="cd14a-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a 10 000 ft hivatkozás kapcsolatának tervek kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="cd14a-140">In other words, a link relationship between an Azure AD user and hello related user in 10,000ft Plans needs toobe established.</span></span>

<span data-ttu-id="cd14a-141">A 10 000 ft csomagokban rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="cd14a-141">In 10,000ft Plans, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cd14a-142">tooconfigure és a 10 000 ft tervek az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="cd14a-142">tooconfigure and test Azure AD single sign-on with 10,000ft Plans, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cd14a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cd14a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cd14a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd14a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd14a-145">**[10 000 ft létrehozása tervek tesztfelhasználó](#creating-a-10000ft-plans-test-user)**  -Britta Simon egy partner, a 10 000 ft, amely a tervek toohave kapcsolódó felhasználói toohello az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="cd14a-145">**[Creating a 10,000ft Plans test user](#creating-a-10000ft-plans-test-user)** - toohave a counterpart of Britta Simon in 10,000ft Plans that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd14a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cd14a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd14a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="cd14a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd14a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cd14a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd14a-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és 10 000 ft tervek alkalmazásában egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="cd14a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 10,000ft Plans application.</span></span>

<span data-ttu-id="cd14a-150">**az Azure AD tooconfigure egyszeri bejelentkezést a 10 000 ft tervek, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cd14a-150">**tooconfigure Azure AD single sign-on with 10,000ft Plans, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd14a-151">Az Azure portál, a hello hello **10 000 ft tervek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-151">In hello Azure portal, on hello **10,000ft Plans** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cd14a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cd14a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. <span data-ttu-id="cd14a-155">A hello **10 000 ft tervek tartományi és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cd14a-155">On hello **10,000ft Plans Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    <span data-ttu-id="cd14a-157">a.</span><span class="sxs-lookup"><span data-stu-id="cd14a-157">a.</span></span> <span data-ttu-id="cd14a-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://app.10000ft.com`</span><span class="sxs-lookup"><span data-stu-id="cd14a-158">In hello **Sign-on URL** textbox, type hello URL: `https://app.10000ft.com`</span></span>

    <span data-ttu-id="cd14a-159">b.</span><span class="sxs-lookup"><span data-stu-id="cd14a-159">b.</span></span> <span data-ttu-id="cd14a-160">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://app.10000ft.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="cd14a-160">In hello **Identifier** textbox, type hello URL: `https://app.10000ft.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cd14a-161">a következő hello **azonosító** eltérő, ha egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="cd14a-161">hello value for **Identifier** is different if you have a custom domain.</span></span> <span data-ttu-id="cd14a-162">Ügyfél [10 000 ft tervek támogatási csoport](https://www.10000ft.com/plans/support) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="cd14a-162">Contact [10,000ft Plans support team](https://www.10000ft.com/plans/support) tooget this value.</span></span> 
 
4. <span data-ttu-id="cd14a-163">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cd14a-163">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. <span data-ttu-id="cd14a-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd14a-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cd14a-167">A hello **10 000 ft-csomagok konfigurációs** területen kattintson **konfigurálása 10 000 ft tervek** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="cd14a-167">On hello **10,000ft Plans Configuration** section, click **Configure 10,000ft Plans** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cd14a-168">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="cd14a-168">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. <span data-ttu-id="cd14a-170">tooconfigure egyszeri bejelentkezést a **10 000 ft tervek** oldalon kell letöltött toosend hello **Certificate(Raw), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** túl[ 10 000 ft tervek támogatási csoport](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="cd14a-170">tooconfigure single sign-on on **10,000ft Plans** side, you need toosend hello downloaded **Certificate(Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

> [!TIP]
> <span data-ttu-id="cd14a-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="cd14a-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cd14a-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="cd14a-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cd14a-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd14a-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd14a-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd14a-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd14a-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="cd14a-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cd14a-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="cd14a-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd14a-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cd14a-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd14a-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd14a-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd14a-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd14a-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cd14a-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd14a-186">a.</span><span class="sxs-lookup"><span data-stu-id="cd14a-186">a.</span></span> <span data-ttu-id="cd14a-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd14a-188">b.</span><span class="sxs-lookup"><span data-stu-id="cd14a-188">b.</span></span> <span data-ttu-id="cd14a-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd14a-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd14a-190">c.</span><span class="sxs-lookup"><span data-stu-id="cd14a-190">c.</span></span> <span data-ttu-id="cd14a-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cd14a-192">d.</span><span class="sxs-lookup"><span data-stu-id="cd14a-192">d.</span></span> <span data-ttu-id="cd14a-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cd14a-193">Click **Create**.</span></span>
 
### <a name="creating-a-10000ft-plans-test-user"></a><span data-ttu-id="cd14a-194">A 10 000 ft létrehozása tervek teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="cd14a-194">Creating a 10,000ft Plans test user</span></span>

<span data-ttu-id="cd14a-195">hello ebben a szakaszban célja toocreate 10 000 ft tervek Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="cd14a-195">hello objective of this section is toocreate a user called Britta Simon in 10,000ft Plans.</span></span> <span data-ttu-id="cd14a-196">10 000 ft tervek támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="cd14a-196">10,000ft Plans supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="cd14a-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="cd14a-197">There is no action item for you in this section.</span></span> <span data-ttu-id="cd14a-198">Új felhasználó jön létre egy kísérlet tooaccess 10 000 ft tervek során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="cd14a-198">A new user is created during an attempt tooaccess 10,000ft Plans if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="cd14a-199">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [10 000 ft tervek támogatási csoport](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="cd14a-199">If you need toocreate a user manually, you need toocontact hello [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cd14a-200">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cd14a-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cd14a-201">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés too10, 000ft tervek megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="cd14a-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too10,000ft Plans.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cd14a-203">**tooassign Britta Simon too10 000ft terveket, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cd14a-203">**tooassign Britta Simon too10,000ft Plans, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd14a-204">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cd14a-206">Hello alkalmazások listában válassza ki a **10 000 ft tervek**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-206">In hello applications list, select **10,000ft Plans**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. <span data-ttu-id="cd14a-208">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cd14a-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cd14a-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd14a-210">Click **Add** button.</span></span> <span data-ttu-id="cd14a-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd14a-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cd14a-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cd14a-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cd14a-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd14a-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd14a-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cd14a-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cd14a-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cd14a-216">Testing single sign-on</span></span>

<span data-ttu-id="cd14a-217">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="cd14a-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="cd14a-218">Hello 10 000 ft tervek hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour 10 000 ft tervek alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cd14a-218">When you click hello 10,000ft Plans tile in hello Access Panel, you should get automatically signed-on tooyour 10,000ft Plans application.</span></span>
 
## <a name="additional-resources"></a><span data-ttu-id="cd14a-219">További források</span><span class="sxs-lookup"><span data-stu-id="cd14a-219">Additional resources</span></span>

* [<span data-ttu-id="cd14a-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="cd14a-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd14a-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cd14a-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

