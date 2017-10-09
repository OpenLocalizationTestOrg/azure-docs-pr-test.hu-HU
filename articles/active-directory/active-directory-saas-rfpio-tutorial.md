---
title: "Oktatóanyag: Azure Active Directoryval integrált RFPIO |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és RFPIO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e0c692276276edd8f859e73d81cf54d75a65957a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a><span data-ttu-id="8f10b-103">Oktatóanyag: Azure Active Directoryval integrált RFPIO</span><span class="sxs-lookup"><span data-stu-id="8f10b-103">Tutorial: Azure Active Directory integration with RFPIO</span></span>

<span data-ttu-id="8f10b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate RFPIO az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8f10b-104">In this tutorial, you learn how toointegrate RFPIO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8f10b-105">RFPIO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8f10b-105">Integrating RFPIO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8f10b-106">Az Azure AD hozzáférési tooRFPIO rendelkező szabályozhatja ki.</span><span class="sxs-lookup"><span data-stu-id="8f10b-106">You can control who in Azure AD who has access tooRFPIO.</span></span>
- <span data-ttu-id="8f10b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRFPIO (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="8f10b-107">You can enable your users tooautomatically get signed-on tooRFPIO (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="8f10b-108">A fiók egyetlen központi helyen – hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="8f10b-108">You can manage your accounts in one central location--hello Azure portal.</span></span>

<span data-ttu-id="8f10b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8f10b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f10b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f10b-110">Prerequisites</span></span>

<span data-ttu-id="8f10b-111">az Azure AD integrálása RFPIO tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8f10b-111">tooconfigure Azure AD integration with RFPIO, you need hello following items:</span></span>

- <span data-ttu-id="8f10b-112">Az Azure AD-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8f10b-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="8f10b-113">RFPIO egyszeri bejelentkezést a alkalmas előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8f10b-113">A RFPIO single sign-on-enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="8f10b-114">Ebben az oktatóanyagban egy éles környezetben tootest hello lépéseket használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="8f10b-114">We don't recommend that you use a production environment tootest hello steps in this tutorial.</span></span>

<span data-ttu-id="8f10b-115">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="8f10b-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="8f10b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8f10b-116">Do not use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="8f10b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f10b-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8f10b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8f10b-118">Scenario description</span></span>
<span data-ttu-id="8f10b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8f10b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8f10b-120">Ez az oktatóanyag a hello a forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8f10b-120">hello scenario that's outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8f10b-121">Hozzáadás RFPIO hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="8f10b-121">Adding RFPIO from hello gallery.</span></span>
2. <span data-ttu-id="8f10b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="8f10b-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-rfpio-from-hello-gallery"></a><span data-ttu-id="8f10b-123">Adja hozzá a RFPIO hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8f10b-123">Add RFPIO from hello gallery</span></span>
<span data-ttu-id="8f10b-124">tooconfigure hello integrációja RFPIO az Azure AD-be, meg kell tooadd RFPIO hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8f10b-124">tooconfigure hello integration of RFPIO into Azure AD, you need tooadd RFPIO from hello gallery tooyour list of managed SaaS apps.</span></span>

### <a name="tooadd-rfpio-from-hello-gallery"></a><span data-ttu-id="8f10b-125">tooadd RFPIO hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8f10b-125">tooadd RFPIO from hello gallery</span></span>

1. <span data-ttu-id="8f10b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, jelölje ki a hello **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f10b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8f10b-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8f10b-130">tooadd egy új alkalmazást, jelölje be hello **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8f10b-130">tooadd a new application, select hello **New application** button on hello top of dialog box.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8f10b-132">Hello keresési mezőbe, írja be a **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-132">In hello search box, type **RFPIO**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. <span data-ttu-id="8f10b-134">Hello eredmények panelen, jelölje ki a **RFPIO**, majd válassza ki a hello **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8f10b-134">In hello results panel, select **RFPIO**, and then select hello **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8f10b-136">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f10b-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="8f10b-137">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján RFPIO.</span><span class="sxs-lookup"><span data-stu-id="8f10b-137">In this section, you configure and test Azure AD single sign-on with RFPIO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8f10b-138">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello kapcsolatot RFPIO megfelelőjére felhasználó, ezért a felhasználó az Azure AD között van.</span><span class="sxs-lookup"><span data-stu-id="8f10b-138">For single sign-on toowork, Azure AD needs tooknow what hello relationship is between counterpart user in RFPIO and a user in Azure AD.</span></span> <span data-ttu-id="8f10b-139">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello RFPIO közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8f10b-139">In other words, a link relationship between an Azure AD user and hello related user in RFPIO needs toobe established.</span></span>

<span data-ttu-id="8f10b-140">RFPIO, rendelje hozzá hello értékének **felhasználónév** hello értékeként Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8f10b-140">In RFPIO, assign hello value of **user name** in Azure AD as hello value of  **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8f10b-141">tooconfigure és az Azure AD az egyszeri bejelentkezés RFPIO-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8f10b-141">tooconfigure and test Azure AD single sign-on with RFPIO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8f10b-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**--tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8f10b-142">**[Configure Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**--tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8f10b-143">**[Hozzon létre egy Azure AD-teszt felhasználó](#creating-an-azure-ad-test-user)**--az Azure AD egyszeri bejelentkezést a Britta Simon tootest.</span><span class="sxs-lookup"><span data-stu-id="8f10b-143">**[Create an Azure AD test user](#creating-an-azure-ad-test-user)**-- tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8f10b-144">**[RFPIO tesztfelhasználó létrehozása](#creating-a-rfpio-test-user)**  --toohave egy megfelelője a Britta Simon a RFPIO, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8f10b-144">**[Create a RFPIO test user](#creating-a-rfpio-test-user)** --toohave a counterpart of Britta Simon in RFPIO that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8f10b-145">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**tooenable--az Azure AD toouse Britta Simon egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="8f10b-145">**[Assign hello Azure AD test user](#assigning-the-azure-ad-test-user)**--tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8f10b-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  --tooverify Ha hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8f10b-146">**[Test Single Sign-On](#testing-single-sign-on)** --tooverify if hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8f10b-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f10b-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8f10b-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az RFPIO alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8f10b-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RFPIO application.</span></span>

<span data-ttu-id="8f10b-149">**az Azure AD tooconfigure egyszeri bejelentkezést a RFPIO, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8f10b-149">**tooconfigure Azure AD single sign-on with RFPIO, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f10b-150">Az Azure portál, a hello hello **RFPIO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-150">In hello Azure portal, on hello **RFPIO** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8f10b-152">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8f10b-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. <span data-ttu-id="8f10b-154">A hello **RFPIO tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="8f10b-154">On hello **RFPIO Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    <span data-ttu-id="8f10b-156">a.</span><span class="sxs-lookup"><span data-stu-id="8f10b-156">a.</span></span> <span data-ttu-id="8f10b-157">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="8f10b-157">In hello **Identifier** textbox, type hello URL: `https://www.rfpio.com`</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    <span data-ttu-id="8f10b-159">b.</span><span class="sxs-lookup"><span data-stu-id="8f10b-159">b.</span></span> <span data-ttu-id="8f10b-160">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-160">Check **Show advanced URL settings**.</span></span>

    <span data-ttu-id="8f10b-161">c.</span><span class="sxs-lookup"><span data-stu-id="8f10b-161">c.</span></span> <span data-ttu-id="8f10b-162">A hello **továbbítási állapotot** szövegmezőben adjon meg egy karakterláncértéket.</span><span class="sxs-lookup"><span data-stu-id="8f10b-162">In hello **Relay State** textbox enter a string value.</span></span> <span data-ttu-id="8f10b-163">Ügyfél [RFPIO támogatási csoport](https://www.rfpio.com/contact/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8f10b-163">Contact [RFPIO support team](https://www.rfpio.com/contact/) tooget this value.</span></span> 

4. <span data-ttu-id="8f10b-164">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-164">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="8f10b-165">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="8f10b-165">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    <span data-ttu-id="8f10b-167">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://www.app.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="8f10b-167">In hello **Sign on URL** textbox, type hello URL: `https://www.app.rfpio.com`</span></span>

5. <span data-ttu-id="8f10b-168">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8f10b-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. <span data-ttu-id="8f10b-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f10b-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="8f10b-172">Egy másik webes böngészőablakban, bejelentkezési toohello **RFPIO** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8f10b-172">In a different web browser window, login toohello **RFPIO** website as an administrator.</span></span>

8. <span data-ttu-id="8f10b-173">Kattintson a hello alsó bal sarok legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="8f10b-173">Click on hello bottom left corner dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. <span data-ttu-id="8f10b-175">Kattintson a hello **szervezeti beállítások**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-175">Click on hello **Organization Settings**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. <span data-ttu-id="8f10b-177">Kattintson a hello **szolgáltatások & integrációs**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-177">Click on hello **FEATURES & INTEGRATION**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. <span data-ttu-id="8f10b-179">A hello **SAML SSO konfigurációs** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-179">In hello **SAML SSO Configuration** Click **Edit**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. <span data-ttu-id="8f10b-181">Ebben a szakaszban hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="8f10b-181">In this Section perform following actions:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    <span data-ttu-id="8f10b-183">a.</span><span class="sxs-lookup"><span data-stu-id="8f10b-183">a.</span></span> <span data-ttu-id="8f10b-184">Hello hello tartalmának másolása **metaadatainak XML-kódja letöltött** és illessze be hello **identitáskonfigurációs** mező.</span><span class="sxs-lookup"><span data-stu-id="8f10b-184">Copy hello content of hello **Downloaded Metadata XML** and paste it into hello **identity configuration** field.</span></span>

    > [!NOTE]
    ><span data-ttu-id="8f10b-185">toocopy hello tartalma letöltött **metaadatainak XML-kódja** használata **Jegyzettömb ++** vagy megfelelő **XML-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-185">toocopy hello content of downloaded **Metadata XML** Use **Notepad++** or proper **XML Editor**.</span></span> 

    <span data-ttu-id="8f10b-186">b.</span><span class="sxs-lookup"><span data-stu-id="8f10b-186">b.</span></span> <span data-ttu-id="8f10b-187">Kattintson a **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-187">Click **Validate**.</span></span>

    <span data-ttu-id="8f10b-188">c.</span><span class="sxs-lookup"><span data-stu-id="8f10b-188">c.</span></span> <span data-ttu-id="8f10b-189">Miután kattintva **ellenőrzése**, tükrözés **SAML(Enabled)** tooon.</span><span class="sxs-lookup"><span data-stu-id="8f10b-189">After Clicking **Validate**, Flip **SAML(Enabled)** tooon.</span></span>

    <span data-ttu-id="8f10b-190">d.</span><span class="sxs-lookup"><span data-stu-id="8f10b-190">d.</span></span> <span data-ttu-id="8f10b-191">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-191">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="8f10b-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8f10b-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8f10b-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8f10b-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8f10b-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8f10b-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8f10b-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="8f10b-195">Create an Azure AD test user</span></span>
<span data-ttu-id="8f10b-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8f10b-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8f10b-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8f10b-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f10b-199">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f10b-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8f10b-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8f10b-203">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f10b-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8f10b-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8f10b-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8f10b-207">a.</span><span class="sxs-lookup"><span data-stu-id="8f10b-207">a.</span></span> <span data-ttu-id="8f10b-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8f10b-209">b.</span><span class="sxs-lookup"><span data-stu-id="8f10b-209">b.</span></span> <span data-ttu-id="8f10b-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8f10b-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8f10b-211">c.</span><span class="sxs-lookup"><span data-stu-id="8f10b-211">c.</span></span> <span data-ttu-id="8f10b-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8f10b-213">d.</span><span class="sxs-lookup"><span data-stu-id="8f10b-213">d.</span></span> <span data-ttu-id="8f10b-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f10b-214">Click **Create**.</span></span>
 
### <a name="create-a-rfpio-test-user"></a><span data-ttu-id="8f10b-215">RFPIO tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f10b-215">Create a RFPIO test user</span></span>

<span data-ttu-id="8f10b-216">az Azure AD tooenable felhasználók toolog a tooRFPIO, akkor ki kell építenie RFPIO be.</span><span class="sxs-lookup"><span data-stu-id="8f10b-216">tooenable Azure AD users toolog in tooRFPIO, they must be provisioned into RFPIO.</span></span>  
<span data-ttu-id="8f10b-217">RFPIO hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8f10b-217">In hello case of RFPIO, provisioning is a manual task.</span></span>

<span data-ttu-id="8f10b-218">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8f10b-218">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f10b-219">Jelentkezzen be tooyour RFPIO vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8f10b-219">Log in tooyour RFPIO company site as an administrator.</span></span>

2. <span data-ttu-id="8f10b-220">Kattintson a hello alsó bal sarok legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="8f10b-220">Click on hello bottom left corner dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. <span data-ttu-id="8f10b-222">Kattintson a hello **szervezeti beállítások**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-222">Click on hello **Organization Settings**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. <span data-ttu-id="8f10b-224">Kattintson a **CSAPATTAGOK**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-224">Click **TEAM MEMBERS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. <span data-ttu-id="8f10b-226">Kattintson a **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-226">Click on **ADD MEMBERS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. <span data-ttu-id="8f10b-228">A hello **új tagok hozzáadása** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8f10b-228">In hello **Add New Members** section.</span></span> <span data-ttu-id="8f10b-229">Hajtsa végre az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="8f10b-229">Perform following actions:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app8.png)

    <span data-ttu-id="8f10b-231">a.</span><span class="sxs-lookup"><span data-stu-id="8f10b-231">a.</span></span> <span data-ttu-id="8f10b-232">Adjon meg **E-mail cím** a hello **adjon meg soronként egy e-mail** mező.</span><span class="sxs-lookup"><span data-stu-id="8f10b-232">Enter **Email address** in hello **Enter one email per line** field.</span></span>

    <span data-ttu-id="8f10b-233">b.</span><span class="sxs-lookup"><span data-stu-id="8f10b-233">b.</span></span> <span data-ttu-id="8f10b-234">Adja válassza **szerepkör** igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="8f10b-234">Plese select **Role** according your requirements.</span></span>

    <span data-ttu-id="8f10b-235">c.</span><span class="sxs-lookup"><span data-stu-id="8f10b-235">c.</span></span> <span data-ttu-id="8f10b-236">Kattintson a **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-236">Click **ADD MEMBERS**.</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="8f10b-237">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="8f10b-237">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="8f10b-238">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="8f10b-238">Assign hello Azure AD test user</span></span>

<span data-ttu-id="8f10b-239">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRFPIO megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8f10b-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRFPIO.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8f10b-241">**tooassign Britta Simon tooRFPIO, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8f10b-241">**tooassign Britta Simon tooRFPIO, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f10b-242">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8f10b-244">Hello alkalmazások listában válassza ki a **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-244">In hello applications list, select **RFPIO**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. <span data-ttu-id="8f10b-246">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8f10b-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8f10b-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f10b-248">Click **Add** button.</span></span> <span data-ttu-id="8f10b-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f10b-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8f10b-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8f10b-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8f10b-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f10b-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8f10b-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f10b-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8f10b-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8f10b-254">Test single sign-on</span></span>

<span data-ttu-id="8f10b-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8f10b-255">In this section, you test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="8f10b-256">Hello RFPIO hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour RFPIO alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8f10b-256">When you click hello RFPIO tile in hello Access Panel, you should get automatically signed-on tooyour RFPIO application.</span></span>
<span data-ttu-id="8f10b-257">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8f10b-257">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8f10b-258">További források</span><span class="sxs-lookup"><span data-stu-id="8f10b-258">Additional resources</span></span>

* [<span data-ttu-id="8f10b-259">Kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8f10b-259">List of tutorials about how toointegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f10b-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8f10b-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

