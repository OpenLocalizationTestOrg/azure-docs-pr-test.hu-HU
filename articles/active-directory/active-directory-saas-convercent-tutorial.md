---
title: "Oktatóanyag: Azure Active Directoryval integrált Convercent |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Convercent között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 7af33cae7448a212734d327570b5082d0278fa0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a><span data-ttu-id="50308-103">Oktatóanyag: Azure Active Directoryval integrált Convercent</span><span class="sxs-lookup"><span data-stu-id="50308-103">Tutorial: Azure Active Directory integration with Convercent</span></span>

<span data-ttu-id="50308-104">Ebben az oktatóanyagban elsajátíthatja Convercent integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="50308-104">In this tutorial, you learn how to integrate Convercent with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="50308-105">Convercent integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="50308-105">Integrating Convercent with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="50308-106">Megadhatja a Convercent hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="50308-106">You can control in Azure AD who has access to Convercent</span></span>
- <span data-ttu-id="50308-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Convercent (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="50308-107">You can enable your users to automatically get signed-on to Convercent (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="50308-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="50308-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="50308-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="50308-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50308-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="50308-110">Prerequisites</span></span>

<span data-ttu-id="50308-111">Konfigurálása az Azure AD-integrációs Convercent, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="50308-111">To configure Azure AD integration with Convercent, you need the following items:</span></span>

- <span data-ttu-id="50308-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="50308-112">An Azure AD subscription</span></span>
- <span data-ttu-id="50308-113">Egy Convercent egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="50308-113">A Convercent single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="50308-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="50308-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="50308-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="50308-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="50308-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="50308-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="50308-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="50308-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="50308-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="50308-118">Scenario description</span></span>
<span data-ttu-id="50308-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="50308-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="50308-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="50308-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="50308-121">A gyűjteményből Convercent hozzáadása</span><span class="sxs-lookup"><span data-stu-id="50308-121">Adding Convercent from the gallery</span></span>
2. <span data-ttu-id="50308-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="50308-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-convercent-from-the-gallery"></a><span data-ttu-id="50308-123">A gyűjteményből Convercent hozzáadása</span><span class="sxs-lookup"><span data-stu-id="50308-123">Adding Convercent from the gallery</span></span>
<span data-ttu-id="50308-124">Az Azure AD integrálása a Convercent konfigurálásához kell hozzáadnia Convercent a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="50308-124">To configure the integration of Convercent into Azure AD, you need to add Convercent from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="50308-125">**A gyűjteményből Convercent hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="50308-125">**To add Convercent from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="50308-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="50308-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="50308-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="50308-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="50308-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="50308-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="50308-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="50308-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="50308-133">Írja be a keresőmezőbe, **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="50308-133">In the search box, type **Convercent**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_search.png)

5. <span data-ttu-id="50308-135">Az eredmények panelen válassza ki a **Convercent**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="50308-135">In the results panel, select **Convercent**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="50308-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="50308-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="50308-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Convercent</span><span class="sxs-lookup"><span data-stu-id="50308-138">In this section, you configure and test Azure AD single sign-on with Convercent based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="50308-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Convercent a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="50308-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Convercent is to a user in Azure AD.</span></span> <span data-ttu-id="50308-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Convercent közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="50308-140">In other words, a link relationship between an Azure AD user and the related user in Convercent needs to be established.</span></span>

<span data-ttu-id="50308-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Convercent a.</span><span class="sxs-lookup"><span data-stu-id="50308-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Convercent.</span></span>

<span data-ttu-id="50308-142">Az Azure AD egyszeri bejelentkezést a Convercent tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="50308-142">To configure and test Azure AD single sign-on with Convercent, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="50308-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="50308-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="50308-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="50308-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="50308-145">**[Convercent tesztfelhasználó létrehozása](#creating-a-convercent-test-user)**  - való Britta Simon valami Convercent, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="50308-145">**[Creating a Convercent test user](#creating-a-convercent-test-user)** - to have a counterpart of Britta Simon in Convercent that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="50308-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="50308-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="50308-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="50308-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="50308-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="50308-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="50308-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Convercent alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="50308-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Convercent application.</span></span>

<span data-ttu-id="50308-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Convercent, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="50308-150">**To configure Azure AD single sign-on with Convercent, perform the following steps:**</span></span>

1. <span data-ttu-id="50308-151">Az Azure portálon a a **Convercent** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="50308-151">In the Azure portal, on the **Convercent** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="50308-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="50308-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_samlbase.png)

3. <span data-ttu-id="50308-155">Az a **Convercent tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="50308-155">On the **Convercent Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url.png)

    <span data-ttu-id="50308-157">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="50308-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.convercent.com/`</span></span>
 
4. <span data-ttu-id="50308-158">Ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, az a **Convercent tartomány és az URL-címek** szakasz a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="50308-158">If you wish to configure the application in **SP initiated mode**, on the **Convercent Domain and URLs** section perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url1.png)

     <span data-ttu-id="50308-160">a.</span><span class="sxs-lookup"><span data-stu-id="50308-160">a.</span></span> <span data-ttu-id="50308-161">Kattintson a **"Megjelenítése speciális URL-beállításainak."**</span><span class="sxs-lookup"><span data-stu-id="50308-161">Click **"Show advanced URL settings."**</span></span> 

     <span data-ttu-id="50308-162">b.</span><span class="sxs-lookup"><span data-stu-id="50308-162">b.</span></span> <span data-ttu-id="50308-163">Az a **URL-cím bejelentkezési** szövegmező, írja be az értéket a következő minta használatával:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="50308-163">In the **Sign On URL** textbox, type the value using the following pattern: `https://<instancename>.convercent.com/`</span></span>

     <span data-ttu-id="50308-164">c.</span><span class="sxs-lookup"><span data-stu-id="50308-164">c.</span></span> <span data-ttu-id="50308-165">Az a **továbbítási állapotot** szövegmező, írja be az értéket a következő minta használatával:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="50308-165">In the **Relay State** textbox, type the value using the following pattern: `https://<instancename>.convercent.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="50308-166">Ezek az értékek nem a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="50308-166">These values are not the real values.</span></span> <span data-ttu-id="50308-167">Frissítheti ezeket az értékeket a tényleges azonosítója, URL-cím bejelentkezési és a továbbítási állapotot.</span><span class="sxs-lookup"><span data-stu-id="50308-167">Update these values with the actual Identifier, Sign On URL and Relay State.</span></span> <span data-ttu-id="50308-168">Ügyfél [Convercent ügyfél-támogatási csoport](http://support.convercent.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="50308-168">Contact [Convercent Client support team](http://support.convercent.com) to get these values.</span></span>

5. <span data-ttu-id="50308-169">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="50308-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_certificate.png) 

6. <span data-ttu-id="50308-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="50308-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-convercent-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="50308-173">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [Convercent támogatási csoport](mailto:support@convercent.com) és adja meg a letöltött **metaadatainak XML-kódja**.</span><span class="sxs-lookup"><span data-stu-id="50308-173">To get SSO configured for your application, contact [Convercent support team](mailto:support@convercent.com) and provide them with the downloaded **Metadata XML**.</span></span>

> [!TIP]
> <span data-ttu-id="50308-174">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="50308-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="50308-175">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="50308-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="50308-176">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="50308-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="50308-177">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="50308-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="50308-178">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="50308-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="50308-180">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="50308-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="50308-181">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="50308-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-convercent-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="50308-183">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="50308-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-convercent-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="50308-185">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="50308-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-convercent-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="50308-187">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="50308-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-convercent-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="50308-189">a.</span><span class="sxs-lookup"><span data-stu-id="50308-189">a.</span></span> <span data-ttu-id="50308-190">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="50308-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="50308-191">b.</span><span class="sxs-lookup"><span data-stu-id="50308-191">b.</span></span> <span data-ttu-id="50308-192">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="50308-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="50308-193">c.</span><span class="sxs-lookup"><span data-stu-id="50308-193">c.</span></span> <span data-ttu-id="50308-194">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="50308-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="50308-195">d.</span><span class="sxs-lookup"><span data-stu-id="50308-195">d.</span></span> <span data-ttu-id="50308-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="50308-196">Click **Create**.</span></span>
 
### <a name="creating-a-convercent-test-user"></a><span data-ttu-id="50308-197">Convercent tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="50308-197">Creating a Convercent test user</span></span>

<span data-ttu-id="50308-198">Együttműködve [Convercent támogatási csoport](mailto:support@convercent.com) a felhasználók hozzáadása a Convercent platform.</span><span class="sxs-lookup"><span data-stu-id="50308-198">Work with [Convercent support team](mailto:support@convercent.com) to add the users in the Convercent platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="50308-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="50308-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="50308-200">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Convercent Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="50308-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Convercent.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="50308-202">**Britta Simon hozzárendelése Convercent, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="50308-202">**To assign Britta Simon to Convercent, perform the following steps:**</span></span>

1. <span data-ttu-id="50308-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="50308-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="50308-205">Az alkalmazások listában válassza ki a **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="50308-205">In the applications list, select **Convercent**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_app.png) 

3. <span data-ttu-id="50308-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="50308-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="50308-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="50308-209">Click **Add** button.</span></span> <span data-ttu-id="50308-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="50308-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="50308-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="50308-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="50308-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="50308-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="50308-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="50308-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="50308-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="50308-215">Testing single sign-on</span></span>

<span data-ttu-id="50308-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="50308-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="50308-217">Ha a hozzáférési panelen Convercent csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Convercent alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="50308-217">When you click the Convercent tile in the Access Panel, you should get automatically signed-on to your Convercent application.</span></span>
<span data-ttu-id="50308-218">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50308-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="50308-219">További források</span><span class="sxs-lookup"><span data-stu-id="50308-219">Additional resources</span></span>

* [<span data-ttu-id="50308-220">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="50308-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="50308-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="50308-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_203.png

