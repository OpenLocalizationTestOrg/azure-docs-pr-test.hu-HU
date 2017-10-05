---
title: "Oktatóanyag: Azure Active Directoryval integrált Cisco Webex |} Microsoft Docs"
description: "Megtudhatja, hogyan Cisco Webex használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a><span data-ttu-id="cbcae-103">Oktatóanyag: Azure Active Directoryval integrált Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="cbcae-103">Tutorial: Azure Active Directory Integration with Cisco Webex</span></span>
<span data-ttu-id="cbcae-104">Ez az oktatóanyag célja az Azure és a Cisco Webex integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="cbcae-104">The objective of this tutorial is to show the integration of Azure and Cisco Webex.</span></span>  
<span data-ttu-id="cbcae-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="cbcae-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="cbcae-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="cbcae-106">A valid Azure subscription</span></span>
* <span data-ttu-id="cbcae-107">A Cisco Webex bérlő</span><span class="sxs-lookup"><span data-stu-id="cbcae-107">A Cisco Webex tenant</span></span>

<span data-ttu-id="cbcae-108">Ez az oktatóanyag befejezése után a Cisco Webex rendelt Azure AD-felhasználók fog tudni egyetlen jelentkezzen be az alkalmazás a Cisco Webex vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési), vagy használja a [a hozzáférési Panel bemutatása ](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cbcae-108">After completing this tutorial, the Azure AD users you have assigned to Cisco Webex will be able to single sign into the application at your Cisco Webex company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="cbcae-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cbcae-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="cbcae-110">A Cisco Webex alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cbcae-110">Enabling the application integration for Cisco Webex</span></span>
* <span data-ttu-id="cbcae-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cbcae-111">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="cbcae-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="cbcae-112">Configuring user provisioning</span></span>
* <span data-ttu-id="cbcae-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cbcae-113">Assigning users</span></span>

<span data-ttu-id="cbcae-114">![A forgatókönyv](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="cbcae-114">![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-cisco-webex"></a><span data-ttu-id="cbcae-115">A Cisco Webex alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cbcae-115">Enable the application integration for Cisco Webex</span></span>
<span data-ttu-id="cbcae-116">Ez a szakasz célja helyzeteit vázolják fel, a Cisco Webex alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cbcae-116">The objective of this section is to outline how to enable the application integration for Cisco Webex.</span></span>

<span data-ttu-id="cbcae-117">**Ahhoz, hogy az alkalmazás-integráció Cisco Webex, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cbcae-117">**To enable the application integration for Cisco Webex, perform the following steps:**</span></span>

1. <span data-ttu-id="cbcae-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="cbcae-119">![Az Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="cbcae-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="cbcae-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="cbcae-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="cbcae-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="cbcae-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="cbcae-122">![Alkalmazások](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="cbcae-122">![Applications](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="cbcae-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="cbcae-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="cbcae-124">![Alkalmazás hozzáadása](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-124">![Add application](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="cbcae-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="cbcae-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-126">![Add an application from gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="cbcae-127">Az a **keresőmezőbe**, típus **Cisco Webex**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-127">In the **search box**, type **Cisco Webex**.</span></span>
   
   <span data-ttu-id="cbcae-128">![Alkalmazáskatalógusában](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="cbcae-128">![Application Gallery](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Application Gallery")</span></span>
7. <span data-ttu-id="cbcae-129">Az eredmények ablaktáblájában válassza **Cisco Webex**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="cbcae-129">In the results pane, select **Cisco Webex**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="cbcae-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span><span class="sxs-lookup"><span data-stu-id="cbcae-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="cbcae-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cbcae-131">Configure single sign-on</span></span>

<span data-ttu-id="cbcae-132">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Cisco Webex fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="cbcae-132">The objective of this section is to outline how to enable users to authenticate to Cisco Webex with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="cbcae-133">Ez az eljárás részeként kötelesek base-64 kódolású tanúsítvány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbcae-133">As part of this procedure, you are required to create a base-64 encoded certificate.</span></span> <span data-ttu-id="cbcae-134">Ha nem ismeri ezt az eljárást, tekintse meg a [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="cbcae-134">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="cbcae-135">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cbcae-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="cbcae-136">A klasszikus Azure portálon a a **Cisco Webex** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cbcae-136">In the Azure classic portal, on the **Cisco Webex** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="cbcae-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-137">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="cbcae-138">Az a **hová felhasználók számára történő bejelentkezést Cisco Webex** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-138">On the **How would you like users to sign on to Cisco Webex** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="cbcae-139">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-139">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="cbcae-140">Az a **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-140">On the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
   <span data-ttu-id="cbcae-141">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-141">![Configure app URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Configure app URL")</span></span>   
   1. <span data-ttu-id="cbcae-142">Az a **Sign URL-cím** szövegmező, írja be a Cisco Webex bérlői URL-cím (pl.: *http://contoso.webex.com*).</span><span class="sxs-lookup"><span data-stu-id="cbcae-142">In the **Sing On URL** textbox, type your Cisco Webex tenant URL (e.g.: *http://contoso.webex.com*).</span></span>
   2. <span data-ttu-id="cbcae-143">Az a **Cisco Webex válasz URL-CÍMEN** szövegmezőhöz típusa a **Cisco Webex AssertionConsumerService URL-cím** (pl.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).</span><span class="sxs-lookup"><span data-stu-id="cbcae-143">In the **Cisco Webex Reply URL** textbox, type your **Cisco Webex AssertionConsumerService URL** (e.g.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span></span>
4. <span data-ttu-id="cbcae-144">A a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** töltse le a tanúsítványt, kattintson **tanúsítvánnyal letöltés**, majd mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cbcae-144">On the **Configure single sign-on at Cisco Webex** page, to download your certificate, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
   <span data-ttu-id="cbcae-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-145">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="cbcae-146">Egy másik webes böngészőablakban jelentkezzen be a Cisco Webex vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cbcae-146">In a different web browser window, log into your Cisco Webex company site as an administrator.</span></span>
6. <span data-ttu-id="cbcae-147">Kattintson a felső menüben **helyfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-147">In the menu on the top, click **Site Administration**.</span></span>
   
   <span data-ttu-id="cbcae-148">![Hely felügyelete](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "hely felügyelete")</span><span class="sxs-lookup"><span data-stu-id="cbcae-148">![Site Administration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Administration")</span></span>
7. <span data-ttu-id="cbcae-149">Az a **kezelése hely** területén kattintson **SSO konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-149">In the **Manage Site** section, click **SSO Configuration**.</span></span>
   
   <span data-ttu-id="cbcae-150">![Egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="cbcae-150">![SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO Configuration")</span></span>
8. <span data-ttu-id="cbcae-151">Az összevont webes egyszeri bejelentkezés konfigurálása a szakaszban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="cbcae-151">In the Federated Web SSO Configuration section, perform the following steps:</span></span>
   
   <span data-ttu-id="cbcae-152">![Összevont egyszeri Bejelentkezéses konfigurációs](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "összevont egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-152">![Federated SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federated SSO Configuration")</span></span>  
   1. <span data-ttu-id="cbcae-153">Az a **összevonási protokoll** listáról válassza ki **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-153">From the **Federation Protocol** list, select **SAML 2.0**.</span></span>
   2. <span data-ttu-id="cbcae-154">Hozzon létre egy **Base-64 kódolású** fájlt a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="cbcae-154">Create a **Base-64 encoded** file from your downloaded certificate.</span></span>  
    >[!TIP]
    ><span data-ttu-id="cbcae-155">További részletekért lásd: [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="cbcae-155">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
    >  
   3. <span data-ttu-id="cbcae-156">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, és másolja a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="cbcae-156">Open your base-64 encoded certificate in notepad, and then copy the content of it.</span></span>
   4. <span data-ttu-id="cbcae-157">Kattintson a **SAML-metaadatok importálása**, majd illessze be a base-64 kódolású tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="cbcae-157">Click **Import SAML Metadata**, and then paste your base-64 encoded certificate.</span></span>
   5. <span data-ttu-id="cbcae-158">A klasszikus Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás a **kiállítójának URL-címe** értékét, és illessze be azt a **SAML (IdP ID) kiállító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cbcae-158">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Issuer URL** value, and then paste it into the **Issuer for SAML (IdP ID)** textbox.</span></span>
   6. <span data-ttu-id="cbcae-159">A klasszikus Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás a **távoli bejelentkezési URL-cím** értékét, és illessze be azt a **ügyfél SSO szolgáltatás bejelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cbcae-159">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Remote Login URL** value, and then paste it into the **Customer SSO Service Login URL** textbox.</span></span>
   7. <span data-ttu-id="cbcae-160">Az a **NameID formátum** listáról válassza ki **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-160">From the **NameID Format** list, select **Email address**.</span></span>
   8. <span data-ttu-id="cbcae-161">Az a **AuthnContextClassRef** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-161">In the **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span>
   9. <span data-ttu-id="cbcae-162">A klasszikus Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás a **távoli kijelentkezési URL-cím** értékét, és illessze be azt a **ügyfél egyszeri bejelentkezési szolgáltatás kijelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cbcae-162">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Remote Logout URL** value, and then paste it into the **Customer SSO Service Logout URL** textbox.</span></span>
   10. <span data-ttu-id="cbcae-163">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-163">Click **Update**.</span></span>
9. <span data-ttu-id="cbcae-164">A klasszikus Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lapon válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-164">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, select the single sign-on configuration confirmation, and then click **Complete**.</span></span>
   
   <span data-ttu-id="cbcae-165">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-165">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="cbcae-166">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cbcae-166">Configure user provisioning</span></span>

<span data-ttu-id="cbcae-167">Ahhoz, hogy az Azure AD-felhasználók Cisco Webex bejelentkezni, akkor ki kell építenie a Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="cbcae-167">In order to enable Azure AD users to log into Cisco Webex, they must be provisioned into Cisco Webex.</span></span>  

* <span data-ttu-id="cbcae-168">Cisco Webex, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="cbcae-168">In the case of Cisco Webex, provisioning is a manual task.</span></span>

<span data-ttu-id="cbcae-169">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cbcae-169">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="cbcae-170">Jelentkezzen be a **Cisco Webex** bérlő.</span><span class="sxs-lookup"><span data-stu-id="cbcae-170">Log in to your **Cisco Webex** tenant.</span></span>
2. <span data-ttu-id="cbcae-171">Ugrás a **felhasználók kezelése \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-171">Go to **Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="cbcae-172">![Felhasználók hozzáadása az](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-172">![Add users](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Add users")</span></span>
3. <span data-ttu-id="cbcae-173">A felhasználó hozzáadása területen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cbcae-173">On the Add User section, perform the following steps:</span></span>
   
   <span data-ttu-id="cbcae-174">![Felhasználó hozzáadása](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cbcae-174">![Add user](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Add user")</span></span>   
   1. <span data-ttu-id="cbcae-175">Mint **fióktípus**, jelölje be **állomás**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-175">As **Account Type**, select **Host**.</span></span>
   2. <span data-ttu-id="cbcae-176">Az adatokat egy meglévő Azure AD-felhasználó írja be a következő szövegmezők: **utónevét, vezetéknevét**, **felhasználónév**, **E-mail**, **jelszó**, **Jelszó megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-176">Type the information of an existing Azure AD user into the following textboxes: **First name, Last name**, **User name**, **Email**, **Password**, **Confirm Password**.</span></span>
   3. <span data-ttu-id="cbcae-177">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="cbcae-177">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="cbcae-178">Bármely más Cisco Webex felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Cisco Webex által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="cbcae-178">You can use any other Cisco Webex user account creation tools or APIs provided by Cisco Webex to provision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="cbcae-179">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cbcae-179">Assign users</span></span>
<span data-ttu-id="cbcae-180">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="cbcae-180">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="cbcae-181">**Felhasználók hozzárendelése Cisco Webex, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cbcae-181">**To assign users to Cisco Webex, perform the following steps:**</span></span>

1. <span data-ttu-id="cbcae-182">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="cbcae-182">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="cbcae-183">Az a **Cisco Webex** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="cbcae-183">On the **Cisco Webex** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="cbcae-184">![Felhasználók hozzárendelése](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="cbcae-184">![Assign users](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Assign users")</span></span>
3. <span data-ttu-id="cbcae-185">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cbcae-185">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="cbcae-186">![Igen](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="cbcae-186">![Yes](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="cbcae-187">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="cbcae-187">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="cbcae-188">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cbcae-188">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

