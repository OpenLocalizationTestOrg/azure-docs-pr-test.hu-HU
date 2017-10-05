---
title: "Oktatóanyag: Azure Active Directoryval integrált MCM |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és MCM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f00799d-e3e9-4ba9-ae4a-fbca843ac5db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: cbbb0f54b7954c0ec7326fb62bb427155527cc84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mcm"></a><span data-ttu-id="d5166-103">Oktatóanyag: Azure Active Directoryval integrált MCM</span><span class="sxs-lookup"><span data-stu-id="d5166-103">Tutorial: Azure Active Directory integration with MCM</span></span>

<span data-ttu-id="d5166-104">Ebben az oktatóanyagban elsajátíthatja MCM integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d5166-104">In this tutorial, you learn how to integrate MCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d5166-105">MCM integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d5166-105">Integrating MCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d5166-106">Megadhatja a MCM hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d5166-106">You can control in Azure AD who has access to MCM</span></span>
- <span data-ttu-id="d5166-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett MCM (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d5166-107">You can enable your users to automatically get signed-on to MCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d5166-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d5166-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d5166-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d5166-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5166-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d5166-110">Prerequisites</span></span>

<span data-ttu-id="d5166-111">Konfigurálása az Azure AD-integrációs MCM, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d5166-111">To configure Azure AD integration with MCM, you need the following items:</span></span>

- <span data-ttu-id="d5166-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d5166-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d5166-113">Egy MCM egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d5166-113">A MCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d5166-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d5166-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d5166-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d5166-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d5166-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d5166-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d5166-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5166-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d5166-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d5166-118">Scenario description</span></span>
<span data-ttu-id="d5166-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d5166-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d5166-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d5166-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d5166-121">A gyűjteményből MCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d5166-121">Adding MCM from the gallery</span></span>
2. <span data-ttu-id="d5166-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d5166-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mcm-from-the-gallery"></a><span data-ttu-id="d5166-123">A gyűjteményből MCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d5166-123">Adding MCM from the gallery</span></span>
<span data-ttu-id="d5166-124">Az Azure AD integrálása a MCM konfigurálásához kell hozzáadnia MCM a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d5166-124">To configure the integration of MCM into Azure AD, you need to add MCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d5166-125">**A gyűjteményből MCM hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5166-125">**To add MCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d5166-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d5166-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d5166-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d5166-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d5166-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d5166-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d5166-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d5166-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d5166-133">Írja be a keresőmezőbe, **MCM**.</span><span class="sxs-lookup"><span data-stu-id="d5166-133">In the search box, type **MCM**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_search.png)

5. <span data-ttu-id="d5166-135">Az eredmények panelen válassza ki a **MCM**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d5166-135">In the results panel, select **MCM**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d5166-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d5166-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d5166-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján MCM.</span><span class="sxs-lookup"><span data-stu-id="d5166-138">In this section, you configure and test Azure AD single sign-on with MCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d5166-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó MCM a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d5166-139">For single sign-on to work, Azure AD needs to know what the counterpart user in MCM is to a user in Azure AD.</span></span> <span data-ttu-id="d5166-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a MCM közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d5166-140">In other words, a link relationship between an Azure AD user and the related user in MCM needs to be established.</span></span>

<span data-ttu-id="d5166-141">MCM, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d5166-141">In MCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d5166-142">Az Azure AD egyszeri bejelentkezést a MCM tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d5166-142">To configure and test Azure AD single sign-on with MCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d5166-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d5166-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d5166-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d5166-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d5166-145">**[Tesztfelhasználó létrehozása egy MCM](#creating-a-mcm-test-user)**  - való Britta Simon valami MCM, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d5166-145">**[Creating a MCM test user](#creating-a-mcm-test-user)** - to have a counterpart of Britta Simon in MCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d5166-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d5166-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d5166-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d5166-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d5166-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5166-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d5166-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az MCM alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d5166-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MCM application.</span></span>

<span data-ttu-id="d5166-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés MCM, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5166-150">**To configure Azure AD single sign-on with MCM, perform the following steps:**</span></span>

1. <span data-ttu-id="d5166-151">Az Azure portálon a a **MCM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d5166-151">In the Azure portal, on the **MCM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d5166-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d5166-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_samlbase.png)

3. <span data-ttu-id="d5166-155">Az a **MCM tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d5166-155">On the **MCM Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_url.png)

    <span data-ttu-id="d5166-157">a.</span><span class="sxs-lookup"><span data-stu-id="d5166-157">a.</span></span> <span data-ttu-id="d5166-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://myaba.co.uk/client-access/<companyname>/saml.php`</span><span class="sxs-lookup"><span data-stu-id="d5166-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://myaba.co.uk/client-access/<companyname>/saml.php`</span></span>

    <span data-ttu-id="d5166-159">b.</span><span class="sxs-lookup"><span data-stu-id="d5166-159">b.</span></span> <span data-ttu-id="d5166-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://myaba.co.uk/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="d5166-160">In the **Identifier** textbox, type a URL using the following pattern: `https://myaba.co.uk/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d5166-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d5166-161">These values are not real.</span></span> <span data-ttu-id="d5166-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d5166-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d5166-163">Ügyfél [MCM ügyfél-támogatási csoport](http://mcmtechnology.com/support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d5166-163">Contact [MCM Client support team](http://mcmtechnology.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="d5166-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d5166-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_certificate.png) 

5. <span data-ttu-id="d5166-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5166-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mcm-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="d5166-168">Egyszeri bejelentkezés konfigurálása **MCM** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [MCM támogatási csoport](http://mcmtechnology.com/support/).</span><span class="sxs-lookup"><span data-stu-id="d5166-168">To configure single sign-on on **MCM** side, you need to send the downloaded **Metadata XML** to [MCM support team](http://mcmtechnology.com/support/).</span></span> <span data-ttu-id="d5166-169">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d5166-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d5166-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d5166-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d5166-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d5166-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d5166-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d5166-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d5166-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5166-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="d5166-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d5166-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d5166-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5166-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d5166-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d5166-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d5166-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d5166-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d5166-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d5166-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d5166-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d5166-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d5166-185">a.</span><span class="sxs-lookup"><span data-stu-id="d5166-185">a.</span></span> <span data-ttu-id="d5166-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d5166-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d5166-187">b.</span><span class="sxs-lookup"><span data-stu-id="d5166-187">b.</span></span> <span data-ttu-id="d5166-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d5166-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d5166-189">c.</span><span class="sxs-lookup"><span data-stu-id="d5166-189">c.</span></span> <span data-ttu-id="d5166-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d5166-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d5166-191">d.</span><span class="sxs-lookup"><span data-stu-id="d5166-191">d.</span></span> <span data-ttu-id="d5166-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d5166-192">Click **Create**.</span></span>
 
### <a name="creating-a-mcm-test-user"></a><span data-ttu-id="d5166-193">MCM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5166-193">Creating a MCM test user</span></span>

<span data-ttu-id="d5166-194">Ebben a szakaszban egy MCM Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d5166-194">In this section, you create a user called Britta Simon in MCM.</span></span> <span data-ttu-id="d5166-195">Együttműködve [MCM támogatási csoport](http://mcmtechnology.com/support/) a felhasználók hozzáadása a MCM platform.</span><span class="sxs-lookup"><span data-stu-id="d5166-195">Work with [MCM support team](http://mcmtechnology.com/support/) to add the users in the MCM platform.</span></span>

> [!NOTE]
> <span data-ttu-id="d5166-196">Bármely más MCM felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz MCM által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="d5166-196">You can use any other MCM user account creation tools or APIs provided by MCM to provision AAD user accounts.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d5166-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d5166-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d5166-198">Ebben a szakaszban Britta Simon MCM biztosított hozzáférés szerint használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d5166-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MCM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d5166-200">**Britta Simon hozzárendelése MCM, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5166-200">**To assign Britta Simon to MCM, perform the following steps:**</span></span>

1. <span data-ttu-id="d5166-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d5166-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d5166-203">Az alkalmazások listában válassza ki a **MCM**.</span><span class="sxs-lookup"><span data-stu-id="d5166-203">In the applications list, select **MCM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_app.png) 

3. <span data-ttu-id="d5166-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d5166-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d5166-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5166-207">Click **Add** button.</span></span> <span data-ttu-id="d5166-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5166-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d5166-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d5166-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d5166-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5166-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d5166-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5166-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d5166-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d5166-213">Testing single sign-on</span></span>

<span data-ttu-id="d5166-214">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="d5166-214">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d5166-215">Ha a hozzáférési panelen MCM csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az MCM alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d5166-215">When you click the MCM tile in the Access Panel, you should get automatically signed-on to your MCM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5166-216">További források</span><span class="sxs-lookup"><span data-stu-id="d5166-216">Additional resources</span></span>

* [<span data-ttu-id="d5166-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d5166-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d5166-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d5166-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_203.png

