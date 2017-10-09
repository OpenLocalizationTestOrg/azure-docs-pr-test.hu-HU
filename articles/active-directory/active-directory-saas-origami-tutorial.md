---
title: "Oktatóanyag: Azure Active Directoryval integrált Origami |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Origami között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a45f2d2b8d2271cf0fc58cb8fad92f007cb5e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a><span data-ttu-id="d06d5-103">Oktatóanyag: Azure Active Directoryval integrált Origami</span><span class="sxs-lookup"><span data-stu-id="d06d5-103">Tutorial: Azure Active Directory integration with Origami</span></span>

<span data-ttu-id="d06d5-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Origami az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d06d5-104">In this tutorial, you learn how toointegrate Origami with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d06d5-105">Origami integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d06d5-105">Integrating Origami with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d06d5-106">Megadhatja a hozzáférés tooOrigami rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d06d5-106">You can control in Azure AD who has access tooOrigami</span></span>
- <span data-ttu-id="d06d5-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOrigami (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d06d5-107">You can enable your users tooautomatically get signed-on tooOrigami (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d06d5-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d06d5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d06d5-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d06d5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d06d5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d06d5-110">Prerequisites</span></span>

<span data-ttu-id="d06d5-111">az Azure AD integrálása Origami tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d06d5-111">tooconfigure Azure AD integration with Origami, you need hello following items:</span></span>

- <span data-ttu-id="d06d5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d06d5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d06d5-113">Egy Origami egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d06d5-113">An Origami single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d06d5-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d06d5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d06d5-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d06d5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d06d5-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d06d5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d06d5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d06d5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d06d5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d06d5-118">Scenario description</span></span>
<span data-ttu-id="d06d5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d06d5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d06d5-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d06d5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d06d5-121">Hello gyűjteményből Origami hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d06d5-121">Adding Origami from hello gallery</span></span>
2. <span data-ttu-id="d06d5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d06d5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-origami-from-hello-gallery"></a><span data-ttu-id="d06d5-123">Hello gyűjteményből Origami hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d06d5-123">Adding Origami from hello gallery</span></span>
<span data-ttu-id="d06d5-124">tooconfigure hello integrációja Origami az Azure AD-be, meg kell tooadd Origami hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d06d5-124">tooconfigure hello integration of Origami into Azure AD, you need tooadd Origami from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d06d5-125">**tooadd Origami hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d06d5-125">**tooadd Origami from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d06d5-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d06d5-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d06d5-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d06d5-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d06d5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d06d5-133">Hello keresési mezőbe, írja be a **Origami**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-133">In hello search box, type **Origami**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. <span data-ttu-id="d06d5-135">A hello eredmények panelen válassza ki a **Origami**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-135">In hello results panel, select **Origami**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d06d5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d06d5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d06d5-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Origami.</span><span class="sxs-lookup"><span data-stu-id="d06d5-138">In this section, you configure and test Azure AD single sign-on with Origami based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d06d5-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Origami tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d06d5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Origami is tooa user in Azure AD.</span></span> <span data-ttu-id="d06d5-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Origami közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d06d5-140">In other words, a link relationship between an Azure AD user and hello related user in Origami needs toobe established.</span></span>

<span data-ttu-id="d06d5-141">Origami, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d06d5-141">In Origami, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d06d5-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Origami-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d06d5-142">tooconfigure and test Azure AD single sign-on with Origami, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d06d5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d06d5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d06d5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d06d5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d06d5-145">**[Egy Origami tesztfelhasználó létrehozása](#creating-an-origami-test-user)**  -toohave egy megfelelője a Britta Simon a Origami, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d06d5-145">**[Creating an Origami test user](#creating-an-origami-test-user)** - toohave a counterpart of Britta Simon in Origami that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d06d5-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d06d5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d06d5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d06d5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d06d5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d06d5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d06d5-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Origami alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="d06d5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Origami application.</span></span>

<span data-ttu-id="d06d5-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Origami, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d06d5-150">**tooconfigure Azure AD single sign-on with Origami, perform hello following steps:**</span></span>

1. <span data-ttu-id="d06d5-151">Az Azure portál, a hello hello **Origami** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-151">In hello Azure portal, on hello **Origami** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d06d5-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d06d5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. <span data-ttu-id="d06d5-155">A hello **Origami tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d06d5-155">On hello **Origami Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    <span data-ttu-id="d06d5-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://live.origamirisk.com/origami/account/login?account=<companyname>`</span><span class="sxs-lookup"><span data-stu-id="d06d5-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://live.origamirisk.com/origami/account/login?account=<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d06d5-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="d06d5-158">hello value is not real.</span></span> <span data-ttu-id="d06d5-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d06d5-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="d06d5-160">Ügyfél [Origami ügyfél-támogatási csoport](https://wordpress.org/support/theme/origami) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="d06d5-160">Contact [Origami Client support team](https://wordpress.org/support/theme/origami) tooget hello value.</span></span> 
 
4. <span data-ttu-id="d06d5-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d06d5-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. <span data-ttu-id="d06d5-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d06d5-165">A hello **Origami konfigurációs** kattintson **konfigurálása Origami** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d06d5-165">On hello **Origami Configuration** section, click **Configure Origami** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d06d5-166">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d06d5-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. <span data-ttu-id="d06d5-168">Jelentkezzen be toohello Origami fiók rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="d06d5-168">Log in toohello Origami account with Admin rights.</span></span>

8. <span data-ttu-id="d06d5-169">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-169">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. <span data-ttu-id="d06d5-171">Hello egyszeri bejelentkezést a telepítő párbeszédpanel lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="d06d5-171">On hello Single Sign On Setup dialog page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    <span data-ttu-id="d06d5-173">a.</span><span class="sxs-lookup"><span data-stu-id="d06d5-173">a.</span></span> <span data-ttu-id="d06d5-174">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-174">Select **Enable Single Sign On**.</span></span>

    <span data-ttu-id="d06d5-175">b.</span><span class="sxs-lookup"><span data-stu-id="d06d5-175">b.</span></span> <span data-ttu-id="d06d5-176">A hello **identitásszolgáltató bejelentkezési lap URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d06d5-176">In hello **Identity Provider's Sign-in Page URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d06d5-177">c.</span><span class="sxs-lookup"><span data-stu-id="d06d5-177">c.</span></span> <span data-ttu-id="d06d5-178">A hello **identitásszolgáltató Sign-out lap URL-címe** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d06d5-178">In hello **Identity Provider's Sign-out Page URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d06d5-179">d.</span><span class="sxs-lookup"><span data-stu-id="d06d5-179">d.</span></span> <span data-ttu-id="d06d5-180">Kattintson a **Tallózás** tooupload hello tanúsítvány már letöltötte az Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="d06d5-180">Click **Browse** tooupload hello certificate you have downloaded from hello Azure portal.</span></span>

    <span data-ttu-id="d06d5-181">e.</span><span class="sxs-lookup"><span data-stu-id="d06d5-181">e.</span></span> <span data-ttu-id="d06d5-182">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="d06d5-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d06d5-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d06d5-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d06d5-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d06d5-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d06d5-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d06d5-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d06d5-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="d06d5-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d06d5-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d06d5-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d06d5-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d06d5-190">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d06d5-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d06d5-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d06d5-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d06d5-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d06d5-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d06d5-198">a.</span><span class="sxs-lookup"><span data-stu-id="d06d5-198">a.</span></span> <span data-ttu-id="d06d5-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d06d5-200">b.</span><span class="sxs-lookup"><span data-stu-id="d06d5-200">b.</span></span> <span data-ttu-id="d06d5-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d06d5-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d06d5-202">c.</span><span class="sxs-lookup"><span data-stu-id="d06d5-202">c.</span></span> <span data-ttu-id="d06d5-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d06d5-204">d.</span><span class="sxs-lookup"><span data-stu-id="d06d5-204">d.</span></span> <span data-ttu-id="d06d5-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-205">Click **Create**.</span></span>
 
### <a name="creating-an-origami-test-user"></a><span data-ttu-id="d06d5-206">Egy Origami tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d06d5-206">Creating an Origami test user</span></span>

<span data-ttu-id="d06d5-207">Ebben a szakaszban egy Origami Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d06d5-207">In this section, you create a user called Britta Simon in Origami.</span></span> 

1. <span data-ttu-id="d06d5-208">Jelentkezzen be toohello Origami fiók rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="d06d5-208">Log in toohello Origami account with Admin rights.</span></span>

2. <span data-ttu-id="d06d5-209">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-209">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. <span data-ttu-id="d06d5-211">A hello **felhasználók és biztonsági** párbeszédpanel, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-211">On hello **Users and Security** dialog, click **Users**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. <span data-ttu-id="d06d5-213">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-213">Click **Add New User**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. <span data-ttu-id="d06d5-215">Hello új felhasználó hozzáadása párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="d06d5-215">On hello Add New User dialog, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    <span data-ttu-id="d06d5-217">a.</span><span class="sxs-lookup"><span data-stu-id="d06d5-217">a.</span></span> <span data-ttu-id="d06d5-218">A hello **felhasználónév** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d06d5-218">In hello **User Name** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="d06d5-219">b.</span><span class="sxs-lookup"><span data-stu-id="d06d5-219">b.</span></span> <span data-ttu-id="d06d5-220">A hello **jelszó** szövegmező, a jelszót írja be.</span><span class="sxs-lookup"><span data-stu-id="d06d5-220">In hello **Password** textbox, type a password.</span></span>

    <span data-ttu-id="d06d5-221">c.</span><span class="sxs-lookup"><span data-stu-id="d06d5-221">c.</span></span> <span data-ttu-id="d06d5-222">A hello **jelszó megerősítése** szövegmezőhöz típus hello jelszót még egyszer.</span><span class="sxs-lookup"><span data-stu-id="d06d5-222">In hello **Confirm Password** textbox, type hello password again.</span></span>

    <span data-ttu-id="d06d5-223">d.</span><span class="sxs-lookup"><span data-stu-id="d06d5-223">d.</span></span> <span data-ttu-id="d06d5-224">A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-224">In hello **First Name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="d06d5-225">e.</span><span class="sxs-lookup"><span data-stu-id="d06d5-225">e.</span></span> <span data-ttu-id="d06d5-226">A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-226">In hello **Last Name** textbox, enter hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="d06d5-227">f.</span><span class="sxs-lookup"><span data-stu-id="d06d5-227">f.</span></span> <span data-ttu-id="d06d5-228">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-228">Click **Save**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. <span data-ttu-id="d06d5-230">Rendelje hozzá **felhasználói szerepkörök** és **ügyfél-hozzáférési** toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d06d5-230">Assign **User Roles** and **Client Access** toohello user.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d06d5-232">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d06d5-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d06d5-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOrigami megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d06d5-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOrigami.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d06d5-235">**tooassign Britta Simon tooOrigami, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d06d5-235">**tooassign Britta Simon tooOrigami, perform hello following steps:**</span></span>

1. <span data-ttu-id="d06d5-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d06d5-238">Hello alkalmazások listában válassza ki a **Origami**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-238">In hello applications list, select **Origami**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. <span data-ttu-id="d06d5-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d06d5-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d06d5-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d06d5-242">Click **Add** button.</span></span> <span data-ttu-id="d06d5-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d06d5-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d06d5-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d06d5-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d06d5-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d06d5-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d06d5-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d06d5-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d06d5-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d06d5-248">Testing single sign-on</span></span>

<span data-ttu-id="d06d5-249">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d06d5-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d06d5-250">Ha a hozzáférési Panel hello hello Origami csempe gombra kattint, automatikusan bejelentkezett tooyour Origami alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d06d5-250">When you click hello Origami tile in hello Access Panel, you should get automatically signed-on tooyour Origami application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d06d5-251">További források</span><span class="sxs-lookup"><span data-stu-id="d06d5-251">Additional resources</span></span>

* [<span data-ttu-id="d06d5-252">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d06d5-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d06d5-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d06d5-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

