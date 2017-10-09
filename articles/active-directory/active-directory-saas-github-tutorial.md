---
title: "Oktatóanyag: Azure Active Directoryval integrált GitHub |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a GitHub között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 688779de4e6627e49c0e3e8a7576f2f8c7a81ab1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a><span data-ttu-id="36646-103">Oktatóanyag: Azure Active Directory-integráció a Githubon</span><span class="sxs-lookup"><span data-stu-id="36646-103">Tutorial: Azure Active Directory integration with GitHub</span></span>

<span data-ttu-id="36646-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Githubon az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36646-104">In this tutorial, you learn how toointegrate GitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="36646-105">GitHub integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="36646-105">Integrating GitHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="36646-106">Megadhatja a hozzáférés tooGitHub rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="36646-106">You can control in Azure AD who has access tooGitHub</span></span>
- <span data-ttu-id="36646-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGitHub (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="36646-107">You can enable your users tooautomatically get signed-on tooGitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="36646-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="36646-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="36646-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="36646-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36646-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="36646-110">Prerequisites</span></span>

<span data-ttu-id="36646-111">tooconfigure az Azure AD-integráció a Githubon, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="36646-111">tooconfigure Azure AD integration with GitHub, you need hello following items:</span></span>

- <span data-ttu-id="36646-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="36646-112">An Azure AD subscription</span></span>
- <span data-ttu-id="36646-113">A GitHub egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="36646-113">A GitHub single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="36646-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="36646-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="36646-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="36646-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="36646-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="36646-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="36646-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="36646-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="36646-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="36646-118">Scenario description</span></span>
<span data-ttu-id="36646-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="36646-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="36646-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="36646-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="36646-121">GitHub hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="36646-121">Adding GitHub from hello gallery</span></span>
2. <span data-ttu-id="36646-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36646-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-github-from-hello-gallery"></a><span data-ttu-id="36646-123">GitHub hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="36646-123">Adding GitHub from hello gallery</span></span>
<span data-ttu-id="36646-124">tooconfigure hello integrálása a Githubon az Azure AD, meg kell tooadd GitHub hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="36646-124">tooconfigure hello integration of GitHub into Azure AD, you need tooadd GitHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="36646-125">**tooadd GitHub hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="36646-125">**tooadd GitHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="36646-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="36646-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="36646-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="36646-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="36646-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="36646-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="36646-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="36646-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="36646-133">Hello keresési mezőbe, írja be a **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="36646-133">In hello search box, type **GitHub.com**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. <span data-ttu-id="36646-135">A hello eredmények panelen válassza ki a **GitHub**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="36646-135">In hello results panel, select **GitHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="36646-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36646-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="36646-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján GitHub.</span><span class="sxs-lookup"><span data-stu-id="36646-138">In this section, you configure and test Azure AD single sign-on with GitHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="36646-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Githubon az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="36646-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in GitHub is tooa user in Azure AD.</span></span> <span data-ttu-id="36646-140">Ez azt jelenti hello kapcsolódó felhasználó a Githubon és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="36646-140">In other words, a link relationship between an Azure AD user and hello related user in GitHub needs toobe established.</span></span>

<span data-ttu-id="36646-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Githubon.</span><span class="sxs-lookup"><span data-stu-id="36646-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in GitHub.</span></span>

<span data-ttu-id="36646-142">tooconfigure és az Azure AD egyszeri bejelentkezést a GitHub-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="36646-142">tooconfigure and test Azure AD single sign-on with GitHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="36646-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="36646-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="36646-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36646-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="36646-145">**[GitHub tesztfelhasználó létrehozása](#creating-a-GitHub-test-user)**  -toohave Britta Simon a Githubon, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="36646-145">**[Creating a GitHub test user](#creating-a-GitHub-test-user)** - toohave a counterpart of Britta Simon in GitHub that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="36646-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="36646-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="36646-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="36646-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="36646-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36646-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="36646-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a GitHub-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="36646-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your GitHub application.</span></span>

<span data-ttu-id="36646-150">**az Azure AD tooconfigure egyszeri bejelentkezést a github webhelyen, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="36646-150">**tooconfigure Azure AD single sign-on with GitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="36646-151">Hello Azure felügyeleti portálon, a hello **GitHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="36646-151">In hello Azure Management portal, on hello **GitHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="36646-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="36646-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. <span data-ttu-id="36646-155">A hello **GitHub-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="36646-155">On hello **GitHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    <span data-ttu-id="36646-157">a.</span><span class="sxs-lookup"><span data-stu-id="36646-157">a.</span></span> <span data-ttu-id="36646-158">A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://github.com/orgs/<entity-id>/sso`</span><span class="sxs-lookup"><span data-stu-id="36646-158">In hello **Sign-on URL** textbox, type hello value as: `https://github.com/orgs/<entity-id>/sso`</span></span>

    <span data-ttu-id="36646-159">b.</span><span class="sxs-lookup"><span data-stu-id="36646-159">b.</span></span> <span data-ttu-id="36646-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://github.com/orgs/<entity-id>`</span><span class="sxs-lookup"><span data-stu-id="36646-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://github.com/orgs/<entity-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="36646-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="36646-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="36646-162">Tooupdate rendelkezik ezekkel az értékekkel hello tényleges rang URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="36646-162">You have tooupdate these values with hello actual Sing-on URL and Identifier.</span></span> <span data-ttu-id="36646-163">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="36646-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="36646-164">Nyissa meg ezeket az értékeket tooGitHub Admin szakasz tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="36646-164">Go tooGitHub Admin section tooretrieve these values.</span></span> 

4. <span data-ttu-id="36646-165">A hello **felhasználói attribútumok** szakaszban jelölje be **felhasználói azonosító** user.mail szerint.</span><span class="sxs-lookup"><span data-stu-id="36646-165">On hello **User Attributes** section, select **User Identifier** as user.mail.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. <span data-ttu-id="36646-167">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="36646-167">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. <span data-ttu-id="36646-169">A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="36646-169">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="36646-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="36646-170">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. <span data-ttu-id="36646-172">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="36646-172">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. <span data-ttu-id="36646-174">A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="36646-174">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="36646-176">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="36646-176">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. <span data-ttu-id="36646-178">A hello **GitHub konfigurációs** területén kattintson **konfigurálása GitHub** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="36646-178">On hello **GitHub Configuration** section, click **Configure GitHub** tooopen **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. <span data-ttu-id="36646-181">Egy másik webes böngészőablakban jelentkezzen be a Githubon szervezet webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="36646-181">In a different web browser window, log into your GitHub organization site as an administrator.</span></span>

12. <span data-ttu-id="36646-182">Keresse meg a túl**beállítások** kattintson **biztonsági**</span><span class="sxs-lookup"><span data-stu-id="36646-182">Navigate too**Settings** and click **Security**</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. <span data-ttu-id="36646-184">Ellenőrizze a hello **engedélyezése SAML-alapú hitelesítés** hello egyszeri bejelentkezés konfigurációs mezők felfedése mezőbe.</span><span class="sxs-lookup"><span data-stu-id="36646-184">Check hello **Enable SAML authentication** box, revealing hello Single Sign-on configuration fields.</span></span> <span data-ttu-id="36646-185">Hello egyetlen bejelentkezési URL-cím érték tooupdate hello egyetlen bejelentkezési URL-címet, majd használja az Azure Active Directory beállítása.</span><span class="sxs-lookup"><span data-stu-id="36646-185">Then, use hello single sign-on URL value tooupdate hello Single sign-on URL on Azure AD configuration.</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. <span data-ttu-id="36646-187">A következő mezők hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="36646-187">Configure hello following fields:</span></span>

    <span data-ttu-id="36646-188">a.</span><span class="sxs-lookup"><span data-stu-id="36646-188">a.</span></span> <span data-ttu-id="36646-189">**Jelentkezzen be az URL-cím**: Adja meg **SAML-alapú egyszeri bejelentkezést szolgáltatás URL-címe** hello a **konfigurálása GitHub** szakasz Azure ad-val</span><span class="sxs-lookup"><span data-stu-id="36646-189">**Sign on URL**: Enter **SAML Single sign-on Service URL** from hello **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="36646-190">b.</span><span class="sxs-lookup"><span data-stu-id="36646-190">b.</span></span> <span data-ttu-id="36646-191">**Kibocsátó**: Adjon meg **SAML Entitásazonosító** a hello **konfigurálása GitHub** szakasz Azure ad-val</span><span class="sxs-lookup"><span data-stu-id="36646-191">**Issuer**: Enter **SAML Entity ID** from hello **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="36646-192">c.</span><span class="sxs-lookup"><span data-stu-id="36646-192">c.</span></span> <span data-ttu-id="36646-193">**Nyilvános tanúsítvány**: Nyissa meg hello tanúsítvány le: Azure AD-t a Jegyzettömbben, és másolja hello tartalommal, beleértve a "BEGIN tanúsítvány" és "END CERTIFICATE"</span><span class="sxs-lookup"><span data-stu-id="36646-193">**Public Certificate**: Open hello downloaded certificate from Azure AD in a notepad and copy hello content including "BEGIN CERTIFICATE" and "END CERTIFICATE"</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. <span data-ttu-id="36646-195">Kattintson a **teszt SAML-alapú konfigurációs** tooconfirm, amely nincs az érvényesítési hibák vagy az egyszeri bejelentkezés során fellépett hibák.</span><span class="sxs-lookup"><span data-stu-id="36646-195">Click on **Test SAML configuration** tooconfirm that no validation failures or errors during SSO.</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. <span data-ttu-id="36646-197">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="36646-197">Click **Save**</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="36646-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="36646-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="36646-199">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="36646-199">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="36646-201">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="36646-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="36646-202">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="36646-202">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="36646-204">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="36646-204">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="36646-206">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36646-206">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="36646-208">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="36646-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="36646-210">a.</span><span class="sxs-lookup"><span data-stu-id="36646-210">a.</span></span> <span data-ttu-id="36646-211">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="36646-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="36646-212">b.</span><span class="sxs-lookup"><span data-stu-id="36646-212">b.</span></span> <span data-ttu-id="36646-213">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="36646-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="36646-214">c.</span><span class="sxs-lookup"><span data-stu-id="36646-214">c.</span></span> <span data-ttu-id="36646-215">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="36646-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="36646-216">d.</span><span class="sxs-lookup"><span data-stu-id="36646-216">d.</span></span> <span data-ttu-id="36646-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="36646-217">Click **Create**.</span></span> 


### <a name="creating-a-github-test-user"></a><span data-ttu-id="36646-218">GitHub tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="36646-218">Creating a GitHub test user</span></span>

<span data-ttu-id="36646-219">A sorrend tooenable az Azure AD felhasználók toolog a Githubon azok ki kell építenie a Githubon.</span><span class="sxs-lookup"><span data-stu-id="36646-219">In order tooenable Azure AD users toolog into GitHub, they must be provisioned into GitHub.</span></span>  
<span data-ttu-id="36646-220">GitHub hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="36646-220">In hello case of GitHub, provisioning is a manual task.</span></span>

<span data-ttu-id="36646-221">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36646-221">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="36646-222">Jelentkezzen be tooyour GitHub vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="36646-222">Log in tooyour GitHub company site as an administrator.</span></span>

2. <span data-ttu-id="36646-223">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="36646-223">Click **People**.</span></span>

    <span data-ttu-id="36646-224">![Személyek](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="36646-224">![People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "People")</span></span>

3. <span data-ttu-id="36646-225">Kattintson a **a meghívás tag**.</span><span class="sxs-lookup"><span data-stu-id="36646-225">Click **Invite member**.</span></span>

    <span data-ttu-id="36646-226">![Meghívott felhasználóknak](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "meghívott felhasználóknak")</span><span class="sxs-lookup"><span data-stu-id="36646-226">![Invite Users](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Invite Users")</span></span>

4. <span data-ttu-id="36646-227">A hello **a meghívás tag** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="36646-227">On hello **Invite member** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="36646-228">a.</span><span class="sxs-lookup"><span data-stu-id="36646-228">a.</span></span> <span data-ttu-id="36646-229">A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="36646-229">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="36646-230">![Felkérése](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="36646-230">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Invite People")</span></span>
    
    <span data-ttu-id="36646-231">b.</span><span class="sxs-lookup"><span data-stu-id="36646-231">b.</span></span> <span data-ttu-id="36646-232">Kattintson a **meghívó küldése**.</span><span class="sxs-lookup"><span data-stu-id="36646-232">Click **Send Invitation**.</span></span>

    <span data-ttu-id="36646-233">![Felkérése](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="36646-233">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Invite People")</span></span>

    > [!NOTE]
    > <span data-ttu-id="36646-234">hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="36646-234">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="36646-235">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="36646-235">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="36646-236">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooGitHub megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="36646-236">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooGitHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="36646-238">**tooassign Britta Simon tooGitHub, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="36646-238">**tooassign Britta Simon tooGitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="36646-239">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="36646-239">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="36646-241">Hello alkalmazások listában válassza ki a **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="36646-241">In hello applications list, select **GitHub.com**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. <span data-ttu-id="36646-243">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="36646-243">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="36646-245">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="36646-245">Click **Add** button.</span></span> <span data-ttu-id="36646-246">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36646-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="36646-248">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="36646-248">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="36646-249">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36646-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="36646-250">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36646-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="36646-251">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="36646-251">Testing single sign-on</span></span>

<span data-ttu-id="36646-252">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="36646-252">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="36646-253">Ha hello GitHub csempe a hozzáférési Panel hello gombra kattint, bejelentkezett tooyour GitHub alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="36646-253">When you click hello GitHub tile in hello Access Panel, you should get signed-on tooyour GitHub application.</span></span> <span data-ttu-id="36646-254">Akkor lesz naplózása egy szervezeti fiók, de a szükséges toolog be a személyes fiókjával.</span><span class="sxs-lookup"><span data-stu-id="36646-254">You'll be logging in as an Organization account but then need toolog in with your personal account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="36646-255">További források</span><span class="sxs-lookup"><span data-stu-id="36646-255">Additional resources</span></span>

* [<span data-ttu-id="36646-256">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="36646-256">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36646-257">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="36646-257">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
