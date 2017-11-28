---
title: "Oktatóanyag: Azure Active Directoryval integrált SmarterU |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SmarterU között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 129d08c8a7b4228d4d5f1a3b7938ab649b2747a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a><span data-ttu-id="84e24-103">Oktatóanyag: Azure Active Directoryval integrált SmarterU</span><span class="sxs-lookup"><span data-stu-id="84e24-103">Tutorial: Azure Active Directory integration with SmarterU</span></span>

<span data-ttu-id="84e24-104">Ebben az oktatóanyagban elsajátíthatja SmarterU integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84e24-104">In this tutorial, you learn how to integrate SmarterU with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84e24-105">SmarterU integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="84e24-105">Integrating SmarterU with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="84e24-106">Megadhatja a SmarterU hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="84e24-106">You can control in Azure AD who has access to SmarterU</span></span>
- <span data-ttu-id="84e24-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett SmarterU (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="84e24-107">You can enable your users to automatically get signed-on to SmarterU (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84e24-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="84e24-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="84e24-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84e24-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84e24-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84e24-110">Prerequisites</span></span>

<span data-ttu-id="84e24-111">Konfigurálása az Azure AD-integrációs SmarterU, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="84e24-111">To configure Azure AD integration with SmarterU, you need the following items:</span></span>

- <span data-ttu-id="84e24-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84e24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84e24-113">Egy SmarterU egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="84e24-113">A SmarterU single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84e24-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="84e24-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84e24-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="84e24-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84e24-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84e24-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84e24-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84e24-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84e24-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84e24-118">Scenario description</span></span>
<span data-ttu-id="84e24-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84e24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84e24-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84e24-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84e24-121">A gyűjteményből SmarterU hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84e24-121">Adding SmarterU from the gallery</span></span>
2. <span data-ttu-id="84e24-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84e24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-smarteru-from-the-gallery"></a><span data-ttu-id="84e24-123">A gyűjteményből SmarterU hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84e24-123">Adding SmarterU from the gallery</span></span>
<span data-ttu-id="84e24-124">Az Azure AD integrálása a SmarterU konfigurálásához kell hozzáadnia SmarterU a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="84e24-124">To configure the integration of SmarterU into Azure AD, you need to add SmarterU from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="84e24-125">**A gyűjteményből SmarterU hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84e24-125">**To add SmarterU from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="84e24-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84e24-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84e24-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84e24-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="84e24-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84e24-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="84e24-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="84e24-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="84e24-133">Írja be a keresőmezőbe, **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="84e24-133">In the search box, type **SmarterU**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. <span data-ttu-id="84e24-135">Az eredmények panelen válassza ki a **SmarterU**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84e24-135">In the results panel, select **SmarterU**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84e24-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84e24-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84e24-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján SmarterU</span><span class="sxs-lookup"><span data-stu-id="84e24-138">In this section, you configure and test Azure AD single sign-on with SmarterU based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="84e24-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SmarterU a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="84e24-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SmarterU is to a user in Azure AD.</span></span> <span data-ttu-id="84e24-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SmarterU közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="84e24-140">In other words, a link relationship between an Azure AD user and the related user in SmarterU needs to be established.</span></span>

<span data-ttu-id="84e24-141">SmarterU, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="84e24-141">In SmarterU, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="84e24-142">Az Azure AD egyszeri bejelentkezést a SmarterU tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="84e24-142">To configure and test Azure AD single sign-on with SmarterU, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="84e24-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="84e24-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="84e24-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="84e24-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84e24-145">**[SmarterU tesztfelhasználó létrehozása](#creating-a-smarteru-test-user)**  - való Britta Simon valami SmarterU, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="84e24-145">**[Creating a SmarterU test user](#creating-a-smarteru-test-user)** - to have a counterpart of Britta Simon in SmarterU that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="84e24-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84e24-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84e24-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="84e24-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84e24-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84e24-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84e24-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az SmarterU alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84e24-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SmarterU application.</span></span>

<span data-ttu-id="84e24-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés SmarterU, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84e24-150">**To configure Azure AD single sign-on with SmarterU, perform the following steps:**</span></span>

1. <span data-ttu-id="84e24-151">Az Azure portálon a a **SmarterU** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84e24-151">In the Azure portal, on the **SmarterU** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="84e24-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84e24-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. <span data-ttu-id="84e24-155">Az a **SmarterU tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="84e24-155">On the **SmarterU Domain and URLs** section, perform the following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    <span data-ttu-id="84e24-157">Az a **azonosító** szövegmező, írja be az URL-cím:`https://www.smarteru.com/`</span><span class="sxs-lookup"><span data-stu-id="84e24-157">In the **Identifier** textbox, type the URL: `https://www.smarteru.com/`</span></span>

4. <span data-ttu-id="84e24-158">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="84e24-158">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. <span data-ttu-id="84e24-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84e24-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="84e24-162">Egy másik webes böngészőablakban jelentkezzen be a SmarterU vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="84e24-162">In a different web browser window, log in to your SmarterU company site as an administrator.</span></span>

7. <span data-ttu-id="84e24-163">A felső eszköztáron kattintson **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="84e24-163">In the toolbar on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="84e24-164">![Fiókbeállítások](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="84e24-164">![Account Settings](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")</span></span>

8. <span data-ttu-id="84e24-165">A fiók konfigurálása lapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84e24-165">On the account configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="84e24-166">![Külső engedélyezési](./media/active-directory-saas-smarteru-tutorial/IC777327.png "külső engedélyezési")</span><span class="sxs-lookup"><span data-stu-id="84e24-166">![External Authorization](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")</span></span> 
 
      <span data-ttu-id="84e24-167">a.</span><span class="sxs-lookup"><span data-stu-id="84e24-167">a.</span></span> <span data-ttu-id="84e24-168">Válassza ki **külső engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="84e24-168">Select **Enable External Authorization**.</span></span>
  
      <span data-ttu-id="84e24-169">b.</span><span class="sxs-lookup"><span data-stu-id="84e24-169">b.</span></span> <span data-ttu-id="84e24-170">Az a **fő bejelentkezési vezérlő** szakaszban jelölje be a **SmarterU** fülre.</span><span class="sxs-lookup"><span data-stu-id="84e24-170">In the **Master Login Control** section, select the **SmarterU** tab.</span></span>
  
      <span data-ttu-id="84e24-171">c.</span><span class="sxs-lookup"><span data-stu-id="84e24-171">c.</span></span> <span data-ttu-id="84e24-172">Az a **felhasználó alapértelmezett bejelentkezési** szakaszban jelölje be a **SmarterU** fülre.</span><span class="sxs-lookup"><span data-stu-id="84e24-172">In the **User Default Login** section, select the **SmarterU** tab.</span></span>
  
      <span data-ttu-id="84e24-173">d.</span><span class="sxs-lookup"><span data-stu-id="84e24-173">d.</span></span> <span data-ttu-id="84e24-174">Válassza ki **Okta engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="84e24-174">Select **Enable Okta**.</span></span>
  
      <span data-ttu-id="84e24-175">e.</span><span class="sxs-lookup"><span data-stu-id="84e24-175">e.</span></span> <span data-ttu-id="84e24-176">Másolja a letöltött metaadatait tartalmazó fájl tartalmát, és illessze be azt a **Okta metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="84e24-176">Copy the content of the downloaded metadata file, and then paste it into the **Okta Metadata** textbox.</span></span>
  
      <span data-ttu-id="84e24-177">f.</span><span class="sxs-lookup"><span data-stu-id="84e24-177">f.</span></span> <span data-ttu-id="84e24-178">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="84e24-178">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="84e24-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="84e24-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="84e24-180">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="84e24-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="84e24-181">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84e24-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84e24-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84e24-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="84e24-183">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="84e24-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="84e24-185">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84e24-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="84e24-186">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84e24-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84e24-188">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="84e24-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84e24-190">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="84e24-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84e24-192">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="84e24-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84e24-194">a.</span><span class="sxs-lookup"><span data-stu-id="84e24-194">a.</span></span> <span data-ttu-id="84e24-195">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84e24-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84e24-196">b.</span><span class="sxs-lookup"><span data-stu-id="84e24-196">b.</span></span> <span data-ttu-id="84e24-197">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84e24-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84e24-198">c.</span><span class="sxs-lookup"><span data-stu-id="84e24-198">c.</span></span> <span data-ttu-id="84e24-199">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="84e24-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="84e24-200">d.</span><span class="sxs-lookup"><span data-stu-id="84e24-200">d.</span></span> <span data-ttu-id="84e24-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84e24-201">Click **Create**.</span></span>
 
### <a name="creating-a-smarteru-test-user"></a><span data-ttu-id="84e24-202">SmarterU tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84e24-202">Creating a SmarterU test user</span></span>

<span data-ttu-id="84e24-203">Ahhoz, hogy az Azure AD-felhasználók SmarterU bejelentkezni, akkor ki kell építenie a SmarterU.</span><span class="sxs-lookup"><span data-stu-id="84e24-203">To enable Azure AD users to log in to SmarterU, they must be provisioned into SmarterU.</span></span>

<span data-ttu-id="84e24-204">SmarterU, ha a kézi tevékenység kiépítés.</span><span class="sxs-lookup"><span data-stu-id="84e24-204">When SmarterU, provisioning is a manual task.</span></span>

<span data-ttu-id="84e24-205">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84e24-205">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="84e24-206">Jelentkezzen be a **SmarterU** bérlő.</span><span class="sxs-lookup"><span data-stu-id="84e24-206">Log in to your **SmarterU** tenant.</span></span>

2. <span data-ttu-id="84e24-207">Ugrás a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="84e24-207">Go to **Users**.</span></span>

3. <span data-ttu-id="84e24-208">A felhasználói csoportban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="84e24-208">In the user section, perform the following steps:</span></span>
   
    <span data-ttu-id="84e24-209">![Új felhasználó](./media/active-directory-saas-smarteru-tutorial/IC777329.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="84e24-209">![New User](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")</span></span>  

    <span data-ttu-id="84e24-210">a.</span><span class="sxs-lookup"><span data-stu-id="84e24-210">a.</span></span> <span data-ttu-id="84e24-211">Kattintson a **+ felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="84e24-211">Click **+User**.</span></span>
    
    <span data-ttu-id="84e24-212">b.</span><span class="sxs-lookup"><span data-stu-id="84e24-212">b.</span></span> <span data-ttu-id="84e24-213">Az Azure AD-felhasználói fiókot kapcsolódó attribútumértékek írja be a következő szövegmezők: **elsődleges E-mail**, **Alkalmazottazonosító**, **jelszó**, **jelszó ellenőrzése**, **Utónév**, **vezetékneve**.</span><span class="sxs-lookup"><span data-stu-id="84e24-213">Type the related attribute values of the Azure AD user account into the following textboxes: **Primary Email**, **Employee ID**, **Password**, **Verify Password**, **Given Name**, **Surname**.</span></span>
    
    <span data-ttu-id="84e24-214">c.</span><span class="sxs-lookup"><span data-stu-id="84e24-214">c.</span></span> <span data-ttu-id="84e24-215">Kattintson a **aktív**.</span><span class="sxs-lookup"><span data-stu-id="84e24-215">Click **Active**.</span></span> 
    
    <span data-ttu-id="84e24-216">d.</span><span class="sxs-lookup"><span data-stu-id="84e24-216">d.</span></span> <span data-ttu-id="84e24-217">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="84e24-217">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="84e24-218">Bármely más SmarterU felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz SmarterU által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="84e24-218">You can use any other SmarterU user account creation tools or APIs provided by SmarterU to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="84e24-219">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="84e24-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="84e24-220">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés SmarterU Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="84e24-220">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SmarterU.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="84e24-222">**Britta Simon hozzárendelése SmarterU, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84e24-222">**To assign Britta Simon to SmarterU, perform the following steps:**</span></span>

1. <span data-ttu-id="84e24-223">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84e24-223">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84e24-225">Az alkalmazások listában válassza ki a **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="84e24-225">In the applications list, select **SmarterU**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. <span data-ttu-id="84e24-227">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84e24-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="84e24-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84e24-229">Click **Add** button.</span></span> <span data-ttu-id="84e24-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84e24-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="84e24-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84e24-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="84e24-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84e24-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84e24-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84e24-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84e24-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84e24-235">Testing single sign-on</span></span>

<span data-ttu-id="84e24-236">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="84e24-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="84e24-237">Ha a hozzáférési panelen SmarterU csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SmarterU alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="84e24-237">When you click the SmarterU tile in the Access Panel, you should get automatically signed-on to your SmarterU application.</span></span>
<span data-ttu-id="84e24-238">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84e24-238">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 


## <a name="additional-resources"></a><span data-ttu-id="84e24-239">További források</span><span class="sxs-lookup"><span data-stu-id="84e24-239">Additional resources</span></span>

* [<span data-ttu-id="84e24-240">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="84e24-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84e24-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84e24-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

