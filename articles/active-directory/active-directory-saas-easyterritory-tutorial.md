---
title: "Oktatóanyag: Azure Active Directoryval integrált EasyTerritory |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és EasyTerritory között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d29b362d-e986-4f67-8ff2-e158e49353aa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 4f1e9fb4d615325f0d57bebaed955529d5dcd9b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-easyterritory"></a><span data-ttu-id="df76e-103">Oktatóanyag: Azure Active Directoryval integrált EasyTerritory</span><span class="sxs-lookup"><span data-stu-id="df76e-103">Tutorial: Azure Active Directory integration with EasyTerritory</span></span>

<span data-ttu-id="df76e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate EasyTerritory az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="df76e-104">In this tutorial, you learn how toointegrate EasyTerritory with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="df76e-105">EasyTerritory integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="df76e-105">Integrating EasyTerritory with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="df76e-106">Az Azure AD hozzáférési tooEasyTerritory rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="df76e-106">You can control in Azure AD who has access tooEasyTerritory.</span></span>
- <span data-ttu-id="df76e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEasyTerritory (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="df76e-107">You can enable your users tooautomatically get signed-on tooEasyTerritory (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="df76e-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="df76e-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="df76e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="df76e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df76e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="df76e-110">Prerequisites</span></span>

<span data-ttu-id="df76e-111">az Azure AD integrálása EasyTerritory tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="df76e-111">tooconfigure Azure AD integration with EasyTerritory, you need hello following items:</span></span>

- <span data-ttu-id="df76e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="df76e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="df76e-113">Egy EasyTerritory egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="df76e-113">A EasyTerritory single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="df76e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="df76e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="df76e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="df76e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="df76e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="df76e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="df76e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="df76e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="df76e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="df76e-118">Scenario description</span></span>
<span data-ttu-id="df76e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="df76e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="df76e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="df76e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="df76e-121">Hello gyűjteményből EasyTerritory hozzáadása</span><span class="sxs-lookup"><span data-stu-id="df76e-121">Adding EasyTerritory from hello gallery</span></span>
2. <span data-ttu-id="df76e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="df76e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-easyterritory-from-hello-gallery"></a><span data-ttu-id="df76e-123">Hello gyűjteményből EasyTerritory hozzáadása</span><span class="sxs-lookup"><span data-stu-id="df76e-123">Adding EasyTerritory from hello gallery</span></span>
<span data-ttu-id="df76e-124">tooconfigure hello integrációja EasyTerritory az Azure AD-be, meg kell tooadd EasyTerritory hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="df76e-124">tooconfigure hello integration of EasyTerritory into Azure AD, you need tooadd EasyTerritory from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="df76e-125">**tooadd EasyTerritory hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="df76e-125">**tooadd EasyTerritory from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="df76e-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="df76e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="df76e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="df76e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="df76e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="df76e-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="df76e-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="df76e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="df76e-133">Hello keresési mezőbe, írja be a **EasyTerritory**, jelölje be **EasyTerritory** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="df76e-133">In hello search box, type **EasyTerritory**, select **EasyTerritory** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában EasyTerritory](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="df76e-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="df76e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="df76e-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján EasyTerritory.</span><span class="sxs-lookup"><span data-stu-id="df76e-136">In this section, you configure and test Azure AD single sign-on with EasyTerritory based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="df76e-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó EasyTerritory tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="df76e-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EasyTerritory is tooa user in Azure AD.</span></span> <span data-ttu-id="df76e-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello EasyTerritory közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="df76e-138">In other words, a link relationship between an Azure AD user and hello related user in EasyTerritory needs toobe established.</span></span>

<span data-ttu-id="df76e-139">EasyTerritory, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="df76e-139">In EasyTerritory, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="df76e-140">tooconfigure és az Azure AD az egyszeri bejelentkezés EasyTerritory-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="df76e-140">tooconfigure and test Azure AD single sign-on with EasyTerritory, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="df76e-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="df76e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="df76e-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df76e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="df76e-143">**[EasyTerritory tesztfelhasználó létrehozása](#create-a-easyterritory-test-user)**  -toohave egy megfelelője a Britta Simon a EasyTerritory, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="df76e-143">**[Create a EasyTerritory test user](#create-a-easyterritory-test-user)** - toohave a counterpart of Britta Simon in EasyTerritory that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="df76e-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="df76e-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="df76e-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="df76e-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="df76e-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="df76e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="df76e-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az EasyTerritory alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="df76e-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EasyTerritory application.</span></span>

<span data-ttu-id="df76e-148">**az Azure AD tooconfigure egyszeri bejelentkezést a EasyTerritory, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="df76e-148">**tooconfigure Azure AD single sign-on with EasyTerritory, perform hello following steps:**</span></span>

1. <span data-ttu-id="df76e-149">Az Azure portál, a hello hello **EasyTerritory** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="df76e-149">In hello Azure portal, on hello **EasyTerritory** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="df76e-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="df76e-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_samlbase.png)

3. <span data-ttu-id="df76e-153">A hello **EasyTerritory tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás IDP kezdeményezett mód hello:</span><span class="sxs-lookup"><span data-stu-id="df76e-153">On hello **EasyTerritory Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk EasyTerritory tartomány és az URL-címek](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_url.png)

    <span data-ttu-id="df76e-155">a.</span><span class="sxs-lookup"><span data-stu-id="df76e-155">a.</span></span> <span data-ttu-id="df76e-156">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://apps.easyterritory.com/<tenant id>/dev/`</span><span class="sxs-lookup"><span data-stu-id="df76e-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://apps.easyterritory.com/<tenant id>/dev/`</span></span>

    <span data-ttu-id="df76e-157">b.</span><span class="sxs-lookup"><span data-stu-id="df76e-157">b.</span></span> <span data-ttu-id="df76e-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://apps.easyterritory.com/<tenant id>/dev/authservices/acs`</span><span class="sxs-lookup"><span data-stu-id="df76e-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://apps.easyterritory.com/<tenant id>/dev/authservices/acs`</span></span>

4. <span data-ttu-id="df76e-159">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="df76e-159">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk EasyTerritory tartomány és az URL-címek](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_url1.png)

    <span data-ttu-id="df76e-161">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.easyterritory.com/`</span><span class="sxs-lookup"><span data-stu-id="df76e-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.easyterritory.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="df76e-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="df76e-162">These values are not real.</span></span> <span data-ttu-id="df76e-163">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="df76e-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="df76e-164">Ügyfél [EasyTerritory ügyfél-támogatási csoport](mailto:sales@easyterritory.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="df76e-164">Contact [EasyTerritory Client support team](mailto:sales@easyterritory.com) tooget these values.</span></span> 

5. <span data-ttu-id="df76e-165">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="df76e-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_certificate.png) 

6. <span data-ttu-id="df76e-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="df76e-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-easyterritory-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="df76e-169">tooconfigure egyszeri bejelentkezést a **EasyTerritory** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[EasyTerritory támogatási csoport](mailto:sales@easyterritory.com).</span><span class="sxs-lookup"><span data-stu-id="df76e-169">tooconfigure single sign-on on **EasyTerritory** side, you need toosend hello downloaded **Metadata XML** too[EasyTerritory support team](mailto:sales@easyterritory.com).</span></span> <span data-ttu-id="df76e-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="df76e-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="df76e-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="df76e-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="df76e-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="df76e-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="df76e-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="df76e-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="df76e-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="df76e-174">Create an Azure AD test user</span></span>

<span data-ttu-id="df76e-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="df76e-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="df76e-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="df76e-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="df76e-178">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="df76e-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="df76e-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="df76e-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="df76e-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="df76e-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="df76e-184">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="df76e-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_04.png)

    <span data-ttu-id="df76e-186">a.</span><span class="sxs-lookup"><span data-stu-id="df76e-186">a.</span></span> <span data-ttu-id="df76e-187">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="df76e-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="df76e-188">b.</span><span class="sxs-lookup"><span data-stu-id="df76e-188">b.</span></span> <span data-ttu-id="df76e-189">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="df76e-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="df76e-190">c.</span><span class="sxs-lookup"><span data-stu-id="df76e-190">c.</span></span> <span data-ttu-id="df76e-191">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="df76e-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="df76e-192">d.</span><span class="sxs-lookup"><span data-stu-id="df76e-192">d.</span></span> <span data-ttu-id="df76e-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="df76e-193">Click **Create**.</span></span>
 
### <a name="create-a-easyterritory-test-user"></a><span data-ttu-id="df76e-194">EasyTerritory tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="df76e-194">Create a EasyTerritory test user</span></span>

<span data-ttu-id="df76e-195">Ebben a szakaszban egy EasyTerritory Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="df76e-195">In this section, you create a user called Britta Simon in EasyTerritory.</span></span> <span data-ttu-id="df76e-196">Adjon együttműködve [EasyTerritory támogatási csoport](mailto:sales@easyterritory.com) tooadd hello felhasználók hello EasyTerritory platform.</span><span class="sxs-lookup"><span data-stu-id="df76e-196">Please work with [EasyTerritory support team](mailto:sales@easyterritory.com) tooadd hello users in hello EasyTerritory platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="df76e-197">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="df76e-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="df76e-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEasyTerritory megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="df76e-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEasyTerritory.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="df76e-200">**tooassign Britta Simon tooEasyTerritory, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="df76e-200">**tooassign Britta Simon tooEasyTerritory, perform hello following steps:**</span></span>

1. <span data-ttu-id="df76e-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="df76e-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="df76e-203">Hello alkalmazások listában válassza ki a **EasyTerritory**.</span><span class="sxs-lookup"><span data-stu-id="df76e-203">In hello applications list, select **EasyTerritory**.</span></span>

    ![hello EasyTerritory hivatkozásra hello alkalmazások listája](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_app.png)  

3. <span data-ttu-id="df76e-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="df76e-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="df76e-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="df76e-207">Click **Add** button.</span></span> <span data-ttu-id="df76e-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="df76e-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="df76e-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="df76e-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="df76e-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="df76e-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="df76e-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="df76e-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="df76e-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="df76e-213">Test single sign-on</span></span>

<span data-ttu-id="df76e-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="df76e-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="df76e-215">Ha a hozzáférési Panel hello hello EasyTerritory csempe gombra kattint, automatikusan bejelentkezett tooyour EasyTerritory alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="df76e-215">When you click hello EasyTerritory tile in hello Access Panel, you should get automatically signed-on tooyour EasyTerritory application.</span></span>
<span data-ttu-id="df76e-216">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="df76e-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="df76e-217">További források</span><span class="sxs-lookup"><span data-stu-id="df76e-217">Additional resources</span></span>

* [<span data-ttu-id="df76e-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="df76e-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="df76e-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="df76e-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)




<!--Image references-->

[1]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_203.png

