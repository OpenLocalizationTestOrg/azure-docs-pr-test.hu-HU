---
title: "Oktatóanyag: Azure Active Directoryval integrált Workrite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Workrite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a663374ae3c8b102b53d8cf05a9cb083b80dbb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a><span data-ttu-id="b2d8f-103">Oktatóanyag: Azure Active Directoryval integrált Workrite</span><span class="sxs-lookup"><span data-stu-id="b2d8f-103">Tutorial: Azure Active Directory integration with Workrite</span></span>

<span data-ttu-id="b2d8f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Workrite az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b2d8f-104">In this tutorial, you learn how toointegrate Workrite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b2d8f-105">Workrite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-105">Integrating Workrite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b2d8f-106">Az Azure AD hozzáférési tooWorkrite rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-106">You can control in Azure AD who has access tooWorkrite.</span></span>
- <span data-ttu-id="b2d8f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkrite (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-107">You can enable your users tooautomatically get signed-on tooWorkrite (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b2d8f-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="b2d8f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b2d8f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2d8f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b2d8f-110">Prerequisites</span></span>

<span data-ttu-id="b2d8f-111">az Azure AD integrálása Workrite tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-111">tooconfigure Azure AD integration with Workrite, you need hello following items:</span></span>

- <span data-ttu-id="b2d8f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b2d8f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b2d8f-113">Egy Workrite egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b2d8f-113">A Workrite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b2d8f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b2d8f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b2d8f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b2d8f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b2d8f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b2d8f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b2d8f-118">Scenario description</span></span>
<span data-ttu-id="b2d8f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b2d8f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b2d8f-121">Hello gyűjteményből Workrite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b2d8f-121">Adding Workrite from hello gallery</span></span>
2. <span data-ttu-id="b2d8f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b2d8f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workrite-from-hello-gallery"></a><span data-ttu-id="b2d8f-123">Hello gyűjteményből Workrite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b2d8f-123">Adding Workrite from hello gallery</span></span>
<span data-ttu-id="b2d8f-124">tooconfigure hello integrációja Workrite az Azure AD-be, meg kell tooadd Workrite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-124">tooconfigure hello integration of Workrite into Azure AD, you need tooadd Workrite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b2d8f-125">**tooadd Workrite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b2d8f-125">**tooadd Workrite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2d8f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="b2d8f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b2d8f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="b2d8f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="b2d8f-133">Hello keresési mezőbe, írja be a **Workrite**, jelölje be **Workrite** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-133">In hello search box, type **Workrite**, select **Workrite** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Workrite](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b2d8f-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2d8f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b2d8f-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Workrite.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-136">In this section, you configure and test Azure AD single sign-on with Workrite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b2d8f-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Workrite tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workrite is tooa user in Azure AD.</span></span> <span data-ttu-id="b2d8f-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Workrite közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-138">In other words, a link relationship between an Azure AD user and hello related user in Workrite needs toobe established.</span></span>

<span data-ttu-id="b2d8f-139">Workrite, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-139">In Workrite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b2d8f-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Workrite-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-140">tooconfigure and test Azure AD single sign-on with Workrite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b2d8f-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b2d8f-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b2d8f-143">**[Workrite tesztfelhasználó létrehozása](#create-a-workrite-test-user)**  -toohave egy megfelelője a Britta Simon a Workrite, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-143">**[Create a Workrite test user](#create-a-workrite-test-user)** - toohave a counterpart of Britta Simon in Workrite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b2d8f-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b2d8f-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b2d8f-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2d8f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b2d8f-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Workrite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workrite application.</span></span>

<span data-ttu-id="b2d8f-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Workrite, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b2d8f-148">**tooconfigure Azure AD single sign-on with Workrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2d8f-149">Az Azure portál, a hello hello **Workrite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-149">In hello Azure portal, on hello **Workrite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="b2d8f-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_samlbase.png)

3. <span data-ttu-id="b2d8f-153">A hello **Workrite tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-153">On hello **Workrite Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Workrite tartomány és az URL-címek](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_url.png)

    <span data-ttu-id="b2d8f-155">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="b2d8f-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b2d8f-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-156">This value is not real.</span></span> <span data-ttu-id="b2d8f-157">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b2d8f-158">Ügyfél [Workrite ügyfél-támogatási csoport](mailto:support@workrite.co.uk) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-158">Contact [Workrite Client support team](mailto:support@workrite.co.uk) tooget this value.</span></span>

4. <span data-ttu-id="b2d8f-159">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_certificate.png) 

5. <span data-ttu-id="b2d8f-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-workrite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b2d8f-163">A hello **Workrite konfigurációs** kattintson **konfigurálása Workrite** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-163">On hello **Workrite Configuration** section, click **Configure Workrite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b2d8f-164">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b2d8f-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Workrite konfiguráció](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_configure.png) 

7. <span data-ttu-id="b2d8f-166">tooconfigure egyszeri bejelentkezést a **Workrite** oldalon kell letöltött toosend hello **Certificate(Base64), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** túl[Workrite támogatási csoport](mailto:support@workrite.co.uk).</span><span class="sxs-lookup"><span data-stu-id="b2d8f-166">tooconfigure single sign-on on **Workrite** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Workrite support team](mailto:support@workrite.co.uk).</span></span>

> [!TIP]
> <span data-ttu-id="b2d8f-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b2d8f-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b2d8f-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b2d8f-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b2d8f-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b2d8f-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b2d8f-170">Create an Azure AD test user</span></span>

<span data-ttu-id="b2d8f-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="b2d8f-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b2d8f-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2d8f-174">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-workrite-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b2d8f-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-workrite-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b2d8f-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b2d8f-180">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png)

    <span data-ttu-id="b2d8f-182">a.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-182">a.</span></span> <span data-ttu-id="b2d8f-183">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b2d8f-184">b.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-184">b.</span></span> <span data-ttu-id="b2d8f-185">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="b2d8f-186">c.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-186">c.</span></span> <span data-ttu-id="b2d8f-187">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="b2d8f-188">d.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-188">d.</span></span> <span data-ttu-id="b2d8f-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-189">Click **Create**.</span></span>
 
### <a name="create-a-workrite-test-user"></a><span data-ttu-id="b2d8f-190">Workrite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2d8f-190">Create a Workrite test user</span></span>

<span data-ttu-id="b2d8f-191">hello ebben a szakaszban célja toocreate Workrite Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-191">hello objective of this section is toocreate a user called Britta Simon in Workrite.</span></span>

<span data-ttu-id="b2d8f-192">**toocreate Britta Simon meghívta Workrite, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b2d8f-192">**toocreate a user called Britta Simon in Workrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2d8f-193">Bejelentkezés tooyour workrite vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-193">Sign on tooyour workrite company site as administrator.</span></span>

2. <span data-ttu-id="b2d8f-194">Hello navigációs ablaktábláján kattintson **Admin**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-194">In hello navigation pane, click **Admin**.</span></span>
   
    ![Felügyeleti vezérlő][400]

3. <span data-ttu-id="b2d8f-196">Nyissa meg tooQuick hivatkozásokat, és kattintson a **felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-196">Go tooQuick Links, and then click **Create a User**.</span></span>
   
    ![Hozzon létre felhasználói szakasz][401]

4. <span data-ttu-id="b2d8f-198">A hello **felhasználó létrehozása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2d8f-198">On hello **Create User** dialog, perform hello following steps:</span></span>
   
    ![Hozzon létre felhasználói Dailog][402]
    
    <span data-ttu-id="b2d8f-200">a.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-200">a.</span></span> <span data-ttu-id="b2d8f-201">A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-201">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="b2d8f-202">b.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-202">b.</span></span> <span data-ttu-id="b2d8f-203">A hello **Keresztnév** szövegmezőhöz típus hello Keresztnév Britta például felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-203">In hello **First Name** textbox, type hello firstname of user like Britta.</span></span>

    <span data-ttu-id="b2d8f-204">c.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-204">c.</span></span> <span data-ttu-id="b2d8f-205">A hello **vezetékneve** szövegmező, például Simon felhasználó típusa hello vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-205">In hello **Surname** textbox, type hello surname of user like Simon.</span></span>
    
    <span data-ttu-id="b2d8f-206">d.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-206">d.</span></span> <span data-ttu-id="b2d8f-207">Válassza ki **ügyfél rendszergazda** , **válassza ki a szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-207">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="b2d8f-208">e.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-208">e.</span></span> <span data-ttu-id="b2d8f-209">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-209">Click **Save**.</span></span>   

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b2d8f-210">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="b2d8f-210">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b2d8f-211">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWorkrite megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkrite.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="b2d8f-213">**tooassign Britta Simon tooWorkrite, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b2d8f-213">**tooassign Britta Simon tooWorkrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2d8f-214">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b2d8f-216">Hello alkalmazások listában válassza ki a **Workrite**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-216">In hello applications list, select **Workrite**.</span></span>

    ![hello Workrite hivatkozásra hello alkalmazások listája](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_app.png)  

3. <span data-ttu-id="b2d8f-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="b2d8f-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-220">Click **Add** button.</span></span> <span data-ttu-id="b2d8f-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="b2d8f-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b2d8f-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b2d8f-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b2d8f-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b2d8f-226">Test single sign-on</span></span>

<span data-ttu-id="b2d8f-227">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="b2d8f-228">Ha a hozzáférési Panel hello hello Workrite csempe gombra kattint, automatikusan bejelentkezett tooyour Workrite alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b2d8f-228">When you click hello Workrite tile in hello Access Panel, you should get automatically signed-on tooyour Workrite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2d8f-229">További források</span><span class="sxs-lookup"><span data-stu-id="b2d8f-229">Additional resources</span></span>

* [<span data-ttu-id="b2d8f-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b2d8f-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b2d8f-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b2d8f-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png

