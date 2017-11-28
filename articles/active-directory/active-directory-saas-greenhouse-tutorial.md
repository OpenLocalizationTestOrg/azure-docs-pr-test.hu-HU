---
title: "Oktatóanyag: Azure Active Directoryval integrált üvegházhatású |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és üvegházhatású között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 1a7cdd00c4f2b15a1afc89522d79af22f4c5d866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a><span data-ttu-id="561ed-103">Oktatóanyag: Azure Active Directoryval integrált üvegházhatású</span><span class="sxs-lookup"><span data-stu-id="561ed-103">Tutorial: Azure Active Directory integration with Greenhouse</span></span>

<span data-ttu-id="561ed-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate üvegházhatású az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="561ed-104">In this tutorial, you learn how toointegrate Greenhouse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="561ed-105">Üvegházhatást integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="561ed-105">Integrating Greenhouse with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="561ed-106">Az Azure AD hozzáférési tooGreenhouse rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="561ed-106">You can control in Azure AD who has access tooGreenhouse.</span></span>
- <span data-ttu-id="561ed-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGreenhouse (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="561ed-107">You can enable your users tooautomatically get signed-on tooGreenhouse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="561ed-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="561ed-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="561ed-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="561ed-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="561ed-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="561ed-110">Prerequisites</span></span>

<span data-ttu-id="561ed-111">tooconfigure üvegházhatású az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="561ed-111">tooconfigure Azure AD integration with Greenhouse, you need hello following items:</span></span>

- <span data-ttu-id="561ed-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="561ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="561ed-113">Egy üvegházhatást egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="561ed-113">A Greenhouse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="561ed-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="561ed-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="561ed-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="561ed-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="561ed-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="561ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="561ed-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="561ed-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="561ed-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="561ed-118">Scenario description</span></span>
<span data-ttu-id="561ed-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="561ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="561ed-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="561ed-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="561ed-121">Hello gyűjteményből üvegházhatású hozzáadása</span><span class="sxs-lookup"><span data-stu-id="561ed-121">Adding Greenhouse from hello gallery</span></span>
2. <span data-ttu-id="561ed-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="561ed-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-greenhouse-from-hello-gallery"></a><span data-ttu-id="561ed-123">Hello gyűjteményből üvegházhatású hozzáadása</span><span class="sxs-lookup"><span data-stu-id="561ed-123">Adding Greenhouse from hello gallery</span></span>
<span data-ttu-id="561ed-124">tooconfigure hello integrációja üvegházhatású az Azure AD-be, meg kell tooadd üvegházhatású hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="561ed-124">tooconfigure hello integration of Greenhouse into Azure AD, you need tooadd Greenhouse from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="561ed-125">**tooadd üvegházhatású hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="561ed-125">**tooadd Greenhouse from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="561ed-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="561ed-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="561ed-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="561ed-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="561ed-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="561ed-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="561ed-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="561ed-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="561ed-133">Hello keresési mezőbe, írja be a **üvegházhatású**, jelölje be **üvegházhatású** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="561ed-133">In hello search box, type **Greenhouse**, select **Greenhouse** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában üvegházhatású](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="561ed-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="561ed-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="561ed-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján üvegházhatású.</span><span class="sxs-lookup"><span data-stu-id="561ed-136">In this section, you configure and test Azure AD single sign-on with Greenhouse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="561ed-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó üvegházhatású tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="561ed-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Greenhouse is tooa user in Azure AD.</span></span> <span data-ttu-id="561ed-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello üvegházhatású közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="561ed-138">In other words, a link relationship between an Azure AD user and hello related user in Greenhouse needs toobe established.</span></span>

<span data-ttu-id="561ed-139">Üvegházhatást, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="561ed-139">In Greenhouse, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="561ed-140">tooconfigure és az Azure AD az egyszeri bejelentkezés üvegházhatású-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="561ed-140">tooconfigure and test Azure AD single sign-on with Greenhouse, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="561ed-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="561ed-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="561ed-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="561ed-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="561ed-143">**[Üvegházhatást tesztfelhasználó létrehozása](#create-a-greenhouse-test-user)**  -toohave egy megfelelője a Britta Simon a üvegházhatású, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="561ed-143">**[Create a Greenhouse test user](#create-a-greenhouse-test-user)** - toohave a counterpart of Britta Simon in Greenhouse that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="561ed-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="561ed-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="561ed-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="561ed-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="561ed-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="561ed-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="561ed-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az üvegházhatású alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="561ed-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Greenhouse application.</span></span>

<span data-ttu-id="561ed-148">**tooconfigure az Azure AD egyszeri bejelentkezést a üvegházhatású, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="561ed-148">**tooconfigure Azure AD single sign-on with Greenhouse, perform hello following steps:**</span></span>

1. <span data-ttu-id="561ed-149">Az Azure portál, a hello hello **üvegházhatású** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="561ed-149">In hello Azure portal, on hello **Greenhouse** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="561ed-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="561ed-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. <span data-ttu-id="561ed-153">A hello **üvegházhatású tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="561ed-153">On hello **Greenhouse Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezési adatokat üvegházhatású tartomány és az URL-címek](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    <span data-ttu-id="561ed-155">a.</span><span class="sxs-lookup"><span data-stu-id="561ed-155">a.</span></span> <span data-ttu-id="561ed-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="561ed-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.greenhouse.io`</span></span>

    <span data-ttu-id="561ed-157">b.</span><span class="sxs-lookup"><span data-stu-id="561ed-157">b.</span></span> <span data-ttu-id="561ed-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="561ed-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.greenhouse.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="561ed-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="561ed-159">These values are not real.</span></span> <span data-ttu-id="561ed-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="561ed-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="561ed-161">Ügyfél [üvegházhatású ügyfél-támogatási csoport](https://www.greenhouse.io/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="561ed-161">Contact [Greenhouse Client support team](https://www.greenhouse.io/contact) tooget these values.</span></span> 
 


4. <span data-ttu-id="561ed-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="561ed-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. <span data-ttu-id="561ed-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="561ed-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="561ed-166">tooconfigure egyszeri bejelentkezést a **üvegházhatású** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[üvegházhatású támogatási csoport](http://www.greenhouse.io/contact).</span><span class="sxs-lookup"><span data-stu-id="561ed-166">tooconfigure single sign-on on **Greenhouse** side, you need toosend hello downloaded **Metadata XML** too[Greenhouse support team](http://www.greenhouse.io/contact).</span></span>

> [!TIP]
> <span data-ttu-id="561ed-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="561ed-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="561ed-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="561ed-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="561ed-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="561ed-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="561ed-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="561ed-170">Create an Azure AD test user</span></span>

<span data-ttu-id="561ed-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="561ed-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="561ed-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="561ed-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="561ed-174">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="561ed-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="561ed-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="561ed-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="561ed-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="561ed-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="561ed-180">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="561ed-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="561ed-182">a.</span><span class="sxs-lookup"><span data-stu-id="561ed-182">a.</span></span> <span data-ttu-id="561ed-183">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="561ed-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="561ed-184">b.</span><span class="sxs-lookup"><span data-stu-id="561ed-184">b.</span></span> <span data-ttu-id="561ed-185">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="561ed-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="561ed-186">c.</span><span class="sxs-lookup"><span data-stu-id="561ed-186">c.</span></span> <span data-ttu-id="561ed-187">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="561ed-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="561ed-188">d.</span><span class="sxs-lookup"><span data-stu-id="561ed-188">d.</span></span> <span data-ttu-id="561ed-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="561ed-189">Click **Create**.</span></span>
 
### <a name="create-a-greenhouse-test-user"></a><span data-ttu-id="561ed-190">Üvegházhatást tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="561ed-190">Create a Greenhouse test user</span></span>

<span data-ttu-id="561ed-191">A sorrend tooenable az Azure AD felhasználók toolog üvegházhatású be azok kell üzembe üvegházhatású be.</span><span class="sxs-lookup"><span data-stu-id="561ed-191">In order tooenable Azure AD users toolog into Greenhouse, they must be provisioned into Greenhouse.</span></span> <span data-ttu-id="561ed-192">Üvegházhatást hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="561ed-192">In hello case of Greenhouse, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="561ed-193">Bármely más üvegházhatású felhasználói fiók létrehozása eszközök vagy üvegházhatású tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="561ed-193">You can use any other Greenhouse user account creation tools or APIs provided by Greenhouse tooprovision AAD user accounts.</span></span> 

<span data-ttu-id="561ed-194">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="561ed-194">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="561ed-195">Jelentkezzen be tooyour **üvegházhatású** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="561ed-195">Log in tooyour **Greenhouse** company site as an administrator.</span></span>

2. <span data-ttu-id="561ed-196">Hello hello felső menüben kattintson a **konfigurálása**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="561ed-196">In hello menu on hello top, click **Configure**, and then click **Users**.</span></span>
   
   <span data-ttu-id="561ed-197">![Felhasználók](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="561ed-197">![Users](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "Users")</span></span>

3. <span data-ttu-id="561ed-198">Kattintson a **új felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="561ed-198">Click **New Users**.</span></span>
   
   <span data-ttu-id="561ed-199">![Új felhasználó](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="561ed-199">![New User](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "New User")</span></span>

4. <span data-ttu-id="561ed-200">A hello **új felhasználó hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="561ed-200">In hello **Add New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="561ed-201">![Új felhasználó hozzáadása](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "új felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="561ed-201">![Add New User](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "Add New User")</span></span>

   <span data-ttu-id="561ed-202">a.</span><span class="sxs-lookup"><span data-stu-id="561ed-202">a.</span></span> <span data-ttu-id="561ed-203">A hello **adja meg a felhasználói e-mailek** szövegmezőhöz típusú hello e-mail cím egy érvényes Azure Active Directory-fióknevet, amelyet tooprovision.</span><span class="sxs-lookup"><span data-stu-id="561ed-203">In hello **Enter user emails** textbox, type hello email address of a valid Azure Active Directory account you want tooprovision.</span></span>

   <span data-ttu-id="561ed-204">b.</span><span class="sxs-lookup"><span data-stu-id="561ed-204">b.</span></span> <span data-ttu-id="561ed-205">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="561ed-205">Click **Save**.</span></span>    
   
      >[!NOTE]
      ><span data-ttu-id="561ed-206">hello Azure Active Directory-fiókhoz tulajdonosainak kap egy e-mailt, egy hivatkozásra tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="561ed-206">hello Azure Active Directory account holders will receive an email including a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="561ed-207">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="561ed-207">Assign hello Azure AD test user</span></span>

<span data-ttu-id="561ed-208">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooGreenhouse megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="561ed-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGreenhouse.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="561ed-210">**tooassign Britta Simon tooGreenhouse, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="561ed-210">**tooassign Britta Simon tooGreenhouse, perform hello following steps:**</span></span>

1. <span data-ttu-id="561ed-211">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="561ed-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="561ed-213">Hello alkalmazások listában válassza ki a **üvegházhatású**.</span><span class="sxs-lookup"><span data-stu-id="561ed-213">In hello applications list, select **Greenhouse**.</span></span>

    ![hello üvegházhatású hivatkozásra hello alkalmazások listája](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. <span data-ttu-id="561ed-215">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="561ed-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="561ed-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="561ed-217">Click **Add** button.</span></span> <span data-ttu-id="561ed-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="561ed-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="561ed-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="561ed-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="561ed-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="561ed-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="561ed-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="561ed-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="561ed-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="561ed-223">Test single sign-on</span></span>

<span data-ttu-id="561ed-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="561ed-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="561ed-225">Hello üvegházhatású csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour üvegházhatású alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="561ed-225">When you click hello Greenhouse tile in hello Access Panel, you should get automatically signed-on tooyour Greenhouse application.</span></span>
<span data-ttu-id="561ed-226">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="561ed-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="561ed-227">További források</span><span class="sxs-lookup"><span data-stu-id="561ed-227">Additional resources</span></span>

* [<span data-ttu-id="561ed-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="561ed-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="561ed-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="561ed-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

