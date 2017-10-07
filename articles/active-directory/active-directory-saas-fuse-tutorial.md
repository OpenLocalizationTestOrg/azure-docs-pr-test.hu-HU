---
title: "Oktatóanyag: Azure Active Directoryval integrált biztosító |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és biztosító között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 720ed8af0b5de1e3bee5a40353ca0ee661766864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a><span data-ttu-id="24e17-103">Oktatóanyag: Azure Active Directoryval integrált biztosító</span><span class="sxs-lookup"><span data-stu-id="24e17-103">Tutorial: Azure Active Directory integration with Fuse</span></span>

<span data-ttu-id="24e17-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate biztosítéka az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="24e17-104">In this tutorial, you learn how toointegrate Fuse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="24e17-105">Biztosító integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="24e17-105">Integrating Fuse with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="24e17-106">Az Azure AD hozzáférési tooFuse rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="24e17-106">You can control in Azure AD who has access tooFuse.</span></span>
- <span data-ttu-id="24e17-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFuse (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="24e17-107">You can enable your users tooautomatically get signed-on tooFuse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="24e17-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="24e17-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="24e17-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="24e17-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24e17-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="24e17-110">Prerequisites</span></span>

<span data-ttu-id="24e17-111">Biztosító tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="24e17-111">tooconfigure Azure AD integration with Fuse, you need hello following items:</span></span>

- <span data-ttu-id="24e17-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="24e17-112">An Azure AD subscription</span></span>
- <span data-ttu-id="24e17-113">Egy biztosító egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="24e17-113">A Fuse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="24e17-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="24e17-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="24e17-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="24e17-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="24e17-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="24e17-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="24e17-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="24e17-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="24e17-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="24e17-118">Scenario description</span></span>
<span data-ttu-id="24e17-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="24e17-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="24e17-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="24e17-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="24e17-121">Adja hozzá a biztosító hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="24e17-121">Add Fuse from hello gallery</span></span>
2. <span data-ttu-id="24e17-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24e17-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-fuse-from-hello-gallery"></a><span data-ttu-id="24e17-123">Adja hozzá a biztosító hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="24e17-123">Add Fuse from hello gallery</span></span>
<span data-ttu-id="24e17-124">tooconfigure hello integrációs mindig az Azure AD-be, meg kell tooadd biztosító hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="24e17-124">tooconfigure hello integration of Fuse into Azure AD, you need tooadd Fuse from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="24e17-125">**tooadd biztosító hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="24e17-125">**tooadd Fuse from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="24e17-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="24e17-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="24e17-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="24e17-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="24e17-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="24e17-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="24e17-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="24e17-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="24e17-133">Hello keresési mezőbe, írja be a **biztosítéka**, jelölje be **biztosítéka** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="24e17-133">In hello search box, type **Fuse**, select **Fuse** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Olvadóbiztosíték a hello eredményeinek listája](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="24e17-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24e17-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="24e17-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezést biztosító "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="24e17-136">In this section, you configure and test Azure AD single sign-on with Fuse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="24e17-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó biztosító tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="24e17-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fuse is tooa user in Azure AD.</span></span> <span data-ttu-id="24e17-138">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználóknak hello biztosító igények toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="24e17-138">In other words, a link relationship between an Azure AD user and hello related user in Fuse needs toobe established.</span></span>

<span data-ttu-id="24e17-139">Biztosító, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="24e17-139">In Fuse, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="24e17-140">tooconfigure és az Azure AD egyszeri bejelentkezést biztosító-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="24e17-140">tooconfigure and test Azure AD single sign-on with Fuse, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="24e17-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="24e17-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="24e17-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="24e17-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="24e17-143">**[Biztosító tesztfelhasználó létrehozása](#create-a-fuse-test-user)**  -toohave egy megfelelője a Britta Simon a biztosító, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="24e17-143">**[Create a Fuse test user](#create-a-fuse-test-user)** - toohave a counterpart of Britta Simon in Fuse that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="24e17-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="24e17-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="24e17-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="24e17-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="24e17-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24e17-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="24e17-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az biztosító alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="24e17-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Fuse application.</span></span>

<span data-ttu-id="24e17-148">**Biztosító, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="24e17-148">**tooconfigure Azure AD single sign-on with Fuse, perform hello following steps:**</span></span>

1. <span data-ttu-id="24e17-149">Az Azure portál, a hello hello **biztosítéka** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="24e17-149">In hello Azure portal, on hello **Fuse** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="24e17-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="24e17-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_samlbase.png)

3. <span data-ttu-id="24e17-153">A hello **biztosítéka tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="24e17-153">On hello **Fuse Domain and URLs** section, perform hello following steps:</span></span>

    ![Biztosító tartomány és az URL-címek egyetlen bejelentkezés információk](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_url.png)
    
    <span data-ttu-id="24e17-155">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant name>.fusion-universal.com/`</span><span class="sxs-lookup"><span data-stu-id="24e17-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant name>.fusion-universal.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="24e17-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="24e17-156">This value is not real.</span></span> <span data-ttu-id="24e17-157">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="24e17-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="24e17-158">Ügyfél [biztosítéka ügyfél-támogatási csoport](mailto:support@fusion-universal.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="24e17-158">Contact [Fuse Client support team](mailto:support@fusion-universal.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="24e17-159">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="24e17-159">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_certificate.png) 

5. <span data-ttu-id="24e17-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="24e17-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-fuse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="24e17-163">A hello **biztosítéka konfigurációs** kattintson **konfigurálása biztosítéka** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="24e17-163">On hello **Fuse Configuration** section, click **Configure Fuse** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="24e17-164">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="24e17-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Biztosító konfiguráció](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_configure.png) 

7. <span data-ttu-id="24e17-166">az alkalmazáshoz konfigurált SSO tooget, forduljon a [biztosító támogatási csoport](mailto:support@fusion-universal.com) és adja meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="24e17-166">tooget SSO configured for your application, contact [Fuse support team](mailto:support@fusion-universal.com) and provide them with hello following:</span></span>

    * <span data-ttu-id="24e17-167">letöltött hello **Tanúsítványfájl (Raw)**</span><span class="sxs-lookup"><span data-stu-id="24e17-167">hello downloaded **Certificate (Raw) file**</span></span>
    * <span data-ttu-id="24e17-168">Hello **SAML-alapú egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="24e17-168">hello **SAML Single Sign-On Service URL**</span></span>
    * <span data-ttu-id="24e17-169">Hello **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="24e17-169">hello **SAML Entity ID**</span></span>
    * <span data-ttu-id="24e17-170">Hello **Sign-Out URL-címe**</span><span class="sxs-lookup"><span data-stu-id="24e17-170">hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="24e17-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="24e17-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="24e17-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="24e17-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="24e17-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="24e17-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="24e17-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="24e17-174">Create an Azure AD test user</span></span>

<span data-ttu-id="24e17-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="24e17-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="24e17-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="24e17-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="24e17-178">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="24e17-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-fuse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="24e17-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="24e17-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-fuse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="24e17-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="24e17-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="24e17-184">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="24e17-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="24e17-186">a.</span><span class="sxs-lookup"><span data-stu-id="24e17-186">a.</span></span> <span data-ttu-id="24e17-187">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="24e17-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="24e17-188">b.</span><span class="sxs-lookup"><span data-stu-id="24e17-188">b.</span></span> <span data-ttu-id="24e17-189">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="24e17-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="24e17-190">c.</span><span class="sxs-lookup"><span data-stu-id="24e17-190">c.</span></span> <span data-ttu-id="24e17-191">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="24e17-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="24e17-192">d.</span><span class="sxs-lookup"><span data-stu-id="24e17-192">d.</span></span> <span data-ttu-id="24e17-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="24e17-193">Click **Create**.</span></span>
 
### <a name="create-a-fuse-test-user"></a><span data-ttu-id="24e17-194">Biztosító tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="24e17-194">Create a Fuse test user</span></span>

<span data-ttu-id="24e17-195">Ebben a szakaszban egy biztosító Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="24e17-195">In this section, you create a user called Britta Simon in Fuse.</span></span> <span data-ttu-id="24e17-196">Adjon együttműködve [biztosító támogatási csoport](mailto:support@fusion-universal.com) tooadd hello felhasználók hello biztosító platform.</span><span class="sxs-lookup"><span data-stu-id="24e17-196">Please work with [Fuse support team](mailto:support@fusion-universal.com) tooadd hello users in hello Fuse platform.</span></span>


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="24e17-197">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="24e17-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="24e17-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFuse megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="24e17-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFuse.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="24e17-200">**tooassign Britta Simon tooFuse, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="24e17-200">**tooassign Britta Simon tooFuse, perform hello following steps:**</span></span>

1. <span data-ttu-id="24e17-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="24e17-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="24e17-203">Hello alkalmazások listában válassza ki a **biztosítéka**.</span><span class="sxs-lookup"><span data-stu-id="24e17-203">In hello applications list, select **Fuse**.</span></span>

    ![hello biztosító hivatkozásra hello alkalmazások listája](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_app.png)  

3. <span data-ttu-id="24e17-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="24e17-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="24e17-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="24e17-207">Click **Add** button.</span></span> <span data-ttu-id="24e17-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="24e17-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="24e17-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="24e17-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="24e17-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="24e17-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="24e17-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="24e17-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="24e17-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="24e17-213">Test single sign-on</span></span>

<span data-ttu-id="24e17-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="24e17-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="24e17-215">Ha a hozzáférési Panel hello hello biztosító csempe gombra kattint, automatikusan bejelentkezett tooyour biztosító alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="24e17-215">When you click hello Fuse tile in hello Access Panel, you should get automatically signed-on tooyour Fuse application.</span></span>
<span data-ttu-id="24e17-216">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="24e17-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="24e17-217">További források</span><span class="sxs-lookup"><span data-stu-id="24e17-217">Additional resources</span></span>

* [<span data-ttu-id="24e17-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="24e17-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="24e17-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="24e17-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png

