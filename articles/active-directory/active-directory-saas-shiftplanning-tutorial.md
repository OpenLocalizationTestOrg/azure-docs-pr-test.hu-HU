---
title: "Oktatóanyag: Azure Active Directoryval integrált tárgyában |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és tárgyában között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 327cc1e3d0836e79376e0a3cd5a4422a967f5943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a><span data-ttu-id="de6ac-103">Oktatóanyag: Azure Active Directoryval integrált tárgyában</span><span class="sxs-lookup"><span data-stu-id="de6ac-103">Tutorial: Azure Active Directory integration with Humanity</span></span>

<span data-ttu-id="de6ac-104">Ebben az oktatóanyagban elsajátíthatja tárgyában integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="de6ac-104">In this tutorial, you learn how to integrate Humanity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="de6ac-105">Tárgyában integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="de6ac-105">Integrating Humanity with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="de6ac-106">Megadhatja a tárgyában hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="de6ac-106">You can control in Azure AD who has access to Humanity</span></span>
- <span data-ttu-id="de6ac-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett tárgyában (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="de6ac-107">You can enable your users to automatically get signed-on to Humanity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="de6ac-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="de6ac-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="de6ac-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="de6ac-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de6ac-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de6ac-110">Prerequisites</span></span>

<span data-ttu-id="de6ac-111">Konfigurálása az Azure AD-integrációs tárgyában, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="de6ac-111">To configure Azure AD integration with Humanity, you need the following items:</span></span>

- <span data-ttu-id="de6ac-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="de6ac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="de6ac-113">Egy tárgyában egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="de6ac-113">A Humanity single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="de6ac-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="de6ac-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="de6ac-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="de6ac-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="de6ac-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="de6ac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="de6ac-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="de6ac-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="de6ac-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="de6ac-118">Scenario description</span></span>
<span data-ttu-id="de6ac-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="de6ac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="de6ac-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="de6ac-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="de6ac-121">A gyűjteményből tárgyában hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de6ac-121">Adding Humanity from the gallery</span></span>
2. <span data-ttu-id="de6ac-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de6ac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-humanity-from-the-gallery"></a><span data-ttu-id="de6ac-123">A gyűjteményből tárgyában hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de6ac-123">Adding Humanity from the gallery</span></span>
<span data-ttu-id="de6ac-124">Az Azure AD-be tárgyában integrálása konfigurálásához kell hozzáadnia tárgyában a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="de6ac-124">To configure the integration of Humanity into Azure AD, you need to add Humanity from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="de6ac-125">**A gyűjteményből tárgyában hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de6ac-125">**To add Humanity from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="de6ac-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="de6ac-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="de6ac-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="de6ac-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="de6ac-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="de6ac-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="de6ac-133">Írja be a keresőmezőbe, **tárgyában**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-133">In the search box, type **Humanity**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. <span data-ttu-id="de6ac-135">Az eredmények panelen válassza ki a **tárgyában**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="de6ac-135">In the results panel, select **Humanity**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="de6ac-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de6ac-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="de6ac-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján tárgyában a</span><span class="sxs-lookup"><span data-stu-id="de6ac-138">In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="de6ac-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó tárgyában a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="de6ac-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Humanity is to a user in Azure AD.</span></span> <span data-ttu-id="de6ac-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a tárgyában közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="de6ac-140">In other words, a link relationship between an Azure AD user and the related user in Humanity needs to be established.</span></span>

<span data-ttu-id="de6ac-141">Tárgyában, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="de6ac-141">In Humanity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="de6ac-142">Tárgyában az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="de6ac-142">To configure and test Azure AD single sign-on with Humanity, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="de6ac-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="de6ac-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="de6ac-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="de6ac-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="de6ac-145">**[Tárgyában tesztfelhasználó létrehozása](#creating-a-humanity-test-user)**  - való egy megfelelője a Britta Simon tárgyában, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="de6ac-145">**[Creating a Humanity test user](#creating-a-humanity-test-user)** - to have a counterpart of Britta Simon in Humanity that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="de6ac-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="de6ac-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="de6ac-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="de6ac-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="de6ac-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de6ac-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="de6ac-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az tárgyában alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="de6ac-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Humanity application.</span></span>

<span data-ttu-id="de6ac-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés tárgyában, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de6ac-150">**To configure Azure AD single sign-on with Humanity, perform the following steps:**</span></span>

1. <span data-ttu-id="de6ac-151">Az Azure portálon a a **tárgyában** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-151">In the Azure portal, on the **Humanity** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="de6ac-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="de6ac-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. <span data-ttu-id="de6ac-155">Az a **tárgyában tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="de6ac-155">On the **Humanity Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    <span data-ttu-id="de6ac-157">a.</span><span class="sxs-lookup"><span data-stu-id="de6ac-157">a.</span></span> <span data-ttu-id="de6ac-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://company.humanity.com/includes/saml/`</span><span class="sxs-lookup"><span data-stu-id="de6ac-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://company.humanity.com/includes/saml/`</span></span>

    <span data-ttu-id="de6ac-159">b.</span><span class="sxs-lookup"><span data-stu-id="de6ac-159">b.</span></span> <span data-ttu-id="de6ac-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://company.humanity.com/app/`</span><span class="sxs-lookup"><span data-stu-id="de6ac-160">In the **Identifier** textbox, type a URL using the following pattern: `https://company.humanity.com/app/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="de6ac-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="de6ac-161">These values are not real.</span></span> <span data-ttu-id="de6ac-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="de6ac-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="de6ac-163">Ügyfél [tárgyában ügyfél-támogatási csoport](https://www.humanity.com/support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="de6ac-163">Contact [Humanity Client support team](https://www.humanity.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="de6ac-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="de6ac-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. <span data-ttu-id="de6ac-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="de6ac-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="de6ac-168">A a **tárgyában konfigurációs** kattintson **konfigurálása tárgyában** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="de6ac-168">On the **Humanity Configuration** section, click **Configure Humanity** to open **Configure sign-on** window.</span></span> <span data-ttu-id="de6ac-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="de6ac-169">Copy the **SAML Single Sign-On Service URL, and Sign-Out URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. <span data-ttu-id="de6ac-171">Egy másik webes böngészőablakban, jelentkezzen be a **tárgyában** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="de6ac-171">In a different web browser window, log in to your **Humanity** company site as an administrator.</span></span>

8. <span data-ttu-id="de6ac-172">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-172">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="de6ac-173">![Felügyeleti](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="de6ac-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

9. <span data-ttu-id="de6ac-174">A **integrációs**, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-174">Under **Integration**, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="de6ac-175">![Egyszeri bejelentkezés](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="de6ac-175">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "Single Sign-On")</span></span>

10. <span data-ttu-id="de6ac-176">Az a **egyszeri bejelentkezés** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="de6ac-176">In the **Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="de6ac-177">![Egyszeri bejelentkezés](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="de6ac-177">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="de6ac-178">a.</span><span class="sxs-lookup"><span data-stu-id="de6ac-178">a.</span></span> <span data-ttu-id="de6ac-179">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-179">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="de6ac-180">b.</span><span class="sxs-lookup"><span data-stu-id="de6ac-180">b.</span></span> <span data-ttu-id="de6ac-181">Válassza ki **engedélyezte a bejelentkezést jelszó**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-181">Select **Allow Password Login**.</span></span>

    <span data-ttu-id="de6ac-182">c.</span><span class="sxs-lookup"><span data-stu-id="de6ac-182">c.</span></span> <span data-ttu-id="de6ac-183">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** be értéket a **SAML kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="de6ac-183">Paste the **SAML Single Sign-On Service URL** value into the **SAML Issuer URL** textbox.</span></span>

    <span data-ttu-id="de6ac-184">d.</span><span class="sxs-lookup"><span data-stu-id="de6ac-184">d.</span></span> <span data-ttu-id="de6ac-185">Beillesztés a **Sign-Out URL-cím** be értéket a **távoli kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="de6ac-185">Paste the **Sign-Out URL** value into the **Remote Logout URL** textbox.</span></span>
   
    <span data-ttu-id="de6ac-186">e.</span><span class="sxs-lookup"><span data-stu-id="de6ac-186">e.</span></span> <span data-ttu-id="de6ac-187">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="de6ac-187">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>

11. <span data-ttu-id="de6ac-188">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-188">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="de6ac-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="de6ac-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="de6ac-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="de6ac-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="de6ac-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="de6ac-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="de6ac-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de6ac-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="de6ac-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="de6ac-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="de6ac-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de6ac-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="de6ac-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="de6ac-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="de6ac-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="de6ac-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="de6ac-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="de6ac-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="de6ac-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="de6ac-204">a.</span><span class="sxs-lookup"><span data-stu-id="de6ac-204">a.</span></span> <span data-ttu-id="de6ac-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="de6ac-206">b.</span><span class="sxs-lookup"><span data-stu-id="de6ac-206">b.</span></span> <span data-ttu-id="de6ac-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="de6ac-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="de6ac-208">c.</span><span class="sxs-lookup"><span data-stu-id="de6ac-208">c.</span></span> <span data-ttu-id="de6ac-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="de6ac-210">d.</span><span class="sxs-lookup"><span data-stu-id="de6ac-210">d.</span></span> <span data-ttu-id="de6ac-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="de6ac-211">Click **Create**.</span></span>
 
### <a name="creating-a-humanity-test-user"></a><span data-ttu-id="de6ac-212">Tárgyában tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de6ac-212">Creating a Humanity test user</span></span>

<span data-ttu-id="de6ac-213">Ahhoz, hogy az Azure AD-felhasználók tárgyában bejelentkezni, akkor ki kell építenie tárgyában történő.</span><span class="sxs-lookup"><span data-stu-id="de6ac-213">In order to enable Azure AD users to log in to Humanity, they must be provisioned into Humanity.</span></span> <span data-ttu-id="de6ac-214">Tárgyában, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="de6ac-214">In the case of Humanity, provisioning is a manual task.</span></span>

<span data-ttu-id="de6ac-215">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de6ac-215">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="de6ac-216">Jelentkezzen be a **tárgyában** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="de6ac-216">Log in to your **Humanity** company site as an administrator.</span></span>

2. <span data-ttu-id="de6ac-217">Kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-217">Click **Admin**.</span></span>
   
    <span data-ttu-id="de6ac-218">![Felügyeleti](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="de6ac-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

3. <span data-ttu-id="de6ac-219">Kattintson a **személyzet**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-219">Click **Staff**.</span></span>
   
    <span data-ttu-id="de6ac-220">![Személyzet](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "személyzet")</span><span class="sxs-lookup"><span data-stu-id="de6ac-220">![Staff](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "Staff")</span></span>

4. <span data-ttu-id="de6ac-221">A **műveletek**, kattintson a **adja hozzá az alkalmazottak**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-221">Under **Actions**, click **Add Employees**.</span></span>
   
    <span data-ttu-id="de6ac-222">![Adja hozzá az alkalmazottak](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "alkalmazottak hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="de6ac-222">![Add Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "Add Employees")</span></span>

5. <span data-ttu-id="de6ac-223">Az a **adja hozzá az alkalmazottak** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="de6ac-223">In the **Add Employees** section, perform the following steps:</span></span>
   
    <span data-ttu-id="de6ac-224">![Az alkalmazottak mentése](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "alkalmazottak mentése")</span><span class="sxs-lookup"><span data-stu-id="de6ac-224">![Save Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "Save Employees")</span></span>
   
    <span data-ttu-id="de6ac-225">a.</span><span class="sxs-lookup"><span data-stu-id="de6ac-225">a.</span></span> <span data-ttu-id="de6ac-226">Típus a **Utónév**, **Vezetéknév**, és **E-mail** szeretné azokat a kapcsolódó szövegmezők rendelkezés érvényes AAD-fiók.</span><span class="sxs-lookup"><span data-stu-id="de6ac-226">Type the **First Name**, **Last Name**, and **Email** of a valid AAD account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="de6ac-227">b.</span><span class="sxs-lookup"><span data-stu-id="de6ac-227">b.</span></span> <span data-ttu-id="de6ac-228">Kattintson a **alkalmazottak mentése**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-228">Click **Save Employees**.</span></span>

>[!NOTE]
><span data-ttu-id="de6ac-229">Bármely más tárgyában felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz tárgyában által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="de6ac-229">You can use any other Humanity user account creation tools or APIs provided by Humanity to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="de6ac-230">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="de6ac-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="de6ac-231">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés tárgyában Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="de6ac-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Humanity.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="de6ac-233">**Britta Simon hozzárendelése tárgyában, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de6ac-233">**To assign Britta Simon to Humanity, perform the following steps:**</span></span>

1. <span data-ttu-id="de6ac-234">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="de6ac-236">Az alkalmazások listában válassza ki a **tárgyában**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-236">In the applications list, select **Humanity**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. <span data-ttu-id="de6ac-238">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="de6ac-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="de6ac-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="de6ac-240">Click **Add** button.</span></span> <span data-ttu-id="de6ac-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de6ac-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="de6ac-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="de6ac-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="de6ac-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de6ac-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="de6ac-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de6ac-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="de6ac-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="de6ac-246">Testing single sign-on</span></span>

<span data-ttu-id="de6ac-247">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="de6ac-247">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="de6ac-248">Ha a hozzáférési panelen tárgyában csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az tárgyában alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="de6ac-248">When you click the Humanity tile in the Access Panel, you should get automatically signed-on to your Humanity application.</span></span>
<span data-ttu-id="de6ac-249">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="de6ac-249">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="de6ac-250">További források</span><span class="sxs-lookup"><span data-stu-id="de6ac-250">Additional resources</span></span>

* [<span data-ttu-id="de6ac-251">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="de6ac-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="de6ac-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="de6ac-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

