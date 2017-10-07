---
title: "Oktatóanyag: Azure Active Directoryval integrált Clarizen |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Clarizen között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: f24ccda3b90e5df9a203a444dfda905043b30276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a><span data-ttu-id="26d8b-103">Oktatóanyag: Azure Active Directoryval integrált Clarizen</span><span class="sxs-lookup"><span data-stu-id="26d8b-103">Tutorial: Azure Active Directory integration with Clarizen</span></span>

<span data-ttu-id="26d8b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Azure Active Directory (Azure AD) a Clarizen.</span><span class="sxs-lookup"><span data-stu-id="26d8b-104">In this tutorial, you learn how toointegrate Azure Active Directory (Azure AD) with Clarizen.</span></span> <span data-ttu-id="26d8b-105">Ez integrációs lehetőséget biztosít, akkor a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="26d8b-105">This integration gives you hello following benefits:</span></span>

- <span data-ttu-id="26d8b-106">Azt is szabályozhatja, az Azure AD-hozzáférés tooClarizen rendelkező.</span><span class="sxs-lookup"><span data-stu-id="26d8b-106">You can control, in Azure AD, who has access tooClarizen.</span></span>
- <span data-ttu-id="26d8b-107">Engedélyezheti a felhasználók toobe automatikusan megtörténik a tooClarizen (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="26d8b-107">You can enable your users toobe automatically signed in tooClarizen (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="26d8b-108">A fiók egyetlen központi helyen, hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="26d8b-108">You can manage your accounts in one central location, hello Azure portal.</span></span>

<span data-ttu-id="26d8b-109">Ebben az oktatóanyagban hello forgatókönyv két fő feladatokat foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="26d8b-109">hello scenario in this tutorial consists of two main tasks:</span></span>

1. <span data-ttu-id="26d8b-110">Adja hozzá a Clarizen hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="26d8b-110">Add Clarizen from hello gallery.</span></span>
2. <span data-ttu-id="26d8b-111">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="26d8b-111">Configure and test Azure AD single sign-on.</span></span>

<span data-ttu-id="26d8b-112">Ha szoftver további adatait, és az Azure AD egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="26d8b-112">If you want more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26d8b-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="26d8b-113">Prerequisites</span></span>
<span data-ttu-id="26d8b-114">az Azure AD integrálása Clarizen tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="26d8b-114">tooconfigure Azure AD integration with Clarizen, you need hello following items:</span></span>

- <span data-ttu-id="26d8b-115">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="26d8b-115">An Azure AD subscription</span></span>
- <span data-ttu-id="26d8b-116">Az egyszeri bejelentkezés engedélyezett Clarizen előfizetés</span><span class="sxs-lookup"><span data-stu-id="26d8b-116">A Clarizen subscription that's enabled for single sign-on</span></span>

<span data-ttu-id="26d8b-117">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="26d8b-117">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="26d8b-118">Az Azure AD az egyszeri bejelentkezés tesztelése egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="26d8b-118">Test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="26d8b-119">Az éles környezetben ne használjon, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="26d8b-119">Don't use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="26d8b-120">Ha még nem rendelkezik az Azure AD-tesztelési környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="26d8b-120">If you don't have an Azure AD test environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="add-clarizen-from-hello-gallery"></a><span data-ttu-id="26d8b-121">Adja hozzá a Clarizen hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="26d8b-121">Add Clarizen from hello gallery</span></span>
<span data-ttu-id="26d8b-122">az Azure AD-be Clarizen tooconfigure hello integrációja Clarizen hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="26d8b-122">tooconfigure hello integration of Clarizen into Azure AD, add Clarizen from hello gallery tooyour list of managed SaaS apps.</span></span>

1. <span data-ttu-id="26d8b-123">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, kattintson a hello **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-123">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory ikonra][1]

2. <span data-ttu-id="26d8b-125">Kattintson a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-125">Click **Enterprise applications**.</span></span> <span data-ttu-id="26d8b-126">Kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-126">Then click **All applications**.</span></span>

    ![Kattintson a "Vállalati alkalmazások" és "Összes alkalmazás"][2]

3. <span data-ttu-id="26d8b-128">Kattintson a hello **Hozzáadás** hello párbeszédpanel hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-128">Click hello **Add** button at hello top of hello dialog box.</span></span>

    ![hello "Hozzáadás" gombra][3]

4. <span data-ttu-id="26d8b-130">Hello keresési mezőbe, írja be a **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-130">In hello search box, type **Clarizen**.</span></span>

    ![Hello keresőmezőbe írja be a "Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. <span data-ttu-id="26d8b-132">Hello eredmények ablaktábláján jelöljön ki **Clarizen**, és kattintson a **Hozzáadás** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26d8b-132">In hello results pane, select **Clarizen**, and then click **Add** tooadd hello application.</span></span>

    ![Válassza ki a Clarizen hello eredmények panelen](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="26d8b-134">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26d8b-134">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="26d8b-135">A következő részekben hello, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Clarizen hello tesztfelhasználó Britta Simon alapján.</span><span class="sxs-lookup"><span data-stu-id="26d8b-135">In hello following sections, you configure and test Azure AD single sign-on with Clarizen based on hello test user Britta Simon.</span></span>

<span data-ttu-id="26d8b-136">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Clarizen tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="26d8b-136">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Clarizen is tooa user in Azure AD.</span></span> <span data-ttu-id="26d8b-137">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Clarizen közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="26d8b-137">In other words, a link relationship between an Azure AD user and hello related user in Clarizen needs toobe established.</span></span> <span data-ttu-id="26d8b-138">A hivatkozás kapcsolatot létesíteni hello értékkel **felhasználónév** hello értékeként Azure AD-ben **felhasználónév** a Clarizen.</span><span class="sxs-lookup"><span data-stu-id="26d8b-138">You establish this link relationship by assigning hello value of **user name** in Azure AD as hello value of **Username** in Clarizen.</span></span>

<span data-ttu-id="26d8b-139">tooconfigure és az Azure AD az egyszeri bejelentkezés Clarizen, a következő építőelemeket teljes hello-teszthez:</span><span class="sxs-lookup"><span data-stu-id="26d8b-139">tooconfigure and test Azure AD single sign-on with Clarizen, complete hello following building blocks:</span></span>

1. <span data-ttu-id="26d8b-140">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="26d8b-140">**[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="26d8b-141">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  Britta Simon az Azure AD az egyszeri bejelentkezés tootest.</span><span class="sxs-lookup"><span data-stu-id="26d8b-141">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="26d8b-142">**[Clarizen tesztfelhasználó létrehozása](#create-a-clarizen-test-user)**  toohave Britta Simon Clarizen, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="26d8b-142">**[Create a Clarizen test user](#create-a-clarizen-test-user)** toohave a counterpart of Britta Simon in Clarizen that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="26d8b-143">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="26d8b-143">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="26d8b-144">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="26d8b-144">**[Test single sign-on](#test-single-sign-on)** tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="26d8b-145">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26d8b-145">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="26d8b-146">Az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az Clarizen alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="26d8b-146">Enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Clarizen application.</span></span>

1. <span data-ttu-id="26d8b-147">Az Azure portál, a hello hello **Clarizen** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-147">In hello Azure portal, on hello **Clarizen** application integration page, click **Single sign-on**.</span></span>

    ![Kattintson az "Egyszeri bejelentkezéshez"][4]

2. <span data-ttu-id="26d8b-149">A hello **egyszeri bejelentkezés** párbeszédpanelen a **mód**, jelölje be **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="26d8b-149">In hello **Single sign-on** dialog box, for **Mode**, select **SAML-based Sign-on** tooenable single sign-on.</span></span>

    !["SAML-alapú bejelentkezés" kiválasztása](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. <span data-ttu-id="26d8b-151">A hello **Clarizen tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26d8b-151">In hello **Clarizen Domain and URLs** section, perform hello following steps:</span></span>

    ![Jelölőnégyzetéből azonosítója és a válasz URL-címe](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    <span data-ttu-id="26d8b-153">a.</span><span class="sxs-lookup"><span data-stu-id="26d8b-153">a.</span></span> <span data-ttu-id="26d8b-154">A hello **azonosító** mezőbe típus hello értékével megegyező: **Clarizen**</span><span class="sxs-lookup"><span data-stu-id="26d8b-154">In hello **Identifier** box, type hello value as: **Clarizen**</span></span>

    <span data-ttu-id="26d8b-155">b.</span><span class="sxs-lookup"><span data-stu-id="26d8b-155">b.</span></span> <span data-ttu-id="26d8b-156">A hello **válasz URL-CÍMEN** mezőbe írja be egy URL-cím a következő mintát hello segítségével: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span><span class="sxs-lookup"><span data-stu-id="26d8b-156">In hello **Reply URL** box, type a URL by using hello following pattern: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span></span>

    > [!NOTE]
    > <span data-ttu-id="26d8b-157">Ezek a hello valódi értékek nem.</span><span class="sxs-lookup"><span data-stu-id="26d8b-157">These are not hello real values.</span></span> <span data-ttu-id="26d8b-158">Toouse hello tényleges azonosítója és válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="26d8b-158">You have toouse hello actual identifier and reply URL.</span></span> <span data-ttu-id="26d8b-159">Itt javasoljuk, hogy Ön hello egyedi értéket használja egy karakterlánc hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="26d8b-159">Here we suggest that you use hello unique value of a string as hello identifier.</span></span> <span data-ttu-id="26d8b-160">tooget hello tényleges értékek, a kapcsolattartási hello [Clarizen támogatási csoport](https://success.clarizen.com/hc/en-us/requests/new).</span><span class="sxs-lookup"><span data-stu-id="26d8b-160">tooget hello actual values, contact hello [Clarizen support team](https://success.clarizen.com/hc/en-us/requests/new).</span></span>

4. <span data-ttu-id="26d8b-161">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-161">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Kattintson a "Hozható létre új tanúsítvány"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. <span data-ttu-id="26d8b-163">A hello **új tanúsítvány létrehozása** párbeszédpanel mezőben, hello naptár ikonra, és válassza ki a lejárati dátummal.</span><span class="sxs-lookup"><span data-stu-id="26d8b-163">In hello **Create New Certificate** dialog box, click hello calendar icon and select an expiry date.</span></span> <span data-ttu-id="26d8b-164">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-164">Then click **Save**.</span></span>

    ![Válassza ki, majd lejárati dátummal mentése](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="26d8b-166">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-166">In hello **SAML Signing Certificate** section, select **Make new certificate active**, and then click **Save**.</span></span>

    ![Adja meg a megfelelő active hello új tanúsítványt a hello jelölőnégyzet](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. <span data-ttu-id="26d8b-168">A hello **helyettesítő tanúsítvány** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-168">In hello **Rollover certificate** dialog box, click **OK**.</span></span>

    ![Kattintson az "OK", amelyet toomake hello tanúsítvány aktív tooconfirm](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="26d8b-170">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="26d8b-170">In hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Kattintson a "Tanúsítvány (Base64)" toostart hello letöltése](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. <span data-ttu-id="26d8b-172">A hello **Clarizen konfigurációs** kattintson **konfigurálása Clarizen** tooopen hello **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="26d8b-172">In hello **Clarizen Configuration** section, click **Configure Clarizen** tooopen hello **Configure sign-on** window.</span></span>

    ![Kattintson a "Clarizen konfigurálása"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    !["Bejelentkezés konfigurálása" ablak, beleértve a fájlok és az URL-címek](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. <span data-ttu-id="26d8b-175">Egy másik webes böngészőablakban a bejelentkezés tooyour Clarizen vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="26d8b-175">In a different web browser window, sign in tooyour Clarizen company site as an administrator.</span></span>

11. <span data-ttu-id="26d8b-176">Kattintson a felhasználónevére, majd **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-176">Click your username, and then click **Settings**.</span></span>

    <span data-ttu-id="26d8b-177">![Kattintson a "Beállítások" a felhasználónév](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="26d8b-177">![Clicking "Settings" under your username](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")</span></span>

12. <span data-ttu-id="26d8b-178">Kattintson a hello **globális beállítások** fülre. Ezt követően következő túl**összevont hitelesítési**, kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-178">Click hello **Global Settings** tab. Then, next too**Federated Authentication**, click **edit**.</span></span>

    <span data-ttu-id="26d8b-179">!["Globális beállítások" lapon](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globális beállítások")</span><span class="sxs-lookup"><span data-stu-id="26d8b-179">!["Global Settings" tab](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")</span></span>

13. <span data-ttu-id="26d8b-180">A hello **összevont hitelesítési** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26d8b-180">In hello **Federated Authentication** dialog box, perform hello following steps:</span></span>

    <span data-ttu-id="26d8b-181">![A párbeszédpanel "Összevont hitelesítési"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "összevont hitelesítési")</span><span class="sxs-lookup"><span data-stu-id="26d8b-181">!["Federated Authentication" dialog box](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")</span></span>

    <span data-ttu-id="26d8b-182">a.</span><span class="sxs-lookup"><span data-stu-id="26d8b-182">a.</span></span> <span data-ttu-id="26d8b-183">Válassza ki **engedélyezése összevont hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-183">Select **Enable Federated Authentication**.</span></span>

    <span data-ttu-id="26d8b-184">b.</span><span class="sxs-lookup"><span data-stu-id="26d8b-184">b.</span></span> <span data-ttu-id="26d8b-185">Kattintson a **feltöltése** tooupload a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="26d8b-185">Click **Upload** tooupload your downloaded certificate.</span></span>

    <span data-ttu-id="26d8b-186">c.</span><span class="sxs-lookup"><span data-stu-id="26d8b-186">c.</span></span> <span data-ttu-id="26d8b-187">A hello **bejelentkezési URL** mezőbe írja be a hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** hello Azure AD alkalmazás-konfigurációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="26d8b-187">In hello **Sign-in URL** box, enter hello value of **SAML Single Sign-On Service URL** from hello Azure AD application configuration window.</span></span>

    <span data-ttu-id="26d8b-188">d.</span><span class="sxs-lookup"><span data-stu-id="26d8b-188">d.</span></span> <span data-ttu-id="26d8b-189">A hello **Sign-out URL-cím** mezőbe írja be a hello értékének **Sign-Out URL-cím** hello Azure AD alkalmazás-konfigurációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="26d8b-189">In hello **Sign-out URL** box, enter hello value of **Sign-Out URL** from hello Azure AD application configuration window.</span></span>

    <span data-ttu-id="26d8b-190">e.</span><span class="sxs-lookup"><span data-stu-id="26d8b-190">e.</span></span> <span data-ttu-id="26d8b-191">Válassza ki **használja a FELADÁS egy vagy több**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-191">Select **Use POST**.</span></span>

    <span data-ttu-id="26d8b-192">f.</span><span class="sxs-lookup"><span data-stu-id="26d8b-192">f.</span></span> <span data-ttu-id="26d8b-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-193">Click **Save**.</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="26d8b-194">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="26d8b-194">Create an Azure AD test user</span></span>
<span data-ttu-id="26d8b-195">Hello Azure-portálon, az úgynevezett Britta Simon tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="26d8b-195">In hello Azure portal, create a test user called Britta Simon.</span></span>

![Név és e-mail cím hello Azure AD-teszt felhasználó][100]

1. <span data-ttu-id="26d8b-197">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-197">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory ikonra](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="26d8b-199">Kattintson a **felhasználók és csoportok**, és kattintson a **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="26d8b-199">Click **Users and groups**, and then click **All users** toodisplay hello list of users.</span></span>

    ![Kattintson a "Felhasználók és csoportok" és "Minden felhasználó"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="26d8b-201">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="26d8b-201">At hello top of hello dialog box, click **Add** tooopen hello **User** dialog box.</span></span>

    ![hello "Hozzáadás" gombra](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="26d8b-203">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26d8b-203">In hello **User** dialog box, perform hello following steps:</span></span>

    !["User" párbeszédpanel a nevét, e-mail címét és jelszavát kitöltve](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    <span data-ttu-id="26d8b-205">a.</span><span class="sxs-lookup"><span data-stu-id="26d8b-205">a.</span></span> <span data-ttu-id="26d8b-206">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-206">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="26d8b-207">b.</span><span class="sxs-lookup"><span data-stu-id="26d8b-207">b.</span></span> <span data-ttu-id="26d8b-208">A hello **felhasználónév** mezőbe típus hello hello Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="26d8b-208">In hello **User name** box, type hello email address of hello Britta Simon account.</span></span>

    <span data-ttu-id="26d8b-209">c.</span><span class="sxs-lookup"><span data-stu-id="26d8b-209">c.</span></span> <span data-ttu-id="26d8b-210">Válassza ki **megjelenítése jelszó** írja le hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-210">Select **Show Password** and write down hello value of **Password**.</span></span>

    <span data-ttu-id="26d8b-211">d.</span><span class="sxs-lookup"><span data-stu-id="26d8b-211">d.</span></span> <span data-ttu-id="26d8b-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-212">Click **Create**.</span></span>

### <a name="create-a-clarizen-test-user"></a><span data-ttu-id="26d8b-213">Clarizen tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="26d8b-213">Create a Clarizen test user</span></span>
<span data-ttu-id="26d8b-214">tooenable az Azure AD felhasználók toosign a tooClarizen, el kell juttatnia felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="26d8b-214">tooenable Azure AD users toosign in tooClarizen, you must provision user accounts.</span></span> <span data-ttu-id="26d8b-215">Clarizen hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="26d8b-215">In hello case of Clarizen, provisioning is a manual task.</span></span>

1. <span data-ttu-id="26d8b-216">Jelentkezzen be tooyour Clarizen vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="26d8b-216">Sign in tooyour Clarizen company site as an administrator.</span></span>

2. <span data-ttu-id="26d8b-217">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-217">Click **People**.</span></span>

    <span data-ttu-id="26d8b-218">![Kattintson a "Személyek"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="26d8b-218">![Clicking "People"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="26d8b-219">Kattintson a **felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-219">Click **Invite User**.</span></span>

    <span data-ttu-id="26d8b-220">!["A felhasználó meghívása" gomb](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "felhasználók meghívása")</span><span class="sxs-lookup"><span data-stu-id="26d8b-220">!["Invite User" button](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="26d8b-221">A hello **meghívása személyek** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26d8b-221">In hello **Invite People** dialog box, perform hello following steps:</span></span>

    <span data-ttu-id="26d8b-222">!["Felkérése" párbeszédpanelen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "személyek meghívása")</span><span class="sxs-lookup"><span data-stu-id="26d8b-222">!["Invite People" dialog box](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="26d8b-223">a.</span><span class="sxs-lookup"><span data-stu-id="26d8b-223">a.</span></span> <span data-ttu-id="26d8b-224">A hello **E-mail** mezőbe típus hello hello Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="26d8b-224">In hello **Email** box, type hello email address of hello Britta Simon account.</span></span>

    <span data-ttu-id="26d8b-225">b.</span><span class="sxs-lookup"><span data-stu-id="26d8b-225">b.</span></span> <span data-ttu-id="26d8b-226">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-226">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="26d8b-227">hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="26d8b-227">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="26d8b-228">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="26d8b-228">Assign hello Azure AD test user</span></span>
<span data-ttu-id="26d8b-229">Britta Simon toouse Azure egyszeri bejelentkezés engedélyezése az access tooClarizen megadásával.</span><span class="sxs-lookup"><span data-stu-id="26d8b-229">Enable Britta Simon toouse Azure single sign-on by granting her access tooClarizen.</span></span>

![Hozzárendelt tesztfelhasználó számára][200]

1. <span data-ttu-id="26d8b-231">Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, toohello könyvtár nézetben keresse meg, kattintson a **vállalati alkalmazások**, és kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-231">In hello Azure portal, open hello applications view, browse toohello directory view, click **Enterprise applications**, and then click **All applications**.</span></span>

    ![Kattintson a "Vállalati alkalmazások" és "Összes alkalmazás"][201]

2. <span data-ttu-id="26d8b-233">Hello alkalmazások listában válassza ki a **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-233">In hello applications list, select **Clarizen**.</span></span>

    ![Hello listában Clarizen kiválasztása](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. <span data-ttu-id="26d8b-235">Hello bal oldali ablaktáblában kattintson **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-235">In hello left pane, click **Users and groups**.</span></span>

    ![Kattintson a "Felhasználók és csoportok"][202]

4. <span data-ttu-id="26d8b-237">Kattintson a hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-237">Click hello **Add** button.</span></span> <span data-ttu-id="26d8b-238">Ezt követően a hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="26d8b-238">Then, in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![hello "Hozzáadás" gombra és hello "Hozzárendelés hozzáadása" párbeszédpanel][203]

5. <span data-ttu-id="26d8b-240">A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **Britta Simon** felhasználók hello listájában.</span><span class="sxs-lookup"><span data-stu-id="26d8b-240">In hello **Users and groups** dialog box, select **Britta Simon** in hello list of users.</span></span>

6. <span data-ttu-id="26d8b-241">A hello **felhasználók és csoportok** párbeszédpanel hello kattintson **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-241">In hello **Users and groups** dialog box, click hello **Select** button.</span></span>

7. <span data-ttu-id="26d8b-242">A hello **hozzáadása hozzárendelés** párbeszédpanel hello kattintson **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="26d8b-242">In hello **Add Assignment** dialog box, click hello **Assign** button.</span></span>

### <a name="test-single-sign-on"></a><span data-ttu-id="26d8b-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="26d8b-243">Test single sign-on</span></span>
<span data-ttu-id="26d8b-244">Tesztelje az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével.</span><span class="sxs-lookup"><span data-stu-id="26d8b-244">Test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="26d8b-245">Ha a hozzáférési Panel hello hello Clarizen csempe gombra kattint, akkor kell automatikusan megtörténik a tooyour Clarizen alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26d8b-245">When you click hello Clarizen tile in hello Access Panel, you should be automatically signed in tooyour Clarizen application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26d8b-246">További források</span><span class="sxs-lookup"><span data-stu-id="26d8b-246">Additional resources</span></span>

* [<span data-ttu-id="26d8b-247">Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="26d8b-247">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="26d8b-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="26d8b-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
