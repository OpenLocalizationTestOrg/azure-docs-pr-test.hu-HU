---
title: "Oktatóanyag: Azure Active Directoryval integrált Sprinklr |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Sprinklr között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 6e1622cd55e3b0e8063604ac9dc0cb0673fa9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a><span data-ttu-id="00945-103">Oktatóanyag: Azure Active Directoryval integrált Sprinklr</span><span class="sxs-lookup"><span data-stu-id="00945-103">Tutorial: Azure Active Directory integration with Sprinklr</span></span>

<span data-ttu-id="00945-104">Ebben az oktatóanyagban elsajátíthatja Sprinklr integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="00945-104">In this tutorial, you learn how to integrate Sprinklr with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="00945-105">Sprinklr integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="00945-105">Integrating Sprinklr with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="00945-106">Megadhatja a Sprinklr hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="00945-106">You can control in Azure AD who has access to Sprinklr</span></span>
- <span data-ttu-id="00945-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Sprinklr (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="00945-107">You can enable your users to automatically get signed-on to Sprinklr (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="00945-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="00945-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="00945-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="00945-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00945-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00945-110">Prerequisites</span></span>

<span data-ttu-id="00945-111">Konfigurálása az Azure AD-integrációs Sprinklr, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="00945-111">To configure Azure AD integration with Sprinklr, you need the following items:</span></span>

- <span data-ttu-id="00945-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="00945-112">An Azure AD subscription</span></span>
- <span data-ttu-id="00945-113">Egy Sprinklr egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="00945-113">A Sprinklr single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="00945-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="00945-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="00945-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="00945-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="00945-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="00945-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="00945-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00945-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="00945-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="00945-118">Scenario description</span></span>
<span data-ttu-id="00945-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="00945-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="00945-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="00945-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="00945-121">A gyűjteményből Sprinklr hozzáadása</span><span class="sxs-lookup"><span data-stu-id="00945-121">Adding Sprinklr from the gallery</span></span>
2. <span data-ttu-id="00945-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="00945-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sprinklr-from-the-gallery"></a><span data-ttu-id="00945-123">A gyűjteményből Sprinklr hozzáadása</span><span class="sxs-lookup"><span data-stu-id="00945-123">Adding Sprinklr from the gallery</span></span>
<span data-ttu-id="00945-124">Az Azure AD integrálása a Sprinklr konfigurálásához kell hozzáadnia Sprinklr a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="00945-124">To configure the integration of Sprinklr into Azure AD, you need to add Sprinklr from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="00945-125">**A gyűjteményből Sprinklr hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00945-125">**To add Sprinklr from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="00945-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="00945-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="00945-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="00945-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="00945-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="00945-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="00945-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="00945-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="00945-133">Írja be a keresőmezőbe, **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="00945-133">In the search box, type **Sprinklr**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. <span data-ttu-id="00945-135">Az eredmények panelen válassza ki a **Sprinklr**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00945-135">In the results panel, select **Sprinklr**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="00945-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="00945-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="00945-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Sprinklr</span><span class="sxs-lookup"><span data-stu-id="00945-138">In this section, you configure and test Azure AD single sign-on with Sprinklr based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="00945-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Sprinklr a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="00945-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sprinklr is to a user in Azure AD.</span></span> <span data-ttu-id="00945-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Sprinklr közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="00945-140">In other words, a link relationship between an Azure AD user and the related user in Sprinklr needs to be established.</span></span>

<span data-ttu-id="00945-141">Sprinklr, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="00945-141">In Sprinklr, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="00945-142">Az Azure AD egyszeri bejelentkezést a Sprinklr tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="00945-142">To configure and test Azure AD single sign-on with Sprinklr, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="00945-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="00945-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="00945-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="00945-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="00945-145">**[Sprinklr tesztfelhasználó létrehozása](#creating-a-sprinklr-test-user)**  - való Britta Simon valami Sprinklr, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="00945-145">**[Creating a Sprinklr test user](#creating-a-sprinklr-test-user)** - to have a counterpart of Britta Simon in Sprinklr that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="00945-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="00945-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="00945-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="00945-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="00945-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="00945-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="00945-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Sprinklr alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="00945-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sprinklr application.</span></span>

<span data-ttu-id="00945-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Sprinklr, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00945-150">**To configure Azure AD single sign-on with Sprinklr, perform the following steps:**</span></span>

1. <span data-ttu-id="00945-151">Az Azure portálon a a **Sprinklr** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="00945-151">In the Azure portal, on the **Sprinklr** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="00945-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="00945-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. <span data-ttu-id="00945-155">Az a **Sprinklr tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="00945-155">On the **Sprinklr Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    <span data-ttu-id="00945-157">a.</span><span class="sxs-lookup"><span data-stu-id="00945-157">a.</span></span> <span data-ttu-id="00945-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="00945-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    <span data-ttu-id="00945-159">b.</span><span class="sxs-lookup"><span data-stu-id="00945-159">b.</span></span> <span data-ttu-id="00945-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="00945-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="00945-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="00945-161">These values are not real.</span></span> <span data-ttu-id="00945-162">Frissítse az értéket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="00945-162">Update the value with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="00945-163">Ügyfél [Sprinklr ügyfél-támogatási csoport](https://www.sprinklr.com/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="00945-163">Contact [Sprinklr Client support team](https://www.sprinklr.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="00945-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="00945-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. <span data-ttu-id="00945-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="00945-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="00945-168">A a **Sprinklr konfigurációs** kattintson **konfigurálása Sprinklr** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="00945-168">On the **Sprinklr Configuration** section, click **Configure Sprinklr** to open **Configure sign-on** window.</span></span> <span data-ttu-id="00945-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="00945-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

7. <span data-ttu-id="00945-170">Egy másik webes böngészőablakban jelentkezzen be a Sprinklr vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="00945-170">In a different web browser window, log in to your Sprinklr company site as an administrator.</span></span>

8. <span data-ttu-id="00945-171">Ugrás a **felügyeleti \> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="00945-171">Go to **Administration \> Settings**.</span></span>
   
    <span data-ttu-id="00945-172">![Felügyeleti](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="00945-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

9. <span data-ttu-id="00945-173">Ugrás a **kezelése Partner \> egyszeri bejelentkezési** a a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="00945-173">Go to **Manage Partner \> Single Sign** on from the left pane.</span></span>
   
    <span data-ttu-id="00945-174">![Partner kezelése](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Partner kezelése")</span><span class="sxs-lookup"><span data-stu-id="00945-174">![Manage Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Manage Partner")</span></span>

10. <span data-ttu-id="00945-175">Kattintson a **+ Hozzáadás egyszeri bejelentkezéssel**.</span><span class="sxs-lookup"><span data-stu-id="00945-175">Click **+Add Single Sign Ons**.</span></span>
   
    <span data-ttu-id="00945-176">![Az egyszeri bejelentkezés le](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "az egyszeri bejelentkezés le")</span><span class="sxs-lookup"><span data-stu-id="00945-176">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "Single Sign-Ons")</span></span>

11. <span data-ttu-id="00945-177">Az a **az egyszeri bejelentkezés** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="00945-177">On the **Single Sign on** page, perform the following steps:</span></span>
   
    <span data-ttu-id="00945-178">![Az egyszeri bejelentkezés le](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "az egyszeri bejelentkezés le")</span><span class="sxs-lookup"><span data-stu-id="00945-178">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "Single Sign-Ons")</span></span>

    <span data-ttu-id="00945-179">a.</span><span class="sxs-lookup"><span data-stu-id="00945-179">a.</span></span> <span data-ttu-id="00945-180">Az a **neve** szövegmező, adja meg a konfiguráció nevét (például: *WAADSSOTest*).</span><span class="sxs-lookup"><span data-stu-id="00945-180">In the **Name** textbox, type a name for your configuration (for example: *WAADSSOTest*).</span></span>

    <span data-ttu-id="00945-181">b.</span><span class="sxs-lookup"><span data-stu-id="00945-181">b.</span></span> <span data-ttu-id="00945-182">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="00945-182">Select **Enabled**.</span></span>

    <span data-ttu-id="00945-183">c.</span><span class="sxs-lookup"><span data-stu-id="00945-183">c.</span></span> <span data-ttu-id="00945-184">Válassza ki **új SSO-tanúsítvány használatára**.</span><span class="sxs-lookup"><span data-stu-id="00945-184">Select **Use new SSO Certificate**.</span></span>
             
    <span data-ttu-id="00945-185">e.</span><span class="sxs-lookup"><span data-stu-id="00945-185">e.</span></span> <span data-ttu-id="00945-186">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a tartalmának másolása a vágólapra és illessze be azt a **szolgáltató Identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="00945-186">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="00945-187">f.</span><span class="sxs-lookup"><span data-stu-id="00945-187">f.</span></span> <span data-ttu-id="00945-188">Beillesztés a **SAML Entitásazonosító** érték, amely az Azure portálról másolta a **entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="00945-188">Paste the **SAML Entity ID** value which you have copied from Azure Portal into the **Entity Id** textbox.</span></span>

    <span data-ttu-id="00945-189">g.</span><span class="sxs-lookup"><span data-stu-id="00945-189">g.</span></span> <span data-ttu-id="00945-190">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** érték, amely az Azure portálról másolta a **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="00945-190">Paste the **SAML Single Sign-On Service URL** value which you have copied from Azure Portal into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="00945-191">h.</span><span class="sxs-lookup"><span data-stu-id="00945-191">h.</span></span> <span data-ttu-id="00945-192">Beillesztés a **Sign-Out URL-cím** érték, amely az Azure portálról másolta a **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="00945-192">Paste the **Sign-Out URL** value which you have copied from Azure Portal into the **Identity Provider Logout URL** textbox.</span></span>
     
    <span data-ttu-id="00945-193">i.</span><span class="sxs-lookup"><span data-stu-id="00945-193">i.</span></span> <span data-ttu-id="00945-194">Mint **SAML felhasználói azonosító típusa**, jelölje be **helyességi feltételt tartalmaz felhasználói "s sprinklr.com felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="00945-194">As **SAML User ID Type**, select **Assertion contains User”s sprinklr.com username**.</span></span>

    <span data-ttu-id="00945-195">j.</span><span class="sxs-lookup"><span data-stu-id="00945-195">j.</span></span> <span data-ttu-id="00945-196">Mint **SAML felhasználói azonosító hely**, jelölje be **felhasználói Azonosítóját a tulajdonos utasítás névazonosítója elemében van**.</span><span class="sxs-lookup"><span data-stu-id="00945-196">As **SAML User ID Location**, select **User ID is in the Name Identifier element of the Subject statement**.</span></span>

    <span data-ttu-id="00945-197">k.</span><span class="sxs-lookup"><span data-stu-id="00945-197">k.</span></span> <span data-ttu-id="00945-198">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="00945-198">Click **Save**.</span></span>
       
    <span data-ttu-id="00945-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="00945-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span></span>

> [!TIP]
> <span data-ttu-id="00945-200">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="00945-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="00945-201">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="00945-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="00945-202">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="00945-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="00945-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="00945-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="00945-204">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="00945-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="00945-206">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00945-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="00945-207">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="00945-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="00945-209">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="00945-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="00945-211">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="00945-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="00945-213">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="00945-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="00945-215">a.</span><span class="sxs-lookup"><span data-stu-id="00945-215">a.</span></span> <span data-ttu-id="00945-216">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="00945-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="00945-217">b.</span><span class="sxs-lookup"><span data-stu-id="00945-217">b.</span></span> <span data-ttu-id="00945-218">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="00945-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="00945-219">c.</span><span class="sxs-lookup"><span data-stu-id="00945-219">c.</span></span> <span data-ttu-id="00945-220">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="00945-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="00945-221">d.</span><span class="sxs-lookup"><span data-stu-id="00945-221">d.</span></span> <span data-ttu-id="00945-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="00945-222">Click **Create**.</span></span>
 
### <a name="creating-a-sprinklr-test-user"></a><span data-ttu-id="00945-223">Sprinklr tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="00945-223">Creating a Sprinklr test user</span></span>

1. <span data-ttu-id="00945-224">Jelentkezzen be rendszergazdaként a Sprinklr vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="00945-224">Log in to your Sprinklr company site as an administrator.</span></span>

2. <span data-ttu-id="00945-225">Ugrás a **felügyeleti \> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="00945-225">Go to **Administration \> Settings**.</span></span>
   
    <span data-ttu-id="00945-226">![Felügyeleti](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="00945-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

3. <span data-ttu-id="00945-227">Ugrás a **kezelése ügyfél \> felhasználók** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="00945-227">Go to **Manage Client \> Users** from the left pane.</span></span>
   
    <span data-ttu-id="00945-228">![Beállítások](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="00945-228">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Settings")</span></span>

4. <span data-ttu-id="00945-229">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="00945-229">Click **Add User**.</span></span>
   
    <span data-ttu-id="00945-230">![Beállítások](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="00945-230">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Settings")</span></span>

5. <span data-ttu-id="00945-231">Az a **Szerkesztés felhasználói** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="00945-231">On the **Edit user** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="00945-232">![Felhasználó szerkesztése](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "felhasználó szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="00945-232">![Edit user](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Edit user")</span></span> 

    <span data-ttu-id="00945-233">a.</span><span class="sxs-lookup"><span data-stu-id="00945-233">a.</span></span> <span data-ttu-id="00945-234">Az a **E-mail**, **Utónév** és **Vezetéknév** szövegmezőből, írja be a kívánt kiépítéséhez az Azure AD felhasználói fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="00945-234">In the **Email**, **First Name** and **Last Name** textboxes, type the information of an Azure AD user account you want to provision.</span></span>

    <span data-ttu-id="00945-235">b.</span><span class="sxs-lookup"><span data-stu-id="00945-235">b.</span></span> <span data-ttu-id="00945-236">Válassza ki **jelszó tiltva**.</span><span class="sxs-lookup"><span data-stu-id="00945-236">Select **Password Disabled**.</span></span>

    <span data-ttu-id="00945-237">c.</span><span class="sxs-lookup"><span data-stu-id="00945-237">c.</span></span> <span data-ttu-id="00945-238">Válassza ki **nyelvi**.</span><span class="sxs-lookup"><span data-stu-id="00945-238">Select **Language**.</span></span>

    <span data-ttu-id="00945-239">d.</span><span class="sxs-lookup"><span data-stu-id="00945-239">d.</span></span> <span data-ttu-id="00945-240">Válassza ki **felhasználótípust**.</span><span class="sxs-lookup"><span data-stu-id="00945-240">Select **User Type**.</span></span>

    <span data-ttu-id="00945-241">e.</span><span class="sxs-lookup"><span data-stu-id="00945-241">e.</span></span> <span data-ttu-id="00945-242">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="00945-242">Click **Update**.</span></span>
   
     >[!IMPORTANT]
     ><span data-ttu-id="00945-243">**Jelszó tiltva** engedélyezése a felhasználónak be kell jelentkeznie az identitásszolgáltató keresztül kell lennie jelölve.</span><span class="sxs-lookup"><span data-stu-id="00945-243">**Password Disabled** must be selected to enable a user to log in via an Identity provider.</span></span> 
     
6. <span data-ttu-id="00945-244">Ugrás a **szerepkör**, majd végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="00945-244">Go to **Role**, and then perform the following steps:</span></span>
   
    <span data-ttu-id="00945-245">![Szerepkörök partneri](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "partneri szerepkörök")</span><span class="sxs-lookup"><span data-stu-id="00945-245">![Partner Roles](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner Roles")</span></span>

    <span data-ttu-id="00945-246">a.</span><span class="sxs-lookup"><span data-stu-id="00945-246">a.</span></span> <span data-ttu-id="00945-247">Az a **globális** listáról válassza ki **összes\_engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="00945-247">From the **Global** list, select **ALL\_Permissions**.</span></span>  

    <span data-ttu-id="00945-248">b.</span><span class="sxs-lookup"><span data-stu-id="00945-248">b.</span></span> <span data-ttu-id="00945-249">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="00945-249">Click **Update**.</span></span>

>[!NOTE]
><span data-ttu-id="00945-250">Bármely más Sprinklr felhasználói fiók létrehozása eszközök vagy Sprinklr kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="00945-250">You can use any other Sprinklr user account creation tools or APIs provided by Sprinklr to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="00945-251">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="00945-251">Assigning the Azure AD test user</span></span>

<span data-ttu-id="00945-252">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Sprinklr Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="00945-252">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sprinklr.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="00945-254">**Britta Simon hozzárendelése Sprinklr, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00945-254">**To assign Britta Simon to Sprinklr, perform the following steps:**</span></span>

1. <span data-ttu-id="00945-255">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="00945-255">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="00945-257">Az alkalmazások listában válassza ki a **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="00945-257">In the applications list, select **Sprinklr**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. <span data-ttu-id="00945-259">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="00945-259">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="00945-261">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="00945-261">Click **Add** button.</span></span> <span data-ttu-id="00945-262">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00945-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="00945-264">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="00945-264">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="00945-265">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00945-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="00945-266">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00945-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="00945-267">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="00945-267">Testing single sign-on</span></span>

<span data-ttu-id="00945-268">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="00945-268">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="00945-269">Ha a hozzáférési panelen Sprinklr csempére kattint, szerezheti be automatikusan bejelentkezett az Sprinklr alkalmazás a hozzáférési Panel további információt lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="00945-269">When you click the Sprinklr tile in the Access Panel, you should get automatically signed-on to your Sprinklr application For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="00945-270">További források</span><span class="sxs-lookup"><span data-stu-id="00945-270">Additional resources</span></span>

* [<span data-ttu-id="00945-271">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="00945-271">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00945-272">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="00945-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

