---
title: "Oktatóanyag: Azure Active Directoryval integrált ThirdLight |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ThirdLight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ee7710cfea3a13907c0cc940a98c875bf83607a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a><span data-ttu-id="6b460-103">Oktatóanyag: Azure Active Directoryval integrált ThirdLight</span><span class="sxs-lookup"><span data-stu-id="6b460-103">Tutorial: Azure Active Directory integration with ThirdLight</span></span>

<span data-ttu-id="6b460-104">Ebben az oktatóanyagban elsajátíthatja ThirdLight integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6b460-104">In this tutorial, you learn how to integrate ThirdLight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6b460-105">ThirdLight integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6b460-105">Integrating ThirdLight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6b460-106">Megadhatja a ThirdLight hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6b460-106">You can control in Azure AD who has access to ThirdLight</span></span>
- <span data-ttu-id="6b460-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett ThirdLight (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6b460-107">You can enable your users to automatically get signed-on to ThirdLight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6b460-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6b460-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6b460-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6b460-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b460-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6b460-110">Prerequisites</span></span>

<span data-ttu-id="6b460-111">Konfigurálása az Azure AD-integrációs ThirdLight, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="6b460-111">To configure Azure AD integration with ThirdLight, you need the following items:</span></span>

- <span data-ttu-id="6b460-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6b460-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6b460-113">Egy ThirdLight egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6b460-113">A ThirdLight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6b460-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6b460-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6b460-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6b460-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6b460-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6b460-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6b460-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b460-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6b460-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6b460-118">Scenario description</span></span>
<span data-ttu-id="6b460-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6b460-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6b460-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6b460-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6b460-121">A gyűjteményből ThirdLight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6b460-121">Adding ThirdLight from the gallery</span></span>
2. <span data-ttu-id="6b460-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6b460-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thirdlight-from-the-gallery"></a><span data-ttu-id="6b460-123">A gyűjteményből ThirdLight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6b460-123">Adding ThirdLight from the gallery</span></span>
<span data-ttu-id="6b460-124">Az Azure AD integrálása a ThirdLight konfigurálásához kell hozzáadnia ThirdLight a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="6b460-124">To configure the integration of ThirdLight into Azure AD, you need to add ThirdLight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6b460-125">**A gyűjteményből ThirdLight hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6b460-125">**To add ThirdLight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6b460-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6b460-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6b460-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6b460-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6b460-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6b460-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6b460-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="6b460-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6b460-133">Írja be a keresőmezőbe, **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="6b460-133">In the search box, type **ThirdLight**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_search.png)

5. <span data-ttu-id="6b460-135">Az eredmények panelen válassza ki a **ThirdLight**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6b460-135">In the results panel, select **ThirdLight**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6b460-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6b460-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6b460-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján ThirdLight</span><span class="sxs-lookup"><span data-stu-id="6b460-138">In this section, you configure and test Azure AD single sign-on with ThirdLight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6b460-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ThirdLight a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6b460-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ThirdLight is to a user in Azure AD.</span></span> <span data-ttu-id="6b460-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ThirdLight közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6b460-140">In other words, a link relationship between an Azure AD user and the related user in ThirdLight needs to be established.</span></span>

<span data-ttu-id="6b460-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** ThirdLight a.</span><span class="sxs-lookup"><span data-stu-id="6b460-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ThirdLight.</span></span>

<span data-ttu-id="6b460-142">Az Azure AD egyszeri bejelentkezést a ThirdLight tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6b460-142">To configure and test Azure AD single sign-on with ThirdLight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6b460-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6b460-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6b460-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6b460-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6b460-145">**[ThirdLight tesztfelhasználó létrehozása](#creating-a-thirdlight-test-user)**  - való Britta Simon valami ThirdLight, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="6b460-145">**[Creating a ThirdLight test user](#creating-a-thirdlight-test-user)** - to have a counterpart of Britta Simon in ThirdLight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6b460-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6b460-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6b460-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6b460-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6b460-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6b460-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6b460-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ThirdLight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6b460-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ThirdLight application.</span></span>

<span data-ttu-id="6b460-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés ThirdLight, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6b460-150">**To configure Azure AD single sign-on with ThirdLight, perform the following steps:**</span></span>

1. <span data-ttu-id="6b460-151">Az Azure portálon a a **ThirdLight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6b460-151">In the Azure portal, on the **ThirdLight** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6b460-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6b460-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. <span data-ttu-id="6b460-155">Az a **ThirdLight tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6b460-155">On the **ThirdLight Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_url.png)

    <span data-ttu-id="6b460-157">a.</span><span class="sxs-lookup"><span data-stu-id="6b460-157">a.</span></span> <span data-ttu-id="6b460-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.thirdlight.com/`</span><span class="sxs-lookup"><span data-stu-id="6b460-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.thirdlight.com/`</span></span>

    <span data-ttu-id="6b460-159">b.</span><span class="sxs-lookup"><span data-stu-id="6b460-159">b.</span></span> <span data-ttu-id="6b460-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.thirdlight.com/saml/sp`</span><span class="sxs-lookup"><span data-stu-id="6b460-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.thirdlight.com/saml/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6b460-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6b460-161">These values are not real.</span></span> <span data-ttu-id="6b460-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és Identiifer.</span><span class="sxs-lookup"><span data-stu-id="6b460-162">Update these values with the actual Sign-On URL and Identiifer.</span></span> <span data-ttu-id="6b460-163">Ügyfél [ThirdLight ügyfél-támogatási csoport](https://www.thirdlight.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6b460-163">Contact [ThirdLight Client support team](https://www.thirdlight.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="6b460-164">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6b460-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. <span data-ttu-id="6b460-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b460-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6b460-168">Egy másik webes böngészőablakban jelentkezzen be a ThirdLight vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6b460-168">In a different web browser window, log in to your ThirdLight company site as an administrator.</span></span>

7. <span data-ttu-id="6b460-169">Ugrás a **konfigurációs \> rendszerfelügyelet**, és kattintson a **egy SAML2**.</span><span class="sxs-lookup"><span data-stu-id="6b460-169">Go to **Configuration \> System Administration**, and then click **SAML2**.</span></span>
   
    <span data-ttu-id="6b460-170">![Rendszer-felügyeleti](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "rendszerfelügyelet")</span><span class="sxs-lookup"><span data-stu-id="6b460-170">![System Administration](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "System Administration")</span></span>

8. <span data-ttu-id="6b460-171">A egy SAML2 konfigurációs szakaszban hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6b460-171">In the SAML2 configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="6b460-172">![SAML-alapú egyszeri bejelentkezést](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML-alapú egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="6b460-172">![SAML Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML Single Sign-On")</span></span>   

     <span data-ttu-id="6b460-173">a.</span><span class="sxs-lookup"><span data-stu-id="6b460-173">a.</span></span> <span data-ttu-id="6b460-174">Válassza ki **egy SAML2 egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="6b460-174">Select **Enable SAML2 Single Sign-On**.</span></span>
 
     <span data-ttu-id="6b460-175">b.</span><span class="sxs-lookup"><span data-stu-id="6b460-175">b.</span></span> <span data-ttu-id="6b460-176">Mint **IdP metaadatok forrást**, jelölje be **terhelés IdP metaadatok XML-fájljából**.</span><span class="sxs-lookup"><span data-stu-id="6b460-176">As **Source for IdP Metadata**, select **Load IdP Metadata from XML**.</span></span>
 
     <span data-ttu-id="6b460-177">c.</span><span class="sxs-lookup"><span data-stu-id="6b460-177">c.</span></span> <span data-ttu-id="6b460-178">Nyissa meg a letöltött metaadatfájl, másolja a tartalmat, és illessze be azt a **IdP metaadatainak XML-kódja** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6b460-178">Open the downloaded metadata file, copy the content, and then paste it into the **IdP Metadata XML** textbox.</span></span> 
     
     <span data-ttu-id="6b460-179">d.</span><span class="sxs-lookup"><span data-stu-id="6b460-179">d.</span></span> <span data-ttu-id="6b460-180">Kattintson a **menteni egy SAML2 beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6b460-180">Click **Save SAML2 settings**.</span></span>

> [!TIP]
> <span data-ttu-id="6b460-181">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="6b460-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6b460-182">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="6b460-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6b460-183">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6b460-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6b460-184">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b460-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="6b460-185">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6b460-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6b460-187">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6b460-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6b460-188">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6b460-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6b460-190">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6b460-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6b460-192">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6b460-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6b460-194">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6b460-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6b460-196">a.</span><span class="sxs-lookup"><span data-stu-id="6b460-196">a.</span></span> <span data-ttu-id="6b460-197">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6b460-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6b460-198">b.</span><span class="sxs-lookup"><span data-stu-id="6b460-198">b.</span></span> <span data-ttu-id="6b460-199">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b460-199">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="6b460-200">c.</span><span class="sxs-lookup"><span data-stu-id="6b460-200">c.</span></span> <span data-ttu-id="6b460-201">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6b460-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6b460-202">d.</span><span class="sxs-lookup"><span data-stu-id="6b460-202">d.</span></span> <span data-ttu-id="6b460-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6b460-203">Click **Create**.</span></span>
 
### <a name="creating-a-thirdlight-test-user"></a><span data-ttu-id="6b460-204">ThirdLight tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b460-204">Creating a ThirdLight test user</span></span>

<span data-ttu-id="6b460-205">Ahhoz, hogy az Azure AD-felhasználók ThirdLight bejelentkezni, akkor ki kell építenie a ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="6b460-205">To enable Azure AD users to log in to ThirdLight, they must be provisioned into ThirdLight.</span></span>  
<span data-ttu-id="6b460-206">ThirdLight, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6b460-206">In the case of ThirdLight, provisioning is a manual task.</span></span>

<span data-ttu-id="6b460-207">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6b460-207">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="6b460-208">Jelentkezzen be a **ThirdLight** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6b460-208">Log in to your **ThirdLight** company site as an administrator.</span></span>

2. <span data-ttu-id="6b460-209">Ugrás a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="6b460-209">Go to **Users** tab.</span></span>

3. <span data-ttu-id="6b460-210">Válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6b460-210">Select **Users and Groups**.</span></span>

4. <span data-ttu-id="6b460-211">Kattintson a **új felhasználó hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b460-211">Click **Add new User** button.</span></span>

5. <span data-ttu-id="6b460-212">Adja meg **a felhasználónév, neve és leírása, e-mailek, válasszon egy előre definiált vagy új csoporttagok** egy érvényes AAD-fióknevet, amelyet kiépítését.</span><span class="sxs-lookup"><span data-stu-id="6b460-212">Enter **the Username, Name or Description, Email, Choose a Preset or Group of New Members** of a valid AAD account you want to provision.</span></span>

6. <span data-ttu-id="6b460-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6b460-213">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="6b460-214">Bármely más Thirdlight felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Thirdlight által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="6b460-214">You can use any other Thirdlight user account creation tools or APIs provided by Thirdlight to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6b460-215">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6b460-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6b460-216">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés ThirdLight Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="6b460-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ThirdLight.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6b460-218">**Britta Simon hozzárendelése ThirdLight, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6b460-218">**To assign Britta Simon to ThirdLight, perform the following steps:**</span></span>

1. <span data-ttu-id="6b460-219">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6b460-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6b460-221">Az alkalmazások listában válassza ki a **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="6b460-221">In the applications list, select **ThirdLight**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. <span data-ttu-id="6b460-223">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6b460-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6b460-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b460-225">Click **Add** button.</span></span> <span data-ttu-id="6b460-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b460-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6b460-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6b460-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6b460-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b460-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6b460-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b460-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6b460-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6b460-231">Testing single sign-on</span></span>

<span data-ttu-id="6b460-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6b460-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6b460-233">Ha a hozzáférési panelen ThirdLight csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ThirdLight alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="6b460-233">When you click the ThirdLight tile in the Access Panel, you should get automatically signed-on to your ThirdLight application.</span></span>
<span data-ttu-id="6b460-234">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6b460-234">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6b460-235">További források</span><span class="sxs-lookup"><span data-stu-id="6b460-235">Additional resources</span></span>

* [<span data-ttu-id="6b460-236">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6b460-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6b460-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6b460-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_203.png

