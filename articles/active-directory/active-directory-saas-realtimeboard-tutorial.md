---
title: "Oktatóanyag: Azure Active Directoryval integrált RealtimeBoard |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és RealtimeBoard között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a37fc1c0-4bae-4173-989b-00de53a0076f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: jeedes
ms.openlocfilehash: 76644c9ba643d61a903295dea4d417716a47774a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-realtimeboard"></a><span data-ttu-id="e056b-103">Oktatóanyag: Azure Active Directoryval integrált RealtimeBoard</span><span class="sxs-lookup"><span data-stu-id="e056b-103">Tutorial: Azure Active Directory integration with RealtimeBoard</span></span>

<span data-ttu-id="e056b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate RealtimeBoard az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e056b-104">In this tutorial, you learn how toointegrate RealtimeBoard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e056b-105">RealtimeBoard integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e056b-105">Integrating RealtimeBoard with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e056b-106">Az Azure AD hozzáférési tooRealtimeBoard rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e056b-106">You can control in Azure AD who has access tooRealtimeBoard.</span></span>
- <span data-ttu-id="e056b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRealtimeBoard (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="e056b-107">You can enable your users tooautomatically get signed-on tooRealtimeBoard (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e056b-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="e056b-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="e056b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e056b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e056b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e056b-110">Prerequisites</span></span>

<span data-ttu-id="e056b-111">az Azure AD integrálása RealtimeBoard tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e056b-111">tooconfigure Azure AD integration with RealtimeBoard, you need hello following items:</span></span>

- <span data-ttu-id="e056b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e056b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e056b-113">Egy RealtimeBoard egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e056b-113">A RealtimeBoard single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e056b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e056b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e056b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e056b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e056b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e056b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e056b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e056b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e056b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e056b-118">Scenario description</span></span>
<span data-ttu-id="e056b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e056b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e056b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e056b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e056b-121">Hello gyűjteményből RealtimeBoard hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e056b-121">Adding RealtimeBoard from hello gallery</span></span>
2. <span data-ttu-id="e056b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e056b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-realtimeboard-from-hello-gallery"></a><span data-ttu-id="e056b-123">Hello gyűjteményből RealtimeBoard hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e056b-123">Adding RealtimeBoard from hello gallery</span></span>
<span data-ttu-id="e056b-124">tooconfigure hello integrációja RealtimeBoard az Azure AD-be, meg kell tooadd RealtimeBoard hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e056b-124">tooconfigure hello integration of RealtimeBoard into Azure AD, you need tooadd RealtimeBoard from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e056b-125">**tooadd RealtimeBoard hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e056b-125">**tooadd RealtimeBoard from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e056b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e056b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="e056b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e056b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e056b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e056b-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="e056b-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e056b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="e056b-133">Hello keresési mezőbe, írja be a **RealtimeBoard**, jelölje be **RealtimeBoard** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e056b-133">In hello search box, type **RealtimeBoard**, select **RealtimeBoard** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában RealtimeBoard](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e056b-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e056b-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e056b-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="e056b-136">In this section, you configure and test Azure AD single sign-on with RealtimeBoard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e056b-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó RealtimeBoard tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e056b-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RealtimeBoard is tooa user in Azure AD.</span></span> <span data-ttu-id="e056b-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello RealtimeBoard közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e056b-138">In other words, a link relationship between an Azure AD user and hello related user in RealtimeBoard needs toobe established.</span></span>

<span data-ttu-id="e056b-139">RealtimeBoard, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e056b-139">In RealtimeBoard, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e056b-140">tooconfigure és az Azure AD az egyszeri bejelentkezés RealtimeBoard-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e056b-140">tooconfigure and test Azure AD single sign-on with RealtimeBoard, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e056b-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e056b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e056b-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e056b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e056b-143">**[RealtimeBoard tesztfelhasználó létrehozása](#create-a-realtimeboard-test-user)**  -toohave egy megfelelője a Britta Simon a RealtimeBoard, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e056b-143">**[Create a RealtimeBoard test user](#create-a-realtimeboard-test-user)** - toohave a counterpart of Britta Simon in RealtimeBoard that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e056b-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e056b-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e056b-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e056b-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e056b-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e056b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e056b-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az RealtimeBoard alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e056b-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RealtimeBoard application.</span></span>

<span data-ttu-id="e056b-148">**az Azure AD tooconfigure egyszeri bejelentkezést a RealtimeBoard, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e056b-148">**tooconfigure Azure AD single sign-on with RealtimeBoard, perform hello following steps:**</span></span>

1. <span data-ttu-id="e056b-149">Az Azure portál, a hello hello **RealtimeBoard** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e056b-149">In hello Azure portal, on hello **RealtimeBoard** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="e056b-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e056b-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_samlbase.png)

3. <span data-ttu-id="e056b-153">A hello **RealtimeBoard tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="e056b-153">On hello **RealtimeBoard Domain and URLs** section, if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk RealtimeBoard tartomány és az URL-címek](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_url.png)

    <span data-ttu-id="e056b-155">A hello **azonosító** szövegmező, adja meg az URL-címet:`https://realtimeboard.com/`</span><span class="sxs-lookup"><span data-stu-id="e056b-155">In hello **Identifier** textbox, type a URL as: `https://realtimeboard.com/`</span></span>

4. <span data-ttu-id="e056b-156">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="e056b-156">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_url2.png)

    <span data-ttu-id="e056b-158">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://realtimeboard.com/sso/saml`</span><span class="sxs-lookup"><span data-stu-id="e056b-158">In hello **Sign-on URL** textbox, type a URL as: `https://realtimeboard.com/sso/saml`</span></span>

5. <span data-ttu-id="e056b-159">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e056b-159">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_certificate.png) 

6. <span data-ttu-id="e056b-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e056b-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="e056b-163">tooconfigure egyszeri bejelentkezést a **RealtimeBoard** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[RealtimeBoard támogatási csoport](mailto:support@realtimeboard.com).</span><span class="sxs-lookup"><span data-stu-id="e056b-163">tooconfigure single sign-on on **RealtimeBoard** side, you need toosend hello downloaded **Metadata XML** too[RealtimeBoard support team](mailto:support@realtimeboard.com).</span></span> <span data-ttu-id="e056b-164">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="e056b-164">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e056b-165">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e056b-165">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e056b-166">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e056b-166">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e056b-167">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e056b-167">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e056b-168">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e056b-168">Create an Azure AD test user</span></span>

<span data-ttu-id="e056b-169">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e056b-169">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="e056b-171">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e056b-171">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e056b-172">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="e056b-172">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e056b-174">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e056b-174">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e056b-176">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e056b-176">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e056b-178">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e056b-178">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e056b-180">a.</span><span class="sxs-lookup"><span data-stu-id="e056b-180">a.</span></span> <span data-ttu-id="e056b-181">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e056b-181">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e056b-182">b.</span><span class="sxs-lookup"><span data-stu-id="e056b-182">b.</span></span> <span data-ttu-id="e056b-183">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="e056b-183">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="e056b-184">c.</span><span class="sxs-lookup"><span data-stu-id="e056b-184">c.</span></span> <span data-ttu-id="e056b-185">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e056b-185">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="e056b-186">d.</span><span class="sxs-lookup"><span data-stu-id="e056b-186">d.</span></span> <span data-ttu-id="e056b-187">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e056b-187">Click **Create**.</span></span>
 
### <a name="create-a-realtimeboard-test-user"></a><span data-ttu-id="e056b-188">RealtimeBoard tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e056b-188">Create a RealtimeBoard test user</span></span>

<span data-ttu-id="e056b-189">hello ebben a szakaszban célja toocreate RealtimeBoard Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e056b-189">hello objective of this section is toocreate a user called Britta Simon in RealtimeBoard.</span></span> <span data-ttu-id="e056b-190">RealtimeBoard támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="e056b-190">RealtimeBoard supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="e056b-191">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="e056b-191">There is no action item for you in this section.</span></span> <span data-ttu-id="e056b-192">Ha a felhasználó nem létezik a RealtimeBoard, egy új tooaccess RealtimeBoard tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="e056b-192">If a user doesn't already exist in RealtimeBoard, a new one is created when you attempt tooaccess RealtimeBoard.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e056b-193">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e056b-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e056b-194">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRealtimeBoard megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e056b-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRealtimeBoard.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="e056b-196">**tooassign Britta Simon tooRealtimeBoard, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e056b-196">**tooassign Britta Simon tooRealtimeBoard, perform hello following steps:**</span></span>

1. <span data-ttu-id="e056b-197">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e056b-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e056b-199">Hello alkalmazások listában válassza ki a **RealtimeBoard**.</span><span class="sxs-lookup"><span data-stu-id="e056b-199">In hello applications list, select **RealtimeBoard**.</span></span>

    ![hello RealtimeBoard hivatkozásra hello alkalmazások listája](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_app.png)  

3. <span data-ttu-id="e056b-201">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e056b-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="e056b-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e056b-203">Click **Add** button.</span></span> <span data-ttu-id="e056b-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e056b-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="e056b-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e056b-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e056b-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e056b-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e056b-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e056b-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e056b-209">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e056b-209">Test single sign-on</span></span>

<span data-ttu-id="e056b-210">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e056b-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e056b-211">Ha a hozzáférési Panel hello hello RealtimeBoard csempe gombra kattint, automatikusan bejelentkezett tooyour RealtimeBoard alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e056b-211">When you click hello RealtimeBoard tile in hello Access Panel, you should get automatically signed-on tooyour RealtimeBoard application.</span></span>
<span data-ttu-id="e056b-212">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e056b-212">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e056b-213">További források</span><span class="sxs-lookup"><span data-stu-id="e056b-213">Additional resources</span></span>

* [<span data-ttu-id="e056b-214">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e056b-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e056b-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e056b-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_203.png

