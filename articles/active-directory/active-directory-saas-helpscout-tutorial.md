---
title: "Oktatóanyag: Azure Active Directoryval integrált súgó Scout |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Súgó Scout között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 58edd140eb1eb5980796ca743b5f7acd891729a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a><span data-ttu-id="972e0-103">Oktatóanyag: Azure Active Directoryval integrált Scout Súgó</span><span class="sxs-lookup"><span data-stu-id="972e0-103">Tutorial: Azure Active Directory integration with Help Scout</span></span>

<span data-ttu-id="972e0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate súgó Scout az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="972e0-104">In this tutorial, you learn how toointegrate Help Scout with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="972e0-105">Súgó Scout integrálása az Azure AD előnyei követően hello elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="972e0-105">You get hello following benefits from integrating Help Scout with Azure AD:</span></span>

- <span data-ttu-id="972e0-106">Az Azure AD-szabályozhatja a hozzáférést tooHelp Scout kik.</span><span class="sxs-lookup"><span data-stu-id="972e0-106">In Azure AD, you can control who has access tooHelp Scout.</span></span>
- <span data-ttu-id="972e0-107">Automatikusan bejelentkezhet a felhasználók tooHelp Scout egyszeri bejelentkezést és a felhasználó Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="972e0-107">You can automatically sign in your users tooHelp Scout by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="972e0-108">A fiók egyetlen, központi helyen, hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="972e0-108">You can manage your accounts in one, central location, hello Azure portal.</span></span>

<span data-ttu-id="972e0-109">További információ az Azure AD-val egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció szoftverként toolearn lásd [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="972e0-109">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="972e0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="972e0-110">Prerequisites</span></span>

<span data-ttu-id="972e0-111">tooset mentése az Azure AD integrálása súgó Scout, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="972e0-111">tooset up Azure AD integration with Help Scout, you need hello following items:</span></span>

- <span data-ttu-id="972e0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="972e0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="972e0-113">Súgó Scout előfizetés, az egyszeri bejelentkezés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="972e0-113">A Help Scout subscription, with single sign-on turned on</span></span> 

> [!NOTE]
> <span data-ttu-id="972e0-114">Ha ebben az oktatóanyagban hello lépéseket teszteli, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="972e0-114">If you test hello steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="972e0-115">Ebben az oktatóanyagban hello lépéseket tesztelési ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="972e0-115">Recommendations for testing hello steps in this tutorial:</span></span>

- <span data-ttu-id="972e0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="972e0-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="972e0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="972e0-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="972e0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="972e0-118">Scenario description</span></span>
<span data-ttu-id="972e0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="972e0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="972e0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="972e0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="972e0-121">Adja hozzá a Súgó Scout hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="972e0-121">Add Help Scout from hello gallery.</span></span>
2. <span data-ttu-id="972e0-122">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="972e0-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-help-scout-from-hello-gallery"></a><span data-ttu-id="972e0-123">Adja hozzá a Súgó Scout hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="972e0-123">Add Help Scout from hello gallery</span></span>
<span data-ttu-id="972e0-124">tooset hello integrálása súgó Scout Azure AD-val hello gyűjteményben, Súgó Scout tooyour lista kezelt SaaS-alkalmazások hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="972e0-124">tooset up hello integration of Help Scout with Azure AD, in hello gallery, add Help Scout tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="972e0-125">tooadd súgó Scout hello gyűjteményből:</span><span class="sxs-lookup"><span data-stu-id="972e0-125">tooadd Help Scout from hello gallery:</span></span>

1. <span data-ttu-id="972e0-126">A hello [Azure-portálon](https://portal.azure.com), a bal oldali menü hello, válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="972e0-126">In hello [Azure portal](https://portal.azure.com), in hello left menu, select **Azure Active Directory**.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="972e0-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="972e0-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![hello vállalati alkalmazások lap][2]
    
3. <span data-ttu-id="972e0-130">tooadd egy új alkalmazást, válassza ki **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="972e0-130">tooadd a new application, select **New application**.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="972e0-132">Hello keresési mezőbe, írja be a **súgó Scout**.</span><span class="sxs-lookup"><span data-stu-id="972e0-132">In hello search box, enter **Help Scout**.</span></span> <span data-ttu-id="972e0-133">Hello keresési eredmények között, válassza ki a **súgó Scout**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="972e0-133">In hello search results, select **Help Scout**, and then select **Add**.</span></span>

    ![Megkönnyíti a Scout hello eredményeinek listája](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="972e0-135">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="972e0-135">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="972e0-136">Ebben a szakaszban állítsa be, és az Azure AD az egyszeri bejelentkezés Scout súgó-teszthez alapján nevű tesztfelhasználó *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="972e0-136">In this section, you set up and test Azure AD single sign-on with Help Scout based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="972e0-137">Az egyszeri bejelentkezés toowork az Azure AD tooknow hello Azure AD megfelelőjére-felhasználó a Súgó Scout van szüksége.</span><span class="sxs-lookup"><span data-stu-id="972e0-137">For single sign-on toowork, Azure AD needs tooknow hello Azure AD counterpart user in Help Scout.</span></span> <span data-ttu-id="972e0-138">Egy Azure AD-felhasználó és a kapcsolódó felhasználó hello segítségével Scout közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="972e0-138">A link relationship between an Azure AD user and hello related user in Help Scout must be established.</span></span>

<span data-ttu-id="972e0-139">tooestablish hello kapcsolat, a Súgó Scout hivatkozás a **felhasználónév**, hello hello értéket **felhasználónév** az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="972e0-139">tooestablish hello link relationship, in Help Scout, for **Username**, assign hello value of hello **user name** in Azure AD.</span></span>

<span data-ttu-id="972e0-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Scout súgó a következő feladatok teljes hello-teszthez:</span><span class="sxs-lookup"><span data-stu-id="972e0-140">tooconfigure and test Azure AD single sign-on with Help Scout, complete hello following tasks:</span></span>

1. <span data-ttu-id="972e0-141">[Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="972e0-141">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="972e0-142">Ez a szolgáltatás állít be egy felhasználó toouse.</span><span class="sxs-lookup"><span data-stu-id="972e0-142">Sets up a user toouse this feature.</span></span>
2. <span data-ttu-id="972e0-143">[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="972e0-143">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="972e0-144">Az Azure AD tesztek egyszeri bejelentkezés Britta Simon hello felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="972e0-144">Tests Azure AD single sign-on with hello user Britta Simon.</span></span>
3. <span data-ttu-id="972e0-145">[Súgó Scout tesztfelhasználó létrehozása](#create-a-help-scout-test-user).</span><span class="sxs-lookup"><span data-stu-id="972e0-145">[Create a Help Scout test user](#create-a-help-scout-test-user).</span></span> <span data-ttu-id="972e0-146">Súgó Scout csatolt toohello hello felhasználói az Azure AD-ábrázolása, amely egy megfelelője a Britta Simon hoz létre.</span><span class="sxs-lookup"><span data-stu-id="972e0-146">Creates a counterpart of Britta Simon in Help Scout that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="972e0-147">[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="972e0-147">[Assign hello Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="972e0-148">Beállítja az Azure AD az egyszeri bejelentkezés Britta Simon toouse.</span><span class="sxs-lookup"><span data-stu-id="972e0-148">Sets up Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="972e0-149">[Egyszeri bejelentkezés tesztelése](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="972e0-149">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="972e0-150">Ellenőrzi, hogy a hello megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="972e0-150">Verifies that hello configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="972e0-151">Az Azure AD az egyszeri bejelentkezés beállítása</span><span class="sxs-lookup"><span data-stu-id="972e0-151">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="972e0-152">Ebben a szakaszban állíthatja be az Azure AD egyszeri bejelentkezést a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="972e0-152">In this section, you set up Azure AD single sign-on in hello Azure portal.</span></span> <span data-ttu-id="972e0-153">Ezután állítsa be az egyszeri bejelentkezés súgó Scout alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="972e0-153">Then, you set up single sign-on in your Help Scout application.</span></span>

<span data-ttu-id="972e0-154">tooset mentése az Azure AD egyszeri bejelentkezést a Súgó Scout:</span><span class="sxs-lookup"><span data-stu-id="972e0-154">tooset up Azure AD single sign-on with Help Scout:</span></span>

1. <span data-ttu-id="972e0-155">Az Azure portál, a hello hello **súgó Scout** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="972e0-155">In hello Azure portal, on hello **Help Scout** application integration page, select **Single sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés hivatkozás beállítása][4]

2. <span data-ttu-id="972e0-157">A hello **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="972e0-157">On hello **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. <span data-ttu-id="972e0-159">A **Scout tartományban Help és URL-címek**, ha azt szeretné, tooset mentése hello alkalmazás módban IDP által kezdeményezett teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="972e0-159">Under **Help Scout Domain and URLs**, if you want tooset up hello application in IDP-initiated mode, complete hello following steps:</span></span>

    1. <span data-ttu-id="972e0-160">A hello **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:`urn:auth0:helpscout:<instancename>`</span><span class="sxs-lookup"><span data-stu-id="972e0-160">In hello **Identifier** box, enter a URL that has hello following pattern: `urn:auth0:helpscout:<instancename>`</span></span>

    2. <span data-ttu-id="972e0-161">A hello **válasz URL-CÍMEN** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:`https://helpscout.auth0.com/login/callback?connection=<instancename>`</span><span class="sxs-lookup"><span data-stu-id="972e0-161">In hello **Reply URL** box, enter a URL that has hello following pattern: `https://helpscout.auth0.com/login/callback?connection=<instancename>`</span></span>

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. <span data-ttu-id="972e0-163">Ha be hello alkalmazás tooset Szolgáltató által kezdeményezett módban, jelölje be a hello **megjelenítése speciális URL-beállításainak** jelölőnégyzetet, majd ezután hello a következő:</span><span class="sxs-lookup"><span data-stu-id="972e0-163">If you want tooset up hello application in SP-initiated mode, select hello **Show advanced URL settings** check box, and then do hello following:</span></span>

    * <span data-ttu-id="972e0-164">A hello **bejelentkezési URL-cím** mezőbe írjon be egy URL-címet, amely rendelkezik a következő formátumban hello:`https://secure.helpscout.net/members/login/`</span><span class="sxs-lookup"><span data-stu-id="972e0-164">In hello **Sign on URL** box, enter a URL that has hello following format: `https://secure.helpscout.net/members/login/`</span></span>

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > <span data-ttu-id="972e0-166">hello URL-értékei csak bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="972e0-166">hello values in these URLs are for demonstration only.</span></span> <span data-ttu-id="972e0-167">Frissítse a hello értékek hello tényleges azonosító URL-t és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="972e0-167">Update hello values with hello actual identifier URL and reply URL.</span></span> <span data-ttu-id="972e0-168">tooget ezeket az értékeket, forduljon a [súgó Scout támogatási csoport](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="972e0-168">tooget these values, contact [Help Scout support team](mailto:help@helpscout.com).</span></span> 

5. <span data-ttu-id="972e0-169">A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, majd mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="972e0-169">Under **SAML Signing Certificate**, select **Metadata XML**, and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. <span data-ttu-id="972e0-171">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="972e0-171">Select **Save**.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="972e0-173">egyetlen mentése tooset bejelentkezés hello segítségével Scout oldalán, küldjön letöltött hello metaadatok XML-fájl toohello [súgó Scout támogatási csoport](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="972e0-173">tooset up single sign-on on hello Help Scout side, send hello downloaded metadata XML file toohello [Help Scout support team](mailto:help@helpscout.com).</span></span> <span data-ttu-id="972e0-174">hello segítségével Scout támogatási csoport érvényes ezt a beállítást, így hello SAML-alapú egyszeri bejelentkezés kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="972e0-174">hello Help Scout support team applies this setting so that hello SAML single sign-on connection is set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="972e0-175">Ezeket az utasításokat a hello tömör verziója olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás!</span><span class="sxs-lookup"><span data-stu-id="972e0-175">You can read a concise version of these instructions in hello [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="972e0-176">Hello app kiválasztásával hozzáadása után **Active Directory** > **vállalati alkalmazások**, jelölje be hello **egyszeri bejelentkezés** fülre. A beágyazott hello dokumentációja a hello **konfigurációs** szakasz hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="972e0-176">After you add hello app by selecting **Active Directory** > **Enterprise Applications**, select hello **Single Sign-On** tab. You can access hello embedded documentation in hello **Configuration** section, at hello bottom of hello page.</span></span> <span data-ttu-id="972e0-177">További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="972e0-177">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="972e0-178">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="972e0-178">Create an Azure AD test user</span></span>

<span data-ttu-id="972e0-179">Ebben a szakaszban az Azure-portálon hello Britta Simon nevű tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="972e0-179">In this section, in hello Azure portal, you create a test user named Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="972e0-181">az Azure AD tesztfelhasználó toocreate:</span><span class="sxs-lookup"><span data-stu-id="972e0-181">toocreate a test user in Azure AD:</span></span>

1. <span data-ttu-id="972e0-182">Hello Azure-portálon, hello bal oldali menüben válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="972e0-182">In hello Azure portal, in hello left menu, select **Azure Active Directory**.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="972e0-184">toodisplay hello listában válassza ki a felhasználók **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="972e0-184">toodisplay hello list of users, select **Users and groups**, and then select **All users**.</span></span>

    ![Felhasználók és csoportok kiválasztása, és válassza a minden felhasználó](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="972e0-186">tooopen hello **felhasználói** párbeszédpanelen hello hello tetején **minden felhasználó** lapon jelölje be **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="972e0-186">tooopen hello **User** dialog box, at hello top of hello **All Users** page, select **Add**.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="972e0-188">A hello **felhasználói** teljes hello lépéseket a következő párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="972e0-188">In hello **User** dialog box, complete hello following steps:</span></span>

    1. <span data-ttu-id="972e0-189">A hello **neve** adja meg a **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="972e0-189">In hello **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="972e0-190">A hello **felhasználónév** mezőbe írja be a felhasználó Britta Simon hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="972e0-190">In hello **User name** box, enter hello email address of user Britta Simon.</span></span>

    3. <span data-ttu-id="972e0-191">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="972e0-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    4. <span data-ttu-id="972e0-192">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="972e0-192">Select **Create**.</span></span>

        ![hello felhasználó párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a><span data-ttu-id="972e0-194">Súgó Scout tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="972e0-194">Create a Help Scout test user</span></span>

<span data-ttu-id="972e0-195">Ez a szakasz hello célja, a felhasználó a Súgó Scout Britta Simon nevű toocreate.</span><span class="sxs-lookup"><span data-stu-id="972e0-195">hello object of this section is toocreate a user named Britta Simon in Help Scout.</span></span> <span data-ttu-id="972e0-196">Súgó Scout támogatja közvetlenül az igény (szerinti JIT) átadása, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="972e0-196">Help Scout supports just-in-time (JIT) provisioning, which is turned on by default.</span></span>

<span data-ttu-id="972e0-197">Ebben a szakaszban nem művelet vagy feladat toocomplete van.</span><span class="sxs-lookup"><span data-stu-id="972e0-197">In this section, there's no action or task toocomplete.</span></span> <span data-ttu-id="972e0-198">Ha a felhasználó nem létezik a Súgó Scout, egy új tooaccess súgó Scout tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="972e0-198">If a user doesn't already exist in Help Scout, a new one is created when you attempt tooaccess Help Scout.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="972e0-199">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="972e0-199">Assign hello Azure AD test user</span></span>

<span data-ttu-id="972e0-200">Ebben a szakaszban engedélyezése hello felhasználó Britta Simon toouse az Azure AD egyszeri bejelentkezést a hello felhasználói fiók hozzáférési tooHelp Scout megadásával.</span><span class="sxs-lookup"><span data-stu-id="972e0-200">In this section, you allow hello user Britta Simon toouse Azure AD single sign-on by granting hello user account access tooHelp Scout.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="972e0-202">tooassign Britta Simon tooHelp Scout:</span><span class="sxs-lookup"><span data-stu-id="972e0-202">tooassign Britta Simon tooHelp Scout:</span></span>

1. <span data-ttu-id="972e0-203">Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, és folytassa a toohello könyvtár nézetben.</span><span class="sxs-lookup"><span data-stu-id="972e0-203">In hello Azure portal, open hello applications view, and then go toohello directory view.</span></span> <span data-ttu-id="972e0-204">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="972e0-204">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="972e0-206">Hello alkalmazások listában válassza ki a **súgó Scout**.</span><span class="sxs-lookup"><span data-stu-id="972e0-206">In hello applications list, select **Help Scout**.</span></span>

    ![hello Scout Súgó hivatkozásra hello alkalmazások listája](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. <span data-ttu-id="972e0-208">Hello bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="972e0-208">In hello left menu, select **Users and groups**.</span></span>

    ![hello felhasználók és csoportok hivatkozás][202]

4. <span data-ttu-id="972e0-210">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="972e0-210">Select **Add**.</span></span> <span data-ttu-id="972e0-211">Ezután a hello **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="972e0-211">Then, on hello **Add Assignment** page, select **Users and groups**.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="972e0-213">A hello **felhasználók és csoportok** lapra, jelölje be a felhasználók hello listáján **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="972e0-213">On hello **Users and groups** page, in hello list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="972e0-214">A hello **felhasználók és csoportok** lapon jelölje be **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="972e0-214">On hello **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="972e0-215">A hello **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="972e0-215">On hello **Add Assignment** page, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="972e0-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="972e0-216">Test single sign-on</span></span>

<span data-ttu-id="972e0-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="972e0-217">In this section, you test your Azure AD single sign-on configuration by using hello access panel.</span></span>

<span data-ttu-id="972e0-218">Hello segítségével Scout csempe hello hozzáférési panelen válassza ki, amikor meg kell automatikusan megtörténik a tooyour súgó Scout alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="972e0-218">When you select hello Help Scout tile in hello access panel, you should be automatically signed in tooyour Help Scout application.</span></span>

<span data-ttu-id="972e0-219">A hozzáférési panel kapcsolatos további információkért lásd: [bemutatása toohello hozzáférési panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="972e0-219">For more information about the access panel, see [Introduction toohello access panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="972e0-220">További források</span><span class="sxs-lookup"><span data-stu-id="972e0-220">Additional resources</span></span>

* [<span data-ttu-id="972e0-221">Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="972e0-221">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="972e0-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="972e0-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

