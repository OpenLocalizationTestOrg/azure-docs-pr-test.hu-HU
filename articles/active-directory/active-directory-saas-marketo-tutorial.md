---
title: "Oktatóanyag: Azure Active Directoryval integrált Marketo |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Marketo között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: e146fd5a8075bc9c7ba049b25e5f301fc2645ed9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a><span data-ttu-id="33813-103">Oktatóanyag: Azure Active Directoryval integrált Marketo</span><span class="sxs-lookup"><span data-stu-id="33813-103">Tutorial: Azure Active Directory integration with Marketo</span></span>

<span data-ttu-id="33813-104">Ebben az oktatóanyagban elsajátíthatja a Marketo integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="33813-104">In this tutorial, you learn how to integrate Marketo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="33813-105">A Marketo integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="33813-105">Integrating Marketo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="33813-106">Szabályozhatja, aki hozzáfér a Marketo Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="33813-106">You can control in Azure AD who has access to Marketo</span></span>
- <span data-ttu-id="33813-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Marketo (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="33813-107">You can enable your users to automatically get signed-on to Marketo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="33813-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="33813-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="33813-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="33813-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33813-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="33813-110">Prerequisites</span></span>

<span data-ttu-id="33813-111">Az Azure AD rendszerrel történő integráció konfigurálása a Marketo, a következő elemeket kell:</span><span class="sxs-lookup"><span data-stu-id="33813-111">To configure Azure AD integration with Marketo, you need the following items:</span></span>

- <span data-ttu-id="33813-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="33813-112">An Azure AD subscription</span></span>
- <span data-ttu-id="33813-113">A Marketo egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="33813-113">A Marketo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="33813-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="33813-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="33813-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="33813-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="33813-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="33813-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="33813-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="33813-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="33813-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="33813-118">Scenario description</span></span>
<span data-ttu-id="33813-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="33813-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="33813-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="33813-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="33813-121">A Marketo hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="33813-121">Adding Marketo from the gallery</span></span>
2. <span data-ttu-id="33813-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="33813-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-marketo-from-the-gallery"></a><span data-ttu-id="33813-123">A Marketo hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="33813-123">Adding Marketo from the gallery</span></span>
<span data-ttu-id="33813-124">Az Azure AD integrálása a Marketo konfigurálásához kell hozzáadnia a Marketo a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="33813-124">To configure the integration of Marketo into Azure AD, you need to add Marketo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="33813-125">**A Marketo hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="33813-125">**To add Marketo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="33813-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="33813-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="33813-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="33813-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="33813-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="33813-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="33813-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="33813-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="33813-133">Írja be a keresőmezőbe, **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="33813-133">In the search box, type **Marketo**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. <span data-ttu-id="33813-135">Az eredmények panelen válassza ki a **Marketo**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33813-135">In the results panel, select **Marketo**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="33813-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="33813-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="33813-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Marketo "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="33813-138">In this section, you configure and test Azure AD single sign-on with Marketo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="33813-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Marketo tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="33813-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Marketo is to a user in Azure AD.</span></span> <span data-ttu-id="33813-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Marketo közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="33813-140">In other words, a link relationship between an Azure AD user and the related user in Marketo needs to be established.</span></span>

<span data-ttu-id="33813-141">A Marketo, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="33813-141">In Marketo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="33813-142">Az Azure AD egyszeri bejelentkezést a Marketo tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="33813-142">To configure and test Azure AD single sign-on with Marketo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="33813-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="33813-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="33813-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="33813-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="33813-145">**[A Marketo tesztfelhasználó létrehozása](#creating-a-marketo-test-user)**  - való Britta Simon valami Marketo, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="33813-145">**[Creating a Marketo test user](#creating-a-marketo-test-user)** - to have a counterpart of Britta Simon in Marketo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="33813-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="33813-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="33813-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="33813-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="33813-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="33813-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="33813-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Marketo-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="33813-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Marketo application.</span></span>

<span data-ttu-id="33813-150">**A Marketo konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="33813-150">**To configure Azure AD single sign-on with Marketo, perform the following steps:**</span></span>

1. <span data-ttu-id="33813-151">Az Azure portálon a a **Marketo** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="33813-151">In the Azure portal, on the **Marketo** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="33813-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="33813-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. <span data-ttu-id="33813-155">Az a **Marketo-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="33813-155">On the **Marketo Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    <span data-ttu-id="33813-157">a.</span><span class="sxs-lookup"><span data-stu-id="33813-157">a.</span></span> <span data-ttu-id="33813-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://saml.marketo.com/sp`</span><span class="sxs-lookup"><span data-stu-id="33813-158">In the **Identifier** textbox, type a URL using the following pattern: `https://saml.marketo.com/sp`</span></span>

    <span data-ttu-id="33813-159">b.</span><span class="sxs-lookup"><span data-stu-id="33813-159">b.</span></span> <span data-ttu-id="33813-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://login.marketo.com/saml/assertion/\<munchkinid\>`</span><span class="sxs-lookup"><span data-stu-id="33813-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://login.marketo.com/saml/assertion/\<munchkinid\>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="33813-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="33813-161">These values are not real.</span></span> <span data-ttu-id="33813-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="33813-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="33813-163">Ügyfél [Marketo támogatási csoport](http://investors.marketo.com/contactus.cfm) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="33813-163">Contact [Marketo support team](http://investors.marketo.com/contactus.cfm) to get these values.</span></span>
 
4. <span data-ttu-id="33813-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="33813-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. <span data-ttu-id="33813-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="33813-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="33813-168">A a **Marketo konfigurációs** kattintson **konfigurálása Marketo** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="33813-168">On the **Marketo Configuration** section, click **Configure Marketo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="33813-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="33813-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. <span data-ttu-id="33813-171">Ahhoz, hogy az alkalmazás Munchkin azonosítója, jelentkezzen be a rendszergazda hitelesítő adataival Marketo, majd hajtsa végre a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="33813-171">To get Munchkin Id of your application, log in to Marketo using admin credentials and perform following actions:</span></span>
   
    <span data-ttu-id="33813-172">a.</span><span class="sxs-lookup"><span data-stu-id="33813-172">a.</span></span> <span data-ttu-id="33813-173">Jelentkezzen be rendszergazdai hitelesítő adataival Marketo-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33813-173">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="33813-174">b.</span><span class="sxs-lookup"><span data-stu-id="33813-174">b.</span></span> <span data-ttu-id="33813-175">Kattintson a **Admin** gombra a felső navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="33813-175">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="33813-177">c.</span><span class="sxs-lookup"><span data-stu-id="33813-177">c.</span></span> <span data-ttu-id="33813-178">Az integráció menüben keresse meg és kattintson a **Munchkin hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="33813-178">Navigate to the Integration menu and click the **Munchkin link**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    <span data-ttu-id="33813-180">d.</span><span class="sxs-lookup"><span data-stu-id="33813-180">d.</span></span> <span data-ttu-id="33813-181">Másolja a Munchkin azonosítót, a képernyőn megjelenő, és fejezze be a válasz URL-CÍMEN az Azure AD konfigurálása varázsló.</span><span class="sxs-lookup"><span data-stu-id="33813-181">Copy the Munchkin Id shown on the screen and complete your Reply URL in the Azure AD configuration wizard.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. <span data-ttu-id="33813-183">Az alkalmazás az SSO konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="33813-183">To configure the SSO in the application, follow the below steps:</span></span>
   
    <span data-ttu-id="33813-184">a.</span><span class="sxs-lookup"><span data-stu-id="33813-184">a.</span></span> <span data-ttu-id="33813-185">Jelentkezzen be rendszergazdai hitelesítő adataival Marketo-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33813-185">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="33813-186">b.</span><span class="sxs-lookup"><span data-stu-id="33813-186">b.</span></span> <span data-ttu-id="33813-187">Kattintson a **Admin** gombra a felső navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="33813-187">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="33813-189">c.</span><span class="sxs-lookup"><span data-stu-id="33813-189">c.</span></span> <span data-ttu-id="33813-190">Az integráció menüben keresse meg és kattintson **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="33813-190">Navigate to the Integration menu and click **Single Sign On**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    <span data-ttu-id="33813-192">d.</span><span class="sxs-lookup"><span data-stu-id="33813-192">d.</span></span> <span data-ttu-id="33813-193">A SAML-beállítások engedélyezéséhez kattintson **szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="33813-193">To enable the SAML Settings, click **Edit** button.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    <span data-ttu-id="33813-195">e.</span><span class="sxs-lookup"><span data-stu-id="33813-195">e.</span></span> <span data-ttu-id="33813-196">**Engedélyezett** egyszeri bejelentkezés beállításait.</span><span class="sxs-lookup"><span data-stu-id="33813-196">**Enabled** Single Sign-On settings.</span></span>
   
    <span data-ttu-id="33813-197">f.</span><span class="sxs-lookup"><span data-stu-id="33813-197">f.</span></span> <span data-ttu-id="33813-198">Beillesztés a **SAML Entitásazonosító**, a a **kibocsátó azonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="33813-198">Paste the **SAML Entity ID**, in the **Issuer ID** textbox.</span></span>
   
    <span data-ttu-id="33813-199">g.</span><span class="sxs-lookup"><span data-stu-id="33813-199">g.</span></span> <span data-ttu-id="33813-200">Az a **Entitásazonosító** szövegmező, adja meg az URL-címet `http://saml.marketo.com/sp`.</span><span class="sxs-lookup"><span data-stu-id="33813-200">In the **Entity ID** textbox, enter the URL as `http://saml.marketo.com/sp`.</span></span>
   
    <span data-ttu-id="33813-201">h.</span><span class="sxs-lookup"><span data-stu-id="33813-201">h.</span></span> <span data-ttu-id="33813-202">Válassza ki a felhasználói azonosító helyének **névazonosítója elem**.</span><span class="sxs-lookup"><span data-stu-id="33813-202">Select the User ID Location as **Name Identifier element**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > <span data-ttu-id="33813-204">Ha a felhasználói azonosító nem UPN-értéket, majd módosítsa az értéket a attribútum fülre.</span><span class="sxs-lookup"><span data-stu-id="33813-204">If your User Identifier is not UPN value then change the value in the Attribute tab.</span></span>
   
    <span data-ttu-id="33813-205">i.</span><span class="sxs-lookup"><span data-stu-id="33813-205">i.</span></span> <span data-ttu-id="33813-206">Töltse fel a tanúsítványt, amely már letöltötte az Azure AD konfigurálása varázsló.</span><span class="sxs-lookup"><span data-stu-id="33813-206">Upload the certificate, which you have downloaded from Azure AD configuration wizard.</span></span> <span data-ttu-id="33813-207">**Mentés** a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="33813-207">**Save** the settings.</span></span>
   
    <span data-ttu-id="33813-208">j.</span><span class="sxs-lookup"><span data-stu-id="33813-208">j.</span></span> <span data-ttu-id="33813-209">Az átirányítási hibalapok beállításainak szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="33813-209">Edit the Redirect Pages settings.</span></span>
   
    <span data-ttu-id="33813-210">k.</span><span class="sxs-lookup"><span data-stu-id="33813-210">k.</span></span> <span data-ttu-id="33813-211">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="33813-211">Paste the **SAML Single Sign-On Service URL** in the **Login URL** textbox.</span></span>
   
    <span data-ttu-id="33813-212">l.</span><span class="sxs-lookup"><span data-stu-id="33813-212">l.</span></span> <span data-ttu-id="33813-213">Beillesztés a **Sign-Out URL-cím** a a **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="33813-213">Paste the **Sign-Out URL** in the **Logout URL** textbox.</span></span>
   
    <span data-ttu-id="33813-214">m.</span><span class="sxs-lookup"><span data-stu-id="33813-214">m.</span></span> <span data-ttu-id="33813-215">Az a **hiba URL-cím**, másolása a **Marketo-példány URL-címe** kattintson **mentése** gombra kattintva mentse a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="33813-215">In the **Error URL**, copy your **Marketo instance URL** and click **Save** button to save settings.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. <span data-ttu-id="33813-217">Ahhoz, hogy a felhasználók számára az egyszeri Bejelentkezést, hajtsa végre a következő műveletet:</span><span class="sxs-lookup"><span data-stu-id="33813-217">To enable the SSO for users, complete the following actions:</span></span>
   
    <span data-ttu-id="33813-218">a.</span><span class="sxs-lookup"><span data-stu-id="33813-218">a.</span></span> <span data-ttu-id="33813-219">Jelentkezzen be rendszergazdai hitelesítő adataival Marketo-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33813-219">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="33813-220">b.</span><span class="sxs-lookup"><span data-stu-id="33813-220">b.</span></span> <span data-ttu-id="33813-221">Kattintson a **Admin** gombra a felső navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="33813-221">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="33813-223">c.</span><span class="sxs-lookup"><span data-stu-id="33813-223">c.</span></span> <span data-ttu-id="33813-224">Keresse meg a **biztonsági** menüre, majd **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="33813-224">Navigate to the **Security** menu and click **Login Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    <span data-ttu-id="33813-226">d.</span><span class="sxs-lookup"><span data-stu-id="33813-226">d.</span></span> <span data-ttu-id="33813-227">Ellenőrizze a **szükséges SSO** beállítás és **mentése** a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="33813-227">Check the **Require SSO** option and **Save** the settings.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> <span data-ttu-id="33813-229">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="33813-229">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="33813-230">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="33813-230">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="33813-231">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="33813-231">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="33813-232">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="33813-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="33813-233">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="33813-233">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="33813-235">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="33813-235">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="33813-236">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="33813-236">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="33813-238">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="33813-238">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="33813-240">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="33813-240">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="33813-242">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="33813-242">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="33813-244">a.</span><span class="sxs-lookup"><span data-stu-id="33813-244">a.</span></span> <span data-ttu-id="33813-245">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="33813-245">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="33813-246">b.</span><span class="sxs-lookup"><span data-stu-id="33813-246">b.</span></span> <span data-ttu-id="33813-247">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="33813-247">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="33813-248">c.</span><span class="sxs-lookup"><span data-stu-id="33813-248">c.</span></span> <span data-ttu-id="33813-249">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="33813-249">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="33813-250">d.</span><span class="sxs-lookup"><span data-stu-id="33813-250">d.</span></span> <span data-ttu-id="33813-251">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="33813-251">Click **Create**.</span></span>
 
### <a name="creating-a-marketo-test-user"></a><span data-ttu-id="33813-252">A Marketo tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="33813-252">Creating a Marketo test user</span></span>

<span data-ttu-id="33813-253">Ebben a szakaszban a Marketo Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="33813-253">In this section, you create a user called Britta Simon in Marketo.</span></span> <span data-ttu-id="33813-254">kövesse az alábbi lépéseket az olyan felhasználók létrehozása az Marketo-platformokon.</span><span class="sxs-lookup"><span data-stu-id="33813-254">follow these steps to create a user in Marketo platform.</span></span>

1. <span data-ttu-id="33813-255">Jelentkezzen be rendszergazdai hitelesítő adataival Marketo-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="33813-255">Log in to Marketo app using admin credentials.</span></span>

2. <span data-ttu-id="33813-256">Kattintson a **Admin** gombra a felső navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="33813-256">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. <span data-ttu-id="33813-258">Keresse meg a **biztonsági** menüre, majd **felhasználók és szerepkörök**</span><span class="sxs-lookup"><span data-stu-id="33813-258">Navigate to the **Security** menu and click **Users & Roles**</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. <span data-ttu-id="33813-260">Kattintson a **hívhat meg új felhasználói** hivatkozás a felhasználók lapon</span><span class="sxs-lookup"><span data-stu-id="33813-260">Click the **Invite New User** link on the Users tab</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. <span data-ttu-id="33813-262">Töltse ki a következő információkat a meghívót, új felhasználó varázsló</span><span class="sxs-lookup"><span data-stu-id="33813-262">In the Invite New User wizard fill the following information</span></span>
   
    <span data-ttu-id="33813-263">a.</span><span class="sxs-lookup"><span data-stu-id="33813-263">a.</span></span> <span data-ttu-id="33813-264">Adja meg a felhasználó **E-mail** cím beviteli mezőben szereplő</span><span class="sxs-lookup"><span data-stu-id="33813-264">Enter the user **Email** address in the textbox</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    <span data-ttu-id="33813-266">b.</span><span class="sxs-lookup"><span data-stu-id="33813-266">b.</span></span> <span data-ttu-id="33813-267">Adja meg a **Keresztnév** a szövegmezőben</span><span class="sxs-lookup"><span data-stu-id="33813-267">Enter the **First Name** in the textbox</span></span>
   
    <span data-ttu-id="33813-268">c.</span><span class="sxs-lookup"><span data-stu-id="33813-268">c.</span></span> <span data-ttu-id="33813-269">Adja meg a **Vezetéknév** a szövegmezőben</span><span class="sxs-lookup"><span data-stu-id="33813-269">Enter the **Last Name**  in the textbox</span></span>
   
    <span data-ttu-id="33813-270">d.</span><span class="sxs-lookup"><span data-stu-id="33813-270">d.</span></span> <span data-ttu-id="33813-271">Kattintson a **Tovább** gombra</span><span class="sxs-lookup"><span data-stu-id="33813-271">Click **Next**</span></span>

6. <span data-ttu-id="33813-272">Az a **engedélyek** lapon jelölje be a **userRoles** kattintson **tovább**</span><span class="sxs-lookup"><span data-stu-id="33813-272">In the **Permissions** tab, select the **userRoles** and click **Next**</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. <span data-ttu-id="33813-274">Kattintson a **küldése** gombra a felhasználó felkérést</span><span class="sxs-lookup"><span data-stu-id="33813-274">Click the **Send** button to send the user invitation</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. <span data-ttu-id="33813-276">Felhasználói az e-mail-értesítést kap, és kattintson a hivatkozásra, és módosítsa a jelszót a fiók aktiválása.</span><span class="sxs-lookup"><span data-stu-id="33813-276">User receives the email notification and has to click the link and change the password to activate the account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="33813-277">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="33813-277">Assigning the Azure AD test user</span></span>

<span data-ttu-id="33813-278">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Marketo Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="33813-278">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Marketo.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="33813-280">**A Marketo Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="33813-280">**To assign Britta Simon to Marketo, perform the following steps:**</span></span>

1. <span data-ttu-id="33813-281">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="33813-281">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="33813-283">Az alkalmazások listában válassza ki a **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="33813-283">In the applications list, select **Marketo**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. <span data-ttu-id="33813-285">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="33813-285">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="33813-287">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="33813-287">Click **Add** button.</span></span> <span data-ttu-id="33813-288">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="33813-288">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="33813-290">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="33813-290">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="33813-291">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="33813-291">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="33813-292">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="33813-292">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="33813-293">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="33813-293">Testing single sign-on</span></span>

<span data-ttu-id="33813-294">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="33813-294">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="33813-295">Ha a hozzáférési panelen Marketo csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Marketo-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="33813-295">When you click the Marketo tile in the Access Panel, you should get automatically signed-on to your Marketo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33813-296">További források</span><span class="sxs-lookup"><span data-stu-id="33813-296">Additional resources</span></span>

* [<span data-ttu-id="33813-297">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="33813-297">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="33813-298">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="33813-298">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

