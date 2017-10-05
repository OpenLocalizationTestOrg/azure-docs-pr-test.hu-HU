---
title: "Oktatóanyag: Azure Active Directoryval integrált Lecorpio |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Lecorpio között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 35c94e2d9d8a938971f85ea732a74a7e1655545e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a><span data-ttu-id="364b2-103">Oktatóanyag: Azure Active Directoryval integrált Lecorpio</span><span class="sxs-lookup"><span data-stu-id="364b2-103">Tutorial: Azure Active Directory integration with Lecorpio</span></span>

<span data-ttu-id="364b2-104">Ebben az oktatóanyagban elsajátíthatja Lecorpio integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="364b2-104">In this tutorial, you learn how to integrate Lecorpio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="364b2-105">Lecorpio integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="364b2-105">Integrating Lecorpio with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="364b2-106">Megadhatja a Lecorpio hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="364b2-106">You can control in Azure AD who has access to Lecorpio</span></span>
- <span data-ttu-id="364b2-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Lecorpio (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="364b2-107">You can enable your users to automatically get signed-on to Lecorpio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="364b2-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="364b2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="364b2-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="364b2-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="364b2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="364b2-110">Prerequisites</span></span>

<span data-ttu-id="364b2-111">Konfigurálása az Azure AD-integrációs Lecorpio, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="364b2-111">To configure Azure AD integration with Lecorpio, you need the following items:</span></span>

- <span data-ttu-id="364b2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="364b2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="364b2-113">Egy Lecorpio egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="364b2-113">A Lecorpio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="364b2-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="364b2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="364b2-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="364b2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="364b2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="364b2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="364b2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="364b2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="364b2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="364b2-118">Scenario description</span></span>
<span data-ttu-id="364b2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="364b2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="364b2-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="364b2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="364b2-121">A gyűjteményből Lecorpio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="364b2-121">Adding Lecorpio from the gallery</span></span>
2. <span data-ttu-id="364b2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="364b2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lecorpio-from-the-gallery"></a><span data-ttu-id="364b2-123">A gyűjteményből Lecorpio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="364b2-123">Adding Lecorpio from the gallery</span></span>
<span data-ttu-id="364b2-124">Az Azure AD integrálása a Lecorpio konfigurálásához kell hozzáadnia Lecorpio a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="364b2-124">To configure the integration of Lecorpio into Azure AD, you need to add Lecorpio from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="364b2-125">**A gyűjteményből Lecorpio hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="364b2-125">**To add Lecorpio from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="364b2-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="364b2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="364b2-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="364b2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="364b2-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="364b2-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="364b2-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="364b2-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="364b2-133">Írja be a keresőmezőbe, **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="364b2-133">In the search box, type **Lecorpio**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. <span data-ttu-id="364b2-135">Az eredmények panelen válassza ki a **Lecorpio**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="364b2-135">In the results panel, select **Lecorpio**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="364b2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="364b2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="364b2-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Lecorpio</span><span class="sxs-lookup"><span data-stu-id="364b2-138">In this section, you configure and test Azure AD single sign-on with Lecorpio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="364b2-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Lecorpio a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="364b2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lecorpio is to a user in Azure AD.</span></span> <span data-ttu-id="364b2-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Lecorpio közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="364b2-140">In other words, a link relationship between an Azure AD user and the related user in Lecorpio needs to be established.</span></span>

<span data-ttu-id="364b2-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Lecorpio a.</span><span class="sxs-lookup"><span data-stu-id="364b2-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Lecorpio.</span></span>

<span data-ttu-id="364b2-142">Az Azure AD egyszeri bejelentkezést a Lecorpio tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="364b2-142">To configure and test Azure AD single sign-on with Lecorpio, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="364b2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="364b2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="364b2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="364b2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="364b2-145">**[Lecorpio tesztfelhasználó létrehozása](#creating-a-lecorpio-test-user)**  - való Britta Simon valami Lecorpio, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="364b2-145">**[Creating a Lecorpio test user](#creating-a-lecorpio-test-user)** - to have a counterpart of Britta Simon in Lecorpio that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="364b2-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="364b2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="364b2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="364b2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="364b2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="364b2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="364b2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Lecorpio alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="364b2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lecorpio application.</span></span>

<span data-ttu-id="364b2-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Lecorpio, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="364b2-150">**To configure Azure AD single sign-on with Lecorpio, perform the following steps:**</span></span>

1. <span data-ttu-id="364b2-151">Az Azure portálon a a **Lecorpio** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="364b2-151">In the Azure portal, on the **Lecorpio** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="364b2-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="364b2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. <span data-ttu-id="364b2-155">Az a **Lecorpio tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="364b2-155">On the **Lecorpio Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    <span data-ttu-id="364b2-157">a.</span><span class="sxs-lookup"><span data-stu-id="364b2-157">a.</span></span> <span data-ttu-id="364b2-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="364b2-158">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    <span data-ttu-id="364b2-159">b.</span><span class="sxs-lookup"><span data-stu-id="364b2-159">b.</span></span> <span data-ttu-id="364b2-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="364b2-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="364b2-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="364b2-161">These values are not the real.</span></span> <span data-ttu-id="364b2-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="364b2-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="364b2-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="364b2-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="364b2-164">Ügyfél [Lecorpio ügyfél-támogatási csoport](mailto:info@lecorpio.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="364b2-164">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) to get these values.</span></span> 
 
4. <span data-ttu-id="364b2-165">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="364b2-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. <span data-ttu-id="364b2-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="364b2-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="364b2-169">Egyszeri bejelentkezés konfigurálása **Lecorpio** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Lecorpio támogatási csoport](mailto:info@lecorpio.com).</span><span class="sxs-lookup"><span data-stu-id="364b2-169">To configure single sign-on on **Lecorpio** side, you need to send the downloaded **Metadata XML** to [Lecorpio support team](mailto:info@lecorpio.com).</span></span>

> [!TIP]
> <span data-ttu-id="364b2-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="364b2-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="364b2-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="364b2-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="364b2-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="364b2-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="364b2-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="364b2-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="364b2-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="364b2-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="364b2-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="364b2-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="364b2-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="364b2-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="364b2-179">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="364b2-179">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="364b2-181">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="364b2-181">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="364b2-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="364b2-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="364b2-185">a.</span><span class="sxs-lookup"><span data-stu-id="364b2-185">a.</span></span> <span data-ttu-id="364b2-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="364b2-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="364b2-187">b.</span><span class="sxs-lookup"><span data-stu-id="364b2-187">b.</span></span> <span data-ttu-id="364b2-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="364b2-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="364b2-189">c.</span><span class="sxs-lookup"><span data-stu-id="364b2-189">c.</span></span> <span data-ttu-id="364b2-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="364b2-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="364b2-191">d.</span><span class="sxs-lookup"><span data-stu-id="364b2-191">d.</span></span> <span data-ttu-id="364b2-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="364b2-192">Click **Create**.</span></span>
 
### <a name="creating-a-lecorpio-test-user"></a><span data-ttu-id="364b2-193">Lecorpio tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="364b2-193">Creating a Lecorpio test user</span></span>

<span data-ttu-id="364b2-194">Ebben a szakaszban egy Lecorpio Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="364b2-194">In this section, you create a user called Britta Simon in Lecorpio.</span></span> 

<span data-ttu-id="364b2-195">Ügyfél [Lecorpio ügyfél-támogatási csoport](mailto:info@lecorpio.com) a felhasználók hozzáadása az Lecorpio alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="364b2-195">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) to add the users in the Lecorpio application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="364b2-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="364b2-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="364b2-197">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Lecorpio Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="364b2-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lecorpio.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="364b2-199">**Britta Simon hozzárendelése Lecorpio, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="364b2-199">**To assign Britta Simon to Lecorpio, perform the following steps:**</span></span>

1. <span data-ttu-id="364b2-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="364b2-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="364b2-202">Az alkalmazások listában válassza ki a **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="364b2-202">In the applications list, select **Lecorpio**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. <span data-ttu-id="364b2-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="364b2-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="364b2-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="364b2-206">Click **Add** button.</span></span> <span data-ttu-id="364b2-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="364b2-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="364b2-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="364b2-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="364b2-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="364b2-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="364b2-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="364b2-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="364b2-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="364b2-212">Testing single sign-on</span></span>

<span data-ttu-id="364b2-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="364b2-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="364b2-214">Ha a hozzáférési panelen Lecorpio csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Lecorpio alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="364b2-214">When you click the Lecorpio tile in the Access Panel, you should get automatically signed-on to your Lecorpio application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="364b2-215">További források</span><span class="sxs-lookup"><span data-stu-id="364b2-215">Additional resources</span></span>

* [<span data-ttu-id="364b2-216">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="364b2-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="364b2-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="364b2-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

