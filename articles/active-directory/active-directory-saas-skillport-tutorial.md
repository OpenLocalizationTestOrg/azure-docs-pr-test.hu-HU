---
title: "Oktatóanyag: Azure Active Directoryval integrált Skillport |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Skillport között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4df349b2-a73f-4b88-a077-ec0fbfc26527
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 668fc5ae4f964bd776904c3a9dbc2b203689d50c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skillport"></a><span data-ttu-id="7049c-103">Oktatóanyag: Azure Active Directoryval integrált Skillport</span><span class="sxs-lookup"><span data-stu-id="7049c-103">Tutorial: Azure Active Directory integration with Skillport</span></span>

<span data-ttu-id="7049c-104">Ebben az oktatóanyagban elsajátíthatja Skillport integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7049c-104">In this tutorial, you learn how to integrate Skillport with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7049c-105">Skillport integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7049c-105">Integrating Skillport with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7049c-106">Megadhatja a Skillport hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7049c-106">You can control in Azure AD who has access to Skillport</span></span>
- <span data-ttu-id="7049c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Skillport (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7049c-107">You can enable your users to automatically get signed-on to Skillport (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7049c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7049c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7049c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7049c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7049c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7049c-110">Prerequisites</span></span>

<span data-ttu-id="7049c-111">Konfigurálása az Azure AD-integrációs Skillport, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7049c-111">To configure Azure AD integration with Skillport, you need the following items:</span></span>

- <span data-ttu-id="7049c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7049c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7049c-113">Egy Skillport egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="7049c-113">A Skillport single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7049c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7049c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7049c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7049c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7049c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7049c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7049c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7049c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7049c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7049c-118">Scenario description</span></span>
<span data-ttu-id="7049c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7049c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7049c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7049c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7049c-121">A gyűjteményből Skillport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7049c-121">Adding Skillport from the gallery</span></span>
2. <span data-ttu-id="7049c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7049c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skillport-from-the-gallery"></a><span data-ttu-id="7049c-123">A gyűjteményből Skillport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7049c-123">Adding Skillport from the gallery</span></span>
<span data-ttu-id="7049c-124">Az Azure AD integrálása a Skillport konfigurálásához kell hozzáadnia Skillport a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7049c-124">To configure the integration of Skillport into Azure AD, you need to add Skillport from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7049c-125">**A gyűjteményből Skillport hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7049c-125">**To add Skillport from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7049c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7049c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7049c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7049c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7049c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7049c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7049c-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7049c-131">Click **New Application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7049c-133">Írja be a keresőmezőbe, **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="7049c-133">In the search box, type **Skillport**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_search.png)

5. <span data-ttu-id="7049c-135">Az eredmények panelen válassza ki a **Skillport**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7049c-135">In the results panel, select **Skillport**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7049c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7049c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7049c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Skillport.</span><span class="sxs-lookup"><span data-stu-id="7049c-138">In this section, you configure and test Azure AD single sign-on with Skillport based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7049c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Skillport a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="7049c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Skillport is to a user in Azure AD.</span></span> <span data-ttu-id="7049c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Skillport közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7049c-140">In other words, a link relationship between an Azure AD user and the related user in Skillport needs to be established.</span></span>

<span data-ttu-id="7049c-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Skillport a.</span><span class="sxs-lookup"><span data-stu-id="7049c-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Skillport.</span></span>

<span data-ttu-id="7049c-142">Az Azure AD egyszeri bejelentkezést a Skillport tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7049c-142">To configure and test Azure AD single sign-on with Skillport, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7049c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7049c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7049c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7049c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7049c-145">**[Skillport tesztfelhasználó létrehozása](#creating-a-skillport-test-user)**  - való Britta Simon valami Skillport, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7049c-145">**[Creating a Skillport test user](#creating-a-skillport-test-user)** - to have a counterpart of Britta Simon in Skillport that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7049c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7049c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7049c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7049c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7049c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7049c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7049c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Skillport alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7049c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Skillport application.</span></span>

<span data-ttu-id="7049c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Skillport, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7049c-150">**To configure Azure AD single sign-on with Skillport, perform the following steps:**</span></span>

1. <span data-ttu-id="7049c-151">Az Azure portálon a a **Skillport** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7049c-151">In the Azure  portal, on the **Skillport** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7049c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7049c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_samlbase.png)

3. <span data-ttu-id="7049c-155">Az a **Skillport tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7049c-155">On the **Skillport Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_url.png)

    <span data-ttu-id="7049c-157">a.</span><span class="sxs-lookup"><span data-stu-id="7049c-157">a.</span></span> <span data-ttu-id="7049c-158">Az a **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="7049c-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span>
      
      <span data-ttu-id="7049c-159">EU Datacenter:`https://<subdomain>.skillport.eu`</span><span class="sxs-lookup"><span data-stu-id="7049c-159">EU Datacenter: `https://<subdomain>.skillport.eu`</span></span>
   
      <span data-ttu-id="7049c-160">USA Datacenter:`https://<subdomain>.skillport.com`</span><span class="sxs-lookup"><span data-stu-id="7049c-160">US Datacenter: `https://<subdomain>.skillport.com`</span></span>
   
    <span data-ttu-id="7049c-161">b.</span><span class="sxs-lookup"><span data-stu-id="7049c-161">b.</span></span> <span data-ttu-id="7049c-162">Az a **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="7049c-162">In the **Reply URL** textbox, type a URL using the following patterns:</span></span>
    
      <span data-ttu-id="7049c-163">EU Datacenter:`https://<subdomain>.skillport.eu/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="7049c-163">EU Datacenter: `https://<subdomain>.skillport.eu/adfs/ls/`</span></span>
    
      <span data-ttu-id="7049c-164">USA Datacenter:`https://<subdomain>.skillport.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="7049c-164">US Datacenter: `https://<subdomain>.skillport.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7049c-165">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="7049c-165">These values are not the real.</span></span> <span data-ttu-id="7049c-166">Frissítheti ezeket az értékeket a tényleges válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="7049c-166">Update these values with the actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="7049c-167">Ügyfél [Skillport ügyfél-támogatási csoport](https://www.skillsoft.com/contact.asp) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7049c-167">Contact [Skillport Client support team](https://www.skillsoft.com/contact.asp) to get these values.</span></span>
 
4. <span data-ttu-id="7049c-168">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7049c-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_certificate.png) 

5. <span data-ttu-id="7049c-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7049c-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7049c-172">Egyszeri bejelentkezés konfigurálása **Skillport** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Skillport támogatási csoport](https://www.skillsoft.com/contact.asp).</span><span class="sxs-lookup"><span data-stu-id="7049c-172">To configure single sign-on on **Skillport** side, you need to send the downloaded **Metadata XML** to [Skillport support team](https://www.skillsoft.com/contact.asp).</span></span> <span data-ttu-id="7049c-173">Ez lesz állítva, a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="7049c-173">They will set it up to have the SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7049c-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7049c-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="7049c-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7049c-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7049c-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7049c-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7049c-178">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7049c-178">In the **Azure  portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7049c-180">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7049c-180">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7049c-182">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7049c-182">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7049c-184">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7049c-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7049c-186">a.</span><span class="sxs-lookup"><span data-stu-id="7049c-186">a.</span></span> <span data-ttu-id="7049c-187">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7049c-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7049c-188">b.</span><span class="sxs-lookup"><span data-stu-id="7049c-188">b.</span></span> <span data-ttu-id="7049c-189">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7049c-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7049c-190">c.</span><span class="sxs-lookup"><span data-stu-id="7049c-190">c.</span></span> <span data-ttu-id="7049c-191">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7049c-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7049c-192">d.</span><span class="sxs-lookup"><span data-stu-id="7049c-192">d.</span></span> <span data-ttu-id="7049c-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7049c-193">Click **Create**.</span></span>
 
### <a name="creating-a-skillport-test-user"></a><span data-ttu-id="7049c-194">Skillport tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7049c-194">Creating a Skillport test user</span></span>

<span data-ttu-id="7049c-195">Tesztfelhasználó Skillport létrehozásához kapcsolatba kell lépnie [Skillport támogatási csoport](https://www.skillsoft.com/contact.asp) több üzleti forgatókönyvek végfelhasználói követelményeinek megfelelően rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="7049c-195">In order to create Skillport test user, you need to contact [Skillport support team](https://www.skillsoft.com/contact.asp) as they have multiple business scenarios according to the requirement of end user.</span></span> <span data-ttu-id="7049c-196">Ezek konfigurálása a felhasználó egyeztetését követően.</span><span class="sxs-lookup"><span data-stu-id="7049c-196">They will configure it after discussion with the users.</span></span>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7049c-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7049c-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7049c-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Skillport Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="7049c-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Skillport.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7049c-200">**Britta Simon hozzárendelése Skillport, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7049c-200">**To assign Britta Simon to Skillport, perform the following steps:**</span></span>

1. <span data-ttu-id="7049c-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7049c-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7049c-203">Az alkalmazások listában válassza ki a **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="7049c-203">In the applications list, select **Skillport**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_app.png) 

3. <span data-ttu-id="7049c-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7049c-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7049c-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7049c-207">Click **Add** button.</span></span> <span data-ttu-id="7049c-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7049c-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7049c-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7049c-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7049c-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7049c-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7049c-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7049c-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7049c-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7049c-213">Testing single sign-on</span></span>

<span data-ttu-id="7049c-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7049c-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7049c-215">Ha a hozzáférési panelen Skillport csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Skillport alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="7049c-215">When you click the Skillport tile in the Access Panel, you should get automatically signed-on to your Skillport application.</span></span>
<span data-ttu-id="7049c-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="7049c-216">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7049c-217">További források</span><span class="sxs-lookup"><span data-stu-id="7049c-217">Additional resources</span></span>

* [<span data-ttu-id="7049c-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7049c-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7049c-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7049c-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_203.png

