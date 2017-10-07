---
title: "Oktatóanyag: Azure Active Directory-integráció Yonyx interaktív útmutatók |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Yonyx interaktív útmutatók között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: 24e30d243143651b8d32535c76dc300931ae5746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a><span data-ttu-id="901a8-103">Oktatóanyag: Azure Active Directory-integráció Yonyx interaktív útmutatók</span><span class="sxs-lookup"><span data-stu-id="901a8-103">Tutorial: Azure Active Directory integration with Yonyx Interactive Guides</span></span>

<span data-ttu-id="901a8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Yonyx interaktív útmutató az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="901a8-104">In this tutorial, you learn how toointegrate Yonyx Interactive Guides with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="901a8-105">Interaktív útmutatók Yonyx integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="901a8-105">Integrating Yonyx Interactive Guides with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="901a8-106">Megadhatja a hozzáférés tooYonyx interaktív útmutatók rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="901a8-106">You can control in Azure AD who has access tooYonyx Interactive Guides</span></span>
- <span data-ttu-id="901a8-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooYonyx interaktív útmutatók (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="901a8-107">You can enable your users tooautomatically get signed-on tooYonyx Interactive Guides (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="901a8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="901a8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="901a8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="901a8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="901a8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="901a8-110">Prerequisites</span></span>

<span data-ttu-id="901a8-111">tooconfigure Yonyx interaktív útmutatók az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="901a8-111">tooconfigure Azure AD integration with Yonyx Interactive Guides, you need hello following items:</span></span>

- <span data-ttu-id="901a8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="901a8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="901a8-113">Egy Yonyx interaktív útmutatók az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="901a8-113">A Yonyx Interactive Guides single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="901a8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="901a8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="901a8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="901a8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="901a8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="901a8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="901a8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="901a8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="901a8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="901a8-118">Scenario description</span></span>
<span data-ttu-id="901a8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="901a8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="901a8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="901a8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="901a8-121">Hello gyűjteményből Yonyx interaktív útmutatók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="901a8-121">Adding Yonyx Interactive Guides from hello gallery</span></span>
2. <span data-ttu-id="901a8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="901a8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yonyx-interactive-guides-from-hello-gallery"></a><span data-ttu-id="901a8-123">Hello gyűjteményből Yonyx interaktív útmutatók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="901a8-123">Adding Yonyx Interactive Guides from hello gallery</span></span>
<span data-ttu-id="901a8-124">tooconfigure hello integrációs Yonyx interaktív útmutatók az Azure AD-be, meg kell tooadd Yonyx interaktív útmutatók hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="901a8-124">tooconfigure hello integration of Yonyx Interactive Guides into Azure AD, you need tooadd Yonyx Interactive Guides from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="901a8-125">**tooadd Yonyx interaktív útmutatók hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="901a8-125">**tooadd Yonyx Interactive Guides from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="901a8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="901a8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="901a8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="901a8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="901a8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="901a8-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="901a8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="901a8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="901a8-133">Hello keresési mezőbe, írja be a **Yonyx interaktív útmutatók**, jelölje be **Yonyx interaktív útmutatók** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="901a8-133">In hello search box, type **Yonyx Interactive Guides**, select  **Yonyx Interactive Guides**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Interaktív útmutatók Yonyx hello eredmények listájában](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="901a8-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="901a8-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="901a8-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Yonyx interaktív útmutatók "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="901a8-136">In this section, you configure and test Azure AD single sign-on with Yonyx Interactive Guides based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="901a8-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Yonyx interaktív útmutatók tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="901a8-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Yonyx Interactive Guides is tooa user in Azure AD.</span></span> <span data-ttu-id="901a8-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Yonyx interaktív útmutatókban található hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="901a8-138">In other words, a link relationship between an Azure AD user and hello related user in Yonyx Interactive Guides needs toobe established.</span></span>

<span data-ttu-id="901a8-139">A Yonyx interaktív útmutatóban, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="901a8-139">In Yonyx Interactive Guides, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="901a8-140">tooconfigure és Yonyx interaktív útmutatók az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="901a8-140">tooconfigure and test Azure AD single sign-on with Yonyx Interactive Guides, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="901a8-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="901a8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="901a8-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="901a8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="901a8-143">**[Interaktív útmutatók Yonyx tesztfelhasználó létrehozása](#create-a-yonyx-interactive-guides-test-user)**  -toohave egy megfelelője a Britta Simon Yonyx interaktív kialakítani, hogy a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="901a8-143">**[Create a Yonyx Interactive Guides test user](#create-a-yonyx-interactive-guides-test-user)** - toohave a counterpart of Britta Simon in Yonyx Interactive Guides that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="901a8-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="901a8-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="901a8-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="901a8-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="901a8-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="901a8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="901a8-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az interaktív útmutatók Yonyx alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="901a8-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="901a8-148">**az Azure AD az egyszeri bejelentkezés tooconfigure Yonyx interaktív útmutatók, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="901a8-148">**tooconfigure Azure AD single sign-on with Yonyx Interactive Guides, perform hello following steps:**</span></span>

1. <span data-ttu-id="901a8-149">Az Azure portál, a hello hello **Yonyx interaktív útmutatók** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="901a8-149">In hello Azure portal, on hello **Yonyx Interactive Guides** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="901a8-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="901a8-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. <span data-ttu-id="901a8-153">A hello **Yonyx interaktív útmutatók tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="901a8-153">On hello **Yonyx Interactive Guides Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Yonyx interaktív útmutatók tartomány és az URL-címek](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    <span data-ttu-id="901a8-155">a.</span><span class="sxs-lookup"><span data-stu-id="901a8-155">a.</span></span> <span data-ttu-id="901a8-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span><span class="sxs-lookup"><span data-stu-id="901a8-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span></span>

    <span data-ttu-id="901a8-157">b.</span><span class="sxs-lookup"><span data-stu-id="901a8-157">b.</span></span> <span data-ttu-id="901a8-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.yonyx.com`</span><span class="sxs-lookup"><span data-stu-id="901a8-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.yonyx.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="901a8-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="901a8-159">These values are not real.</span></span> <span data-ttu-id="901a8-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="901a8-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="901a8-161">Ügyfél [Yonyx interaktív útmutatók ügyfél-támogatási csoport](mailto:support@yonyx.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="901a8-161">Contact [Yonyx Interactive Guides Client support team](mailto:support@yonyx.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="901a8-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="901a8-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. <span data-ttu-id="901a8-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="901a8-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-yonyx-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="901a8-166">A hello **Yonyx interaktív útmutatók konfigurációs** kattintson **Yonyx interaktív útmutatók konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="901a8-166">On hello **Yonyx Interactive Guides Configuration** section, click **Configure Yonyx Interactive Guides** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="901a8-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="901a8-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Yonyx interaktív útmutatók konfiguráció](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. <span data-ttu-id="901a8-169">tooconfigure egyszeri bejelentkezést a **Yonyx interaktív útmutatók** oldalon kell letöltött toosend hello **Certificate(Base64)**, **Sign-Out URL-cím**, **SAML Az egyszeri bejelentkezési URL-címe** **SAML Entitásazonosító** túl[Yonyx interaktív útmutatók támogatási csoport](mailto:support@yonyx.com).</span><span class="sxs-lookup"><span data-stu-id="901a8-169">tooconfigure single sign-on on **Yonyx Interactive Guides** side, you need toosend hello downloaded **Certificate(Base64)**, **Sign-Out URL**, **SAML Single Sign-On Service URL** **SAML Entity ID** too[Yonyx Interactive Guides support team](mailto:support@yonyx.com).</span></span> <span data-ttu-id="901a8-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="901a8-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="901a8-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="901a8-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="901a8-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="901a8-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="901a8-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="901a8-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="901a8-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="901a8-174">Create an Azure AD test user</span></span>

<span data-ttu-id="901a8-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="901a8-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

  ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="901a8-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="901a8-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="901a8-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="901a8-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-yonyx-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="901a8-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="901a8-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-yonyx-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="901a8-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901a8-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="901a8-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="901a8-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="901a8-186">a.</span><span class="sxs-lookup"><span data-stu-id="901a8-186">a.</span></span> <span data-ttu-id="901a8-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="901a8-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="901a8-188">b.</span><span class="sxs-lookup"><span data-stu-id="901a8-188">b.</span></span> <span data-ttu-id="901a8-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="901a8-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="901a8-190">c.</span><span class="sxs-lookup"><span data-stu-id="901a8-190">c.</span></span> <span data-ttu-id="901a8-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="901a8-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="901a8-192">d.</span><span class="sxs-lookup"><span data-stu-id="901a8-192">d.</span></span> <span data-ttu-id="901a8-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="901a8-193">Click **Create**.</span></span>
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a><span data-ttu-id="901a8-194">Interaktív útmutatók Yonyx tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="901a8-194">Create a Yonyx Interactive Guides test user</span></span>

<span data-ttu-id="901a8-195">hello ebben a szakaszban célja toocreate Britta Simon nevű Yonyx interaktív útmutatókban található felhasználó.</span><span class="sxs-lookup"><span data-stu-id="901a8-195">hello objective of this section is toocreate a user called Britta Simon in Yonyx Interactive Guides.</span></span> <span data-ttu-id="901a8-196">Yonyx interaktív útmutatók támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="901a8-196">Yonyx Interactive Guides supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="901a8-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="901a8-197">There is no action item for you in this section.</span></span> <span data-ttu-id="901a8-198">Új felhasználó jön létre egy kísérlet tooaccess Yonyx interaktív útmutatók során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="901a8-198">A new user is created during an attempt tooaccess Yonyx Interactive Guides if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="901a8-199">Toocreate a felhasználó manuálisan kell, ha szüksége van-e Yonyx interaktív útmutatók támogatási csoport keresztül toocontact hello < mailto:support@yonyx.com >.</span><span class="sxs-lookup"><span data-stu-id="901a8-199">If you need toocreate a user manually, you need toocontact hello Yonyx Interactive Guides support team via <mailto:support@yonyx.com>.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="901a8-200">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="901a8-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="901a8-201">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooYonyx interaktív útmutatók megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="901a8-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYonyx Interactive Guides.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="901a8-203">**tooassign Britta Simon tooYonyx interaktív útmutatók, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="901a8-203">**tooassign Britta Simon tooYonyx Interactive Guides, perform hello following steps:**</span></span>

1. <span data-ttu-id="901a8-204">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="901a8-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="901a8-206">Hello alkalmazások listában válassza ki a **Yonyx interaktív útmutatók**.</span><span class="sxs-lookup"><span data-stu-id="901a8-206">In hello applications list, select **Yonyx Interactive Guides**.</span></span>

    ![hello Yonyx interaktív útmutatók hivatkozásra hello alkalmazások listája](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. <span data-ttu-id="901a8-208">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="901a8-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="901a8-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="901a8-210">Click **Add** button.</span></span> <span data-ttu-id="901a8-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901a8-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="901a8-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="901a8-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="901a8-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901a8-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="901a8-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="901a8-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="901a8-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="901a8-216">Test single sign-on</span></span>

<span data-ttu-id="901a8-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="901a8-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="901a8-218">Hello Yonyx interaktív útmutatók hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Yonyx interaktív útmutatók alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="901a8-218">When you click hello Yonyx Interactive Guides tile in hello Access Panel, you should get automatically signed-on tooyour Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="901a8-219">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="901a8-219">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="901a8-220">További források</span><span class="sxs-lookup"><span data-stu-id="901a8-220">Additional resources</span></span>

* [<span data-ttu-id="901a8-221">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="901a8-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="901a8-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="901a8-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png

