---
title: "Oktatóanyag: Azure Active Directoryval integrált Replicon |} Microsoft Docs"
description: "Megtudhatja, hogyan Replicon használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a><span data-ttu-id="075eb-103">Oktatóanyag: Azure Active Directoryval integrált Replicon</span><span class="sxs-lookup"><span data-stu-id="075eb-103">Tutorial: Azure Active Directory integration with Replicon</span></span>
<span data-ttu-id="075eb-104">Ez az oktatóanyag célja az Azure és Replicon integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="075eb-104">The objective of this tutorial is to show the integration of Azure and Replicon.</span></span> <span data-ttu-id="075eb-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="075eb-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="075eb-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="075eb-106">A valid Azure subscription</span></span>
* <span data-ttu-id="075eb-107">A Replicon bérlő</span><span class="sxs-lookup"><span data-stu-id="075eb-107">A Replicon tenant</span></span>

<span data-ttu-id="075eb-108">Ez az oktatóanyag befejezése után az Azure AD-felhasználók Replicon rendelt fog tudni egyetlen jelentkezzen be az alkalmazás a Replicon vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési), vagy használja a [a hozzáférési Panelbemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="075eb-108">After completing this tutorial, the Azure AD users you have assigned to Replicon will be able to single sign into the application at your Replicon company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="075eb-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="075eb-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="075eb-110">Az alkalmazás Replicon-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="075eb-110">Enabling the application integration for Replicon</span></span>
2. <span data-ttu-id="075eb-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="075eb-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="075eb-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="075eb-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="075eb-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="075eb-113">Assigning users</span></span>

<span data-ttu-id="075eb-114">![A forgatókönyv](./media/active-directory-saas-replicon-tutorial/IC777798.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="075eb-114">![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-replicon"></a><span data-ttu-id="075eb-115">Az alkalmazás Replicon-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="075eb-115">Enable the application integration for Replicon</span></span>
<span data-ttu-id="075eb-116">Ez a szakasz célja felvázoló Replicon az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="075eb-116">The objective of this section is to outline how to enable the application integration for Replicon.</span></span>

<span data-ttu-id="075eb-117">**Ahhoz, hogy az alkalmazás-integráció Replicon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="075eb-117">**To enable the application integration for Replicon, perform the following steps:**</span></span>

1. <span data-ttu-id="075eb-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="075eb-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="075eb-119">![Az Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="075eb-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="075eb-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="075eb-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="075eb-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="075eb-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="075eb-122">![Alkalmazások](./media/active-directory-saas-replicon-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="075eb-122">![Applications](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="075eb-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="075eb-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="075eb-124">![Alkalmazás hozzáadása](./media/active-directory-saas-replicon-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="075eb-124">![Add application](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="075eb-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="075eb-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="075eb-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="075eb-126">![Add an application from gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="075eb-127">Az a **keresőmezőbe**, típus **Replicon**.</span><span class="sxs-lookup"><span data-stu-id="075eb-127">In the **search box**, type **Replicon**.</span></span>
   
    <span data-ttu-id="075eb-128">![Alkalmazáskatalógusában](./media/active-directory-saas-replicon-tutorial/IC777799.png "alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="075eb-128">![Application gallery](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")</span></span>
7. <span data-ttu-id="075eb-129">Az eredmények ablaktáblájában válassza **Replicon**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="075eb-129">In the results pane, select **Replicon**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="075eb-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span><span class="sxs-lookup"><span data-stu-id="075eb-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="075eb-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="075eb-131">Configure single sign-on</span></span>

<span data-ttu-id="075eb-132">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Replicon fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="075eb-132">The objective of this section is to outline how to enable users to authenticate to Replicon with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="075eb-133">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="075eb-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="075eb-134">A klasszikus Azure portálon a a **Replicon** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="075eb-134">In the Azure classic portal, on the **Replicon** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="075eb-135">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777801.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="075eb-135">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="075eb-136">Az a **hová bejelentkezni Replicon felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="075eb-136">On the **How would you like users to sign on to Replicon** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="075eb-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777802.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="075eb-137">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="075eb-138">Az a **alkalmazás URL-cím konfigurálása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="075eb-138">On the **Configure App URL** page, perform the following steps:</span></span>
   
    <span data-ttu-id="075eb-139">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777803.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="075eb-139">![Configure app URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")</span></span>
  1. <span data-ttu-id="075eb-140">Az a **Replicon bejelentkezési URL-cím** szövegmező, írja be a Replicon bérlői URL-cím (pl.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span><span class="sxs-lookup"><span data-stu-id="075eb-140">In the **Replicon Sign On URL** textbox, type your Replicon tenant URL (e.g.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span></span>
  2. <span data-ttu-id="075eb-141">Az a **Replicon válasz URL-CÍMEN** szövegmező, írja be a Replicon **AssertionConsumerService** URL-cím (pl.: *https://global.replicon.com/! / post egy saml2/vállalati/sso*).</span><span class="sxs-lookup"><span data-stu-id="075eb-141">In the **Replicon Reply URL** textbox, type your Replicon **AssertionConsumerService** URL(e.g.: *https://global.replicon.com/!/saml2/company/sso/post*).</span></span>  
      
     >[!NOTE]
     ><span data-ttu-id="075eb-142">Az URL-cím beszerzése lévő Replicon metaadatok: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.</span><span class="sxs-lookup"><span data-stu-id="075eb-142">You can get the URL from the Replicon metadata at: **https://global.replicon.com/!/saml2/\<YourCompanyKey\>**.</span></span>
     > 
     > 
 
  3. <span data-ttu-id="075eb-143">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="075eb-143">Click **Next**.</span></span>

4. <span data-ttu-id="075eb-144">A a **konfigurálhatja az egyszeri bejelentkezés Replicon** töltse le a metaadatokat, kattintson **metaadatok letöltése**, és mentse a metaadatok a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="075eb-144">On the **Configure single sign-on at Replicon** page, to download your metadata, click **Download metadata**, and then save the metadata on your computer.</span></span>
   
    <span data-ttu-id="075eb-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777804.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="075eb-145">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="075eb-146">Egy másik webes böngészőablakban jelentkezzen be a Replicon vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="075eb-146">In a different web browser window, log into your Replicon company site as an administrator.</span></span>

6. <span data-ttu-id="075eb-147">SAML 2.0 konfigurálásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="075eb-147">To configure SAML 2.0, perform the following steps:</span></span>
   
    <span data-ttu-id="075eb-148">![SAML-alapú hitelesítés engedélyezéséhez](./media/active-directory-saas-replicon-tutorial/IC777805.png "engedélyezése SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="075eb-148">![Enable SAML authentication](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")</span></span>
  
  1. <span data-ttu-id="075eb-149">A megjelenítendő a **EnableSAML Authentication2** párbeszédpanel, az URL-cím, a következő bővítése után a vállalat kulcs: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="075eb-149">To display the **EnableSAML Authentication2** dialog, append the following to your URL, after your company key: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>  
    * <span data-ttu-id="075eb-150">Az alábbiakban látható a teljes URL-cím sémája:</span><span class="sxs-lookup"><span data-stu-id="075eb-150">The following shows the schema of the complete URL:</span></span>  
   <span data-ttu-id="075eb-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="075eb-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>
   2. <span data-ttu-id="075eb-152">Kattintson a  **+**  bontsa ki a **v20Configuration** szakasz.</span><span class="sxs-lookup"><span data-stu-id="075eb-152">Click the **+** to expand the **v20Configuration** section.</span></span>
   3. <span data-ttu-id="075eb-153">Kattintson a  **+**  bontsa ki a **metaDataConfiguration** szakasz.</span><span class="sxs-lookup"><span data-stu-id="075eb-153">Click the **+** to expand the **metaDataConfiguration** section.</span></span>
   4. <span data-ttu-id="075eb-154">Kattintson a **Choose File**ki az identity provider metaadatok XML-fájlt, majd kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="075eb-154">Click **Choose File**, to select your identity provider metadata XML file, and click **Submit**.</span></span>

7. <span data-ttu-id="075eb-155">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="075eb-155">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="075eb-156">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC778418.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="075eb-156">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="075eb-157">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="075eb-157">Configure user provisioning</span></span>

<span data-ttu-id="075eb-158">Ahhoz, hogy az Azure AD-felhasználók Replicon bejelentkezni, akkor ki kell építenie Replicon be.</span><span class="sxs-lookup"><span data-stu-id="075eb-158">In order to enable Azure AD users to log into Replicon, they must be provisioned into Replicon.</span></span>  

<span data-ttu-id="075eb-159">Replicon, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="075eb-159">In the case of Replicon, provisioning is a manual task.</span></span>

<span data-ttu-id="075eb-160">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="075eb-160">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="075eb-161">A webböngésző ablakának jelentkezzen be a Replicon vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="075eb-161">In a web browser window, log into your Replicon company site as an administrator.</span></span>
2. <span data-ttu-id="075eb-162">Ugrás a **felügyeleti \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="075eb-162">Go to **Administration \> Users**.</span></span>
   
    <span data-ttu-id="075eb-163">![Felhasználók](./media/active-directory-saas-replicon-tutorial/IC777806.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="075eb-163">![Users](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")</span></span>
3. <span data-ttu-id="075eb-164">Kattintson a **hozzáadni a felhasználót +**.</span><span class="sxs-lookup"><span data-stu-id="075eb-164">Click **+Add User**.</span></span>
   
    <span data-ttu-id="075eb-165">![Felhasználó hozzáadása](./media/active-directory-saas-replicon-tutorial/IC777807.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="075eb-165">![Add User](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")</span></span>
4. <span data-ttu-id="075eb-166">Az a **felhasználói profil** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="075eb-166">In the **User Profile** section, perform the following steps:</span></span>
   
    <span data-ttu-id="075eb-167">![Felhasználói profil](./media/active-directory-saas-replicon-tutorial/IC777808.png "felhasználói profil")</span><span class="sxs-lookup"><span data-stu-id="075eb-167">![User profile](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")</span></span>
   
  1. <span data-ttu-id="075eb-168">Az a **bejelentkezési név** szövegmezőhöz rendelkezés kívánt Azure AD-felhasználó e-mail címe típus az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="075eb-168">In the **Login Name** textbox, type the Azure AD email address of the Azure AD user you want to provision.</span></span>
  2. <span data-ttu-id="075eb-169">Mint **hitelesítési típus**, jelölje be **SSO**.</span><span class="sxs-lookup"><span data-stu-id="075eb-169">As **Authentication Type**, select **SSO**.</span></span>
  3. <span data-ttu-id="075eb-170">Az a **részleg** szövegmező, írja be a felhasználói osztály.</span><span class="sxs-lookup"><span data-stu-id="075eb-170">In the **Department** textbox, type the user’s department.</span></span>
  4. <span data-ttu-id="075eb-171">Mint **alkalmazott típus**, jelölje be **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="075eb-171">As **Employee Type**, select **Administrator**.</span></span>
  5. <span data-ttu-id="075eb-172">Kattintson a **felhasználói profil mentése**.</span><span class="sxs-lookup"><span data-stu-id="075eb-172">Click **Save User Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="075eb-173">Bármely más Replicon felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Replicon által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="075eb-173">You can use any other Replicon user account creation tools or APIs provided by Replicon to provision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="075eb-174">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="075eb-174">Assign users</span></span>
<span data-ttu-id="075eb-175">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="075eb-175">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="075eb-176">**Felhasználók hozzárendelése Replicon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="075eb-176">**To assign users to Replicon, perform the following steps:**</span></span>

1. <span data-ttu-id="075eb-177">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="075eb-177">In the Azure classic portal, create a test account.</span></span>

2. <span data-ttu-id="075eb-178">Az a **Replicon** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="075eb-178">On the **Replicon** application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="075eb-179">![Felhasználók hozzárendelése](./media/active-directory-saas-replicon-tutorial/IC777809.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="075eb-179">![Assign users](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")</span></span>

3. <span data-ttu-id="075eb-180">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="075eb-180">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="075eb-181">![Igen](./media/active-directory-saas-replicon-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="075eb-181">![Yes](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="075eb-182">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="075eb-182">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="075eb-183">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="075eb-183">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

