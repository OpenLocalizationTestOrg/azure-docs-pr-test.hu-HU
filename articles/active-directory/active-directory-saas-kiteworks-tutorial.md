---
title: "Oktatóanyag: Azure Active Directoryval integrált Kiteworks |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Kiteworks között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7984aaf-ab1f-4a85-9646-a9523f5275d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 406417dd7f58cc3f1fa0d9e86b5cad0c1d7be750
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a><span data-ttu-id="045a8-103">Oktatóanyag: Azure Active Directoryval integrált Kiteworks</span><span class="sxs-lookup"><span data-stu-id="045a8-103">Tutorial: Azure Active Directory integration with Kiteworks</span></span>

<span data-ttu-id="045a8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kiteworks az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="045a8-104">In this tutorial, you learn how toointegrate Kiteworks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="045a8-105">Kiteworks integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="045a8-105">Integrating Kiteworks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="045a8-106">Megadhatja a hozzáférés tooKiteworks rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="045a8-106">You can control in Azure AD who has access tooKiteworks</span></span>
- <span data-ttu-id="045a8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKiteworks (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="045a8-107">You can enable your users tooautomatically get signed-on tooKiteworks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="045a8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="045a8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="045a8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="045a8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="045a8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="045a8-110">Prerequisites</span></span>

<span data-ttu-id="045a8-111">az Azure AD integrálása Kiteworks tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="045a8-111">tooconfigure Azure AD integration with Kiteworks, you need hello following items:</span></span>

- <span data-ttu-id="045a8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="045a8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="045a8-113">Egy Kiteworks egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="045a8-113">A Kiteworks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="045a8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="045a8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="045a8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="045a8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="045a8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="045a8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="045a8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="045a8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="045a8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="045a8-118">Scenario description</span></span>
<span data-ttu-id="045a8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="045a8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="045a8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="045a8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="045a8-121">Hello gyűjteményből Kiteworks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="045a8-121">Adding Kiteworks from hello gallery</span></span>
2. <span data-ttu-id="045a8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="045a8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kiteworks-from-hello-gallery"></a><span data-ttu-id="045a8-123">Hello gyűjteményből Kiteworks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="045a8-123">Adding Kiteworks from hello gallery</span></span>
<span data-ttu-id="045a8-124">tooconfigure hello integrációja Kiteworks az Azure AD-be, meg kell tooadd Kiteworks hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="045a8-124">tooconfigure hello integration of Kiteworks into Azure AD, you need tooadd Kiteworks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="045a8-125">**tooadd Kiteworks hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="045a8-125">**tooadd Kiteworks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="045a8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="045a8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="045a8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="045a8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="045a8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="045a8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="045a8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="045a8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="045a8-133">Hello keresési mezőbe, írja be a **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="045a8-133">In hello search box, type **Kiteworks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_search.png)

5. <span data-ttu-id="045a8-135">A hello eredmények panelen válassza ki a **Kiteworks**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="045a8-135">In hello results panel, select **Kiteworks**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="045a8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="045a8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="045a8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="045a8-138">In this section, you configure and test Azure AD single sign-on with Kiteworks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="045a8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kiteworks tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="045a8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kiteworks is tooa user in Azure AD.</span></span> <span data-ttu-id="045a8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Kiteworks közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="045a8-140">In other words, a link relationship between an Azure AD user and hello related user in Kiteworks needs toobe established.</span></span>

<span data-ttu-id="045a8-141">Kiteworks, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="045a8-141">In Kiteworks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="045a8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Kiteworks-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="045a8-142">tooconfigure and test Azure AD single sign-on with Kiteworks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="045a8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="045a8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="045a8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="045a8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="045a8-145">**[Kiteworks tesztfelhasználó létrehozása](#creating-a-kiteworks-test-user)**  -toohave egy megfelelője a Britta Simon a Kiteworks, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="045a8-145">**[Creating a Kiteworks test user](#creating-a-kiteworks-test-user)** - toohave a counterpart of Britta Simon in Kiteworks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="045a8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="045a8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="045a8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="045a8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="045a8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="045a8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="045a8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Kiteworks alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="045a8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kiteworks application.</span></span>

<span data-ttu-id="045a8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Kiteworks, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="045a8-150">**tooconfigure Azure AD single sign-on with Kiteworks, perform hello following steps:**</span></span>

1. <span data-ttu-id="045a8-151">Az Azure portál, a hello hello **Kiteworks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="045a8-151">In hello Azure portal, on hello **Kiteworks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="045a8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="045a8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_samlbase.png)

3. <span data-ttu-id="045a8-155">A hello **Kiteworks tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="045a8-155">On hello **Kiteworks Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_url.png)

    <span data-ttu-id="045a8-157">a.</span><span class="sxs-lookup"><span data-stu-id="045a8-157">a.</span></span> <span data-ttu-id="045a8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.kiteworks.com`</span><span class="sxs-lookup"><span data-stu-id="045a8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.kiteworks.com`</span></span>

    <span data-ttu-id="045a8-159">b.</span><span class="sxs-lookup"><span data-stu-id="045a8-159">b.</span></span> <span data-ttu-id="045a8-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span><span class="sxs-lookup"><span data-stu-id="045a8-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="045a8-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="045a8-161">These values are not real.</span></span> <span data-ttu-id="045a8-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="045a8-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="045a8-163">Ügyfél [Kiteworks ügyfél-támogatási csoport](http://accellion.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="045a8-163">Contact [Kiteworks Client support team](http://accellion.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="045a8-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="045a8-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_certificate.png) 

5. <span data-ttu-id="045a8-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="045a8-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="045a8-168">A hello **Kiteworks konfigurációs** kattintson **konfigurálása Kiteworks** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="045a8-168">On hello **Kiteworks Configuration** section, click **Configure Kiteworks** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="045a8-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="045a8-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_configure.png) 

7. <span data-ttu-id="045a8-171">Bejelentkezés tooyour Kiteworks vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="045a8-171">Sign on tooyour Kiteworks company site as an administrator.</span></span>

8. <span data-ttu-id="045a8-172">Hello hello felső eszköztárán kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="045a8-172">In hello toolbar on hello top, click **Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 

9. <span data-ttu-id="045a8-174">A hello **hitelesítési és engedélyezési** kattintson **egyszeri bejelentkezés beállítása**.</span><span class="sxs-lookup"><span data-stu-id="045a8-174">In hello **Authentication and Authorization** section, click **SSO Setup**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png)
 
10. <span data-ttu-id="045a8-176">Hello egyszeri bejelentkezés beállítása lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="045a8-176">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png)   

    <span data-ttu-id="045a8-178">a.</span><span class="sxs-lookup"><span data-stu-id="045a8-178">a.</span></span> <span data-ttu-id="045a8-179">Válassza ki **hitelesítés egyszeri Bejelentkezéssel keresztül**.</span><span class="sxs-lookup"><span data-stu-id="045a8-179">Select **Authenticate via SSO**.</span></span>

    <span data-ttu-id="045a8-180">b.</span><span class="sxs-lookup"><span data-stu-id="045a8-180">b.</span></span> <span data-ttu-id="045a8-181">Válassza ki **AuthnRequest kezdeményezése**.</span><span class="sxs-lookup"><span data-stu-id="045a8-181">Select **Initiate AuthnRequest**.</span></span>

    <span data-ttu-id="045a8-182">c.</span><span class="sxs-lookup"><span data-stu-id="045a8-182">c.</span></span> <span data-ttu-id="045a8-183">A hello **IDP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="045a8-183">In hello **IDP Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="045a8-184">d.</span><span class="sxs-lookup"><span data-stu-id="045a8-184">d.</span></span> <span data-ttu-id="045a8-185">A hello **egyszeri bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="045a8-185">In hello **Single Sign-On Service URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="045a8-186">e.</span><span class="sxs-lookup"><span data-stu-id="045a8-186">e.</span></span> <span data-ttu-id="045a8-187">A hello **egyetlen kijelentkezési URL-címe** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="045a8-187">In hello **Single Logout Service URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="045a8-188">f.</span><span class="sxs-lookup"><span data-stu-id="045a8-188">f.</span></span> <span data-ttu-id="045a8-189">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom, és illessze be hello **RSA nyilvános kulcs tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="045a8-189">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **RSA Public Key Certificate** textbox.</span></span>
 
    <span data-ttu-id="045a8-190">g.</span><span class="sxs-lookup"><span data-stu-id="045a8-190">g.</span></span> <span data-ttu-id="045a8-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="045a8-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="045a8-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="045a8-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="045a8-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="045a8-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="045a8-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="045a8-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="045a8-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="045a8-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="045a8-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="045a8-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="045a8-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="045a8-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="045a8-199">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="045a8-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="045a8-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="045a8-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="045a8-203">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="045a8-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="045a8-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="045a8-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="045a8-207">a.</span><span class="sxs-lookup"><span data-stu-id="045a8-207">a.</span></span> <span data-ttu-id="045a8-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="045a8-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="045a8-209">b.</span><span class="sxs-lookup"><span data-stu-id="045a8-209">b.</span></span> <span data-ttu-id="045a8-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="045a8-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="045a8-211">c.</span><span class="sxs-lookup"><span data-stu-id="045a8-211">c.</span></span> <span data-ttu-id="045a8-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="045a8-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="045a8-213">d.</span><span class="sxs-lookup"><span data-stu-id="045a8-213">d.</span></span> <span data-ttu-id="045a8-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="045a8-214">Click **Create**.</span></span>
 
### <a name="creating-a-kiteworks-test-user"></a><span data-ttu-id="045a8-215">Kiteworks tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="045a8-215">Creating a Kiteworks test user</span></span>

<span data-ttu-id="045a8-216">hello ebben a szakaszban célja toocreate Kiteworks Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="045a8-216">hello objective of this section is toocreate a user called Britta Simon in Kiteworks.</span></span>

<span data-ttu-id="045a8-217">Kiteworks támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="045a8-217">Kiteworks supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="045a8-218">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="045a8-218">There is no action item for you in this section.</span></span> <span data-ttu-id="045a8-219">Új felhasználó jön létre egy kísérlet tooaccess Kitewors során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="045a8-219">A new user is created during an attempt tooaccess Kitewors if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="045a8-220">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [Kiteworks támogatási csoport](http://accellion.com/support).</span><span class="sxs-lookup"><span data-stu-id="045a8-220">If you need toocreate a user manually, you need toocontact hello [Kiteworks support team](http://accellion.com/support).</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="045a8-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="045a8-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="045a8-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooKiteworks megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="045a8-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKiteworks.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="045a8-224">**tooassign Britta Simon tooKiteworks, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="045a8-224">**tooassign Britta Simon tooKiteworks, perform hello following steps:**</span></span>

1. <span data-ttu-id="045a8-225">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="045a8-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="045a8-227">Hello alkalmazások listában válassza ki a **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="045a8-227">In hello applications list, select **Kiteworks**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_app.png) 

3. <span data-ttu-id="045a8-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="045a8-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="045a8-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="045a8-231">Click **Add** button.</span></span> <span data-ttu-id="045a8-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="045a8-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="045a8-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="045a8-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="045a8-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="045a8-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="045a8-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="045a8-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="045a8-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="045a8-237">Testing single sign-on</span></span>

<span data-ttu-id="045a8-238">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="045a8-238">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="045a8-239">Hello Kiteworks hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Kiteworks alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="045a8-239">When you click hello Kiteworks tile in hello Access Panel, you should get automatically signed-on tooyour Kiteworks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="045a8-240">További források</span><span class="sxs-lookup"><span data-stu-id="045a8-240">Additional resources</span></span>

* [<span data-ttu-id="045a8-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="045a8-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="045a8-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="045a8-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png

