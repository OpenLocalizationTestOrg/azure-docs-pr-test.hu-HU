---
title: "Oktatóanyag: Azure Active Directoryval integrált RFPIO |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és RFPIO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 26a8bb17dad5a01b401ce7f9b484f09822825cbf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a><span data-ttu-id="65921-103">Oktatóanyag: Azure Active Directoryval integrált RFPIO</span><span class="sxs-lookup"><span data-stu-id="65921-103">Tutorial: Azure Active Directory integration with RFPIO</span></span>

<span data-ttu-id="65921-104">Ebben az oktatóanyagban elsajátíthatja RFPIO integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="65921-104">In this tutorial, you learn how to integrate RFPIO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="65921-105">RFPIO integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="65921-105">Integrating RFPIO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="65921-106">Az Azure AD, aki hozzáfér RFPIO ki szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="65921-106">You can control who in Azure AD who has access to RFPIO.</span></span>
- <span data-ttu-id="65921-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett RFPIO (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="65921-107">You can enable your users to automatically get signed-on to RFPIO (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="65921-108">A fiók egyetlen központi helyen--az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="65921-108">You can manage your accounts in one central location--the Azure portal.</span></span>

<span data-ttu-id="65921-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="65921-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65921-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="65921-110">Prerequisites</span></span>

<span data-ttu-id="65921-111">Konfigurálása az Azure AD-integrációs RFPIO, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="65921-111">To configure Azure AD integration with RFPIO, you need the following items:</span></span>

- <span data-ttu-id="65921-112">Az Azure AD-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="65921-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="65921-113">RFPIO egyszeri bejelentkezést a alkalmas előfizetés.</span><span class="sxs-lookup"><span data-stu-id="65921-113">A RFPIO single sign-on-enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="65921-114">Nem ajánlott, hogy éles környezetben segítségével ellenőrizze a lépéseket, ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="65921-114">We don't recommend that you use a production environment to test the steps in this tutorial.</span></span>

<span data-ttu-id="65921-115">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="65921-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="65921-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="65921-116">Do not use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="65921-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65921-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="65921-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="65921-118">Scenario description</span></span>
<span data-ttu-id="65921-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="65921-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="65921-120">Az ebben az oktatóanyagban a forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="65921-120">The scenario that's outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="65921-121">RFPIO hozzáadása a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="65921-121">Adding RFPIO from the gallery.</span></span>
2. <span data-ttu-id="65921-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="65921-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-rfpio-from-the-gallery"></a><span data-ttu-id="65921-123">Adja hozzá a RFPIO a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="65921-123">Add RFPIO from the gallery</span></span>
<span data-ttu-id="65921-124">Az Azure AD integrálása a RFPIO konfigurálásához kell hozzáadnia RFPIO a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="65921-124">To configure the integration of RFPIO into Azure AD, you need to add RFPIO from the gallery to your list of managed SaaS apps.</span></span>

### <a name="to-add-rfpio-from-the-gallery"></a><span data-ttu-id="65921-125">A gyűjteményből RFPIO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65921-125">To add RFPIO from the gallery</span></span>

1. <span data-ttu-id="65921-126">Az a  **[Azure-portálon](https://portal.azure.com)**, a bal oldali navigációs panelen válassza ki a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="65921-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation pane, select the **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="65921-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="65921-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="65921-130">Egy új alkalmazást szeretne telepíteni, válassza ki a **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="65921-130">To add a new application, select the **New application** button on the top of dialog box.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="65921-132">Írja be a keresőmezőbe, **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="65921-132">In the search box, type **RFPIO**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. <span data-ttu-id="65921-134">Az eredmények panelen válassza ki a **RFPIO**, majd válassza ki a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65921-134">In the results panel, select **RFPIO**, and then select the **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="65921-136">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65921-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="65921-137">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján RFPIO.</span><span class="sxs-lookup"><span data-stu-id="65921-137">In this section, you configure and test Azure AD single sign-on with RFPIO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="65921-138">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a kapcsolat Újdonságok RFPIO megfelelőjére felhasználó, ezért a felhasználó az Azure AD között.</span><span class="sxs-lookup"><span data-stu-id="65921-138">For single sign-on to work, Azure AD needs to know what the relationship is between counterpart user in RFPIO and a user in Azure AD.</span></span> <span data-ttu-id="65921-139">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a RFPIO közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="65921-139">In other words, a link relationship between an Azure AD user and the related user in RFPIO needs to be established.</span></span>

<span data-ttu-id="65921-140">RFPIO, rendelje hozzá a értékének **felhasználónév** értékeként Azure AD-ben **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="65921-140">In RFPIO, assign the value of **user name** in Azure AD as the value of  **Username** to establish the link relationship.</span></span>

<span data-ttu-id="65921-141">Az Azure AD egyszeri bejelentkezést a RFPIO tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="65921-141">To configure and test Azure AD single sign-on with RFPIO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="65921-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**– ahhoz, hogy a felhasználók számára a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="65921-142">**[Configure Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**--to enable your users to use this feature.</span></span>
2. <span data-ttu-id="65921-143">**[Hozzon létre egy Azure AD-teszt felhasználó](#creating-an-azure-ad-test-user)**--az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="65921-143">**[Create an Azure AD test user](#creating-an-azure-ad-test-user)**-- to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="65921-144">**[RFPIO tesztfelhasználó létrehozása](#creating-a-rfpio-test-user)**  --való Britta Simon valami RFPIO, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="65921-144">**[Create a RFPIO test user](#creating-a-rfpio-test-user)** --to have a counterpart of Britta Simon in RFPIO that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="65921-145">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assigning-the-azure-ad-test-user)**--az Azure AD egyszeri bejelentkezéshez használandó Britta Simon engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="65921-145">**[Assign the Azure AD test user](#assigning-the-azure-ad-test-user)**--to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="65921-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  --működik, ha a konfiguráció ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="65921-146">**[Test Single Sign-On](#testing-single-sign-on)** --to verify if the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="65921-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65921-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="65921-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az RFPIO alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="65921-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RFPIO application.</span></span>

<span data-ttu-id="65921-149">**Konfigurálása az Azure AD az egyszeri bejelentkezés RFPIO, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="65921-149">**To configure Azure AD single sign-on with RFPIO, perform the following steps:**</span></span>

1. <span data-ttu-id="65921-150">Az Azure portálon a a **RFPIO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="65921-150">In the Azure portal, on the **RFPIO** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="65921-152">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="65921-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. <span data-ttu-id="65921-154">Az a **RFPIO tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="65921-154">On the **RFPIO Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    <span data-ttu-id="65921-156">a.</span><span class="sxs-lookup"><span data-stu-id="65921-156">a.</span></span> <span data-ttu-id="65921-157">Az a **azonosító** szövegmező, írja be az URL-cím:`https://www.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="65921-157">In the **Identifier** textbox, type the URL: `https://www.rfpio.com`</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    <span data-ttu-id="65921-159">b.</span><span class="sxs-lookup"><span data-stu-id="65921-159">b.</span></span> <span data-ttu-id="65921-160">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="65921-160">Check **Show advanced URL settings**.</span></span>

    <span data-ttu-id="65921-161">c.</span><span class="sxs-lookup"><span data-stu-id="65921-161">c.</span></span> <span data-ttu-id="65921-162">Az a **továbbítási állapotot** szövegmezőben adjon meg egy karakterláncértéket.</span><span class="sxs-lookup"><span data-stu-id="65921-162">In the **Relay State** textbox enter a string value.</span></span> <span data-ttu-id="65921-163">Ügyfél [RFPIO támogatási csoport](https://www.rfpio.com/contact/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="65921-163">Contact [RFPIO support team](https://www.rfpio.com/contact/) to get this value.</span></span> 

4. <span data-ttu-id="65921-164">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="65921-164">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="65921-165">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="65921-165">If you wish to configure the application in **SP** initiated mode:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    <span data-ttu-id="65921-167">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://www.app.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="65921-167">In the **Sign on URL** textbox, type the URL: `https://www.app.rfpio.com`</span></span>

5. <span data-ttu-id="65921-168">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="65921-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. <span data-ttu-id="65921-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="65921-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="65921-172">Egy másik webes böngészőablakban, jelentkezzen be a **RFPIO** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="65921-172">In a different web browser window, login to the **RFPIO** website as an administrator.</span></span>

8. <span data-ttu-id="65921-173">Kattintson az alsó bal sarok legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="65921-173">Click on the bottom left corner dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. <span data-ttu-id="65921-175">Kattintson a **szervezeti beállítások**.</span><span class="sxs-lookup"><span data-stu-id="65921-175">Click on the **Organization Settings**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. <span data-ttu-id="65921-177">Kattintson a **szolgáltatások & integrációs**.</span><span class="sxs-lookup"><span data-stu-id="65921-177">Click on the **FEATURES & INTEGRATION**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. <span data-ttu-id="65921-179">Az a **SAML SSO konfigurációs** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="65921-179">In the **SAML SSO Configuration** Click **Edit**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. <span data-ttu-id="65921-181">Ebben a szakaszban hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="65921-181">In this Section perform following actions:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    <span data-ttu-id="65921-183">a.</span><span class="sxs-lookup"><span data-stu-id="65921-183">a.</span></span> <span data-ttu-id="65921-184">Másolja a **metaadatainak XML-kódja letöltött** és illessze be azt a **identitáskonfigurációs** mező.</span><span class="sxs-lookup"><span data-stu-id="65921-184">Copy the content of the **Downloaded Metadata XML** and paste it into the **identity configuration** field.</span></span>

    > [!NOTE]
    ><span data-ttu-id="65921-185">Másolja a tartalmat a letöltött **metaadatainak XML-kódja** használata **Jegyzettömb ++** vagy megfelelő **XML-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="65921-185">To copy the content of downloaded **Metadata XML** Use **Notepad++** or proper **XML Editor**.</span></span> 

    <span data-ttu-id="65921-186">b.</span><span class="sxs-lookup"><span data-stu-id="65921-186">b.</span></span> <span data-ttu-id="65921-187">Kattintson a **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="65921-187">Click **Validate**.</span></span>

    <span data-ttu-id="65921-188">c.</span><span class="sxs-lookup"><span data-stu-id="65921-188">c.</span></span> <span data-ttu-id="65921-189">Miután rákattintott **érvényesítése**, tükrözés **SAML(Enabled)** a on.</span><span class="sxs-lookup"><span data-stu-id="65921-189">After Clicking **Validate**, Flip **SAML(Enabled)** to on.</span></span>

    <span data-ttu-id="65921-190">d.</span><span class="sxs-lookup"><span data-stu-id="65921-190">d.</span></span> <span data-ttu-id="65921-191">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="65921-191">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="65921-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="65921-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="65921-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="65921-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="65921-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="65921-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="65921-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="65921-195">Create an Azure AD test user</span></span>
<span data-ttu-id="65921-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="65921-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="65921-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="65921-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="65921-199">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="65921-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="65921-201">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="65921-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="65921-203">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="65921-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="65921-205">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="65921-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="65921-207">a.</span><span class="sxs-lookup"><span data-stu-id="65921-207">a.</span></span> <span data-ttu-id="65921-208">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="65921-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="65921-209">b.</span><span class="sxs-lookup"><span data-stu-id="65921-209">b.</span></span> <span data-ttu-id="65921-210">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="65921-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="65921-211">c.</span><span class="sxs-lookup"><span data-stu-id="65921-211">c.</span></span> <span data-ttu-id="65921-212">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="65921-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="65921-213">d.</span><span class="sxs-lookup"><span data-stu-id="65921-213">d.</span></span> <span data-ttu-id="65921-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="65921-214">Click **Create**.</span></span>
 
### <a name="create-a-rfpio-test-user"></a><span data-ttu-id="65921-215">RFPIO tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="65921-215">Create a RFPIO test user</span></span>

<span data-ttu-id="65921-216">Ahhoz, hogy az Azure AD-felhasználók RFPIO bejelentkezni, akkor ki kell építenie a RFPIO.</span><span class="sxs-lookup"><span data-stu-id="65921-216">To enable Azure AD users to log in to RFPIO, they must be provisioned into RFPIO.</span></span>  
<span data-ttu-id="65921-217">RFPIO, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="65921-217">In the case of RFPIO, provisioning is a manual task.</span></span>

<span data-ttu-id="65921-218">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="65921-218">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="65921-219">Jelentkezzen be rendszergazdaként a RFPIO vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="65921-219">Log in to your RFPIO company site as an administrator.</span></span>

2. <span data-ttu-id="65921-220">Kattintson az alsó bal sarok legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="65921-220">Click on the bottom left corner dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. <span data-ttu-id="65921-222">Kattintson a **szervezeti beállítások**.</span><span class="sxs-lookup"><span data-stu-id="65921-222">Click on the **Organization Settings**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. <span data-ttu-id="65921-224">Kattintson a **CSAPATTAGOK**.</span><span class="sxs-lookup"><span data-stu-id="65921-224">Click **TEAM MEMBERS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. <span data-ttu-id="65921-226">Kattintson a **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="65921-226">Click on **ADD MEMBERS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. <span data-ttu-id="65921-228">Az a **új tagok hozzáadása** szakasz.</span><span class="sxs-lookup"><span data-stu-id="65921-228">In the **Add New Members** section.</span></span> <span data-ttu-id="65921-229">Hajtsa végre az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="65921-229">Perform following actions:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app8.png)

    <span data-ttu-id="65921-231">a.</span><span class="sxs-lookup"><span data-stu-id="65921-231">a.</span></span> <span data-ttu-id="65921-232">Adjon meg **E-mail cím** a a **adjon meg soronként egy e-mail** mező.</span><span class="sxs-lookup"><span data-stu-id="65921-232">Enter **Email address** in the **Enter one email per line** field.</span></span>

    <span data-ttu-id="65921-233">b.</span><span class="sxs-lookup"><span data-stu-id="65921-233">b.</span></span> <span data-ttu-id="65921-234">Adja válassza **szerepkör** igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="65921-234">Plese select **Role** according your requirements.</span></span>

    <span data-ttu-id="65921-235">c.</span><span class="sxs-lookup"><span data-stu-id="65921-235">c.</span></span> <span data-ttu-id="65921-236">Kattintson a **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="65921-236">Click **ADD MEMBERS**.</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="65921-237">Az Azure Active Directory fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="65921-237">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="65921-238">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="65921-238">Assign the Azure AD test user</span></span>

<span data-ttu-id="65921-239">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés RFPIO Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="65921-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RFPIO.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="65921-241">**Britta Simon hozzárendelése RFPIO, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="65921-241">**To assign Britta Simon to RFPIO, perform the following steps:**</span></span>

1. <span data-ttu-id="65921-242">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="65921-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="65921-244">Az alkalmazások listában válassza ki a **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="65921-244">In the applications list, select **RFPIO**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. <span data-ttu-id="65921-246">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="65921-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="65921-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="65921-248">Click **Add** button.</span></span> <span data-ttu-id="65921-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65921-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="65921-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="65921-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="65921-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65921-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="65921-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65921-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="65921-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="65921-254">Test single sign-on</span></span>

<span data-ttu-id="65921-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="65921-255">In this section, you test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="65921-256">Ha a hozzáférési panelen RFPIO csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az RFPIO alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="65921-256">When you click the RFPIO tile in the Access Panel, you should get automatically signed-on to your RFPIO application.</span></span>
<span data-ttu-id="65921-257">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="65921-257">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65921-258">További források</span><span class="sxs-lookup"><span data-stu-id="65921-258">Additional resources</span></span>

* [<span data-ttu-id="65921-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="65921-259">List of tutorials about how to integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="65921-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="65921-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

