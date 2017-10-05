---
title: "Oktatóanyag: Azure Active Directoryval integrált Pingboard |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Pingboard között."
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
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 008c670a8043da0c67ccefde48d5ef721c75d97c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a><span data-ttu-id="817a4-103">Oktatóanyag: Azure Active Directoryval integrált Pingboard</span><span class="sxs-lookup"><span data-stu-id="817a4-103">Tutorial: Azure Active Directory integration with Pingboard</span></span>

<span data-ttu-id="817a4-104">Ebben az oktatóanyagban elsajátíthatja Pingboard integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="817a4-104">In this tutorial, you learn how to integrate Pingboard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="817a4-105">Pingboard integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="817a4-105">Integrating Pingboard with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="817a4-106">Megadhatja a Pingboard hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="817a4-106">You can control in Azure AD who has access to Pingboard</span></span>
- <span data-ttu-id="817a4-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Pingboard (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="817a4-107">You can enable your users to automatically get signed-on to Pingboard (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="817a4-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="817a4-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="817a4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="817a4-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="817a4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="817a4-110">Prerequisites</span></span>

<span data-ttu-id="817a4-111">Konfigurálása az Azure AD-integrációs Pingboard, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="817a4-111">To configure Azure AD integration with Pingboard, you need the following items:</span></span>

- <span data-ttu-id="817a4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="817a4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="817a4-113">Egy Pingboard egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="817a4-113">A Pingboard single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="817a4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="817a4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="817a4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="817a4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="817a4-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="817a4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="817a4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="817a4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="817a4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="817a4-118">Scenario description</span></span>
<span data-ttu-id="817a4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="817a4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="817a4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="817a4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="817a4-121">A gyűjteményből Pingboard hozzáadása</span><span class="sxs-lookup"><span data-stu-id="817a4-121">Adding Pingboard from the gallery</span></span>
2. <span data-ttu-id="817a4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="817a4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pingboard-from-the-gallery"></a><span data-ttu-id="817a4-123">A gyűjteményből Pingboard hozzáadása</span><span class="sxs-lookup"><span data-stu-id="817a4-123">Adding Pingboard from the gallery</span></span>
<span data-ttu-id="817a4-124">Az Azure AD integrálása a Pingboard konfigurálásához kell hozzáadnia Pingboard a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="817a4-124">To configure the integration of Pingboard into Azure AD, you need to add Pingboard from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="817a4-125">**A gyűjteményből Pingboard hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="817a4-125">**To add Pingboard from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="817a4-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="817a4-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="817a4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="817a4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="817a4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="817a4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="817a4-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="817a4-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="817a4-133">Írja be a keresőmezőbe, **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="817a4-133">In the search box, type **Pingboard**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. <span data-ttu-id="817a4-135">Az eredmények panelen válassza ki a **Pingboard**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="817a4-135">In the results panel, select **Pingboard**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="817a4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="817a4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="817a4-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pingboard.</span><span class="sxs-lookup"><span data-stu-id="817a4-138">In this section, you configure and test Azure AD single sign-on with Pingboard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="817a4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Pingboard a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="817a4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pingboard is to a user in Azure AD.</span></span> <span data-ttu-id="817a4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Pingboard közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="817a4-140">In other words, a link relationship between an Azure AD user and the related user in Pingboard needs to be established.</span></span>

<span data-ttu-id="817a4-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Pingboard a.</span><span class="sxs-lookup"><span data-stu-id="817a4-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Pingboard.</span></span>

<span data-ttu-id="817a4-142">Az Azure AD egyszeri bejelentkezést a Pingboard tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="817a4-142">To configure and test Azure AD single sign-on with Pingboard, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="817a4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="817a4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="817a4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="817a4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="817a4-145">**[Pingboard tesztfelhasználó létrehozása](#creating-a-pingboard-test-user)**  - való Britta Simon valami Pingboard, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="817a4-145">**[Creating a Pingboard test user](#creating-a-pingboard-test-user)** - to have a counterpart of Britta Simon in Pingboard that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="817a4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="817a4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="817a4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="817a4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="817a4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="817a4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="817a4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Pingboard alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="817a4-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Pingboard application.</span></span>

<span data-ttu-id="817a4-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Pingboard, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="817a4-150">**To configure Azure AD single sign-on with Pingboard, perform the following steps:**</span></span>

1. <span data-ttu-id="817a4-151">Az Azure felügyeleti portálján a a **Pingboard** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="817a4-151">In the Azure Management portal, on the **Pingboard** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="817a4-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="817a4-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. <span data-ttu-id="817a4-155">Az a **Pingboard tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="817a4-155">On the **Pingboard Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    <span data-ttu-id="817a4-157">a.</span><span class="sxs-lookup"><span data-stu-id="817a4-157">a.</span></span> <span data-ttu-id="817a4-158">Az a **azonosító** szövegmező, írja be az értéket, mint:`http://<entity-id>.pingboard.com/sp`</span><span class="sxs-lookup"><span data-stu-id="817a4-158">In the **Identifier** textbox, type the value as: `http://<entity-id>.pingboard.com/sp`</span></span>

    <span data-ttu-id="817a4-159">b.</span><span class="sxs-lookup"><span data-stu-id="817a4-159">b.</span></span> <span data-ttu-id="817a4-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<entity-id>.pingboard.com/auth/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="817a4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<entity-id>.pingboard.com/auth/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="817a4-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="817a4-161">Please note that these are not the real values.</span></span> <span data-ttu-id="817a4-162">Akkor kell frissíteni ezeket az értékeket a tényleges azonosítóját és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="817a4-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="817a4-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="817a4-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="817a4-164">Ügyfél [Pingboard ügyfél-támogatási csoport](https://support.pingboard.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="817a4-164">Contact [Pingboard Client support team](https://support.pingboard.com/) to get these values.</span></span> 

4. <span data-ttu-id="817a4-165">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="817a4-165">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    <span data-ttu-id="817a4-167">a.</span><span class="sxs-lookup"><span data-stu-id="817a4-167">a.</span></span> <span data-ttu-id="817a4-168">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket, mint:`http://<sub-domain>.pingboard.com/sign_in`</span><span class="sxs-lookup"><span data-stu-id="817a4-168">In the **Sign-on URL** textbox, type the value as: `http://<sub-domain>.pingboard.com/sign_in`</span></span>
     
5. <span data-ttu-id="817a4-169">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="817a4-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. <span data-ttu-id="817a4-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="817a4-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="817a4-173">Egyszeri bejelentkezés konfigurálása Pingboard oldalon, nyisson meg egy új böngészőablakot, és jelentkezzen be a Pingboard fiókjához.</span><span class="sxs-lookup"><span data-stu-id="817a4-173">To configure SSO on Pingboard side, open a new browser window and log in to your Pingboard Account.</span></span> <span data-ttu-id="817a4-174">Az egyszeri bejelentkezés beállítása Pingboard rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="817a4-174">You must be a Pingboard admin to set up single sign on.</span></span>

8. <span data-ttu-id="817a4-175">A felső menüben válassza ki a **alkalmazások > integrációja**</span><span class="sxs-lookup"><span data-stu-id="817a4-175">From the top menu select **Apps > Integrations**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  <span data-ttu-id="817a4-177">Az a **integrációja** lapon, keresse meg a **"Azure Active Directory"** csempére, majd kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="817a4-177">On the **Integrations** page, find the **"Azure Active Directory"** tile, and click it.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. <span data-ttu-id="817a4-179">A következő kattintson a modális **"Beállítása"**</span><span class="sxs-lookup"><span data-stu-id="817a4-179">In the modal that follows click **"Configure"**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. <span data-ttu-id="817a4-181">A következő oldalon megfigyelheti, hogy "Azure SSO-integráció engedélyezve van.".</span><span class="sxs-lookup"><span data-stu-id="817a4-181">On the following page, you will notice that "Azure SSO Integration is enabled.".</span></span> <span data-ttu-id="817a4-182">A letöltött metaadatok XML-fájl megnyitása a Jegyzettömbben, és illessze be a tartalom **IDP metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="817a4-182">Open the downloaded Metadata XML file in a notepad and paste the content in **IDP Metadata**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. <span data-ttu-id="817a4-184">A fájl érvényesíti, és ha mindent helyes, egyszeri bejelentkezési ezután lesz engedélyezve</span><span class="sxs-lookup"><span data-stu-id="817a4-184">The file will be validated, and if everything is correct, single sign on will now be enabled</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="817a4-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="817a4-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="817a4-186">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="817a4-186">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="817a4-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="817a4-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="817a4-189">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="817a4-189">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="817a4-191">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="817a4-191">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="817a4-193">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="817a4-193">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="817a4-195">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="817a4-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="817a4-197">a.</span><span class="sxs-lookup"><span data-stu-id="817a4-197">a.</span></span> <span data-ttu-id="817a4-198">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="817a4-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="817a4-199">b.</span><span class="sxs-lookup"><span data-stu-id="817a4-199">b.</span></span> <span data-ttu-id="817a4-200">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="817a4-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="817a4-201">c.</span><span class="sxs-lookup"><span data-stu-id="817a4-201">c.</span></span> <span data-ttu-id="817a4-202">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="817a4-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="817a4-203">d.</span><span class="sxs-lookup"><span data-stu-id="817a4-203">d.</span></span> <span data-ttu-id="817a4-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="817a4-204">Click **Create**.</span></span>
 
### <a name="creating-a-pingboard-test-user"></a><span data-ttu-id="817a4-205">Pingboard tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="817a4-205">Creating a Pingboard test user</span></span>

<span data-ttu-id="817a4-206">Ahhoz, hogy az Azure AD-felhasználók Pingboard bejelentkezni, akkor ki kell építenie Pingboard be.</span><span class="sxs-lookup"><span data-stu-id="817a4-206">In order to enable Azure AD users to log into Pingboard, they must be provisioned into Pingboard.</span></span>  
<span data-ttu-id="817a4-207">Pingboard, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="817a4-207">In the case of Pingboard, provisioning is a manual task.</span></span>

<span data-ttu-id="817a4-208">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="817a4-208">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="817a4-209">Jelentkezzen be rendszergazdaként a Pingboard vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="817a4-209">Log in to your Pingboard company site as an administrator.</span></span>

2. <span data-ttu-id="817a4-210">Kattintson a **"Alkalmazott hozzáadása"** gombra **Directory** lap.</span><span class="sxs-lookup"><span data-stu-id="817a4-210">Click **“Add Employee”** button on **Directory** page.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. <span data-ttu-id="817a4-212">Az a **"Alkalmazott hozzáadása"** párbeszédpanel lapon, a következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="817a4-212">On the **“Add Employee”** dialog page, perform the following steps.</span></span>

    ![Felkérése](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    <span data-ttu-id="817a4-214">a.</span><span class="sxs-lookup"><span data-stu-id="817a4-214">a.</span></span> <span data-ttu-id="817a4-215">Az a **teljes nevét** szövegmezőhöz Britta Simon teljes neve.</span><span class="sxs-lookup"><span data-stu-id="817a4-215">In the **Full Name** textbox, type the full name of Britta Simon.</span></span>

    <span data-ttu-id="817a4-216">b.</span><span class="sxs-lookup"><span data-stu-id="817a4-216">b.</span></span> <span data-ttu-id="817a4-217">Az a **E-mail** szövegmezőhöz Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="817a4-217">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="817a4-218">c.</span><span class="sxs-lookup"><span data-stu-id="817a4-218">c.</span></span> <span data-ttu-id="817a4-219">Az a **beosztás** szövegmező, írja be a feladat Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="817a4-219">In the **Job Title** textbox, type the job title of Britta Simon.</span></span>

    <span data-ttu-id="817a4-220">d.</span><span class="sxs-lookup"><span data-stu-id="817a4-220">d.</span></span> <span data-ttu-id="817a4-221">A a **hely** legördülő menüben válassza ki a helyet a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="817a4-221">In the **Location** dropdown, select the location  of Britta Simon.</span></span>
    
    <span data-ttu-id="817a4-222">e.</span><span class="sxs-lookup"><span data-stu-id="817a4-222">e.</span></span> <span data-ttu-id="817a4-223">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="817a4-223">Click **Add**.</span></span>   

4. <span data-ttu-id="817a4-224">A megerősítő képernyőn erősítse meg a felhasználó hozzáadását fog indulni.</span><span class="sxs-lookup"><span data-stu-id="817a4-224">A confirmation screen will come up to confirm the addition of user.</span></span>
    
    ![Erősítse meg](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > <span data-ttu-id="817a4-226">Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="817a4-226">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="817a4-227">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="817a4-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="817a4-228">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés Pingboard Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="817a4-228">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Pingboard.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="817a4-230">**Britta Simon hozzárendelése Pingboard, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="817a4-230">**To assign Britta Simon to Pingboard, perform the following steps:**</span></span>

1. <span data-ttu-id="817a4-231">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="817a4-231">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="817a4-233">Az alkalmazások listában válassza ki a **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="817a4-233">In the applications list, select **Pingboard**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. <span data-ttu-id="817a4-235">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="817a4-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="817a4-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="817a4-237">Click **Add** button.</span></span> <span data-ttu-id="817a4-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="817a4-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="817a4-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="817a4-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="817a4-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="817a4-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="817a4-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="817a4-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="817a4-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="817a4-243">Testing single sign-on</span></span>

<span data-ttu-id="817a4-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="817a4-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="817a4-245">Ha a hozzáférési panelen Pingboard csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Pingboard alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="817a4-245">When you click the Pingboard tile in the Access Panel, you should get automatically signed-on to your Pingboard application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="817a4-246">További források</span><span class="sxs-lookup"><span data-stu-id="817a4-246">Additional resources</span></span>

* [<span data-ttu-id="817a4-247">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="817a4-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="817a4-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="817a4-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
