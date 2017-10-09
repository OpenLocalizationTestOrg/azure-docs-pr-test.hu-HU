---
title: "Oktatóanyag: Azure Active Directoryval integrált Predictix szortiment megtervezése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Predictix szortiment tervezési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1294b712caf12fdafaf65d70a02ee9fbdc3e84a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a><span data-ttu-id="27246-103">Oktatóanyag: Azure Active Directoryval integrált Predictix szortiment tervezése</span><span class="sxs-lookup"><span data-stu-id="27246-103">Tutorial: Azure Active Directory integration with Predictix Assortment Planning</span></span>

<span data-ttu-id="27246-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Predictix szortiment tervezése az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="27246-104">In this tutorial, you learn how toointegrate Predictix Assortment Planning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="27246-105">Predictix szortiment tervezési integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="27246-105">Integrating Predictix Assortment Planning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="27246-106">Az Azure AD hozzáférési tooPredictix szortiment tervezési rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="27246-106">You can control in Azure AD who has access tooPredictix Assortment Planning.</span></span>
- <span data-ttu-id="27246-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPredictix szortiment tervezési (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="27246-107">You can enable your users tooautomatically get signed-on tooPredictix Assortment Planning (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="27246-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="27246-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="27246-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="27246-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27246-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="27246-110">Prerequisites</span></span>

<span data-ttu-id="27246-111">tooconfigure Predictix szortiment tervezési integrálása az Azure AD, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="27246-111">tooconfigure Azure AD integration with Predictix Assortment Planning, you need hello following items:</span></span>

- <span data-ttu-id="27246-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="27246-112">An Azure AD subscription</span></span>
- <span data-ttu-id="27246-113">Egy Predictix szortiment tervezési egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="27246-113">A Predictix Assortment Planning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="27246-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="27246-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="27246-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="27246-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="27246-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="27246-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="27246-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="27246-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="27246-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="27246-118">Scenario description</span></span>
<span data-ttu-id="27246-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="27246-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="27246-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="27246-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="27246-121">Hozzáadás a Predictix szortiment tervezési hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="27246-121">Adding Predictix Assortment Planning from hello gallery</span></span>
2. <span data-ttu-id="27246-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="27246-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-assortment-planning-from-hello-gallery"></a><span data-ttu-id="27246-123">Hozzáadás a Predictix szortiment tervezési hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="27246-123">Adding Predictix Assortment Planning from hello gallery</span></span>
<span data-ttu-id="27246-124">tooconfigure hello integrációs Predictix szortiment tervezési, az Azure AD-be, meg kell tooadd Predictix szortiment tervezési hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="27246-124">tooconfigure hello integration of Predictix Assortment Planning into Azure AD, you need tooadd Predictix Assortment Planning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="27246-125">**tooadd Predictix szortiment tervezési hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="27246-125">**tooadd Predictix Assortment Planning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="27246-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="27246-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="27246-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="27246-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="27246-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="27246-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="27246-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="27246-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="27246-133">Hello keresési mezőbe, írja be a **Predictix szortiment tervezési**, jelölje be **Predictix szortiment tervezési** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="27246-133">In hello search box, type **Predictix Assortment Planning**, select **Predictix Assortment Planning** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Predictix szortiment hello eredménylistában tervezése](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="27246-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="27246-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="27246-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Predictix szortiment tervezésének "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="27246-136">In this section, you configure and test Azure AD single sign-on with Predictix Assortment Planning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="27246-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Predictix szortiment tervezési tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="27246-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Predictix Assortment Planning is tooa user in Azure AD.</span></span> <span data-ttu-id="27246-138">Ez azt jelenti hello kapcsolódó felhasználói Predictix szortiment tervezésével és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="27246-138">In other words, a link relationship between an Azure AD user and hello related user in Predictix Assortment Planning needs toobe established.</span></span>

<span data-ttu-id="27246-139">A tervezés szortiment Predictix, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="27246-139">In Predictix Assortment Planning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="27246-140">tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés Predictix szortiment tervezésének, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="27246-140">tooconfigure and test Azure AD single sign-on with Predictix Assortment Planning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="27246-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="27246-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="27246-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27246-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="27246-143">**[Predictix szortiment tervezési tesztfelhasználó létrehozása](#create-a-predictix-assortment-planning-test-user)**  -toohave egy megfelelője a Britta Simon Predictix szortiment tervezésével, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="27246-143">**[Create a Predictix Assortment Planning test user](#create-a-predictix-assortment-planning-test-user)** - toohave a counterpart of Britta Simon in Predictix Assortment Planning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="27246-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="27246-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="27246-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="27246-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="27246-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="27246-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="27246-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Predictix szortiment tervezési alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="27246-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Predictix Assortment Planning application.</span></span>

<span data-ttu-id="27246-148">**tooconfigure az Azure AD az egyszeri bejelentkezés Predictix szortiment tervezésének, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="27246-148">**tooconfigure Azure AD single sign-on with Predictix Assortment Planning, perform hello following steps:**</span></span>

1. <span data-ttu-id="27246-149">Az Azure portál, a hello hello **Predictix szortiment tervezési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="27246-149">In hello Azure portal, on hello **Predictix Assortment Planning** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="27246-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="27246-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_samlbase.png)

3. <span data-ttu-id="27246-153">A hello **Predictix szortiment tervezési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="27246-153">On hello **Predictix Assortment Planning Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Predictix szortiment tervezési tartomány és az URL-címek](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_url.png)

    <span data-ttu-id="27246-155">a.</span><span class="sxs-lookup"><span data-stu-id="27246-155">a.</span></span> <span data-ttu-id="27246-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="27246-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com/sso/request`|
    | `https://<sub-domain>.dev.ap.predictix.com/`|

    <span data-ttu-id="27246-157">b.</span><span class="sxs-lookup"><span data-stu-id="27246-157">b.</span></span> <span data-ttu-id="27246-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="27246-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com`|
    | `https://<sub-domain>.dev.ap.predictix.com`|
    
    > [!NOTE] 
    > <span data-ttu-id="27246-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="27246-159">These values are not real.</span></span> <span data-ttu-id="27246-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="27246-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="27246-161">Ügyfél [Predictix szortiment tervezési ügyfél-támogatási csoport](http://www.infor.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="27246-161">Contact [Predictix Assortment Planning Client support team](http://www.infor.com/support) tooget these values.</span></span> 
 


4. <span data-ttu-id="27246-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="27246-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_certificate.png) 

5. <span data-ttu-id="27246-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="27246-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="27246-166">A hello **Predictix szortiment tervezési konfigurációs** területén kattintson **konfigurálása Predictix szortiment tervezési** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="27246-166">On hello **Predictix Assortment Planning Configuration** section, click **Configure Predictix Assortment Planning** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="27246-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="27246-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Predictix szortiment tervezési konfiguráció](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_configure.png) 

7. <span data-ttu-id="27246-169">tooconfigure egyszeri bejelentkezést a **Predictix szortiment tervezési** oldalon kell letöltött toosend hello **Certificate(Base64)**, **SAML Entitásazonosító**,  **SAML-alapú egyszeri bejelentkezési URL-címe**, és **Sign-Out URL-cím** túl[Predictix szortiment tervezési támogatási csoport](http://www.infor.com/support).</span><span class="sxs-lookup"><span data-stu-id="27246-169">tooconfigure single sign-on on **Predictix Assortment Planning** side, you need toosend hello downloaded **Certificate(Base64)**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL**  too[Predictix Assortment Planning support team](http://www.infor.com/support).</span></span> <span data-ttu-id="27246-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="27246-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="27246-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="27246-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="27246-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="27246-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="27246-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="27246-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="27246-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="27246-174">Create an Azure AD test user</span></span>

<span data-ttu-id="27246-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="27246-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="27246-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="27246-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="27246-178">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="27246-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="27246-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="27246-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="27246-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="27246-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="27246-184">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="27246-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png)

    <span data-ttu-id="27246-186">a.</span><span class="sxs-lookup"><span data-stu-id="27246-186">a.</span></span> <span data-ttu-id="27246-187">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="27246-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="27246-188">b.</span><span class="sxs-lookup"><span data-stu-id="27246-188">b.</span></span> <span data-ttu-id="27246-189">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="27246-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="27246-190">c.</span><span class="sxs-lookup"><span data-stu-id="27246-190">c.</span></span> <span data-ttu-id="27246-191">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="27246-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="27246-192">d.</span><span class="sxs-lookup"><span data-stu-id="27246-192">d.</span></span> <span data-ttu-id="27246-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="27246-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-assortment-planning-test-user"></a><span data-ttu-id="27246-194">Predictix szortiment tervezési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="27246-194">Create a Predictix Assortment Planning test user</span></span>

<span data-ttu-id="27246-195">Ebben a szakaszban egy felhasználó Britta Simon meghívta Predictix szortiment tervezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="27246-195">In this section, you create a user called Britta Simon in Predictix Assortment Planning.</span></span> <span data-ttu-id="27246-196">Adjon együttműködve [Predictix szortiment tervezési támogatási csoport](http://www.infor.com/contact/) tooadd hello felhasználók hello Predictix szortiment tervezési platform.</span><span class="sxs-lookup"><span data-stu-id="27246-196">Please work with [Predictix Assortment Planning support team](http://www.infor.com/contact/) tooadd hello users in hello Predictix Assortment Planning platform.</span></span>
 > [!NOTE]
 > <span data-ttu-id="27246-197">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="27246-197">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="27246-198">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="27246-198">Assign hello Azure AD test user</span></span>

<span data-ttu-id="27246-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés szortiment tervezési hozzáférés tooPredictix megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="27246-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPredictix Assortment Planning.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="27246-201">**tooassign Britta Simon tooPredictix szortiment tervezési, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="27246-201">**tooassign Britta Simon tooPredictix Assortment Planning, perform hello following steps:**</span></span>

1. <span data-ttu-id="27246-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="27246-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="27246-204">Hello alkalmazások listában válassza ki a **Predictix szortiment tervezési**.</span><span class="sxs-lookup"><span data-stu-id="27246-204">In hello applications list, select **Predictix Assortment Planning**.</span></span>

    ![hello Predictix szortiment tervezési hivatkozásra hello alkalmazások listája](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_app.png)  

3. <span data-ttu-id="27246-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="27246-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="27246-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="27246-208">Click **Add** button.</span></span> <span data-ttu-id="27246-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="27246-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="27246-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="27246-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="27246-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="27246-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="27246-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="27246-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="27246-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="27246-214">Test single sign-on</span></span>

<span data-ttu-id="27246-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="27246-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="27246-216">Hello Predictix szortiment tervezési csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Predictix szortiment tervezési alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="27246-216">When you click hello Predictix Assortment Planning tile in hello Access Panel, you should get automatically signed-on tooyour Predictix Assortment Planning application.</span></span>
<span data-ttu-id="27246-217">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="27246-217">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="27246-218">További források</span><span class="sxs-lookup"><span data-stu-id="27246-218">Additional resources</span></span>

* [<span data-ttu-id="27246-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="27246-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="27246-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="27246-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png

