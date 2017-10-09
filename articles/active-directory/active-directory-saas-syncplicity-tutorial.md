---
title: "Oktatóanyag: Azure Active Directoryval integrált Syncplicity |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Syncplicity között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6148112a959232ed24d76d1c7b8773f06568fee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a><span data-ttu-id="81417-103">Oktatóanyag: Azure Active Directoryval integrált Syncplicity</span><span class="sxs-lookup"><span data-stu-id="81417-103">Tutorial: Azure Active Directory integration with Syncplicity</span></span>

<span data-ttu-id="81417-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Syncplicity az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="81417-104">In this tutorial, you learn how toointegrate Syncplicity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="81417-105">Syncplicity integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="81417-105">Integrating Syncplicity with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="81417-106">Megadhatja a hozzáférés tooSyncplicity rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="81417-106">You can control in Azure AD who has access tooSyncplicity</span></span>
- <span data-ttu-id="81417-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSyncplicity (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="81417-107">You can enable your users tooautomatically get signed-on tooSyncplicity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="81417-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="81417-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="81417-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="81417-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81417-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81417-110">Prerequisites</span></span>

<span data-ttu-id="81417-111">az Azure AD integrálása Syncplicity tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="81417-111">tooconfigure Azure AD integration with Syncplicity, you need hello following items:</span></span>

- <span data-ttu-id="81417-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="81417-112">An Azure AD subscription</span></span>
- <span data-ttu-id="81417-113">Egy Syncplicity egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="81417-113">A Syncplicity single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="81417-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="81417-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="81417-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="81417-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="81417-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="81417-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="81417-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="81417-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="81417-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="81417-118">Scenario description</span></span>
<span data-ttu-id="81417-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81417-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="81417-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="81417-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="81417-121">Hello gyűjteményből Syncplicity hozzáadása</span><span class="sxs-lookup"><span data-stu-id="81417-121">Adding Syncplicity from hello gallery</span></span>
2. <span data-ttu-id="81417-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="81417-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-syncplicity-from-hello-gallery"></a><span data-ttu-id="81417-123">Hello gyűjteményből Syncplicity hozzáadása</span><span class="sxs-lookup"><span data-stu-id="81417-123">Adding Syncplicity from hello gallery</span></span>
<span data-ttu-id="81417-124">tooconfigure hello integrációja Syncplicity az Azure AD-be, meg kell tooadd Syncplicity hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="81417-124">tooconfigure hello integration of Syncplicity into Azure AD, you need tooadd Syncplicity from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="81417-125">**tooadd Syncplicity hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="81417-125">**tooadd Syncplicity from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="81417-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="81417-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="81417-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="81417-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="81417-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="81417-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="81417-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="81417-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="81417-133">Hello keresési mezőbe, írja be a **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="81417-133">In hello search box, type **Syncplicity**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. <span data-ttu-id="81417-135">A hello eredmények panelen válassza ki a **Syncplicity**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="81417-135">In hello results panel, select **Syncplicity**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="81417-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="81417-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="81417-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Syncplicity</span><span class="sxs-lookup"><span data-stu-id="81417-138">In this section, you configure and test Azure AD single sign-on with Syncplicity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="81417-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Syncplicity tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="81417-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Syncplicity is tooa user in Azure AD.</span></span> <span data-ttu-id="81417-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Syncplicity közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="81417-140">In other words, a link relationship between an Azure AD user and hello related user in Syncplicity needs toobe established.</span></span>

<span data-ttu-id="81417-141">Syncplicity, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="81417-141">In Syncplicity, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="81417-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Syncplicity-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="81417-142">tooconfigure and test Azure AD single sign-on with Syncplicity, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="81417-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="81417-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="81417-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81417-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="81417-145">**[Syncplicity tesztfelhasználó létrehozása](#creating-a-syncplicity-test-user)**  -toohave egy megfelelője a Britta Simon a Syncplicity, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="81417-145">**[Creating a Syncplicity test user](#creating-a-syncplicity-test-user)** - toohave a counterpart of Britta Simon in Syncplicity that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="81417-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="81417-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="81417-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="81417-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="81417-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81417-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="81417-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Syncplicity alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="81417-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Syncplicity application.</span></span>

<span data-ttu-id="81417-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Syncplicity, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="81417-150">**tooconfigure Azure AD single sign-on with Syncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="81417-151">Az Azure portál, a hello hello **Syncplicity** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="81417-151">In hello Azure portal, on hello **Syncplicity** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="81417-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="81417-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. <span data-ttu-id="81417-155">A hello **Syncplicity tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="81417-155">On hello **Syncplicity Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    <span data-ttu-id="81417-157">a.</span><span class="sxs-lookup"><span data-stu-id="81417-157">a.</span></span> <span data-ttu-id="81417-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.syncplicity.com`</span><span class="sxs-lookup"><span data-stu-id="81417-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.syncplicity.com`</span></span>

    <span data-ttu-id="81417-159">b.</span><span class="sxs-lookup"><span data-stu-id="81417-159">b.</span></span> <span data-ttu-id="81417-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.syncplicity.com/sp`</span><span class="sxs-lookup"><span data-stu-id="81417-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.syncplicity.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="81417-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="81417-161">These values are not real.</span></span> <span data-ttu-id="81417-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="81417-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="81417-163">Ügyfél [Syncplicity ügyfél-támogatási csoport](https://www.syncplicity.com/contact-us) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="81417-163">Contact [Syncplicity Client support team](https://www.syncplicity.com/contact-us) tooget these values.</span></span> 
 

4. <span data-ttu-id="81417-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="81417-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. <span data-ttu-id="81417-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="81417-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="81417-168">A hello **Syncplicity konfigurációs** kattintson **konfigurálása Syncplicity** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="81417-168">On hello **Syncplicity Configuration** section, click **Configure Syncplicity** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="81417-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="81417-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. <span data-ttu-id="81417-171">Jelentkezzen be tooyour **Syncplicity** bérlő.</span><span class="sxs-lookup"><span data-stu-id="81417-171">Sign in tooyour **Syncplicity** tenant.</span></span>

8. <span data-ttu-id="81417-172">Hello hello felső menüben kattintson a **admin**, jelölje be **beállítások**, és kattintson a **egyéni tartomány és az egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="81417-172">In hello menu on hello top, click **admin**, select **settings**, and then click **Custom domain and single sign-on**.</span></span>
   
    <span data-ttu-id="81417-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span><span class="sxs-lookup"><span data-stu-id="81417-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span></span>

9. <span data-ttu-id="81417-174">A hello **egyszeri bejelentkezés (SSO)** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="81417-174">On hello **Single Sign-On (SSO)** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="81417-175">![Egyszeri bejelentkezés \(egyszeri bejelentkezés\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span><span class="sxs-lookup"><span data-stu-id="81417-175">![Single Sign-On \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span></span>   

    <span data-ttu-id="81417-176">a.</span><span class="sxs-lookup"><span data-stu-id="81417-176">a.</span></span> <span data-ttu-id="81417-177">A hello **egyéni tartomány** szövegmezőhöz típus hello a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="81417-177">In hello **Custom Domain** textbox, type hello name of your domain.</span></span>
  
    <span data-ttu-id="81417-178">b.</span><span class="sxs-lookup"><span data-stu-id="81417-178">b.</span></span> <span data-ttu-id="81417-179">Válassza ki **engedélyezett** , **az egyszeri bejelentkezés állapot**.</span><span class="sxs-lookup"><span data-stu-id="81417-179">Select **Enabled** as **Single Sign-On Status**.</span></span>

    <span data-ttu-id="81417-180">c.</span><span class="sxs-lookup"><span data-stu-id="81417-180">c.</span></span> <span data-ttu-id="81417-181">A hello **entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="81417-181">In hello **Entity Id** textbox, Paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="81417-182">d.</span><span class="sxs-lookup"><span data-stu-id="81417-182">d.</span></span> <span data-ttu-id="81417-183">A hello **bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="81417-183">In hello **Sign-in page URL** textbox, Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="81417-184">e.</span><span class="sxs-lookup"><span data-stu-id="81417-184">e.</span></span> <span data-ttu-id="81417-185">A hello **kijelentkezési URL-címe** szövegmezőhöz Beillesztés hello **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="81417-185">In hello **Logout page URL** textbox, Paste hello **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="81417-186">f.</span><span class="sxs-lookup"><span data-stu-id="81417-186">f.</span></span> <span data-ttu-id="81417-187">A **szolgáltató Identitástanúsítvány**, kattintson a **fájl kiválasztása**, majd töltse fel a hello Azure-portálon a letöltött hello tartalmazó tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="81417-187">In **Identity Provider Certificate**, click **Choose file**, and then upload hello certificate which you have downloaded from hello Azure portal.</span></span> 

    <span data-ttu-id="81417-188">g.</span><span class="sxs-lookup"><span data-stu-id="81417-188">g.</span></span> <span data-ttu-id="81417-189">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="81417-189">Click **SAVE CHANGES**.</span></span>

> [!TIP]
> <span data-ttu-id="81417-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="81417-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="81417-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="81417-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="81417-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="81417-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="81417-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81417-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="81417-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="81417-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="81417-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="81417-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="81417-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="81417-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="81417-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="81417-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="81417-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81417-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="81417-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="81417-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="81417-205">a.</span><span class="sxs-lookup"><span data-stu-id="81417-205">a.</span></span> <span data-ttu-id="81417-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="81417-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="81417-207">b.</span><span class="sxs-lookup"><span data-stu-id="81417-207">b.</span></span> <span data-ttu-id="81417-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="81417-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="81417-209">c.</span><span class="sxs-lookup"><span data-stu-id="81417-209">c.</span></span> <span data-ttu-id="81417-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="81417-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="81417-211">d.</span><span class="sxs-lookup"><span data-stu-id="81417-211">d.</span></span> <span data-ttu-id="81417-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="81417-212">Click **Create**.</span></span>
 
### <a name="creating-a-syncplicity-test-user"></a><span data-ttu-id="81417-213">Syncplicity tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81417-213">Creating a Syncplicity test user</span></span>
<span data-ttu-id="81417-214">Az aad-ben felhasználók toobe képes toosign a kiosztott tooSyncplicity alkalmazás kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="81417-214">For AAD users toobe able toosign in, they must be provisioned tooSyncplicity application.</span></span> <span data-ttu-id="81417-215">Ez a szakasz ismerteti, hogyan Syncplicity toocreate AAD felhasználói fiókjait.</span><span class="sxs-lookup"><span data-stu-id="81417-215">This section describes how toocreate AAD user accounts in Syncplicity.</span></span>

<span data-ttu-id="81417-216">**egy felhasználói fiók tooSyncplicity tooprovision hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="81417-216">**tooprovision a user account tooSyncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="81417-217">Jelentkezzen be tooyour **Syncplicity** bérlő (például: `https://company.Syncplicity.com`).</span><span class="sxs-lookup"><span data-stu-id="81417-217">Log in tooyour **Syncplicity** tenant (for example: `https://company.Syncplicity.com`).</span></span>

2. <span data-ttu-id="81417-218">Kattintson a **admin** válassza **felhasználói fiókok**.</span><span class="sxs-lookup"><span data-stu-id="81417-218">Click **admin** and select **user accounts**.</span></span>

3. <span data-ttu-id="81417-219">Kattintson a **hozzáadni egy felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="81417-219">Click **ADD A USER**.</span></span>
   
    <span data-ttu-id="81417-220">![Felhasználók kezelése](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="81417-220">![Manage Users](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Manage Users")</span></span>

4. <span data-ttu-id="81417-221">Típus hello **E-mail címről** egy AAD-fiókba szeretne tooprovision, jelölje be a **felhasználói** , **szerepkör**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="81417-221">Type hello **Email addressess** of an AAD account you want tooprovision, select **User** as **Role**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="81417-222">![Fiókadatok](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "fiókadatok")</span><span class="sxs-lookup"><span data-stu-id="81417-222">![Account Information](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "Account Information")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="81417-223">hello AAD fióktulajdonos kap egy e-mailt, beleértve a hivatkozás tooconfirm, és aktiválja hello fiókot.</span><span class="sxs-lookup"><span data-stu-id="81417-223">hello AAD account holder  gets an email including a link tooconfirm and activate hello account.</span></span> 
    > 

5. <span data-ttu-id="81417-224">Válasszon ki egy csoportot, amely az új felhasználót kell tagjává válik, és kattintson a vállalat **következő**.</span><span class="sxs-lookup"><span data-stu-id="81417-224">Select a group in your company that your new user should become a member of, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="81417-225">![Csoporttagságát](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "csoporttagságát")</span><span class="sxs-lookup"><span data-stu-id="81417-225">![Group Membership](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "Group Membership")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="81417-226">Ha nincsenek felsorolva csoportok, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="81417-226">If there are no groups listed, click **NEXT**.</span></span> 
    > 

6. <span data-ttu-id="81417-227">Válassza ki a hello mappák kívánja tooplace Syncplicity tartozó vezérlőelem hello felhasználó alatt, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="81417-227">Select hello folders you would like tooplace under Syncplicity’s control on hello user’s computer, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="81417-228">![Syncplicity mappák](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappák")</span><span class="sxs-lookup"><span data-stu-id="81417-228">![Syncplicity Folders](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity Folders")</span></span>

>[!NOTE]
><span data-ttu-id="81417-229">Bármely más Syncplicity felhasználói fiók létrehozása eszközök vagy Syncplicity tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="81417-229">You can use any other Syncplicity user account creation tools or APIs provided by Syncplicity tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="81417-230">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="81417-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="81417-231">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSyncplicity megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="81417-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSyncplicity.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="81417-233">**tooassign Britta Simon tooSyncplicity, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="81417-233">**tooassign Britta Simon tooSyncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="81417-234">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="81417-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="81417-236">Hello alkalmazások listában válassza ki a **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="81417-236">In hello applications list, select **Syncplicity**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. <span data-ttu-id="81417-238">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="81417-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="81417-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="81417-240">Click **Add** button.</span></span> <span data-ttu-id="81417-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81417-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="81417-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="81417-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="81417-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81417-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="81417-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81417-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="81417-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="81417-246">Testing single sign-on</span></span>

<span data-ttu-id="81417-247">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="81417-247">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="81417-248">Ha a hozzáférési Panel hello hello Syncplicity csempe gombra kattint, automatikusan bejelentkezett tooyour Syncplicity alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="81417-248">When you click hello Syncplicity tile in hello Access Panel, you should get automatically signed-on tooyour Syncplicity application.</span></span>
## <a name="additional-resources"></a><span data-ttu-id="81417-249">További források</span><span class="sxs-lookup"><span data-stu-id="81417-249">Additional resources</span></span>

* [<span data-ttu-id="81417-250">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="81417-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="81417-251">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="81417-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

