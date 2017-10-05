---
title: "Oktatóanyag: Azure Active Directoryval integrált Jive |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Jive között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 6d2d4b777d7afd74598d2eba4a7e3571a8a18d6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a><span data-ttu-id="f018f-103">Oktatóanyag: Azure Active Directoryval integrált Jive</span><span class="sxs-lookup"><span data-stu-id="f018f-103">Tutorial: Azure Active Directory integration with Jive</span></span>

<span data-ttu-id="f018f-104">Ebben az oktatóanyagban elsajátíthatja Jive integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f018f-104">In this tutorial, you learn how to integrate Jive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f018f-105">Jive integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f018f-105">Integrating Jive with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f018f-106">Megadhatja a Jive hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f018f-106">You can control in Azure AD who has access to Jive</span></span>
- <span data-ttu-id="f018f-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Jive (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f018f-107">You can enable your users to automatically get signed-on to Jive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f018f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f018f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f018f-109">Ha szeretné tudni, hogy az Azure AD SaaS integrálásáról további információt, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f018f-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f018f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f018f-110">Prerequisites</span></span>

<span data-ttu-id="f018f-111">Konfigurálása az Azure AD-integrációs Jive, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f018f-111">To configure Azure AD integration with Jive, you need the following items:</span></span>

- <span data-ttu-id="f018f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f018f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f018f-113">Egy Jive egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f018f-113">A Jive single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f018f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f018f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f018f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f018f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f018f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f018f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f018f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f018f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f018f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f018f-118">Scenario description</span></span>
<span data-ttu-id="f018f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f018f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f018f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f018f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f018f-121">A gyűjteményből Jive hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f018f-121">Adding Jive from the gallery</span></span>
2. <span data-ttu-id="f018f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f018f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jive-from-the-gallery"></a><span data-ttu-id="f018f-123">A gyűjteményből Jive hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f018f-123">Adding Jive from the gallery</span></span>
<span data-ttu-id="f018f-124">Az Azure AD-be Jive integrálása konfigurálásához kell hozzáadnia Jive a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f018f-124">To configure the integration of Jive into Azure AD, you need to add Jive from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f018f-125">**A gyűjteményből Jive hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f018f-125">**To add Jive from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f018f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f018f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f018f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f018f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f018f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f018f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f018f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f018f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f018f-133">Írja be a keresőmezőbe, **Jive**.</span><span class="sxs-lookup"><span data-stu-id="f018f-133">In the search box, type **Jive**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/tutorial_jive_search.png)

5. <span data-ttu-id="f018f-135">Az eredmények panelen válassza ki a **Jive**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f018f-135">In the results panel, select **Jive**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f018f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f018f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f018f-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jive</span><span class="sxs-lookup"><span data-stu-id="f018f-138">In this section, you configure and test Azure AD single sign-on with Jive based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f018f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Jive a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f018f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jive is to a user in Azure AD.</span></span> <span data-ttu-id="f018f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Jive közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f018f-140">In other words, a link relationship between an Azure AD user and the related user in Jive needs to be established.</span></span>

<span data-ttu-id="f018f-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Jive a.</span><span class="sxs-lookup"><span data-stu-id="f018f-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Jive.</span></span>

<span data-ttu-id="f018f-142">Az Azure AD egyszeri bejelentkezést a Jive tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f018f-142">To configure and test Azure AD single sign-on with Jive, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f018f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f018f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f018f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f018f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f018f-145">**[Jive tesztfelhasználó létrehozása](#creating-a-jive-test-user)**  - való Britta Simon valami Jive, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f018f-145">**[Creating a Jive test user](#creating-a-jive-test-user)** - to have a counterpart of Britta Simon in Jive that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f018f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f018f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f018f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f018f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f018f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f018f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f018f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Jive alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f018f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jive application.</span></span>

<span data-ttu-id="f018f-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Jive, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f018f-150">**To configure Azure AD single sign-on with Jive, perform the following steps:**</span></span>

1. <span data-ttu-id="f018f-151">Az Azure portálon a a **Jive** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f018f-151">In the Azure portal, on the **Jive** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f018f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f018f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_samlbase.png)

3. <span data-ttu-id="f018f-155">Az a **Jive tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f018f-155">On the **Jive Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_url.png)

    <span data-ttu-id="f018f-157">a.</span><span class="sxs-lookup"><span data-stu-id="f018f-157">a.</span></span> <span data-ttu-id="f018f-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instance name>.jivecustom.com`</span><span class="sxs-lookup"><span data-stu-id="f018f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instance name>.jivecustom.com`</span></span>

    <span data-ttu-id="f018f-159">b.</span><span class="sxs-lookup"><span data-stu-id="f018f-159">b.</span></span> <span data-ttu-id="f018f-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instance name>.jiveon.com`</span><span class="sxs-lookup"><span data-stu-id="f018f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instance name>.jiveon.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f018f-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="f018f-161">These values are not the real.</span></span> <span data-ttu-id="f018f-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f018f-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="f018f-163">Ügyfél [Jive ügyfél-támogatási csoport](https://www.jivesoftware.com/services-support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f018f-163">Contact [Jive Client support team](https://www.jivesoftware.com/services-support/) to get these values.</span></span> 
 
4. <span data-ttu-id="f018f-164">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f018f-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_certificate.png) 

5. <span data-ttu-id="f018f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f018f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f018f-168">Egyszeri bejelentkezés konfigurálása **Jive** oldalon, a bejelentkezés a Jive bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f018f-168">To configure single sign-on on **Jive** side, sign-on to your Jive tenant as an administrator.</span></span>

7. <span data-ttu-id="f018f-169">A felső menüben kattintson a "**Saml**."</span><span class="sxs-lookup"><span data-stu-id="f018f-169">In the menu on the top, Click "**Saml**."</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    <span data-ttu-id="f018f-171">a.</span><span class="sxs-lookup"><span data-stu-id="f018f-171">a.</span></span> <span data-ttu-id="f018f-172">Válassza ki **engedélyezve** alatt a **általános** fülre.</span><span class="sxs-lookup"><span data-stu-id="f018f-172">Select **Enabled** under the **General** tab.</span></span>   
    <span data-ttu-id="f018f-173">b.</span><span class="sxs-lookup"><span data-stu-id="f018f-173">b.</span></span> <span data-ttu-id="f018f-174">Kattintson a "**összes saml-beállításainak mentése**" gombra.</span><span class="sxs-lookup"><span data-stu-id="f018f-174">Click the "**Save all saml settings**" button.</span></span>

8. <span data-ttu-id="f018f-175">Keresse meg a "**Idp metaadatok**" lapon.</span><span class="sxs-lookup"><span data-stu-id="f018f-175">Navigate to the "**Idp Metadata**" tab.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)
   
    <span data-ttu-id="f018f-177">a.</span><span class="sxs-lookup"><span data-stu-id="f018f-177">a.</span></span> <span data-ttu-id="f018f-178">Másolja a letöltött metaadatok XML-fájl tartalmát, és illessze be azt a **Identity Provider (IDP) metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f018f-178">Copy the content of the downloaded metadata XML file, and then paste it into the **Identity Provider (IDP) Metadata** textbox.</span></span>
    
    <span data-ttu-id="f018f-179">b.</span><span class="sxs-lookup"><span data-stu-id="f018f-179">b.</span></span> <span data-ttu-id="f018f-180">Kattintson a "**összes saml-beállításainak mentése**" gombra.</span><span class="sxs-lookup"><span data-stu-id="f018f-180">Click the "**Save all saml settings**" button.</span></span> 

9. <span data-ttu-id="f018f-181">Lépjen a "**attribútum Felhasználóleképezés**" lapon.</span><span class="sxs-lookup"><span data-stu-id="f018f-181">Go to the "**User Attribute Mapping**" tab.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)
   
    <span data-ttu-id="f018f-183">a.</span><span class="sxs-lookup"><span data-stu-id="f018f-183">a.</span></span> <span data-ttu-id="f018f-184">Az a **E-mail** szövegmező, másolja be az attribútum nevét **mail** érték.</span><span class="sxs-lookup"><span data-stu-id="f018f-184">In the **Email** textbox, copy and paste the attribute name of **mail** value.</span></span>
   
    <span data-ttu-id="f018f-185">b.</span><span class="sxs-lookup"><span data-stu-id="f018f-185">b.</span></span> <span data-ttu-id="f018f-186">Az a **Utónév** szövegmező, másolja be a telefonmelléket **givenname** érték.</span><span class="sxs-lookup"><span data-stu-id="f018f-186">In the **First Name** textbox, copy and paste the attribute name of **givenname** value.</span></span>
   
    <span data-ttu-id="f018f-187">c.</span><span class="sxs-lookup"><span data-stu-id="f018f-187">c.</span></span> <span data-ttu-id="f018f-188">Az a **Vezetéknév** szövegmező, másolja be a telefonmelléket **vezetékneve** érték.</span><span class="sxs-lookup"><span data-stu-id="f018f-188">In the **Last Name** textbox, copy and paste the attribute name of **surname** value.</span></span>

> [!TIP]
> <span data-ttu-id="f018f-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f018f-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f018f-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f018f-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f018f-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f018f-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f018f-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f018f-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="f018f-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f018f-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f018f-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f018f-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f018f-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f018f-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f018f-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f018f-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f018f-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f018f-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f018f-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f018f-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f018f-204">a.</span><span class="sxs-lookup"><span data-stu-id="f018f-204">a.</span></span> <span data-ttu-id="f018f-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f018f-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f018f-206">b.</span><span class="sxs-lookup"><span data-stu-id="f018f-206">b.</span></span> <span data-ttu-id="f018f-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f018f-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f018f-208">c.</span><span class="sxs-lookup"><span data-stu-id="f018f-208">c.</span></span> <span data-ttu-id="f018f-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f018f-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f018f-210">d.</span><span class="sxs-lookup"><span data-stu-id="f018f-210">d.</span></span> <span data-ttu-id="f018f-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f018f-211">Click **Create**.</span></span>
 
### <a name="creating-a-jive-test-user"></a><span data-ttu-id="f018f-212">Jive tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f018f-212">Creating a Jive test user</span></span>

<span data-ttu-id="f018f-213">Együttműködve [Jive ügyfél-támogatási csoport](https://www.jivesoftware.com/services-support/) a felhasználók hozzáadása a Jive platform.</span><span class="sxs-lookup"><span data-stu-id="f018f-213">Work with [Jive Client support team](https://www.jivesoftware.com/services-support/) to add the users in the Jive platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f018f-214">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f018f-214">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f018f-215">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Jive Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f018f-215">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jive.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f018f-217">**Britta Simon hozzárendelése Jive, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f018f-217">**To assign Britta Simon to Jive, perform the following steps:**</span></span>

1. <span data-ttu-id="f018f-218">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f018f-218">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f018f-220">Az alkalmazások listában válassza ki a **Jive**.</span><span class="sxs-lookup"><span data-stu-id="f018f-220">In the applications list, select **Jive**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jive-tutorial/tutorial_jive_app.png) 

3. <span data-ttu-id="f018f-222">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f018f-222">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f018f-224">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f018f-224">Click **Add** button.</span></span> <span data-ttu-id="f018f-225">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f018f-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f018f-227">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f018f-227">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f018f-228">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f018f-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f018f-229">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f018f-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f018f-230">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f018f-230">Testing single sign-on</span></span>

<span data-ttu-id="f018f-231">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f018f-231">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f018f-232">Ha a hozzáférési panelen Jive csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Jive alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f018f-232">When you click the Jive tile in the Access Panel, you should get automatically signed-on to your Jive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f018f-233">További források</span><span class="sxs-lookup"><span data-stu-id="f018f-233">Additional resources</span></span>

* [<span data-ttu-id="f018f-234">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f018f-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f018f-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f018f-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="f018f-236">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f018f-236">Configure User Provisioning</span></span>](active-directory-saas-jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jive-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png

