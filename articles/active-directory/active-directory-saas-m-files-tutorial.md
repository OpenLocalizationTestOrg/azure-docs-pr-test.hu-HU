---
title: "Oktatóanyag: Azure Active Directoryval integrált M-fájlok |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az M-fájlok között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f2682cf7cd3e11a5a7156938fbe9d4c7f541312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a><span data-ttu-id="6706a-103">Oktatóanyag: Azure Active Directoryval integrált M-fájlok</span><span class="sxs-lookup"><span data-stu-id="6706a-103">Tutorial: Azure Active Directory integration with M-Files</span></span>

<span data-ttu-id="6706a-104">Ebben az oktatóanyagban elsajátíthatja M-fájlok integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6706a-104">In this tutorial, you learn how to integrate M-Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6706a-105">M-fájlok integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6706a-105">Integrating M-Files with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6706a-106">Szabályozhatja az Azure AD, aki hozzáfér M-fájlok</span><span class="sxs-lookup"><span data-stu-id="6706a-106">You can control in Azure AD who has access to M-Files</span></span>
- <span data-ttu-id="6706a-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett M-fájlok (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6706a-107">You can enable your users to automatically get signed-on to M-Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6706a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6706a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6706a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6706a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6706a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6706a-110">Prerequisites</span></span>

<span data-ttu-id="6706a-111">M-fájlok az Azure AD-integráció konfigurálásához szüksége van a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="6706a-111">To configure Azure AD integration with M-Files, you need the following items:</span></span>

- <span data-ttu-id="6706a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6706a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6706a-113">A M-fájlok az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6706a-113">A M-Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6706a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6706a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6706a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6706a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6706a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6706a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6706a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6706a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6706a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6706a-118">Scenario description</span></span>
<span data-ttu-id="6706a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6706a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6706a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6706a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6706a-121">M-fájlok hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6706a-121">Adding M-Files from the gallery</span></span>
2. <span data-ttu-id="6706a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6706a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-m-files-from-the-gallery"></a><span data-ttu-id="6706a-123">M-fájlok hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6706a-123">Adding M-Files from the gallery</span></span>
<span data-ttu-id="6706a-124">Az Azure AD integrálása a M-fájlok konfigurálásához szüksége M-fájlok hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="6706a-124">To configure the integration of M-Files into Azure AD, you need to add M-Files from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6706a-125">**M-fájlok hozzáadása a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6706a-125">**To add M-Files from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6706a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6706a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6706a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6706a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6706a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6706a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6706a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="6706a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6706a-133">Írja be a keresőmezőbe, **M-fájlok**.</span><span class="sxs-lookup"><span data-stu-id="6706a-133">In the search box, type **M-Files**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. <span data-ttu-id="6706a-135">Az eredmények panelen válassza ki a **M-fájlok**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6706a-135">In the results panel, select **M-Files**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6706a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6706a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6706a-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés M-fájlokat a "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="6706a-138">In this section, you configure and test Azure AD single sign-on with M-Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6706a-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó M-fájlok a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6706a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in M-Files is to a user in Azure AD.</span></span> <span data-ttu-id="6706a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó M-fájlok közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6706a-140">In other words, a link relationship between an Azure AD user and the related user in M-Files needs to be established.</span></span>

<span data-ttu-id="6706a-141">M-fájlokban, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6706a-141">In M-Files, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6706a-142">Az Azure AD az egyszeri bejelentkezés M-fájlokkal tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6706a-142">To configure and test Azure AD single sign-on with M-Files, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6706a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6706a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6706a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6706a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6706a-145">**[M-fájlok tesztfelhasználó létrehozása](#creating-a-m-files-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon M-fájlban, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="6706a-145">**[Creating a M-Files test user](#creating-a-m-files-test-user)** - to have a counterpart of Britta Simon in M-Files that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6706a-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6706a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6706a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6706a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6706a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6706a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6706a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az M-fájlok alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6706a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your M-Files application.</span></span>

<span data-ttu-id="6706a-150">**M-fájlok konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6706a-150">**To configure Azure AD single sign-on with M-Files, perform the following steps:**</span></span>

1. <span data-ttu-id="6706a-151">Az Azure portálon a a **M-fájlok** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6706a-151">In the Azure portal, on the **M-Files** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6706a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6706a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. <span data-ttu-id="6706a-155">Az a **M-fájlok tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6706a-155">On the **M-Files Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    <span data-ttu-id="6706a-157">a.</span><span class="sxs-lookup"><span data-stu-id="6706a-157">a.</span></span> <span data-ttu-id="6706a-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span><span class="sxs-lookup"><span data-stu-id="6706a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span></span>

    <span data-ttu-id="6706a-159">b.</span><span class="sxs-lookup"><span data-stu-id="6706a-159">b.</span></span> <span data-ttu-id="6706a-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.cloudvault.m-files.com`</span><span class="sxs-lookup"><span data-stu-id="6706a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.cloudvault.m-files.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6706a-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6706a-161">These values are not real.</span></span> <span data-ttu-id="6706a-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6706a-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6706a-163">Ügyfél [M-fájlok ügyfél-támogatási csoport](mailto:support@m-files.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6706a-163">Contact [M-Files Client support team](mailto:support@m-files.com) to get these values.</span></span> 
 
4. <span data-ttu-id="6706a-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6706a-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. <span data-ttu-id="6706a-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6706a-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6706a-168">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [M-fájlok támogatási csoport](mailto:support@m-files.com) és adja meg a letöltött metaadatok.</span><span class="sxs-lookup"><span data-stu-id="6706a-168">To get SSO configured for your application, contact [M-Files support team](mailto:support@m-files.com) and provide them the downloaded Metadata.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="6706a-169">Hajtsa végre a következő lépéseket, ha azt szeretné, hogy egyszeri bejelentkezés konfigurálása az Ön M-fájl asztali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6706a-169">Follow the next steps if you want to configure SSO for you M-File desktop application.</span></span> <span data-ttu-id="6706a-170">Nincsenek további lépésre szükség, ha szeretné egyszeri bejelentkezés konfigurálása M-fájlok webes verziója.</span><span class="sxs-lookup"><span data-stu-id="6706a-170">No extra steps are required if you only want to configure SSO for M-Files web version.</span></span>  

7. <span data-ttu-id="6706a-171">A következő lépések segítségével állítsa be az M-fájl asztali alkalmazás engedélyezése az egyszeri bejelentkezés az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="6706a-171">Follow the next steps to configure the M-File desktop application to enable SSO with Azure AD.</span></span> <span data-ttu-id="6706a-172">M-fájlok letöltéséhez keresse fel [M-fájlok letöltése](https://www.m-files.com/en/download-latest-version) lap.</span><span class="sxs-lookup"><span data-stu-id="6706a-172">To download M-Files, go to [M-Files download](https://www.m-files.com/en/download-latest-version) page.</span></span>

8. <span data-ttu-id="6706a-173">Nyissa meg a **M-fájlok asztali beállítások** ablak.</span><span class="sxs-lookup"><span data-stu-id="6706a-173">Open the **M-Files Desktop Settings** window.</span></span> <span data-ttu-id="6706a-174">Kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6706a-174">Then, click **Add**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. <span data-ttu-id="6706a-176">Az a **tárolói kapcsolat dokumentumtulajdonságok** ablak, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6706a-176">On the **Document Vault Connection Properties** window, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    <span data-ttu-id="6706a-178">A kiszolgáló alatt szakasz típusa, az értékek az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6706a-178">Under the Server section type, the values as follows:</span></span>  

    <span data-ttu-id="6706a-179">a.</span><span class="sxs-lookup"><span data-stu-id="6706a-179">a.</span></span> <span data-ttu-id="6706a-180">A **neve**, típus `<tenant-name>.cloudvault.m-files.com`.</span><span class="sxs-lookup"><span data-stu-id="6706a-180">For **Name**, type `<tenant-name>.cloudvault.m-files.com`.</span></span> 
 
    <span data-ttu-id="6706a-181">b.</span><span class="sxs-lookup"><span data-stu-id="6706a-181">b.</span></span> <span data-ttu-id="6706a-182">A **portszám**, típus **4466**.</span><span class="sxs-lookup"><span data-stu-id="6706a-182">For **Port Number**, type **4466**.</span></span> 

    <span data-ttu-id="6706a-183">c.</span><span class="sxs-lookup"><span data-stu-id="6706a-183">c.</span></span> <span data-ttu-id="6706a-184">A **protokoll**, jelölje be **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="6706a-184">For **Protocol**, select **HTTPS**.</span></span> 

    <span data-ttu-id="6706a-185">d.</span><span class="sxs-lookup"><span data-stu-id="6706a-185">d.</span></span> <span data-ttu-id="6706a-186">Az a **hitelesítési** mezőben válassza **adott Windows felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="6706a-186">In the **Authentication** field, select **Specific Windows user**.</span></span> <span data-ttu-id="6706a-187">Ezt követően kéri aláíró lapot.</span><span class="sxs-lookup"><span data-stu-id="6706a-187">Then, you are prompted with a signing page.</span></span> <span data-ttu-id="6706a-188">Helyezze be az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="6706a-188">Insert your Azure AD credentials.</span></span> 

    <span data-ttu-id="6706a-189">e.</span><span class="sxs-lookup"><span data-stu-id="6706a-189">e.</span></span> <span data-ttu-id="6706a-190">Az a **tároló kiszolgálón**, válassza ki a megfelelő tárolót a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="6706a-190">For the **Vault on Server**,  select the corresponding vault on server.</span></span>
 
    <span data-ttu-id="6706a-191">f.</span><span class="sxs-lookup"><span data-stu-id="6706a-191">f.</span></span> <span data-ttu-id="6706a-192">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6706a-192">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="6706a-193">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="6706a-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6706a-194">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="6706a-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6706a-195">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6706a-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6706a-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6706a-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="6706a-197">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6706a-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6706a-199">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6706a-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6706a-200">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6706a-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6706a-202">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6706a-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6706a-204">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6706a-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6706a-206">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6706a-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6706a-208">a.</span><span class="sxs-lookup"><span data-stu-id="6706a-208">a.</span></span> <span data-ttu-id="6706a-209">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6706a-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6706a-210">b.</span><span class="sxs-lookup"><span data-stu-id="6706a-210">b.</span></span> <span data-ttu-id="6706a-211">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6706a-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6706a-212">c.</span><span class="sxs-lookup"><span data-stu-id="6706a-212">c.</span></span> <span data-ttu-id="6706a-213">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6706a-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6706a-214">d.</span><span class="sxs-lookup"><span data-stu-id="6706a-214">d.</span></span> <span data-ttu-id="6706a-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6706a-215">Click **Create**.</span></span>
 
### <a name="creating-a-m-files-test-user"></a><span data-ttu-id="6706a-216">M-fájlok tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6706a-216">Creating a M-Files test user</span></span>

<span data-ttu-id="6706a-217">Ez a szakasz célja Britta Simon nevű M-fájlokban szereplő felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6706a-217">The objective of this section is to create a user called Britta Simon in M-Files.</span></span> <span data-ttu-id="6706a-218">Együttműködve [M-fájlok támogatási csoport](mailto:support@m-files.com) felhasználót is hozzáadhat az M-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6706a-218">Work with  [M-Files support team](mailto:support@m-files.com) to add the users in the M-Files.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6706a-219">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6706a-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6706a-220">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által biztosított hozzáférés M-fájlok használata.</span><span class="sxs-lookup"><span data-stu-id="6706a-220">In this section, you enable Britta Simon to use Azure single sign-on by granting access to M-Files.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6706a-222">**Fájlokhoz hozzárendelni kívánt Britta Simon M-, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6706a-222">**To assign Britta Simon to M-Files, perform the following steps:**</span></span>

1. <span data-ttu-id="6706a-223">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6706a-223">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6706a-225">Az alkalmazások listában válassza ki a **M-fájlok**.</span><span class="sxs-lookup"><span data-stu-id="6706a-225">In the applications list, select **M-Files**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. <span data-ttu-id="6706a-227">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6706a-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6706a-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6706a-229">Click **Add** button.</span></span> <span data-ttu-id="6706a-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6706a-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6706a-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6706a-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6706a-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6706a-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6706a-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6706a-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6706a-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6706a-235">Testing single sign-on</span></span>

<span data-ttu-id="6706a-236">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6706a-236">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="6706a-237">Ha a hozzáférési panelen M-fájlok csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az M-fájlok alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="6706a-237">When you click the M-Files tile in the Access Panel, you should get automatically signed-on to your M-Files application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6706a-238">További források</span><span class="sxs-lookup"><span data-stu-id="6706a-238">Additional resources</span></span>

* [<span data-ttu-id="6706a-239">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6706a-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6706a-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6706a-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

