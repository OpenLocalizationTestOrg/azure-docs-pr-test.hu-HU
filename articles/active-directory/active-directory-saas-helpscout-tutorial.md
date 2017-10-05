---
title: "Oktatóanyag: Azure Active Directoryval integrált súgó Scout |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Súgó Scout között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a><span data-ttu-id="edddf-103">Oktatóanyag: Azure Active Directoryval integrált Scout Súgó</span><span class="sxs-lookup"><span data-stu-id="edddf-103">Tutorial: Azure Active Directory integration with Help Scout</span></span>

<span data-ttu-id="edddf-104">Ebben az oktatóanyagban elsajátíthatja súgó Scout integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="edddf-104">In this tutorial, you learn how to integrate Help Scout with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="edddf-105">A következő előnyöket lekérése érdekében Scout integrálása az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="edddf-105">You get the following benefits from integrating Help Scout with Azure AD:</span></span>

- <span data-ttu-id="edddf-106">Az Azure ad-ben szabályozhatja, kik férhetnek hozzá Scout segítségével.</span><span class="sxs-lookup"><span data-stu-id="edddf-106">In Azure AD, you can control who has access to Help Scout.</span></span>
- <span data-ttu-id="edddf-107">Automatikusan bejelentkezhet Scout segítségével a felhasználók az egyszeri bejelentkezés és a felhasználó Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="edddf-107">You can automatically sign in your users to Help Scout by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="edddf-108">A fiók egyetlen, központi helyen, az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="edddf-108">You can manage your accounts in one, central location, the Azure portal.</span></span>

<span data-ttu-id="edddf-109">További információért, egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatban lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="edddf-109">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="edddf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="edddf-110">Prerequisites</span></span>

<span data-ttu-id="edddf-111">Súgó Scout az Azure AD-integráció beállítása, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="edddf-111">To set up Azure AD integration with Help Scout, you need the following items:</span></span>

- <span data-ttu-id="edddf-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="edddf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="edddf-113">Súgó Scout előfizetés, az egyszeri bejelentkezés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="edddf-113">A Help Scout subscription, with single sign-on turned on</span></span> 

> [!NOTE]
> <span data-ttu-id="edddf-114">Ha ebben az oktatóanyagban teszteli a lépéseket, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="edddf-114">If you test the steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="edddf-115">Ebben az oktatóanyagban a lépéseket tesztelési ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="edddf-115">Recommendations for testing the steps in this tutorial:</span></span>

- <span data-ttu-id="edddf-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="edddf-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="edddf-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="edddf-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="edddf-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="edddf-118">Scenario description</span></span>
<span data-ttu-id="edddf-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="edddf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="edddf-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="edddf-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="edddf-121">Súgó Scout hozzáadása a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="edddf-121">Add Help Scout from the gallery.</span></span>
2. <span data-ttu-id="edddf-122">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="edddf-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-help-scout-from-the-gallery"></a><span data-ttu-id="edddf-123">Súgó Scout hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="edddf-123">Add Help Scout from the gallery</span></span>
<span data-ttu-id="edddf-124">A gyűjteményben, Súgó Scout az Azure AD integrálása beállításához vegye fel Scout súgó a felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="edddf-124">To set up the integration of Help Scout with Azure AD, in the gallery, add Help Scout to your list of managed SaaS apps.</span></span>

<span data-ttu-id="edddf-125">Súgó Scout hozzáadása a gyűjteményből:</span><span class="sxs-lookup"><span data-stu-id="edddf-125">To add Help Scout from the gallery:</span></span>

1. <span data-ttu-id="edddf-126">Az a [Azure-portálon](https://portal.azure.com), a bal oldali menüben válassza ki a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="edddf-126">In the [Azure portal](https://portal.azure.com), in the left menu, select **Azure Active Directory**.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="edddf-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="edddf-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![A vállalati alkalmazások lap][2]
    
3. <span data-ttu-id="edddf-130">Új alkalmazás hozzáadásához válassza **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="edddf-130">To add a new application, select **New application**.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="edddf-132">A keresési mezőbe, írja be a **súgó Scout**.</span><span class="sxs-lookup"><span data-stu-id="edddf-132">In the search box, enter **Help Scout**.</span></span> <span data-ttu-id="edddf-133">A keresési eredmények között, válassza ki a **súgó Scout**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="edddf-133">In the search results, select **Help Scout**, and then select **Add**.</span></span>

    ![Az eredménylistában Scout Súgó](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="edddf-135">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="edddf-135">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="edddf-136">Ebben a szakaszban állítsa be, és az Azure AD az egyszeri bejelentkezés Scout súgó-teszthez alapján nevű tesztfelhasználó *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="edddf-136">In this section, you set up and test Azure AD single sign-on with Help Scout based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="edddf-137">Az egyszeri bejelentkezés működéséhez az Azure AD a Azure AD-partner felhasználó a Súgó Scout tudnia kell.</span><span class="sxs-lookup"><span data-stu-id="edddf-137">For single sign-on to work, Azure AD needs to know the Azure AD counterpart user in Help Scout.</span></span> <span data-ttu-id="edddf-138">Egy Azure AD-felhasználó és a kapcsolódó felhasználó a Súgó Scout közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="edddf-138">A link relationship between an Azure AD user and the related user in Help Scout must be established.</span></span>

<span data-ttu-id="edddf-139">A hivatkozás viszony létrehozásához, a Súgó Scout a **felhasználónév**, rendelje az értékét a **felhasználónév** Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="edddf-139">To establish the link relationship, in Help Scout, for **Username**, assign the value of the **user name** in Azure AD.</span></span>

<span data-ttu-id="edddf-140">Az Azure AD egyszeri bejelentkezést a Súgó Scout tesztelése és konfigurálása, végezze el a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="edddf-140">To configure and test Azure AD single sign-on with Help Scout, complete the following tasks:</span></span>

1. <span data-ttu-id="edddf-141">[Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="edddf-141">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="edddf-142">A felhasználó beállítja a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="edddf-142">Sets up a user to use this feature.</span></span>
2. <span data-ttu-id="edddf-143">[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="edddf-143">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="edddf-144">Az Azure AD tesztek egyszeri bejelentkezés Britta Simon felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="edddf-144">Tests Azure AD single sign-on with the user Britta Simon.</span></span>
3. <span data-ttu-id="edddf-145">[Súgó Scout tesztfelhasználó létrehozása](#create-a-help-scout-test-user).</span><span class="sxs-lookup"><span data-stu-id="edddf-145">[Create a Help Scout test user](#create-a-help-scout-test-user).</span></span> <span data-ttu-id="edddf-146">Létrehoz egy megfelelője a Britta Simon súgó Scout, amely csatolva van a felhasználó az Azure AD ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="edddf-146">Creates a counterpart of Britta Simon in Help Scout that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="edddf-147">[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="edddf-147">[Assign the Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="edddf-148">Állítja be az Azure AD használatára Britta Simon egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="edddf-148">Sets up Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="edddf-149">[Egyszeri bejelentkezés tesztelése](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="edddf-149">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="edddf-150">Ellenőrzi, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="edddf-150">Verifies that the configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="edddf-151">Az Azure AD az egyszeri bejelentkezés beállítása</span><span class="sxs-lookup"><span data-stu-id="edddf-151">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="edddf-152">Ebben a szakaszban állíthatja be az Azure AD az egyszeri bejelentkezés az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="edddf-152">In this section, you set up Azure AD single sign-on in the Azure portal.</span></span> <span data-ttu-id="edddf-153">Ezután állítsa be az egyszeri bejelentkezés súgó Scout alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="edddf-153">Then, you set up single sign-on in your Help Scout application.</span></span>

<span data-ttu-id="edddf-154">Beállítása az Azure AD egyszeri bejelentkezést a Súgó Scout:</span><span class="sxs-lookup"><span data-stu-id="edddf-154">To set up Azure AD single sign-on with Help Scout:</span></span>

1. <span data-ttu-id="edddf-155">Az Azure portálon a a **súgó Scout** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="edddf-155">In the Azure portal, on the **Help Scout** application integration page, select **Single sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés hivatkozás beállítása][4]

2. <span data-ttu-id="edddf-157">Az a **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="edddf-157">On the **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. <span data-ttu-id="edddf-159">A **Scout tartományban Help és URL-címek**, ha azt szeretné, állíthatja be az alkalmazás a kiállító terjesztési hely által kezdeményezett módban, teljes az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="edddf-159">Under **Help Scout Domain and URLs**, if you want to set up the application in IDP-initiated mode, complete the following steps:</span></span>

    1. <span data-ttu-id="edddf-160">Az a **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát:`urn:auth0:helpscout:<instancename>`</span><span class="sxs-lookup"><span data-stu-id="edddf-160">In the **Identifier** box, enter a URL that has the following pattern: `urn:auth0:helpscout:<instancename>`</span></span>

    2. <span data-ttu-id="edddf-161">Az a **válasz URL-CÍMEN** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát:`https://helpscout.auth0.com/login/callback?connection=<instancename>`</span><span class="sxs-lookup"><span data-stu-id="edddf-161">In the **Reply URL** box, enter a URL that has the following pattern: `https://helpscout.auth0.com/login/callback?connection=<instancename>`</span></span>

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. <span data-ttu-id="edddf-163">Ha azt szeretné, a Szolgáltató által kezdeményezett módban alkalmazás beállításához, válassza a **megjelenítése speciális URL-beállításainak** jelölőnégyzetet, majd tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="edddf-163">If you want to set up the application in SP-initiated mode, select the **Show advanced URL settings** check box, and then do the following:</span></span>

    * <span data-ttu-id="edddf-164">Az a **bejelentkezési URL-cím** mezőbe írja be az URL-címnek a következő formátumban:`https://secure.helpscout.net/members/login/`</span><span class="sxs-lookup"><span data-stu-id="edddf-164">In the **Sign on URL** box, enter a URL that has the following format: `https://secure.helpscout.net/members/login/`</span></span>

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > <span data-ttu-id="edddf-166">Az URL-címek értékei csak bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="edddf-166">The values in these URLs are for demonstration only.</span></span> <span data-ttu-id="edddf-167">Módosítsa a tényleges azonosító URL-t és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="edddf-167">Update the values with the actual identifier URL and reply URL.</span></span> <span data-ttu-id="edddf-168">Ahhoz, hogy ezeket az értékeket, lépjen kapcsolatba [súgó Scout támogatási csoport](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="edddf-168">To get these values, contact [Help Scout support team](mailto:help@helpscout.com).</span></span> 

5. <span data-ttu-id="edddf-169">A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="edddf-169">Under **SAML Signing Certificate**, select **Metadata XML**, and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. <span data-ttu-id="edddf-171">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="edddf-171">Select **Save**.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="edddf-173">Állítsa be az egyszeri bejelentkezés a Súgó Scout oldalon, küldeni a letöltött metaadatok XML-fájl a [súgó Scout támogatási csoport](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="edddf-173">To set up single sign-on on the Help Scout side, send the downloaded metadata XML file to the [Help Scout support team](mailto:help@helpscout.com).</span></span> <span data-ttu-id="edddf-174">A Súgó Scout támogatási csoport alkalmazza ezt a beállítást, hogy a SAML-alapú egyszeri bejelentkezés kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="edddf-174">The Help Scout support team applies this setting so that the SAML single sign-on connection is set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="edddf-175">Ezek az utasítások a tömör verzióját el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás!</span><span class="sxs-lookup"><span data-stu-id="edddf-175">You can read a concise version of these instructions in the [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="edddf-176">Miután hozzáadta az alkalmazás kiválasztásával **Active Directory** > **vállalati alkalmazások**, jelölje be a **egyszeri bejelentkezés** lapon. A beágyazott dokumentációja a **konfigurációs** szakaszban, a lap alján.</span><span class="sxs-lookup"><span data-stu-id="edddf-176">After you add the app by selecting **Active Directory** > **Enterprise Applications**, select the **Single Sign-On** tab. You can access the embedded documentation in the **Configuration** section, at the bottom of the page.</span></span> <span data-ttu-id="edddf-177">További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="edddf-177">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="edddf-178">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="edddf-178">Create an Azure AD test user</span></span>

<span data-ttu-id="edddf-179">Ebben a szakaszban az Azure-portálon Britta Simon nevű tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="edddf-179">In this section, in the Azure portal, you create a test user named Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="edddf-181">Tesztfelhasználó létrehozása az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="edddf-181">To create a test user in Azure AD:</span></span>

1. <span data-ttu-id="edddf-182">Az Azure portálon a bal oldali menüben válassza ki a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="edddf-182">In the Azure portal, in the left menu, select **Azure Active Directory**.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="edddf-184">Válassza ki azon felhasználók listájának megjelenítéséhez **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="edddf-184">To display the list of users, select **Users and groups**, and then select **All users**.</span></span>

    ![Felhasználók és csoportok kiválasztása, és válassza a minden felhasználó](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="edddf-186">Lehetőségre a **felhasználói** párbeszédpanel tetején a **minden felhasználó** lapon jelölje be **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="edddf-186">To open the **User** dialog box, at the top of the **All Users** page, select **Add**.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="edddf-188">Az a **felhasználói** párbeszédpanelen töltse ki az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="edddf-188">In the **User** dialog box, complete the following steps:</span></span>

    1. <span data-ttu-id="edddf-189">Az a **neve** adja meg a **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="edddf-189">In the **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="edddf-190">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="edddf-190">In the **User name** box, enter the email address of user Britta Simon.</span></span>

    3. <span data-ttu-id="edddf-191">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="edddf-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    4. <span data-ttu-id="edddf-192">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="edddf-192">Select **Create**.</span></span>

        ![A felhasználó párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a><span data-ttu-id="edddf-194">Súgó Scout tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="edddf-194">Create a Help Scout test user</span></span>

<span data-ttu-id="edddf-195">Az ebben a szakaszban, hogy a Súgó Scout Britta Simon nevű felhasználót kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="edddf-195">The object of this section is to create a user named Britta Simon in Help Scout.</span></span> <span data-ttu-id="edddf-196">Súgó Scout támogatja közvetlenül az igény (szerinti JIT) átadása, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="edddf-196">Help Scout supports just-in-time (JIT) provisioning, which is turned on by default.</span></span>

<span data-ttu-id="edddf-197">Ebben a szakaszban nincs művelet vagy feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="edddf-197">In this section, there's no action or task to complete.</span></span> <span data-ttu-id="edddf-198">Ha a felhasználó nem létezik a Súgó Scout, egy új súgó Scout elérésére tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="edddf-198">If a user doesn't already exist in Help Scout, a new one is created when you attempt to access Help Scout.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="edddf-199">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="edddf-199">Assign the Azure AD test user</span></span>

<span data-ttu-id="edddf-200">Ebben a szakaszban engedélyezze a felhasználó által a felhasználóifiók-hozzáférés engedélyezése a Súgó Scout az Azure AD egyszeri bejelentkezéshez használandó Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="edddf-200">In this section, you allow the user Britta Simon to use Azure AD single sign-on by granting the user account access to Help Scout.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="edddf-202">Súgó Scout Britta Simon hozzárendelése:</span><span class="sxs-lookup"><span data-stu-id="edddf-202">To assign Britta Simon to Help Scout:</span></span>

1. <span data-ttu-id="edddf-203">Az Azure portálon az alkalmazások nézet megnyitásához, és keresse meg a könyvtár nézet.</span><span class="sxs-lookup"><span data-stu-id="edddf-203">In the Azure portal, open the applications view, and then go to the directory view.</span></span> <span data-ttu-id="edddf-204">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="edddf-204">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="edddf-206">Az alkalmazások listában válassza ki a **súgó Scout**.</span><span class="sxs-lookup"><span data-stu-id="edddf-206">In the applications list, select **Help Scout**.</span></span>

    ![Az alkalmazások listáját a Scout Súgó hivatkozásra](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. <span data-ttu-id="edddf-208">A bal oldali menüben válassza ki a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="edddf-208">In the left menu, select **Users and groups**.</span></span>

    ![A felhasználók és csoportok hivatkozás][202]

4. <span data-ttu-id="edddf-210">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="edddf-210">Select **Add**.</span></span> <span data-ttu-id="edddf-211">Ezt követően a **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="edddf-211">Then, on the **Add Assignment** page, select **Users and groups**.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="edddf-213">Az a **felhasználók és csoportok** lapra, jelölje be a felhasználók listáján **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="edddf-213">On the **Users and groups** page, in the list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="edddf-214">Az a **felhasználók és csoportok** lapon jelölje be **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="edddf-214">On the **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="edddf-215">Az a **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="edddf-215">On the **Add Assignment** page, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="edddf-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="edddf-216">Test single sign-on</span></span>

<span data-ttu-id="edddf-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="edddf-217">In this section, you test your Azure AD single sign-on configuration by using the access panel.</span></span>

<span data-ttu-id="edddf-218">A Súgó Scout csempe a hozzáférési panelen válassza ki, amikor be kell automatikusan jelentkeznie az súgó Scout alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="edddf-218">When you select the Help Scout tile in the access panel, you should be automatically signed in to your Help Scout application.</span></span>

<span data-ttu-id="edddf-219">A hozzáférési panel kapcsolatos további információkért lásd: [a hozzáférési panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="edddf-219">For more information about the access panel, see [Introduction to the access panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="edddf-220">További források</span><span class="sxs-lookup"><span data-stu-id="edddf-220">Additional resources</span></span>

* [<span data-ttu-id="edddf-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="edddf-221">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="edddf-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="edddf-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

