---
title: "Oktatóanyag: Azure Active Directory-integráció való összefolyás felett az SAML-alapú egyszeri felbontása GmbH |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a SAML SSO való összefolyás felett a felbontása GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a36d686ba39b5168860a20e8c4db357888df6a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a><span data-ttu-id="49c78-103">Oktatóanyag: Azure Active Directory-integráció való összefolyás felett az SAML-alapú egyszeri felbontása GmbH</span><span class="sxs-lookup"><span data-stu-id="49c78-103">Tutorial: Azure Active Directory integration with SAML SSO for Confluence by resolution GmbH</span></span>

<span data-ttu-id="49c78-104">Ebben az oktatóanyagban elsajátíthatja, hogyan integrálását a SAML SSO való összefolyás felett a feloldási GmbH az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="49c78-104">In this tutorial, you learn how to integrate SAML SSO for Confluence by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="49c78-105">SAML SSO való összefolyás felett a felbontása GmbH az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="49c78-105">Integrating SAML SSO for Confluence by resolution GmbH with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="49c78-106">Szabályozhatja, aki hozzáféréssel rendelkezik SAML SSO való összefolyás felett a felbontása GmbH Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="49c78-106">You can control in Azure AD who has access to SAML SSO for Confluence by resolution GmbH</span></span>
- <span data-ttu-id="49c78-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a SAML SSO való összefolyás felett a felbontása GmbH (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="49c78-107">You can enable your users to automatically get signed-on to SAML SSO for Confluence by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="49c78-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49c78-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="49c78-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="49c78-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49c78-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49c78-110">Prerequisites</span></span>

<span data-ttu-id="49c78-111">Konfigurálása az Azure AD-integrációs való összefolyás felett a SAML SSO feloldási GmbH, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="49c78-111">To configure Azure AD integration with SAML SSO for Confluence by resolution GmbH, you need the following items:</span></span>

- <span data-ttu-id="49c78-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="49c78-112">An Azure AD subscription</span></span>
- <span data-ttu-id="49c78-113">A SAML SSO a felbontása GmbH egyszeri bejelentkezés engedélyezve van az előfizetésben való összefolyás felett</span><span class="sxs-lookup"><span data-stu-id="49c78-113">A SAML SSO for Confluence by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="49c78-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="49c78-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="49c78-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="49c78-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="49c78-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="49c78-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="49c78-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="49c78-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="49c78-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="49c78-118">Scenario description</span></span>
<span data-ttu-id="49c78-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="49c78-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="49c78-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="49c78-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="49c78-121">Megoldási GmbH való összefolyás felett a SAML SSO hozzáadását a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="49c78-121">Adding SAML SSO for Confluence by resolution GmbH from the gallery</span></span>
2. <span data-ttu-id="49c78-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="49c78-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a><span data-ttu-id="49c78-123">Megoldási GmbH való összefolyás felett a SAML SSO hozzáadását a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="49c78-123">Adding SAML SSO for Confluence by resolution GmbH from the gallery</span></span>

<span data-ttu-id="49c78-124">A integrálása a SAML SSO való összefolyás felett a feloldási GmbH által az Azure AD konfigurálásához kell hozzáadnia való összefolyás felett a SAML SSO felbontása GmbH a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="49c78-124">To configure the integration of SAML SSO for Confluence by resolution GmbH into Azure AD, you need to add SAML SSO for Confluence by resolution GmbH from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="49c78-125">**Adja hozzá a SAML SSO való összefolyás felett a felbontása GmbH a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-125">**To add SAML SSO for Confluence by resolution GmbH from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="49c78-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="49c78-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="49c78-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="49c78-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49c78-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="49c78-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="49c78-133">Írja be a keresőmezőbe, **felbontása GmbH való összefolyás felett a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="49c78-133">In the search box, type **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. <span data-ttu-id="49c78-135">Az eredmények panelen válassza ki a **felbontása GmbH való összefolyás felett a SAML SSO**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49c78-135">In the results panel, select **SAML SSO for Confluence by resolution GmbH**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="49c78-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="49c78-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="49c78-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez való összefolyás felett az SAML-alapú egyszeri által GmbH alapuló "Britta Simon." nevű tesztfelhasználó felbontás</span><span class="sxs-lookup"><span data-stu-id="49c78-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="49c78-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó a SAML SSO való összefolyás felett a az Azure AD egy felhasználónak van GmbH felbontása.</span><span class="sxs-lookup"><span data-stu-id="49c78-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAML SSO for Confluence by resolution GmbH is to a user in Azure AD.</span></span> <span data-ttu-id="49c78-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SAML SSO való összefolyás felett a felbontása hivatkozás kapcsolatának GmbH kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="49c78-140">In other words, a link relationship between an Azure AD user and the related user in SAML SSO for Confluence by resolution GmbH needs to be established.</span></span>

<span data-ttu-id="49c78-141">A SAML SSO a felbontása GmbH való összefolyás felett, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="49c78-141">In SAML SSO for Confluence by resolution GmbH, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="49c78-142">Az Azure AD egyszeri bejelentkezéshez a SAML SSO való összefolyás felett a felbontása GmbH tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="49c78-142">To configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="49c78-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="49c78-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="49c78-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="49c78-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="49c78-145">**[A SAML SSO a feloldási GmbH teszt felhasználó való összefolyás felett létrehozása](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  - való egy megfelelője a Britta Simon való összefolyás felett a SAML SSO felbontása GmbH, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="49c78-145">**[Creating a SAML SSO for Confluence by resolution GmbH test user](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)** - to have a counterpart of Britta Simon in SAML SSO for Confluence by resolution GmbH that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="49c78-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49c78-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="49c78-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="49c78-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="49c78-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="49c78-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="49c78-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, majd állítsa egyszeri bejelentkezéshez a SAML SSO való összefolyás felett a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="49c78-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAML SSO for Confluence by resolution GmbH application.</span></span>

<span data-ttu-id="49c78-150">**Konfigurálja az Azure AD egyszeri bejelentkezéssel való összefolyás felett az SAML-alapú egyszeri feloldási GmbH, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-150">**To configure Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-151">Az Azure portálon a a **felbontása GmbH való összefolyás felett a SAML SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="49c78-151">In the Azure portal, on the **SAML SSO for Confluence by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="49c78-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49c78-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. <span data-ttu-id="49c78-155">Az a **SAML SSO való összefolyás felett felbontása GmbH tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="49c78-155">On the **SAML SSO for Confluence by resolution GmbH Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    <span data-ttu-id="49c78-157">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-157">a.</span></span> <span data-ttu-id="49c78-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="49c78-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="49c78-159">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-159">b.</span></span> <span data-ttu-id="49c78-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="49c78-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="49c78-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="49c78-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="49c78-162">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="49c78-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    <span data-ttu-id="49c78-164">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="49c78-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="49c78-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="49c78-165">These values are not real.</span></span> <span data-ttu-id="49c78-166">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="49c78-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="49c78-167">Ügyfél [való összefolyás felett feloldási GmbH ügyfél által a SAML SSO támogatási csoport](https://www.resolution.de/go/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="49c78-167">Contact [SAML SSO for Confluence by resolution GmbH Client support team](https://www.resolution.de/go/support) to get these values.</span></span> 

5. <span data-ttu-id="49c78-168">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="49c78-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. <span data-ttu-id="49c78-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. <span data-ttu-id="49c78-172">Egy másik webes böngészőablakban, jelentkezzen be a **való összefolyás felett feloldási GmbH felügyeleti portál a SAML SSO** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="49c78-172">In a different web browser window, log in to your **SAML SSO for Confluence by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="49c78-173">Vigye a mutatót a ikonjára, majd kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="49c78-173">Hover on cog and click the **Add-ons**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. <span data-ttu-id="49c78-175">Rendszergazdai hozzáférés lap van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="49c78-175">You are redirected to Administrator Access page.</span></span> <span data-ttu-id="49c78-176">Adja meg a jelszót, és kattintson a **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-176">Enter the password and click **Confirm** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. <span data-ttu-id="49c78-178">A **ATLASSIAN piactér** lapra, majd **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="49c78-178">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. <span data-ttu-id="49c78-180">Keresési **SAML-alapú egyszeri bejelentkezés (SSO) való összefolyás felett a** kattintson **telepítése** az új SAML-alapú beépülő modul telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-180">Search **SAML Single Sign On (SSO) for Confluence** and click **Install** button to install the new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. <span data-ttu-id="49c78-182">A beépülő modul telepítése elindul.</span><span class="sxs-lookup"><span data-stu-id="49c78-182">The plugin installation will start.</span></span> <span data-ttu-id="49c78-183">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-183">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. <span data-ttu-id="49c78-186">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-186">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. <span data-ttu-id="49c78-188">Kattintson a **konfigurálása** konfigurálása az új beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="49c78-188">Click **Configure** to configure the new plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. <span data-ttu-id="49c78-190">Az új beépülő modult is található **felhasználók és biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="49c78-190">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. <span data-ttu-id="49c78-192">A **SAML SingleSignOn beépülő modul konfigurációs** kattintson **adja hozzá a további identitásszolgáltató** gombra kattintva adja meg a beállításokat az identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="49c78-192">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button to configure the settings of Identity Provider.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. <span data-ttu-id="49c78-194">Hajtsa végre az ezen a lapon a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="49c78-194">Perform following steps on this page:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    <span data-ttu-id="49c78-196">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-196">a.</span></span> <span data-ttu-id="49c78-197">Adja hozzá **neve** az Identity Provider (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="49c78-197">Add **Name** of the Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="49c78-198">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-198">b.</span></span> <span data-ttu-id="49c78-199">Adja hozzá **leírás** az Identity Provider (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="49c78-199">Add **Description** of the Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="49c78-200">c.</span><span class="sxs-lookup"><span data-stu-id="49c78-200">c.</span></span> <span data-ttu-id="49c78-201">Kattintson a **XML** válassza ki a **metaadatok** Azure portálról letöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="49c78-201">Click **XML** and select the **Metadata** file that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="49c78-202">d.</span><span class="sxs-lookup"><span data-stu-id="49c78-202">d.</span></span> <span data-ttu-id="49c78-203">Kattintson a **terhelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-203">Click **Load** button.</span></span>

    <span data-ttu-id="49c78-204">e.</span><span class="sxs-lookup"><span data-stu-id="49c78-204">e.</span></span> <span data-ttu-id="49c78-205">Beolvassa a kiállító terjesztési hely metaadatok, és feltölti a mezőt is ezen a képernyőfelvételen kiemelt.</span><span class="sxs-lookup"><span data-stu-id="49c78-205">It reads the IdP metadata and populates the fields as highlighted in the screenshot.</span></span> 
18. <span data-ttu-id="49c78-206">Kattintson a **beállítások mentése** gombra a beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="49c78-206">Click **Save settings** button to save the settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="49c78-208">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="49c78-208">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="49c78-209">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="49c78-209">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="49c78-210">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="49c78-210">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="49c78-211">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="49c78-211">Creating an Azure AD test user</span></span>
<span data-ttu-id="49c78-212">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="49c78-212">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="49c78-214">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-214">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-215">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="49c78-215">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="49c78-217">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="49c78-217">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="49c78-219">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="49c78-219">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="49c78-221">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="49c78-221">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="49c78-223">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-223">a.</span></span> <span data-ttu-id="49c78-224">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="49c78-224">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="49c78-225">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-225">b.</span></span> <span data-ttu-id="49c78-226">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="49c78-226">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="49c78-227">c.</span><span class="sxs-lookup"><span data-stu-id="49c78-227">c.</span></span> <span data-ttu-id="49c78-228">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="49c78-228">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="49c78-229">d.</span><span class="sxs-lookup"><span data-stu-id="49c78-229">d.</span></span> <span data-ttu-id="49c78-230">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-230">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a><span data-ttu-id="49c78-231">A SAML SSO a való összefolyás felett feloldási GmbH teszt felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="49c78-231">Creating a SAML SSO for Confluence by resolution GmbH test user</span></span>

<span data-ttu-id="49c78-232">Ahhoz, hogy a SAML SSO való összefolyás felett a feloldási GmbH jelentkezzen be az Azure AD-felhasználók, azok kell üzembe való összefolyás felett a SAML SSO be névfeloldási GmbH által.</span><span class="sxs-lookup"><span data-stu-id="49c78-232">To enable Azure AD users to log in to SAML SSO for Confluence by resolution GmbH, they must be provisioned into SAML SSO for Confluence by resolution GmbH.</span></span>  
<span data-ttu-id="49c78-233">A SAML SSO a felbontása GmbH való összefolyás felett egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="49c78-233">In SAML SSO for Confluence by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="49c78-234">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-234">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-235">Jelentkezzen be rendszergazdaként a feloldási GmbH vállalati hely való összefolyás felett a SAML SSO.</span><span class="sxs-lookup"><span data-stu-id="49c78-235">Log in to your SAML SSO for Confluence by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="49c78-236">Vigye a mutatót a ikonjára, majd kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="49c78-236">Hover on cog and click the **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. <span data-ttu-id="49c78-238">A felhasználók szakaszban kattintson **felhasználók hozzáadása az** fülre.</span><span class="sxs-lookup"><span data-stu-id="49c78-238">Under Users section, click **Add users** tab.</span></span> <span data-ttu-id="49c78-239">Az a **"A felhasználó hozzáadása"** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="49c78-239">On the **“Add a User”** dialog page, perform the following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    <span data-ttu-id="49c78-241">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-241">a.</span></span> <span data-ttu-id="49c78-242">Az a **felhasználónév** szövegmező, írja be a felhasználó Britta Simon például az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="49c78-242">In the **Username** textbox, type the email of user like Britta Simon.</span></span>

    <span data-ttu-id="49c78-243">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-243">b.</span></span> <span data-ttu-id="49c78-244">Az a **teljes nevét** szövegmező, írja be például a Britta Simon felhasználói teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="49c78-244">In the **Full Name** textbox, type the full name of user like Britta Simon.</span></span>

    <span data-ttu-id="49c78-245">c.</span><span class="sxs-lookup"><span data-stu-id="49c78-245">c.</span></span> <span data-ttu-id="49c78-246">Az a **E-mail** szövegmező, a felhasználó e-mail címe típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="49c78-246">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="49c78-247">d.</span><span class="sxs-lookup"><span data-stu-id="49c78-247">d.</span></span> <span data-ttu-id="49c78-248">Az a **jelszó** szövegmezőhöz Britta Simon írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="49c78-248">In the **Password** textbox, type the password for Britta Simon.</span></span>

    <span data-ttu-id="49c78-249">e.</span><span class="sxs-lookup"><span data-stu-id="49c78-249">e.</span></span> <span data-ttu-id="49c78-250">Kattintson a **jelszó megerősítése** írja be újból a jelszót.</span><span class="sxs-lookup"><span data-stu-id="49c78-250">Click **Confirm Password** reenter the password.</span></span>
    
    <span data-ttu-id="49c78-251">f.</span><span class="sxs-lookup"><span data-stu-id="49c78-251">f.</span></span> <span data-ttu-id="49c78-252">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-252">Click **Add** button.</span></span>    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="49c78-253">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="49c78-253">Assigning the Azure AD test user</span></span>

<span data-ttu-id="49c78-254">Ebben a szakaszban Britta Simon való összefolyás felett a SAML SSO GmbH megoldás által biztosított hozzáférés szerint használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49c78-254">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAML SSO for Confluence by resolution GmbH.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="49c78-256">**Britta Simon GmbH megoldási történő való összefolyás felett a SAML SSO hozzárendelését, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-256">**To assign Britta Simon to SAML SSO for Confluence by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-257">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49c78-257">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="49c78-259">Az alkalmazások listában válassza ki a **felbontása GmbH való összefolyás felett a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="49c78-259">In the applications list, select **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. <span data-ttu-id="49c78-261">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="49c78-261">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="49c78-263">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-263">Click **Add** button.</span></span> <span data-ttu-id="49c78-264">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="49c78-266">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="49c78-266">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="49c78-267">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="49c78-268">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="49c78-269">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="49c78-269">Testing single sign-on</span></span>

<span data-ttu-id="49c78-270">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="49c78-270">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="49c78-271">A SAML SSO való összefolyás felett a kattintson a feloldás GmbH csempe a hozzáférési panelen által, meg kell beolvasása automatikusan aláírt a a SAML SSO való összefolyás felett a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="49c78-271">When you click the SAML SSO for Confluence by resolution GmbH tile in the Access Panel, you should get automatically signed-on to your SAML SSO for Confluence by resolution GmbH application.</span></span>
<span data-ttu-id="49c78-272">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="49c78-272">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="49c78-273">További források</span><span class="sxs-lookup"><span data-stu-id="49c78-273">Additional resources</span></span>

* [<span data-ttu-id="49c78-274">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="49c78-274">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="49c78-275">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="49c78-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

