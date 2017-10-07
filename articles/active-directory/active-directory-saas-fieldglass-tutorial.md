---
title: "Oktatóanyag: Azure Active Directoryval integrált Fieldglass |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Fieldglass között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2510195f-d5b1-4684-b3da-283fb8619df2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d953996bc3bf5721b8280dae4b9992aef7934b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fieldglass"></a><span data-ttu-id="6e3eb-103">Oktatóanyag: Azure Active Directoryval integrált Fieldglass</span><span class="sxs-lookup"><span data-stu-id="6e3eb-103">Tutorial: Azure Active Directory integration with Fieldglass</span></span>

<span data-ttu-id="6e3eb-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Fieldglass az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e3eb-104">In this tutorial, you learn how toointegrate Fieldglass with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6e3eb-105">Fieldglass integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-105">Integrating Fieldglass with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6e3eb-106">Megadhatja a hozzáférés tooFieldglass rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6e3eb-106">You can control in Azure AD who has access tooFieldglass</span></span>
- <span data-ttu-id="6e3eb-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFieldglass (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6e3eb-107">You can enable your users tooautomatically get signed-on tooFieldglass (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6e3eb-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6e3eb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6e3eb-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6e3eb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e3eb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6e3eb-110">Prerequisites</span></span>

<span data-ttu-id="6e3eb-111">az Azure AD integrálása Fieldglass tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-111">tooconfigure Azure AD integration with Fieldglass, you need hello following items:</span></span>

- <span data-ttu-id="6e3eb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6e3eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6e3eb-113">Egy Fieldglass egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6e3eb-113">A Fieldglass single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6e3eb-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6e3eb-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6e3eb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6e3eb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6e3eb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6e3eb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6e3eb-118">Scenario description</span></span>
<span data-ttu-id="6e3eb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6e3eb-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6e3eb-121">Hello gyűjteményből Fieldglass hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6e3eb-121">Adding Fieldglass from hello gallery</span></span>
2. <span data-ttu-id="6e3eb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6e3eb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fieldglass-from-hello-gallery"></a><span data-ttu-id="6e3eb-123">Hello gyűjteményből Fieldglass hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6e3eb-123">Adding Fieldglass from hello gallery</span></span>
<span data-ttu-id="6e3eb-124">tooconfigure hello integrációja Fieldglass az Azure AD-be, meg kell tooadd Fieldglass hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-124">tooconfigure hello integration of Fieldglass into Azure AD, you need tooadd Fieldglass from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6e3eb-125">**tooadd Fieldglass hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6e3eb-125">**tooadd Fieldglass from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e3eb-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6e3eb-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6e3eb-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6e3eb-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6e3eb-133">Hello keresési mezőbe, írja be a **Fieldglass**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-133">In hello search box, type **Fieldglass**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_search.png)

5. <span data-ttu-id="6e3eb-135">A hello eredmények panelen válassza ki a **Fieldglass**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-135">In hello results panel, select **Fieldglass**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6e3eb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6e3eb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6e3eb-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Fieldglass</span><span class="sxs-lookup"><span data-stu-id="6e3eb-138">In this section, you configure and test Azure AD single sign-on with Fieldglass based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6e3eb-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Fieldglass tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fieldglass is tooa user in Azure AD.</span></span> <span data-ttu-id="6e3eb-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Fieldglass közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-140">In other words, a link relationship between an Azure AD user and hello related user in Fieldglass needs toobe established.</span></span>

<span data-ttu-id="6e3eb-141">Fieldglass, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-141">In Fieldglass, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6e3eb-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Fieldglass-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-142">tooconfigure and test Azure AD single sign-on with Fieldglass, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6e3eb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6e3eb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6e3eb-145">**[Fieldglass tesztfelhasználó létrehozása](#creating-a-fieldglass-test-user)**  -toohave egy megfelelője a Britta Simon a Fieldglass, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-145">**[Creating a Fieldglass test user](#creating-a-fieldglass-test-user)** - toohave a counterpart of Britta Simon in Fieldglass that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6e3eb-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6e3eb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6e3eb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e3eb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6e3eb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Fieldglass alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Fieldglass application.</span></span>

<span data-ttu-id="6e3eb-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Fieldglass, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6e3eb-150">**tooconfigure Azure AD single sign-on with Fieldglass, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e3eb-151">Az Azure portál, a hello hello **Fieldglass** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-151">In hello Azure portal, on hello **Fieldglass** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6e3eb-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_samlbase.png)

3. <span data-ttu-id="6e3eb-155">A hello **Fieldglass tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-155">On hello **Fieldglass Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_url.png)

    <span data-ttu-id="6e3eb-157">a.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-157">a.</span></span> <span data-ttu-id="6e3eb-158">A hello **azonosító** szövegmező, adja meg az URL-címet `https://www.fieldglass.com` hello mintát követi:`https://<company name>.fgvms.com`</span><span class="sxs-lookup"><span data-stu-id="6e3eb-158">In hello **Identifier** textbox, type a URL as  `https://www.fieldglass.com` or follow hello pattern:  `https://<company name>.fgvms.com`</span></span>

    <span data-ttu-id="6e3eb-159">b.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-159">b.</span></span> <span data-ttu-id="6e3eb-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://www.fieldglass.net/<company name>`|
    | `https://<company name>.fgvms.com/<company name>`|

    > [!NOTE] 
    > <span data-ttu-id="6e3eb-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-161">These values are not real.</span></span> <span data-ttu-id="6e3eb-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="6e3eb-163">Ügyfél [Fieldglass támogatási csoport](http://www.fieldglass.com/solutions/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-163">Contact [Fieldglass support team](http://www.fieldglass.com/solutions/support) tooget these values.</span></span>
 
4. <span data-ttu-id="6e3eb-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_certificate.png) 

5. <span data-ttu-id="6e3eb-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fieldglass-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6e3eb-168">A hello **Fieldglass konfigurációs** kattintson **konfigurálása Fieldglass** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-168">On hello **Fieldglass Configuration** section, click **Configure Fieldglass** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6e3eb-169">Másolás hello **Sign-Out URL-cím és a SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6e3eb-169">Copy hello **Sign-Out URL and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_configure.png) 

7. <span data-ttu-id="6e3eb-171">tooconfigure egyszeri bejelentkezést a **Fieldglass** oldalon kell letöltött toosend hello **Certificate(Base64)** és **Sign-Out URL, SAML Entitásazonosító** túl[ Fieldglass támogatási csoport](http://www.fieldglass.com/solutions/support).</span><span class="sxs-lookup"><span data-stu-id="6e3eb-171">tooconfigure single sign-on on **Fieldglass** side, you need toosend hello downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID** too[Fieldglass support team](http://www.fieldglass.com/solutions/support).</span></span> <span data-ttu-id="6e3eb-172">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="6e3eb-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="6e3eb-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6e3eb-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6e3eb-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6e3eb-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6e3eb-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e3eb-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="6e3eb-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6e3eb-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6e3eb-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e3eb-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fieldglass-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6e3eb-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fieldglass-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6e3eb-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fieldglass-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6e3eb-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6e3eb-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fieldglass-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6e3eb-188">a.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-188">a.</span></span> <span data-ttu-id="6e3eb-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6e3eb-190">b.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-190">b.</span></span> <span data-ttu-id="6e3eb-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6e3eb-192">c.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-192">c.</span></span> <span data-ttu-id="6e3eb-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6e3eb-194">d.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-194">d.</span></span> <span data-ttu-id="6e3eb-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-195">Click **Create**.</span></span>
 
### <a name="creating-a-fieldglass-test-user"></a><span data-ttu-id="6e3eb-196">Fieldglass tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e3eb-196">Creating a Fieldglass test user</span></span>

<span data-ttu-id="6e3eb-197">hello ebben a szakaszban célja toocreate FieldGlass Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-197">hello objective of this section is toocreate a user called Britta Simon in FieldGlass.</span></span> <span data-ttu-id="6e3eb-198">Adjon dolgozni a [Fieldglass támogatási csoport](http://www.fieldglass.com/solutions/support) tooadd hello felhasználók hello Fieldglass fiók.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-198">Please work with your [Fieldglass support team](http://www.fieldglass.com/solutions/support) tooadd hello users in hello Fieldglass account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6e3eb-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6e3eb-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6e3eb-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFieldglass megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFieldglass.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6e3eb-202">**tooassign Britta Simon tooFieldglass, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6e3eb-202">**tooassign Britta Simon tooFieldglass, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e3eb-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6e3eb-205">Hello alkalmazások listában válassza ki a **Fieldglass**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-205">In hello applications list, select **Fieldglass**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fieldglass-tutorial/tutorial_fieldglass_app.png) 

3. <span data-ttu-id="6e3eb-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6e3eb-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-209">Click **Add** button.</span></span> <span data-ttu-id="6e3eb-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6e3eb-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6e3eb-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6e3eb-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6e3eb-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6e3eb-215">Testing single sign-on</span></span>

<span data-ttu-id="6e3eb-216">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-216">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6e3eb-217">Hello Fieldglass hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Fieldglass alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6e3eb-217">When you click hello Fieldglass tile in hello Access Panel, you should get automatically signed-on tooyour Fieldglass application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e3eb-218">További források</span><span class="sxs-lookup"><span data-stu-id="6e3eb-218">Additional resources</span></span>

* [<span data-ttu-id="6e3eb-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6e3eb-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6e3eb-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6e3eb-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fieldglass-tutorial/tutorial_general_203.png

