---
title: "Oktatóanyag: Azure Active Directory-integráció SAML-alapú egyszeri a Jira felbontása GmbH |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a SAML SSO Jira a felbontása GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: cde5983710185d1e46a5601b16bbfb1c0fcae382
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a><span data-ttu-id="c16f0-103">Oktatóanyag: Azure Active Directory-integráció SAML-alapú egyszeri a Jira felbontása GmbH</span><span class="sxs-lookup"><span data-stu-id="c16f0-103">Tutorial: Azure Active Directory integration with SAML SSO for Jira by resolution GmbH</span></span>

<span data-ttu-id="c16f0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan integrálását a SAML SSO Jira a feloldási GmbH az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c16f0-104">In this tutorial, you learn how to integrate SAML SSO for Jira by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c16f0-105">SAML SSO Jira a felbontása GmbH az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c16f0-105">Integrating SAML SSO for Jira by resolution GmbH with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c16f0-106">Szabályozhatja, aki hozzáféréssel rendelkezik SAML SSO Jira a felbontása GmbH Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c16f0-106">You can control in Azure AD who has access to SAML SSO for Jira by resolution GmbH</span></span>
- <span data-ttu-id="c16f0-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a SAML SSO Jira a felbontása GmbH (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c16f0-107">You can enable your users to automatically get signed-on to SAML SSO for Jira by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c16f0-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c16f0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c16f0-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c16f0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c16f0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c16f0-110">Prerequisites</span></span>

<span data-ttu-id="c16f0-111">Konfigurálása az Azure AD-integrációs Jira a SAML SSO feloldási GmbH, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c16f0-111">To configure Azure AD integration with SAML SSO for Jira by resolution GmbH, you need the following items:</span></span>

- <span data-ttu-id="c16f0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c16f0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c16f0-113">A SAML SSO a Jira felbontása GmbH egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="c16f0-113">A SAML SSO for Jira by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c16f0-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c16f0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c16f0-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c16f0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c16f0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c16f0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c16f0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c16f0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c16f0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c16f0-118">Scenario description</span></span>
<span data-ttu-id="c16f0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c16f0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c16f0-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c16f0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c16f0-121">Megoldási GmbH Jira a SAML SSO hozzáadását a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c16f0-121">Adding SAML SSO for Jira by resolution GmbH from the gallery</span></span>
2. <span data-ttu-id="c16f0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c16f0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a><span data-ttu-id="c16f0-123">Megoldási GmbH Jira a SAML SSO hozzáadását a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c16f0-123">Adding SAML SSO for Jira by resolution GmbH from the gallery</span></span>
<span data-ttu-id="c16f0-124">A integrálása a SAML SSO Jira a feloldási GmbH által az Azure AD konfigurálása, kell hozzáadnia Jira a SAML SSO felbontása GmbH a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c16f0-124">To configure the integration of SAML SSO for Jira by resolution GmbH into Azure AD, you need to add SAML SSO for Jira by resolution GmbH from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c16f0-125">**Adja hozzá a SAML SSO Jira a felbontása GmbH a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c16f0-125">**To add SAML SSO for Jira by resolution GmbH from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c16f0-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c16f0-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c16f0-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c16f0-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c16f0-133">Írja be a keresőmezőbe, **Jira felbontása GmbH a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-133">In the search box, type **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. <span data-ttu-id="c16f0-135">Az eredmények panelen válassza ki a **Jira felbontása GmbH a SAML SSO**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c16f0-135">In the results panel, select **SAML SSO for Jira by resolution GmbH**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c16f0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c16f0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c16f0-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri a Jira GmbH alapján "Britta Simon." nevű tesztfelhasználó felbontása</span><span class="sxs-lookup"><span data-stu-id="c16f0-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c16f0-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, milyen a párjukhoz felhasználó Jira a SAML SSO az Azure AD egy felhasználónak van GmbH felbontása.</span><span class="sxs-lookup"><span data-stu-id="c16f0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAML SSO for Jira by resolution GmbH is to a user in Azure AD.</span></span> <span data-ttu-id="c16f0-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SAML SSO Jira a felbontása hivatkozás kapcsolatának GmbH kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c16f0-140">In other words, a link relationship between an Azure AD user and the related user in SAML SSO for Jira by resolution GmbH needs to be established.</span></span>

<span data-ttu-id="c16f0-141">Felbontása GmbH Jira SAML-alapú egyszeri Bejelentkezést, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c16f0-141">In SAML SSO for Jira by resolution GmbH, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c16f0-142">Az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri a Jira felbontása GmbH tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c16f0-142">To configure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c16f0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c16f0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c16f0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c16f0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c16f0-145">**[A SAML SSO a feloldási GmbH teszt felhasználó Jira létrehozása](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  - való egy megfelelője a Britta Simon Jira a SAML SSO felbontása GmbH, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c16f0-145">**[Creating a SAML SSO for Jira by resolution GmbH test user](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)** - to have a counterpart of Britta Simon in SAML SSO for Jira by resolution GmbH that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c16f0-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c16f0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c16f0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c16f0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c16f0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c16f0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c16f0-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, majd állítsa egyszeri bejelentkezéshez a SAML SSO Jira a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c16f0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAML SSO for Jira by resolution GmbH application.</span></span>

<span data-ttu-id="c16f0-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri Jira a feloldási GmbH, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c16f0-150">**To configure Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="c16f0-151">Az Azure portálon a a **Jira felbontása GmbH a SAML SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-151">In the Azure portal, on the **SAML SSO for Jira by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c16f0-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c16f0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. <span data-ttu-id="c16f0-155">Az a **SAML SSO Jira felbontása GmbH tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="c16f0-155">On the **SAML SSO for Jira by resolution GmbH Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    <span data-ttu-id="c16f0-157">a.</span><span class="sxs-lookup"><span data-stu-id="c16f0-157">a.</span></span> <span data-ttu-id="c16f0-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="c16f0-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="c16f0-159">b.</span><span class="sxs-lookup"><span data-stu-id="c16f0-159">b.</span></span> <span data-ttu-id="c16f0-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="c16f0-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="c16f0-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="c16f0-162">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="c16f0-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    <span data-ttu-id="c16f0-164">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="c16f0-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="c16f0-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c16f0-165">These values are not real.</span></span> <span data-ttu-id="c16f0-166">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="c16f0-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="c16f0-167">Ügyfél [Jira feloldási GmbH ügyfél által a SAML SSO támogatási csoport](https://www.resolution.de/go/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c16f0-167">Contact [SAML SSO for Jira by resolution GmbH Client support team](https://www.resolution.de/go/support) to get these values.</span></span> 

5. <span data-ttu-id="c16f0-168">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c16f0-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. <span data-ttu-id="c16f0-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="c16f0-172">Egy másik webes böngészőablakban, jelentkezzen be a **Jira feloldási GmbH felügyeleti portál a SAML SSO** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c16f0-172">In a different web browser window, log in to your **SAML SSO for Jira by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="c16f0-173">Vigye a mutatót a ikonjára, majd kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-173">Hover on cog and click the **Add-ons**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. <span data-ttu-id="c16f0-175">Rendszergazdai hozzáférés lap van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="c16f0-175">You are redirected to Administrator Access page.</span></span> <span data-ttu-id="c16f0-176">Adja meg a **jelszó** kattintson **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-176">Enter the **Password** and click **Confirm** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. <span data-ttu-id="c16f0-178">A bővítmények lap szakaszban kattintson **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-178">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="c16f0-179">Keresési **SAML-alapú egyszeri bejelentkezés (SSO) a JIRA** kattintson **telepítése** az új SAML-alapú beépülő modul telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-179">Search **SAML Single Sign On (SSO) for JIRA** and click **Install** button to install the new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. <span data-ttu-id="c16f0-181">A beépülő modul telepítése elindul.</span><span class="sxs-lookup"><span data-stu-id="c16f0-181">The plugin installation will start.</span></span> <span data-ttu-id="c16f0-182">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-182">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. <span data-ttu-id="c16f0-185">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-185">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. <span data-ttu-id="c16f0-187">Kattintson a **konfigurálása** konfigurálása az új beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="c16f0-187">Click **Configure** to configure the new plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. <span data-ttu-id="c16f0-189">A **SAML SingleSignOn beépülő modul konfigurációs** kattintson **adja hozzá a további identitásszolgáltató** gombra kattintva adja meg a beállításokat az identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="c16f0-189">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button to configure the settings of Identity Provider.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. <span data-ttu-id="c16f0-191">Hajtsa végre az ezen a lapon a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c16f0-191">Perform following steps on this page:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon5.png)
 
    <span data-ttu-id="c16f0-193">a.</span><span class="sxs-lookup"><span data-stu-id="c16f0-193">a.</span></span> <span data-ttu-id="c16f0-194">Adja hozzá **neve** az Identity Provider (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c16f0-194">Add **Name** of the Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="c16f0-195">b.</span><span class="sxs-lookup"><span data-stu-id="c16f0-195">b.</span></span> <span data-ttu-id="c16f0-196">Adja hozzá **leírás** az Identity Provider (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c16f0-196">Add **Description** of the Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="c16f0-197">c.</span><span class="sxs-lookup"><span data-stu-id="c16f0-197">c.</span></span> <span data-ttu-id="c16f0-198">Kattintson a **XML** válassza ki a **metaadatok** fájlt, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="c16f0-198">Click **XML** and select the **Metadata** file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="c16f0-199">d.</span><span class="sxs-lookup"><span data-stu-id="c16f0-199">d.</span></span> <span data-ttu-id="c16f0-200">Kattintson a **terhelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-200">Click **Load** button.</span></span>

    <span data-ttu-id="c16f0-201">e.</span><span class="sxs-lookup"><span data-stu-id="c16f0-201">e.</span></span> <span data-ttu-id="c16f0-202">Beolvassa a kiállító terjesztési hely metaadatok, és feltölti a mezőt is ezen a képernyőfelvételen kiemelt.</span><span class="sxs-lookup"><span data-stu-id="c16f0-202">It reads the IdP metadata and populates the fields as highlighted in the screenshot.</span></span> 

16. <span data-ttu-id="c16f0-203">Kattintson a **beállítások mentése** gombra a beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c16f0-203">Click **Save settings** button to save the settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="c16f0-205">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c16f0-205">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c16f0-206">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c16f0-206">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c16f0-207">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c16f0-207">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c16f0-208">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c16f0-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="c16f0-209">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c16f0-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c16f0-211">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c16f0-211">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c16f0-212">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-212">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c16f0-214">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-214">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c16f0-216">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c16f0-216">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c16f0-218">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c16f0-218">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c16f0-220">a.</span><span class="sxs-lookup"><span data-stu-id="c16f0-220">a.</span></span> <span data-ttu-id="c16f0-221">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-221">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c16f0-222">b.</span><span class="sxs-lookup"><span data-stu-id="c16f0-222">b.</span></span> <span data-ttu-id="c16f0-223">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c16f0-223">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c16f0-224">c.</span><span class="sxs-lookup"><span data-stu-id="c16f0-224">c.</span></span> <span data-ttu-id="c16f0-225">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-225">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c16f0-226">d.</span><span class="sxs-lookup"><span data-stu-id="c16f0-226">d.</span></span> <span data-ttu-id="c16f0-227">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-227">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a><span data-ttu-id="c16f0-228">A SAML SSO a Jira feloldási GmbH teszt felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c16f0-228">Creating a SAML SSO for Jira by resolution GmbH test user</span></span>

<span data-ttu-id="c16f0-229">Ahhoz, hogy a SAML SSO Jira a feloldási GmbH jelentkezzen be az Azure AD-felhasználók, azok kell üzembe Jira a SAML SSO be névfeloldási GmbH által.</span><span class="sxs-lookup"><span data-stu-id="c16f0-229">To enable Azure AD users to log in to SAML SSO for Jira by resolution GmbH, they must be provisioned into SAML SSO for Jira by resolution GmbH.</span></span>  
<span data-ttu-id="c16f0-230">A SAML SSO a felbontása GmbH Jira kézi tevékenység kiépítése.</span><span class="sxs-lookup"><span data-stu-id="c16f0-230">In SAML SSO for Jira by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="c16f0-231">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c16f0-231">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="c16f0-232">Jelentkezzen be rendszergazdaként a feloldási GmbH vállalati hely Jira a SAML SSO.</span><span class="sxs-lookup"><span data-stu-id="c16f0-232">Log in to your SAML SSO for Jira by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="c16f0-233">Vigye a mutatót a ikonjára, majd kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-233">Hover on cog and click the **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. <span data-ttu-id="c16f0-235">Ekkor megnyílik a rendszergazdai hozzáférés lapon megadja a **jelszó** kattintson **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-235">You are redirected to Administrator Access page to enter **Password** and click **Confirm** button.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. <span data-ttu-id="c16f0-237">A **felhasználókezelés** szakasz lapra, majd **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-237">Under **User management** tab section, click **create user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. <span data-ttu-id="c16f0-239">Az a **"Új felhasználó létrehozása"** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c16f0-239">On the **“Create new user”** dialog page, perform the following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    <span data-ttu-id="c16f0-241">a.</span><span class="sxs-lookup"><span data-stu-id="c16f0-241">a.</span></span> <span data-ttu-id="c16f0-242">Az a **E-mail cím** szövegmező, a felhasználó e-mail címe típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="c16f0-242">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="c16f0-243">b.</span><span class="sxs-lookup"><span data-stu-id="c16f0-243">b.</span></span> <span data-ttu-id="c16f0-244">Az a **teljes nevét** szövegmező, a felhasználó például Britta Simon típus teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="c16f0-244">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>

    <span data-ttu-id="c16f0-245">c.</span><span class="sxs-lookup"><span data-stu-id="c16f0-245">c.</span></span> <span data-ttu-id="c16f0-246">Az a **felhasználónév** szövegmezőben, az e-mailt a felhasználó típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="c16f0-246">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="c16f0-247">d.</span><span class="sxs-lookup"><span data-stu-id="c16f0-247">d.</span></span> <span data-ttu-id="c16f0-248">Az a **jelszó** szövegmező, írja be a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c16f0-248">In the **Password** textbox, type the password of user.</span></span>

    <span data-ttu-id="c16f0-249">e.</span><span class="sxs-lookup"><span data-stu-id="c16f0-249">e.</span></span> <span data-ttu-id="c16f0-250">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-250">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c16f0-251">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c16f0-251">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c16f0-252">Ebben a szakaszban Britta Simon által biztosított hozzáférés Jira a SAML SSO feloldási GmbH által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c16f0-252">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAML SSO for Jira by resolution GmbH.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c16f0-254">**Britta Simon GmbH megoldási történő Jira a SAML SSO hozzárendelését, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c16f0-254">**To assign Britta Simon to SAML SSO for Jira by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="c16f0-255">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-255">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c16f0-257">Az alkalmazások listában válassza ki a **Jira felbontása GmbH a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-257">In the applications list, select **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. <span data-ttu-id="c16f0-259">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c16f0-259">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c16f0-261">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c16f0-261">Click **Add** button.</span></span> <span data-ttu-id="c16f0-262">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c16f0-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c16f0-264">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c16f0-264">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c16f0-265">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c16f0-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c16f0-266">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c16f0-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c16f0-267">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c16f0-267">Testing single sign-on</span></span>

<span data-ttu-id="c16f0-268">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c16f0-268">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c16f0-269">A SAML SSO Jira a kattintson a feloldás GmbH csempe a hozzáférési panelen által, meg kell beolvasása automatikusan aláírt a a SAML SSO Jira a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c16f0-269">When you click the SAML SSO for Jira by resolution GmbH tile in the Access Panel, you should get automatically signed-on to your SAML SSO for Jira by resolution GmbH application.</span></span>
<span data-ttu-id="c16f0-270">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c16f0-270">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c16f0-271">További források</span><span class="sxs-lookup"><span data-stu-id="c16f0-271">Additional resources</span></span>

* [<span data-ttu-id="c16f0-272">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c16f0-272">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c16f0-273">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c16f0-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

