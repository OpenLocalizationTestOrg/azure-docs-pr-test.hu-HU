---
title: "Oktatóanyag: Azure Active Directoryval integrált HireVue |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és HireVue között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aadfc342-14db-4d74-a83d-f0c76f0cf63c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f890633ee4f13919c32a43d7b6cf30f2270ef6a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hirevue"></a><span data-ttu-id="d386e-103">Oktatóanyag: Azure Active Directoryval integrált HireVue</span><span class="sxs-lookup"><span data-stu-id="d386e-103">Tutorial: Azure Active Directory integration with HireVue</span></span>

<span data-ttu-id="d386e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate HireVue az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d386e-104">In this tutorial, you learn how toointegrate HireVue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d386e-105">HireVue integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d386e-105">Integrating HireVue with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d386e-106">Megadhatja a hozzáférés tooHireVue rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d386e-106">You can control in Azure AD who has access tooHireVue</span></span>
- <span data-ttu-id="d386e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHireVue (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d386e-107">You can enable your users tooautomatically get signed-on tooHireVue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d386e-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d386e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d386e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d386e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d386e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d386e-110">Prerequisites</span></span>

<span data-ttu-id="d386e-111">az Azure AD integrálása HireVue tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d386e-111">tooconfigure Azure AD integration with HireVue, you need hello following items:</span></span>

- <span data-ttu-id="d386e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d386e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d386e-113">Egy HireVue egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d386e-113">A HireVue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d386e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d386e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d386e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d386e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d386e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d386e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d386e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d386e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d386e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d386e-118">Scenario description</span></span>
<span data-ttu-id="d386e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d386e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d386e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d386e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d386e-121">Hello gyűjteményből HireVue hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d386e-121">Adding HireVue from hello gallery</span></span>
2. <span data-ttu-id="d386e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d386e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hirevue-from-hello-gallery"></a><span data-ttu-id="d386e-123">Hello gyűjteményből HireVue hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d386e-123">Adding HireVue from hello gallery</span></span>
<span data-ttu-id="d386e-124">tooconfigure hello integrációja HireVue az Azure AD-be, meg kell tooadd HireVue hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d386e-124">tooconfigure hello integration of HireVue into Azure AD, you need tooadd HireVue from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d386e-125">**tooadd HireVue hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d386e-125">**tooadd HireVue from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d386e-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d386e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d386e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d386e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d386e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d386e-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d386e-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d386e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d386e-133">Hello keresési mezőbe, írja be a **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="d386e-133">In hello search box, type **HireVue**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_search.png)

5. <span data-ttu-id="d386e-135">A hello eredmények panelen válassza ki a **HireVue**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d386e-135">In hello results panel, select **HireVue**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d386e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d386e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d386e-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján HireVue.</span><span class="sxs-lookup"><span data-stu-id="d386e-138">In this section, you configure and test Azure AD single sign-on with HireVue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d386e-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó HireVue tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d386e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HireVue is tooa user in Azure AD.</span></span> <span data-ttu-id="d386e-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello HireVue közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d386e-140">In other words, a link relationship between an Azure AD user and hello related user in HireVue needs toobe established.</span></span>

<span data-ttu-id="d386e-141">HireVue, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d386e-141">In HireVue, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d386e-142">tooconfigure és az Azure AD az egyszeri bejelentkezés HireVue-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d386e-142">tooconfigure and test Azure AD single sign-on with HireVue, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d386e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d386e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d386e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d386e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d386e-145">**[HireVue tesztfelhasználó létrehozása](#creating-a-hirevue-test-user)**  -toohave egy megfelelője a Britta Simon a HireVue, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d386e-145">**[Creating a HireVue test user](#creating-a-hirevue-test-user)** - toohave a counterpart of Britta Simon in HireVue that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d386e-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d386e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d386e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d386e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d386e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d386e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d386e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az HireVue alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d386e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HireVue application.</span></span>

<span data-ttu-id="d386e-150">**az Azure AD tooconfigure egyszeri bejelentkezést a HireVue, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d386e-150">**tooconfigure Azure AD single sign-on with HireVue, perform hello following steps:**</span></span>

1. <span data-ttu-id="d386e-151">Az Azure portál, a hello hello **HireVue** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d386e-151">In hello Azure portal, on hello **HireVue** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d386e-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d386e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_samlbase.png)

3. <span data-ttu-id="d386e-155">A hello **HireVue tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d386e-155">On hello **HireVue Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_url.png)

    <span data-ttu-id="d386e-157">a.</span><span class="sxs-lookup"><span data-stu-id="d386e-157">a.</span></span> <span data-ttu-id="d386e-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="d386e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>

    | <span data-ttu-id="d386e-159">Környezet</span><span class="sxs-lookup"><span data-stu-id="d386e-159">Environment</span></span> | <span data-ttu-id="d386e-160">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="d386e-160">URL</span></span> |
    |-------------|---|
    | <span data-ttu-id="d386e-161">Éles</span><span class="sxs-lookup"><span data-stu-id="d386e-161">Production</span></span> | `https://<companyname>.hirevue.com` |
    | <span data-ttu-id="d386e-162">Átmeneti</span><span class="sxs-lookup"><span data-stu-id="d386e-162">Staging</span></span>    | `https://<companyname>.stghv.com` |
    
    <span data-ttu-id="d386e-163">b.</span><span class="sxs-lookup"><span data-stu-id="d386e-163">b.</span></span> <span data-ttu-id="d386e-164">A hello **azonosító** szövegmező, adja meg az URL-címet:</span><span class="sxs-lookup"><span data-stu-id="d386e-164">In hello **Identifier** textbox, type a URL as:</span></span>
    
    | <span data-ttu-id="d386e-165">Környezet</span><span class="sxs-lookup"><span data-stu-id="d386e-165">Environment</span></span> | <span data-ttu-id="d386e-166">URN</span><span class="sxs-lookup"><span data-stu-id="d386e-166">URN</span></span> |
    |-------------|-----|
    | <span data-ttu-id="d386e-167">Éles</span><span class="sxs-lookup"><span data-stu-id="d386e-167">Production</span></span> |`urn:federation:hirevue.com:saml:sp:prod` |
    | <span data-ttu-id="d386e-168">Átmeneti</span><span class="sxs-lookup"><span data-stu-id="d386e-168">Staging</span></span>    | `urn:federation:hirevue.com:saml:sp:staging`|
    
    > [!NOTE] 
    > <span data-ttu-id="d386e-169">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d386e-169">These values are not real.</span></span> <span data-ttu-id="d386e-170">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="d386e-170">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d386e-171">Ügyfél [HireVue ügyfél-támogatási csoport](mailto:samlsupport@hirevue.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d386e-171">Contact [HireVue Client support team](mailto:samlsupport@hirevue.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="d386e-172">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d386e-172">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_certificate.png) 

5. <span data-ttu-id="d386e-174">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d386e-174">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d386e-176">A hello **HireVue konfigurációs** kattintson **konfigurálása HireVue** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d386e-176">On hello **HireVue Configuration** section, click **Configure HireVue** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d386e-177">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d386e-177">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_configure.png) 

7. <span data-ttu-id="d386e-179">tooconfigure egyszeri bejelentkezést a **HireVue** oldalon kell letöltött toosend hello **Certificate(Base64)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[HireVue támogatási csoport](mailto:samlsupport@hirevue.com).</span><span class="sxs-lookup"><span data-stu-id="d386e-179">tooconfigure single sign-on on **HireVue** side, you need toosend hello downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[HireVue support team](mailto:samlsupport@hirevue.com).</span></span> <span data-ttu-id="d386e-180">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d386e-180">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d386e-181">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d386e-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d386e-182">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d386e-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d386e-183">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d386e-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d386e-184">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d386e-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="d386e-185">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d386e-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d386e-187">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d386e-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d386e-188">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d386e-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d386e-190">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d386e-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d386e-192">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d386e-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d386e-194">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d386e-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d386e-196">a.</span><span class="sxs-lookup"><span data-stu-id="d386e-196">a.</span></span> <span data-ttu-id="d386e-197">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d386e-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d386e-198">b.</span><span class="sxs-lookup"><span data-stu-id="d386e-198">b.</span></span> <span data-ttu-id="d386e-199">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d386e-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d386e-200">c.</span><span class="sxs-lookup"><span data-stu-id="d386e-200">c.</span></span> <span data-ttu-id="d386e-201">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d386e-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d386e-202">d.</span><span class="sxs-lookup"><span data-stu-id="d386e-202">d.</span></span> <span data-ttu-id="d386e-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d386e-203">Click **Create**.</span></span>
 
### <a name="creating-a-hirevue-test-user"></a><span data-ttu-id="d386e-204">HireVue tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d386e-204">Creating a HireVue test user</span></span>

<span data-ttu-id="d386e-205">Ebben a szakaszban egy HireVue Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d386e-205">In this section, you create a user called Britta Simon in HireVue.</span></span> <span data-ttu-id="d386e-206">Adjon együttműködve [HireVue támogatási csoport](mailto:samlsupport@hirevue.com) tooadd hello felhasználók hello HireVue platform.</span><span class="sxs-lookup"><span data-stu-id="d386e-206">Please work with [HireVue support team](mailto:samlsupport@hirevue.com) tooadd hello users in hello HireVue platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d386e-207">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d386e-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d386e-208">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHireVue megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d386e-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHireVue.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d386e-210">**tooassign Britta Simon tooHireVue, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d386e-210">**tooassign Britta Simon tooHireVue, perform hello following steps:**</span></span>

1. <span data-ttu-id="d386e-211">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d386e-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d386e-213">Hello alkalmazások listában válassza ki a **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="d386e-213">In hello applications list, select **HireVue**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_app.png) 

3. <span data-ttu-id="d386e-215">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d386e-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d386e-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d386e-217">Click **Add** button.</span></span> <span data-ttu-id="d386e-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d386e-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d386e-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d386e-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d386e-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d386e-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d386e-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d386e-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d386e-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d386e-223">Testing single sign-on</span></span>

<span data-ttu-id="d386e-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d386e-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d386e-225">Ha a hozzáférési Panel hello hello HireVue csempe gombra kattint, automatikusan bejelentkezett tooyour HireVue alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d386e-225">When you click hello HireVue tile in hello Access Panel, you should get automatically signed-on tooyour HireVue application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d386e-226">További források</span><span class="sxs-lookup"><span data-stu-id="d386e-226">Additional resources</span></span>

* [<span data-ttu-id="d386e-227">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d386e-227">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d386e-228">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d386e-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_203.png

