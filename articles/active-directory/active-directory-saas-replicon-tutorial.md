---
title: "Oktatóanyag: Azure Active Directoryval integrált Replicon |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Replicon az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
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
ms.openlocfilehash: 4949eaf959265cfa4f732a2b73317fffe6312a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a><span data-ttu-id="99f1a-103">Oktatóanyag: Azure Active Directoryval integrált Replicon</span><span class="sxs-lookup"><span data-stu-id="99f1a-103">Tutorial: Azure Active Directory integration with Replicon</span></span>
<span data-ttu-id="99f1a-104">hello Ez az oktatóanyag célja az Azure és Replicon tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="99f1a-104">hello objective of this tutorial is tooshow hello integration of Azure and Replicon.</span></span> <span data-ttu-id="99f1a-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="99f1a-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="99f1a-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="99f1a-106">A valid Azure subscription</span></span>
* <span data-ttu-id="99f1a-107">A Replicon bérlő</span><span class="sxs-lookup"><span data-stu-id="99f1a-107">A Replicon tenant</span></span>

<span data-ttu-id="99f1a-108">Ez az oktatóanyag befejezése után a tooReplicon hozzárendelt hello Azure AD felhasználók fognak képes toosingle jelentkezzen be a Replicon vállalati helyen (szolgáltatás szolgáltató által kezdeményezett bejelentkezés), vagy hello segítségével hello alkalmazás [bemutatása toohello hozzáférés Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="99f1a-108">After completing this tutorial, hello Azure AD users you have assigned tooReplicon will be able toosingle sign into hello application at your Replicon company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="99f1a-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="99f1a-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="99f1a-110">Replicon hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="99f1a-110">Enabling hello application integration for Replicon</span></span>
2. <span data-ttu-id="99f1a-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99f1a-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="99f1a-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="99f1a-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="99f1a-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="99f1a-113">Assigning users</span></span>

<span data-ttu-id="99f1a-114">![A forgatókönyv](./media/active-directory-saas-replicon-tutorial/IC777798.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="99f1a-114">![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-replicon"></a><span data-ttu-id="99f1a-115">Replicon hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="99f1a-115">Enable hello application integration for Replicon</span></span>
<span data-ttu-id="99f1a-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Replicon.</span><span class="sxs-lookup"><span data-stu-id="99f1a-116">hello objective of this section is toooutline how tooenable hello application integration for Replicon.</span></span>

<span data-ttu-id="99f1a-117">**tooenable hello alkalmazásintegráció Replicon, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="99f1a-117">**tooenable hello application integration for Replicon, perform hello following steps:**</span></span>

1. <span data-ttu-id="99f1a-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="99f1a-119">![Az Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="99f1a-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="99f1a-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="99f1a-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="99f1a-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="99f1a-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="99f1a-122">![Alkalmazások](./media/active-directory-saas-replicon-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="99f1a-122">![Applications](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="99f1a-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="99f1a-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="99f1a-124">![Alkalmazás hozzáadása](./media/active-directory-saas-replicon-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-124">![Add application](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="99f1a-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="99f1a-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-126">![Add an application from gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="99f1a-127">A hello **keresőmezőbe**, típus **Replicon**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-127">In hello **search box**, type **Replicon**.</span></span>
   
    <span data-ttu-id="99f1a-128">![Alkalmazáskatalógusában](./media/active-directory-saas-replicon-tutorial/IC777799.png "alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="99f1a-128">![Application gallery](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")</span></span>
7. <span data-ttu-id="99f1a-129">Hello eredmények ablaktábláján jelöljön ki **Replicon**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="99f1a-129">In hello results pane, select **Replicon**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="99f1a-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span><span class="sxs-lookup"><span data-stu-id="99f1a-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="99f1a-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99f1a-131">Configure single sign-on</span></span>

<span data-ttu-id="99f1a-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooReplicon fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="99f1a-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooReplicon with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="99f1a-133">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="99f1a-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="99f1a-134">A klasszikus Azure portálon, a hello hello **Replicon** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="99f1a-134">In hello Azure classic portal, on hello **Replicon** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="99f1a-135">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777801.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-135">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="99f1a-136">A hello **hogyan szeretné tooReplicon a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-136">On hello **How would you like users toosign on tooReplicon** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="99f1a-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777802.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-137">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="99f1a-138">A hello **alkalmazás URL-cím konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="99f1a-138">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="99f1a-139">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777803.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-139">![Configure app URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")</span></span>
  1. <span data-ttu-id="99f1a-140">A hello **Replicon bejelentkezési URL-cím** szövegmező, írja be a Replicon bérlői URL-cím (pl.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span><span class="sxs-lookup"><span data-stu-id="99f1a-140">In hello **Replicon Sign On URL** textbox, type your Replicon tenant URL (e.g.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span></span>
  2. <span data-ttu-id="99f1a-141">A hello **Replicon válasz URL-CÍMEN** szövegmező, írja be a Replicon **AssertionConsumerService** URL-cím (pl.: *https://global.replicon.com/! / post egy saml2/vállalati/sso*).</span><span class="sxs-lookup"><span data-stu-id="99f1a-141">In hello **Replicon Reply URL** textbox, type your Replicon **AssertionConsumerService** URL(e.g.: *https://global.replicon.com/!/saml2/company/sso/post*).</span></span>  
      
     >[!NOTE]
     ><span data-ttu-id="99f1a-142">Hello URL-cím beszerzése hello Replicon metaadatok:: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-142">You can get hello URL from hello Replicon metadata at: **https://global.replicon.com/!/saml2/\<YourCompanyKey\>**.</span></span>
     > 
     > 
 
  3. <span data-ttu-id="99f1a-143">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="99f1a-143">Click **Next**.</span></span>

4. <span data-ttu-id="99f1a-144">Hello a **konfigurálhatja az egyszeri bejelentkezés Replicon** lap, toodownload a metaadatok kattintson **metaadatok letöltése**, és mentse a hello metaadatok a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="99f1a-144">On hello **Configure single sign-on at Replicon** page, toodownload your metadata, click **Download metadata**, and then save hello metadata on your computer.</span></span>
   
    <span data-ttu-id="99f1a-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777804.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-145">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="99f1a-146">Egy másik webes böngészőablakban jelentkezzen be a Replicon vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="99f1a-146">In a different web browser window, log into your Replicon company site as an administrator.</span></span>

6. <span data-ttu-id="99f1a-147">tooconfigure SAML 2.0, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="99f1a-147">tooconfigure SAML 2.0, perform hello following steps:</span></span>
   
    <span data-ttu-id="99f1a-148">![SAML-alapú hitelesítés engedélyezéséhez](./media/active-directory-saas-replicon-tutorial/IC777805.png "engedélyezése SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="99f1a-148">![Enable SAML authentication](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")</span></span>
  
  1. <span data-ttu-id="99f1a-149">toodisplay hello **EnableSAML Authentication2** párbeszédpanelen tooyour URL-címe, után a vállalat kulcsot következő hello hozzáfűzése: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="99f1a-149">toodisplay hello **EnableSAML Authentication2** dialog, append hello following tooyour URL, after your company key: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>  
    * <span data-ttu-id="99f1a-150">hello következő hello sémája hello teljes URL-CÍMÉT jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="99f1a-150">hello following shows hello schema of hello complete URL:</span></span>  
   <span data-ttu-id="99f1a-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="99f1a-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>
   2. <span data-ttu-id="99f1a-152">Kattintson a hello  **+**  tooexpand hello **v20Configuration** szakasz.</span><span class="sxs-lookup"><span data-stu-id="99f1a-152">Click hello **+** tooexpand hello **v20Configuration** section.</span></span>
   3. <span data-ttu-id="99f1a-153">Kattintson a hello  **+**  tooexpand hello **metaDataConfiguration** szakasz.</span><span class="sxs-lookup"><span data-stu-id="99f1a-153">Click hello **+** tooexpand hello **metaDataConfiguration** section.</span></span>
   4. <span data-ttu-id="99f1a-154">Kattintson a **Choose File**, tooselect az identity provider metaadatok XML-fájlt, majd kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-154">Click **Choose File**, tooselect your identity provider metadata XML file, and click **Submit**.</span></span>

7. <span data-ttu-id="99f1a-155">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="99f1a-155">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="99f1a-156">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC778418.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-156">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="99f1a-157">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99f1a-157">Configure user provisioning</span></span>

<span data-ttu-id="99f1a-158">A sorrend tooenable az Azure AD felhasználók toolog Replicon be azok ki kell építenie Replicon be.</span><span class="sxs-lookup"><span data-stu-id="99f1a-158">In order tooenable Azure AD users toolog into Replicon, they must be provisioned into Replicon.</span></span>  

<span data-ttu-id="99f1a-159">Replicon hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="99f1a-159">In hello case of Replicon, provisioning is a manual task.</span></span>

<span data-ttu-id="99f1a-160">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="99f1a-160">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="99f1a-161">A webböngésző ablakának jelentkezzen be a Replicon vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="99f1a-161">In a web browser window, log into your Replicon company site as an administrator.</span></span>
2. <span data-ttu-id="99f1a-162">Nyissa meg túl**felügyeleti \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-162">Go too**Administration \> Users**.</span></span>
   
    <span data-ttu-id="99f1a-163">![Felhasználók](./media/active-directory-saas-replicon-tutorial/IC777806.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="99f1a-163">![Users](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")</span></span>
3. <span data-ttu-id="99f1a-164">Kattintson a **hozzáadni a felhasználót +**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-164">Click **+Add User**.</span></span>
   
    <span data-ttu-id="99f1a-165">![Felhasználó hozzáadása](./media/active-directory-saas-replicon-tutorial/IC777807.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="99f1a-165">![Add User](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")</span></span>
4. <span data-ttu-id="99f1a-166">A hello **felhasználói profil** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="99f1a-166">In hello **User Profile** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="99f1a-167">![Felhasználói profil](./media/active-directory-saas-replicon-tutorial/IC777808.png "felhasználói profil")</span><span class="sxs-lookup"><span data-stu-id="99f1a-167">![User profile](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")</span></span>
   
  1. <span data-ttu-id="99f1a-168">A hello **bejelentkezési név** szövegmezőhöz típus hello Azure AD e-mail cím kívánt hello Azure AD-felhasználó tooprovision.</span><span class="sxs-lookup"><span data-stu-id="99f1a-168">In hello **Login Name** textbox, type hello Azure AD email address of hello Azure AD user you want tooprovision.</span></span>
  2. <span data-ttu-id="99f1a-169">Mint **hitelesítési típus**, jelölje be **SSO**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-169">As **Authentication Type**, select **SSO**.</span></span>
  3. <span data-ttu-id="99f1a-170">A hello **részleg** szövegmező, írja be a hello felhasználói osztály.</span><span class="sxs-lookup"><span data-stu-id="99f1a-170">In hello **Department** textbox, type hello user’s department.</span></span>
  4. <span data-ttu-id="99f1a-171">Mint **alkalmazott típus**, jelölje be **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-171">As **Employee Type**, select **Administrator**.</span></span>
  5. <span data-ttu-id="99f1a-172">Kattintson a **felhasználói profil mentése**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-172">Click **Save User Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="99f1a-173">Bármely más Replicon felhasználói fiók létrehozása eszközök vagy Replicon tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="99f1a-173">You can use any other Replicon user account creation tools or APIs provided by Replicon tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="99f1a-174">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="99f1a-174">Assign users</span></span>
<span data-ttu-id="99f1a-175">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="99f1a-175">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="99f1a-176">**tooassign felhasználók tooReplicon, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="99f1a-176">**tooassign users tooReplicon, perform hello following steps:**</span></span>

1. <span data-ttu-id="99f1a-177">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="99f1a-177">In hello Azure classic portal, create a test account.</span></span>

2. <span data-ttu-id="99f1a-178">A hello **Replicon** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="99f1a-178">On hello **Replicon** application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="99f1a-179">![Felhasználók hozzárendelése](./media/active-directory-saas-replicon-tutorial/IC777809.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="99f1a-179">![Assign users](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")</span></span>

3. <span data-ttu-id="99f1a-180">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="99f1a-180">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="99f1a-181">![Igen](./media/active-directory-saas-replicon-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="99f1a-181">![Yes](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="99f1a-182">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="99f1a-182">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="99f1a-183">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="99f1a-183">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

