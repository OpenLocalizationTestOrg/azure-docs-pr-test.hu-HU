---
title: "Oktatóanyag: Azure Active Directoryval integrált Workstars |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Workstars között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 86250d7538f058d2a821ded7dda0b2fc185d80df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a><span data-ttu-id="e4bc4-103">Oktatóanyag: Azure Active Directoryval integrált Workstars</span><span class="sxs-lookup"><span data-stu-id="e4bc4-103">Tutorial: Azure Active Directory integration with Workstars</span></span>

<span data-ttu-id="e4bc4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Workstars az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e4bc4-104">In this tutorial, you learn how toointegrate Workstars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e4bc4-105">Workstars integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-105">Integrating Workstars with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e4bc4-106">Az Azure AD hozzáférési tooWorkstars rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-106">You can control in Azure AD who has access tooWorkstars.</span></span>
- <span data-ttu-id="e4bc4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkstars (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-107">You can enable your users tooautomatically get signed-on tooWorkstars (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e4bc4-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="e4bc4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e4bc4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4bc4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e4bc4-110">Prerequisites</span></span>

<span data-ttu-id="e4bc4-111">az Azure AD integrálása Workstars tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-111">tooconfigure Azure AD integration with Workstars, you need hello following items:</span></span>

- <span data-ttu-id="e4bc4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e4bc4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e4bc4-113">Egy Workstars egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e4bc4-113">A Workstars single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e4bc4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e4bc4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e4bc4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e4bc4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4bc4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e4bc4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e4bc4-118">Scenario description</span></span>
<span data-ttu-id="e4bc4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e4bc4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e4bc4-121">Hello gyűjteményből Workstars hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e4bc4-121">Adding Workstars from hello gallery</span></span>
2. <span data-ttu-id="e4bc4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e4bc4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workstars-from-hello-gallery"></a><span data-ttu-id="e4bc4-123">Hello gyűjteményből Workstars hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e4bc4-123">Adding Workstars from hello gallery</span></span>
<span data-ttu-id="e4bc4-124">tooconfigure hello integrációja Workstars az Azure AD-be, meg kell tooadd Workstars hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-124">tooconfigure hello integration of Workstars into Azure AD, you need tooadd Workstars from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e4bc4-125">**tooadd Workstars hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e4bc4-125">**tooadd Workstars from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4bc4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="e4bc4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e4bc4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="e4bc4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="e4bc4-133">Hello keresési mezőbe, írja be a **Workstars**, jelölje be **Workstars** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-133">In hello search box, type **Workstars**, select **Workstars** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e4bc4-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e4bc4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e4bc4-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Workstars.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-136">In this section, you configure and test Azure AD single sign-on with Workstars based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e4bc4-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Workstars tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workstars is tooa user in Azure AD.</span></span> <span data-ttu-id="e4bc4-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Workstars közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-138">In other words, a link relationship between an Azure AD user and hello related user in Workstars needs toobe established.</span></span>

<span data-ttu-id="e4bc4-139">Workstars, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-139">In Workstars, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e4bc4-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Workstars-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-140">tooconfigure and test Azure AD single sign-on with Workstars, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e4bc4-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e4bc4-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e4bc4-143">**[Workstars tesztfelhasználó létrehozása](#create-a-workstars-test-user)**  -toohave egy megfelelője a Britta Simon a Workstars, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-143">**[Create a Workstars test user](#create-a-workstars-test-user)** - toohave a counterpart of Britta Simon in Workstars that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e4bc4-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e4bc4-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e4bc4-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e4bc4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e4bc4-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Workstars alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workstars application.</span></span>

<span data-ttu-id="e4bc4-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Workstars, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e4bc4-148">**tooconfigure Azure AD single sign-on with Workstars, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4bc4-149">Az Azure portál, a hello hello **Workstars** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-149">In hello Azure portal, on hello **Workstars** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="e4bc4-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_samlbase.png)

3. <span data-ttu-id="e4bc4-153">A hello **Workstars tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-153">On hello **Workstars Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Workstars tartomány és az URL-címek](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_url.png)

    <span data-ttu-id="e4bc4-155">a.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-155">a.</span></span> <span data-ttu-id="e4bc4-156">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://workstars.com`</span><span class="sxs-lookup"><span data-stu-id="e4bc4-156">In hello **Identifier** textbox, type hello URL: `https://workstars.com`</span></span>

    <span data-ttu-id="e4bc4-157">b.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-157">b.</span></span> <span data-ttu-id="e4bc4-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.workstars.com/saml/login_check`</span><span class="sxs-lookup"><span data-stu-id="e4bc4-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.workstars.com/saml/login_check`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e4bc4-159">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-159">hello value is not real.</span></span> <span data-ttu-id="e4bc4-160">Frissítés hello értékének hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-160">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="e4bc4-161">Ügyfél [Workstars támogatási csoport](https://support.workstars.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-161">Contact [Workstars support team](https://support.workstars.com) tooget hello value.</span></span>
 
4. <span data-ttu-id="e4bc4-162">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_certificate.png) 

5. <span data-ttu-id="e4bc4-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-workstars-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e4bc4-166">A hello **Workstars konfigurációs** kattintson **konfigurálása Workstars** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-166">On hello **Workstars Configuration** section, click **Configure Workstars** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e4bc4-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e4bc4-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Workstars konfiguráció](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_configure.png) 

7. <span data-ttu-id="e4bc4-169">Egy másik böngészőablakban bejelentkezéskor tooyour Workstars vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-169">In another browser window, sign on tooyour Workstars company site as an administrator.</span></span>

8. <span data-ttu-id="e4bc4-170">Hello eszköztárán kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-170">In hello main toolbar, click **Settings**.</span></span>

    ![Workstars Dokumentumb](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_sett.png)

9. <span data-ttu-id="e4bc4-172">Nyissa meg túl**bejelentkezés** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-172">Go too**Sign On** > **Settings**.</span></span>

    ![Workstars bejelentkezés](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_signon.png)

    ![Workstars beállítások](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_settings.png)

10. <span data-ttu-id="e4bc4-175">A hello **egyszeri bejelentkezési a (SAML) - beállítások** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-175">On hello **Single Sign On (SAML) - Settings** page, perform hello following steps:</span></span>
    
    ![Workstars saml](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_saml.png)

    <span data-ttu-id="e4bc4-177">a.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-177">a.</span></span> <span data-ttu-id="e4bc4-178">A **identitás szolgáltatónevet** szövegmezőhöz típus **Office 365**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-178">In **Identity Provider Name** textbox, type **Office 365**.</span></span>

    <span data-ttu-id="e4bc4-179">b.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-179">b.</span></span> <span data-ttu-id="e4bc4-180">A hello **Identity Provider Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-180">In hello **Identity Provider Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e4bc4-181">c.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-181">c.</span></span> <span data-ttu-id="e4bc4-182">Másolja a letöltött tanúsítványfájl hello hello tartalmat a Jegyzettömbben, és illessze be hello **x509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-182">Copy hello content of hello downloaded certificate file in notepad, and then paste it into hello **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="e4bc4-183">d.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-183">d.</span></span> <span data-ttu-id="e4bc4-184">A hello **SAML SSO URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-184">In hello **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="e4bc4-185">e.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-185">e.</span></span> <span data-ttu-id="e4bc4-186">A hello **távoli kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-186">In hello **Remote Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="e4bc4-187">f.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-187">f.</span></span> <span data-ttu-id="e4bc4-188">Válassza ki **Névazonosító** , **E-mail (alapértelmezett)**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-188">select **Name ID** as **Email (Default)**.</span></span>

    <span data-ttu-id="e4bc4-189">g.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-189">g.</span></span> <span data-ttu-id="e4bc4-190">Kattintson a **megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-190">Click **Confirm**.</span></span>
    
> [!TIP]
> <span data-ttu-id="e4bc4-191">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e4bc4-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e4bc4-192">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e4bc4-193">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e4bc4-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e4bc4-194">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e4bc4-194">Create an Azure AD test user</span></span>

<span data-ttu-id="e4bc4-195">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="e4bc4-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e4bc4-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4bc4-198">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-198">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-workstars-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e4bc4-200">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-200">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-workstars-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e4bc4-202">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-202">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-workstars-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e4bc4-204">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e4bc4-204">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-workstars-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e4bc4-206">a.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-206">a.</span></span> <span data-ttu-id="e4bc4-207">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-207">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e4bc4-208">b.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-208">b.</span></span> <span data-ttu-id="e4bc4-209">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-209">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="e4bc4-210">c.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-210">c.</span></span> <span data-ttu-id="e4bc4-211">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-211">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="e4bc4-212">d.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-212">d.</span></span> <span data-ttu-id="e4bc4-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-213">Click **Create**.</span></span>
  
### <a name="create-a-workstars-test-user"></a><span data-ttu-id="e4bc4-214">Workstars tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4bc4-214">Create a Workstars test user</span></span>

<span data-ttu-id="e4bc4-215">Ebben a szakaszban egy Workstars Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-215">In this section, you create a user called Britta Simon in Workstars.</span></span> <span data-ttu-id="e4bc4-216">Együttműködve [Workstars támogatási csoport](https://support.workstars.com) tooadd hello felhasználók hello Workstars platform.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-216">Work with [Workstars support team](https://support.workstars.com) tooadd hello users in hello Workstars platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e4bc4-217">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e4bc4-217">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e4bc4-218">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWorkstars megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-218">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkstars.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="e4bc4-220">**tooassign Britta Simon tooWorkstars, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e4bc4-220">**tooassign Britta Simon tooWorkstars, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4bc4-221">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-221">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e4bc4-223">Hello alkalmazások listában válassza ki a **Workstars**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-223">In hello applications list, select **Workstars**.</span></span>

    ![hello Workstars hivatkozásra hello alkalmazások listája](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_app.png)  

3. <span data-ttu-id="e4bc4-225">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-225">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="e4bc4-227">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-227">Click **Add** button.</span></span> <span data-ttu-id="e4bc4-228">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="e4bc4-230">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-230">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e4bc4-231">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e4bc4-232">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e4bc4-233">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e4bc4-233">Test single sign-on</span></span>

<span data-ttu-id="e4bc4-234">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-234">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e4bc4-235">Hello Workstars hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Workstars alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e4bc4-235">When you click hello Workstars tile in hello Access Panel, you should get automatically signed-on tooyour Workstars application.</span></span>
<span data-ttu-id="e4bc4-236">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e4bc4-236">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e4bc4-237">További források</span><span class="sxs-lookup"><span data-stu-id="e4bc4-237">Additional resources</span></span>

* [<span data-ttu-id="e4bc4-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e4bc4-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e4bc4-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e4bc4-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_203.png

