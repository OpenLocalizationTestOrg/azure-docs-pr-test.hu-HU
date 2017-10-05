---
title: "Oktatóanyag: Azure Active Directoryval integrált GitHub |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a GitHub között."
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
ms.openlocfilehash: 9dc12bc2e313bcb2000724d035156c5054d14e1c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a><span data-ttu-id="9aaca-103">Oktatóanyag: Azure Active Directory-integráció a Githubon</span><span class="sxs-lookup"><span data-stu-id="9aaca-103">Tutorial: Azure Active Directory integration with GitHub</span></span>

<span data-ttu-id="9aaca-104">Ebben az oktatóanyagban elsajátíthatja a GitHub integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9aaca-104">In this tutorial, you learn how to integrate GitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9aaca-105">GitHub integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9aaca-105">Integrating GitHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9aaca-106">Szabályozhatja, aki elérheti a Githubon az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9aaca-106">You can control in Azure AD who has access to GitHub</span></span>
- <span data-ttu-id="9aaca-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett a GitHub (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9aaca-107">You can enable your users to automatically get signed-on to GitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9aaca-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="9aaca-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="9aaca-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9aaca-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9aaca-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9aaca-110">Prerequisites</span></span>

<span data-ttu-id="9aaca-111">Az Azure AD-integráció konfigurálása a Githubon, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9aaca-111">To configure Azure AD integration with GitHub, you need the following items:</span></span>

- <span data-ttu-id="9aaca-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9aaca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9aaca-113">A GitHub egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9aaca-113">A GitHub single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="9aaca-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9aaca-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="9aaca-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9aaca-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9aaca-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9aaca-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9aaca-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9aaca-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="9aaca-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9aaca-118">Scenario description</span></span>
<span data-ttu-id="9aaca-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9aaca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9aaca-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9aaca-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9aaca-121">GitHub hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9aaca-121">Adding GitHub from the gallery</span></span>
2. <span data-ttu-id="9aaca-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9aaca-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-github-from-the-gallery"></a><span data-ttu-id="9aaca-123">GitHub hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9aaca-123">Adding GitHub from the gallery</span></span>
<span data-ttu-id="9aaca-124">Az Azure AD integrálása a Githubon konfigurálásához kell hozzáadnia GitHub a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9aaca-124">To configure the integration of GitHub into Azure AD, you need to add GitHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9aaca-125">**Adja hozzá a Githubon a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9aaca-125">**To add GitHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9aaca-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9aaca-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9aaca-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9aaca-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9aaca-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9aaca-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9aaca-133">Írja be a keresőmezőbe, **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-133">In the search box, type **GitHub.com**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. <span data-ttu-id="9aaca-135">Az eredmények panelen válassza ki a **GitHub**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9aaca-135">In the results panel, select **GitHub**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9aaca-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9aaca-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9aaca-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján GitHub.</span><span class="sxs-lookup"><span data-stu-id="9aaca-138">In this section, you configure and test Azure AD single sign-on with GitHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9aaca-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a github megfelelőjére felhasználó a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9aaca-139">For single sign-on to work, Azure AD needs to know what the counterpart user in GitHub is to a user in Azure AD.</span></span> <span data-ttu-id="9aaca-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Githubon közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9aaca-140">In other words, a link relationship between an Azure AD user and the related user in GitHub needs to be established.</span></span>

<span data-ttu-id="9aaca-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Githubon.</span><span class="sxs-lookup"><span data-stu-id="9aaca-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in GitHub.</span></span>

<span data-ttu-id="9aaca-142">Az Azure AD egyszeri bejelentkezést a Githubon tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9aaca-142">To configure and test Azure AD single sign-on with GitHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9aaca-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9aaca-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9aaca-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9aaca-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9aaca-145">**[GitHub tesztfelhasználó létrehozása](#creating-a-GitHub-test-user)**  - való Britta Simon egy megfelelője a Githubon, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9aaca-145">**[Creating a GitHub test user](#creating-a-GitHub-test-user)** - to have a counterpart of Britta Simon in GitHub that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="9aaca-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9aaca-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9aaca-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9aaca-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9aaca-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9aaca-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9aaca-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és a GitHub-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9aaca-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your GitHub application.</span></span>

<span data-ttu-id="9aaca-150">**Konfigurálja az Azure AD egyszeri bejelentkezést a Githubon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9aaca-150">**To configure Azure AD single sign-on with GitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="9aaca-151">Az Azure felügyeleti portálján a a **GitHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-151">In the Azure Management portal, on the **GitHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9aaca-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="9aaca-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. <span data-ttu-id="9aaca-155">Az a **GitHub-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9aaca-155">On the **GitHub Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    <span data-ttu-id="9aaca-157">a.</span><span class="sxs-lookup"><span data-stu-id="9aaca-157">a.</span></span> <span data-ttu-id="9aaca-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket, mint:`https://github.com/orgs/<entity-id>/sso`</span><span class="sxs-lookup"><span data-stu-id="9aaca-158">In the **Sign-on URL** textbox, type the value as: `https://github.com/orgs/<entity-id>/sso`</span></span>

    <span data-ttu-id="9aaca-159">b.</span><span class="sxs-lookup"><span data-stu-id="9aaca-159">b.</span></span> <span data-ttu-id="9aaca-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://github.com/orgs/<entity-id>`</span><span class="sxs-lookup"><span data-stu-id="9aaca-160">In the **Identifier** textbox, type a URL using the following pattern: `https://github.com/orgs/<entity-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9aaca-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="9aaca-161">Please note that these are not the real values.</span></span> <span data-ttu-id="9aaca-162">Akkor frissítheti ezeket az értékeket a tényleges rang URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9aaca-162">You have to update these values with the actual Sing-on URL and Identifier.</span></span> <span data-ttu-id="9aaca-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="9aaca-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="9aaca-164">Ugrás a GitHub felügyeleti szakasz beolvasása ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9aaca-164">Go to GitHub Admin section to retrieve these values.</span></span> 

4. <span data-ttu-id="9aaca-165">Az a **felhasználói attribútumok** szakaszban jelölje be **felhasználói azonosító** user.mail szerint.</span><span class="sxs-lookup"><span data-stu-id="9aaca-165">On the **User Attributes** section, select **User Identifier** as user.mail.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. <span data-ttu-id="9aaca-167">Az a **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-167">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. <span data-ttu-id="9aaca-169">A a **új tanúsítvány létrehozása** párbeszédpanel, kattintson a naptár ikonra, és válasszon egy **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-169">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="9aaca-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9aaca-170">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. <span data-ttu-id="9aaca-172">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9aaca-172">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. <span data-ttu-id="9aaca-174">Az előugró **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-174">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="9aaca-176">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9aaca-176">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. <span data-ttu-id="9aaca-178">A a **GitHub konfigurációs** kattintson **konfigurálása GitHub** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9aaca-178">On the **GitHub Configuration** section, click **Configure GitHub** to open **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. <span data-ttu-id="9aaca-181">Egy másik webes böngészőablakban jelentkezzen be a Githubon szervezet webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9aaca-181">In a different web browser window, log into your GitHub organization site as an administrator.</span></span>

12. <span data-ttu-id="9aaca-182">Navigáljon a **beállítások** kattintson **biztonsági**</span><span class="sxs-lookup"><span data-stu-id="9aaca-182">Navigate to **Settings** and click **Security**</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. <span data-ttu-id="9aaca-184">Ellenőrizze a **engedélyezése SAML-alapú hitelesítés** mezőbe, hogy nem jeleníti meg az egyszeri bejelentkezés konfigurációs mezők.</span><span class="sxs-lookup"><span data-stu-id="9aaca-184">Check the **Enable SAML authentication** box, revealing the Single Sign-on configuration fields.</span></span> <span data-ttu-id="9aaca-185">Ezután az egyszeri bejelentkezési URL-cím segítségével egyetlen bejelentkezési URL-CÍMÉT az Azure AD-konfiguráció frissítése.</span><span class="sxs-lookup"><span data-stu-id="9aaca-185">Then, use the single sign-on URL value to update the Single sign-on URL on Azure AD configuration.</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. <span data-ttu-id="9aaca-187">Adja meg a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="9aaca-187">Configure the following fields:</span></span>

    <span data-ttu-id="9aaca-188">a.</span><span class="sxs-lookup"><span data-stu-id="9aaca-188">a.</span></span> <span data-ttu-id="9aaca-189">**Jelentkezzen be az URL-cím**: Adja meg **SAML-alapú egyszeri bejelentkezést szolgáltatás URL-címe** a a **konfigurálása GitHub** szakasz Azure ad-val</span><span class="sxs-lookup"><span data-stu-id="9aaca-189">**Sign on URL**: Enter **SAML Single sign-on Service URL** from the **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="9aaca-190">b.</span><span class="sxs-lookup"><span data-stu-id="9aaca-190">b.</span></span> <span data-ttu-id="9aaca-191">**Kibocsátó**: Adjon meg **SAML Entitásazonosító** a a **konfigurálása GitHub** szakasz Azure ad-val</span><span class="sxs-lookup"><span data-stu-id="9aaca-191">**Issuer**: Enter **SAML Entity ID** from the **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="9aaca-192">c.</span><span class="sxs-lookup"><span data-stu-id="9aaca-192">c.</span></span> <span data-ttu-id="9aaca-193">**Nyilvános tanúsítvány**: Nyissa meg a letöltött tanúsítvány az Azure AD egy fájlt, és másolja a tartalmat, beleértve a "BEGIN tanúsítvány" és "END CERTIFICATE"</span><span class="sxs-lookup"><span data-stu-id="9aaca-193">**Public Certificate**: Open the downloaded certificate from Azure AD in a notepad and copy the content including "BEGIN CERTIFICATE" and "END CERTIFICATE"</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. <span data-ttu-id="9aaca-195">Kattintson a **teszt SAML-alapú konfigurációs** annak ellenőrzésére, hogy nincs érvényesítési hibák vagy hibák egyszeri bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="9aaca-195">Click on **Test SAML configuration** to confirm that no validation failures or errors during SSO.</span></span>

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. <span data-ttu-id="9aaca-197">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="9aaca-197">Click **Save**</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9aaca-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9aaca-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="9aaca-199">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9aaca-199">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9aaca-201">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9aaca-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9aaca-202">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9aaca-202">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9aaca-204">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9aaca-204">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9aaca-206">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9aaca-206">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9aaca-208">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9aaca-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9aaca-210">a.</span><span class="sxs-lookup"><span data-stu-id="9aaca-210">a.</span></span> <span data-ttu-id="9aaca-211">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9aaca-212">b.</span><span class="sxs-lookup"><span data-stu-id="9aaca-212">b.</span></span> <span data-ttu-id="9aaca-213">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9aaca-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9aaca-214">c.</span><span class="sxs-lookup"><span data-stu-id="9aaca-214">c.</span></span> <span data-ttu-id="9aaca-215">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9aaca-216">d.</span><span class="sxs-lookup"><span data-stu-id="9aaca-216">d.</span></span> <span data-ttu-id="9aaca-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9aaca-217">Click **Create**.</span></span> 


### <a name="creating-a-github-test-user"></a><span data-ttu-id="9aaca-218">GitHub tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9aaca-218">Creating a GitHub test user</span></span>

<span data-ttu-id="9aaca-219">Ahhoz, hogy jelentkezzen be a Githubon az Azure AD-felhasználók, akkor ki kell építenie a Githubon.</span><span class="sxs-lookup"><span data-stu-id="9aaca-219">In order to enable Azure AD users to log into GitHub, they must be provisioned into GitHub.</span></span>  
<span data-ttu-id="9aaca-220">GitHub, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9aaca-220">In the case of GitHub, provisioning is a manual task.</span></span>

<span data-ttu-id="9aaca-221">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9aaca-221">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="9aaca-222">Jelentkezzen be rendszergazdaként a Githubon vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="9aaca-222">Log in to your GitHub company site as an administrator.</span></span>

2. <span data-ttu-id="9aaca-223">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-223">Click **People**.</span></span>

    <span data-ttu-id="9aaca-224">![Személyek](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="9aaca-224">![People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "People")</span></span>

3. <span data-ttu-id="9aaca-225">Kattintson a **a meghívás tag**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-225">Click **Invite member**.</span></span>

    <span data-ttu-id="9aaca-226">![Meghívott felhasználóknak](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "meghívott felhasználóknak")</span><span class="sxs-lookup"><span data-stu-id="9aaca-226">![Invite Users](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Invite Users")</span></span>

4. <span data-ttu-id="9aaca-227">Az a **a meghívás tag** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9aaca-227">On the **Invite member** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="9aaca-228">a.</span><span class="sxs-lookup"><span data-stu-id="9aaca-228">a.</span></span> <span data-ttu-id="9aaca-229">Az a **E-mail** szövegmezőhöz Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="9aaca-229">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="9aaca-230">![Felkérése](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="9aaca-230">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Invite People")</span></span>
    
    <span data-ttu-id="9aaca-231">b.</span><span class="sxs-lookup"><span data-stu-id="9aaca-231">b.</span></span> <span data-ttu-id="9aaca-232">Kattintson a **meghívó küldése**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-232">Click **Send Invitation**.</span></span>

    <span data-ttu-id="9aaca-233">![Felkérése](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="9aaca-233">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Invite People")</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaca-234">Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="9aaca-234">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9aaca-235">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9aaca-235">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9aaca-236">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által használt hozzáférés biztosítása a GitHub használatára.</span><span class="sxs-lookup"><span data-stu-id="9aaca-236">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to GitHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9aaca-238">**A GitHub Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="9aaca-238">**To assign Britta Simon to GitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="9aaca-239">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-239">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9aaca-241">Az alkalmazások listában válassza ki a **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-241">In the applications list, select **GitHub.com**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. <span data-ttu-id="9aaca-243">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9aaca-243">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9aaca-245">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9aaca-245">Click **Add** button.</span></span> <span data-ttu-id="9aaca-246">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9aaca-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9aaca-248">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9aaca-248">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9aaca-249">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9aaca-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9aaca-250">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9aaca-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="9aaca-251">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9aaca-251">Testing single sign-on</span></span>

<span data-ttu-id="9aaca-252">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9aaca-252">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9aaca-253">Ha a hozzáférési Panel GitHub mozaik gombra kattint, meg kell beolvasása bejelentkezett a GitHub-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="9aaca-253">When you click the GitHub tile in the Access Panel, you should get signed-on to your GitHub application.</span></span> <span data-ttu-id="9aaca-254">Akkor lesz naplózása szervezeti fiók azonban akkor kell jelentkezzen be a személyes fiókjával.</span><span class="sxs-lookup"><span data-stu-id="9aaca-254">You'll be logging in as an Organization account but then need to log in with your personal account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9aaca-255">További források</span><span class="sxs-lookup"><span data-stu-id="9aaca-255">Additional resources</span></span>

* [<span data-ttu-id="9aaca-256">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9aaca-256">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9aaca-257">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9aaca-257">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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
