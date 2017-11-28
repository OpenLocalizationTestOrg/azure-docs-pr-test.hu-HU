---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező személyek |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és személyek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: caa287a2ed8774965ef722685e4e950336e5e0ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-people"></a><span data-ttu-id="6eb01-103">Oktatóanyag: Azure Active Directory-integrációval rendelkező személyek</span><span class="sxs-lookup"><span data-stu-id="6eb01-103">Tutorial: Azure Active Directory integration with People</span></span>

<span data-ttu-id="6eb01-104">Ebben az oktatóanyagban elsajátíthatja személyek integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6eb01-104">In this tutorial, you learn how to integrate People with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6eb01-105">Személyek integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6eb01-105">Integrating People with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6eb01-106">Megadhatja a felhasználók hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6eb01-106">You can control in Azure AD who has access to People</span></span>
- <span data-ttu-id="6eb01-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6eb01-107">You can enable your users to automatically get signed-on to People (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6eb01-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6eb01-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6eb01-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6eb01-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6eb01-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6eb01-110">Prerequisites</span></span>

<span data-ttu-id="6eb01-111">Az Azure AD-integráció konfigurálása személyekkel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="6eb01-111">To configure Azure AD integration with People, you need the following items:</span></span>

- <span data-ttu-id="6eb01-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6eb01-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6eb01-113">A felhasználók az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6eb01-113">A People single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6eb01-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6eb01-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6eb01-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6eb01-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6eb01-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eb01-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6eb01-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6eb01-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6eb01-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6eb01-118">Scenario description</span></span>
<span data-ttu-id="6eb01-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6eb01-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6eb01-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6eb01-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6eb01-121">Felhasználók hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6eb01-121">Adding People from the gallery</span></span>
2. <span data-ttu-id="6eb01-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6eb01-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-people-from-the-gallery"></a><span data-ttu-id="6eb01-123">Felhasználók hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6eb01-123">Adding People from the gallery</span></span>
<span data-ttu-id="6eb01-124">Az Azure AD integrálása a személyek konfigurálásához kell hozzáadnia személyek a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="6eb01-124">To configure the integration of People into Azure AD, you need to add People from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6eb01-125">**Felhasználók hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6eb01-125">**To add People from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6eb01-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6eb01-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6eb01-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6eb01-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6eb01-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="6eb01-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6eb01-133">Írja be a keresőmezőbe, **személyek**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-133">In the search box, type **People**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/tutorial_people_search.png)

5. <span data-ttu-id="6eb01-135">Az eredmények panelen válassza ki a **személyek**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6eb01-135">In the results panel, select **People**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/tutorial_people_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6eb01-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6eb01-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6eb01-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="6eb01-138">In this section, you configure and test Azure AD single sign-on with People based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6eb01-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó személyek a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6eb01-139">For single sign-on to work, Azure AD needs to know what the counterpart user in People is to a user in Azure AD.</span></span> <span data-ttu-id="6eb01-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a személyek közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6eb01-140">In other words, a link relationship between an Azure AD user and the related user in People needs to be established.</span></span>

<span data-ttu-id="6eb01-141">A személyeket, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6eb01-141">In People, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6eb01-142">Az Azure AD az egyszeri bejelentkezés személyekkel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6eb01-142">To configure and test Azure AD single sign-on with People, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6eb01-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6eb01-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6eb01-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6eb01-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6eb01-145">**[Személyek tesztfelhasználó létrehozása](#creating-a-people-test-user)**  - való egy megfelelője a Britta Simon személyek, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="6eb01-145">**[Creating a People test user](#creating-a-people-test-user)** - to have a counterpart of Britta Simon in People that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6eb01-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6eb01-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6eb01-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6eb01-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6eb01-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6eb01-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6eb01-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és személyek alkalmazásában egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6eb01-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your People application.</span></span>

<span data-ttu-id="6eb01-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés személyekkel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6eb01-150">**To configure Azure AD single sign-on with People, perform the following steps:**</span></span>

1. <span data-ttu-id="6eb01-151">Az Azure portálon a a **személyek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-151">In the Azure portal, on the **People** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6eb01-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6eb01-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_samlbase.png)

3. <span data-ttu-id="6eb01-155">Az a **személyek tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6eb01-155">On the **People Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_url.png)

    <span data-ttu-id="6eb01-157">a.</span><span class="sxs-lookup"><span data-stu-id="6eb01-157">a.</span></span> <span data-ttu-id="6eb01-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.peoplehr.com/`</span><span class="sxs-lookup"><span data-stu-id="6eb01-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.peoplehr.com/`</span></span>

    <span data-ttu-id="6eb01-159">b.</span><span class="sxs-lookup"><span data-stu-id="6eb01-159">b.</span></span> <span data-ttu-id="6eb01-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.peoplehr.com`</span><span class="sxs-lookup"><span data-stu-id="6eb01-160">In the **Identifier** textbox, type a URL using the following pattern: `https://www.peoplehr.com`</span></span>

    <span data-ttu-id="6eb01-161">c.</span><span class="sxs-lookup"><span data-stu-id="6eb01-161">c.</span></span> <span data-ttu-id="6eb01-162">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span><span class="sxs-lookup"><span data-stu-id="6eb01-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6eb01-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6eb01-163">These values are not real.</span></span> <span data-ttu-id="6eb01-164">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="6eb01-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="6eb01-165">Ügyfél [személyek ügyfél-támogatási csoport](mailto:customerservices@peoplehr.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6eb01-165">Contact [People Client support team](mailto:customerservices@peoplehr.com) to get these values.</span></span>

5. <span data-ttu-id="6eb01-166">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6eb01-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_certificate.png) 

6. <span data-ttu-id="6eb01-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6eb01-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="6eb01-170">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, meg kell bejelentkezés a személyek bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6eb01-170">To get SSO configured for your application, you need to sign-on to your People tenant as an administrator.</span></span>
   
8. <span data-ttu-id="6eb01-171">A bal oldali menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-171">In the menu on the left side, click **Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_001.png)

9. <span data-ttu-id="6eb01-173">Kattintson a **vállalati**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-173">Click **Company**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_002.png)

10. <span data-ttu-id="6eb01-175">Az a **feltöltés "Egyszeri bejelentkezés" SAML metaadatok fájl**, kattintson a **Tallózás** feltölteni a fájlt a letöltött metaadat.</span><span class="sxs-lookup"><span data-stu-id="6eb01-175">On the **Upload 'Single Sign On' SAML meta-data file**, click **Browse** to upload the downloaded metadata file.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)

> [!TIP]
> <span data-ttu-id="6eb01-177">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="6eb01-177">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6eb01-178">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="6eb01-178">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6eb01-179">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6eb01-179">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6eb01-180">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb01-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="6eb01-181">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6eb01-181">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6eb01-183">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6eb01-183">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6eb01-184">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6eb01-184">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6eb01-186">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-186">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6eb01-188">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6eb01-188">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6eb01-190">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6eb01-190">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6eb01-192">a.</span><span class="sxs-lookup"><span data-stu-id="6eb01-192">a.</span></span> <span data-ttu-id="6eb01-193">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-193">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6eb01-194">b.</span><span class="sxs-lookup"><span data-stu-id="6eb01-194">b.</span></span> <span data-ttu-id="6eb01-195">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6eb01-195">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6eb01-196">c.</span><span class="sxs-lookup"><span data-stu-id="6eb01-196">c.</span></span> <span data-ttu-id="6eb01-197">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-197">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6eb01-198">d.</span><span class="sxs-lookup"><span data-stu-id="6eb01-198">d.</span></span> <span data-ttu-id="6eb01-199">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6eb01-199">Click **Create**.</span></span>
 
### <a name="creating-a-people-test-user"></a><span data-ttu-id="6eb01-200">Személyek tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb01-200">Creating a People test user</span></span>

<span data-ttu-id="6eb01-201">Ebben a szakaszban egy Britta Simon személyek nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6eb01-201">In this section, you create a user called Britta Simon in People.</span></span> <span data-ttu-id="6eb01-202">Együttműködve [személyek ügyfél-támogatási csoport](mailto:customerservices@peoplehr.com) a felhasználók hozzáadása a személyek platform.</span><span class="sxs-lookup"><span data-stu-id="6eb01-202">Work with [People Client support team](mailto:customerservices@peoplehr.com) to add the users in the People platform.</span></span> <span data-ttu-id="6eb01-203">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="6eb01-203">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6eb01-204">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6eb01-204">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6eb01-205">Ebben a szakaszban Britta Simon hozzáférést biztosít a felhasználók által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6eb01-205">In this section, you enable Britta Simon to use Azure single sign-on by granting access to People.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6eb01-207">**Britta Simon hozzárendelése személyek, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6eb01-207">**To assign Britta Simon to People, perform the following steps:**</span></span>

1. <span data-ttu-id="6eb01-208">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-208">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6eb01-210">Az alkalmazások listában válassza ki a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-210">In the applications list, select **People**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-people-tutorial/tutorial_people_app.png) 

3. <span data-ttu-id="6eb01-212">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6eb01-212">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6eb01-214">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6eb01-214">Click **Add** button.</span></span> <span data-ttu-id="6eb01-215">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6eb01-215">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6eb01-217">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6eb01-217">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6eb01-218">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6eb01-218">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6eb01-219">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6eb01-219">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6eb01-220">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6eb01-220">Testing single sign-on</span></span>

<span data-ttu-id="6eb01-221">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6eb01-221">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="6eb01-222">Ha a hozzáférési panelen személyek csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az emberek alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6eb01-222">When you click the People tile in the Access Panel, you should get automatically signed-on to your People application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6eb01-223">További források</span><span class="sxs-lookup"><span data-stu-id="6eb01-223">Additional resources</span></span>

* [<span data-ttu-id="6eb01-224">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6eb01-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6eb01-225">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6eb01-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-people-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png

