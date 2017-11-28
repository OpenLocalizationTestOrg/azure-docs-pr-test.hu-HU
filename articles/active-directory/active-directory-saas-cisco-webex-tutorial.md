---
title: "Oktatóanyag: Azure Active Directoryval integrált Cisco Webex |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Cisco Webex az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
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
ms.openlocfilehash: 9fc11e58a7acaa6fbfb32eeccbfbf85984950e67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a><span data-ttu-id="9352e-103">Oktatóanyag: Azure Active Directoryval integrált Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="9352e-103">Tutorial: Azure Active Directory Integration with Cisco Webex</span></span>
<span data-ttu-id="9352e-104">hello Ez az oktatóanyag célja az Azure és a Cisco Webex tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="9352e-104">hello objective of this tutorial is tooshow hello integration of Azure and Cisco Webex.</span></span>  
<span data-ttu-id="9352e-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9352e-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="9352e-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="9352e-106">A valid Azure subscription</span></span>
* <span data-ttu-id="9352e-107">A Cisco Webex bérlő</span><span class="sxs-lookup"><span data-stu-id="9352e-107">A Cisco Webex tenant</span></span>

<span data-ttu-id="9352e-108">Ez az oktatóanyag befejezése után a hozzárendelt tooCisco Webex lesz képes toosingle bejelentkezési hello alkalmazásba a Cisco Webex vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési) Azure AD-felhasználók hello, vagy a hello segítségével [toohello bemutatása Hozzáférési Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9352e-108">After completing this tutorial, hello Azure AD users you have assigned tooCisco Webex will be able toosingle sign into hello application at your Cisco Webex company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="9352e-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="9352e-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="9352e-110">Cisco Webex hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9352e-110">Enabling hello application integration for Cisco Webex</span></span>
* <span data-ttu-id="9352e-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9352e-111">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="9352e-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="9352e-112">Configuring user provisioning</span></span>
* <span data-ttu-id="9352e-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9352e-113">Assigning users</span></span>

<span data-ttu-id="9352e-114">![A forgatókönyv](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="9352e-114">![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-cisco-webex"></a><span data-ttu-id="9352e-115">Cisco Webex hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9352e-115">Enable hello application integration for Cisco Webex</span></span>
<span data-ttu-id="9352e-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="9352e-116">hello objective of this section is toooutline how tooenable hello application integration for Cisco Webex.</span></span>

<span data-ttu-id="9352e-117">**tooenable hello alkalmazásintegráció Cisco Webex, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9352e-117">**tooenable hello application integration for Cisco Webex, perform hello following steps:**</span></span>

1. <span data-ttu-id="9352e-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9352e-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="9352e-119">![Az Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="9352e-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="9352e-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="9352e-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="9352e-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="9352e-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="9352e-122">![Alkalmazások](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="9352e-122">![Applications](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="9352e-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="9352e-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="9352e-124">![Alkalmazás hozzáadása](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="9352e-124">![Add application](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="9352e-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9352e-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="9352e-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="9352e-126">![Add an application from gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="9352e-127">A hello **keresőmezőbe**, típus **Cisco Webex**.</span><span class="sxs-lookup"><span data-stu-id="9352e-127">In hello **search box**, type **Cisco Webex**.</span></span>
   
   <span data-ttu-id="9352e-128">![Alkalmazáskatalógusában](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="9352e-128">![Application Gallery](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Application Gallery")</span></span>
7. <span data-ttu-id="9352e-129">Hello eredmények ablaktábláján jelöljön ki **Cisco Webex**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9352e-129">In hello results pane, select **Cisco Webex**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="9352e-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span><span class="sxs-lookup"><span data-stu-id="9352e-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="9352e-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9352e-131">Configure single sign-on</span></span>

<span data-ttu-id="9352e-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooCisco Webex fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="9352e-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooCisco Webex with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="9352e-133">Ez az eljárás részeként szükség toocreate base-64 kódolású tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="9352e-133">As part of this procedure, you are required toocreate a base-64 encoded certificate.</span></span> <span data-ttu-id="9352e-134">Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="9352e-134">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="9352e-135">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="9352e-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="9352e-136">A klasszikus Azure portálon, a hello hello **Cisco Webex** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9352e-136">In hello Azure classic portal, on hello **Cisco Webex** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="9352e-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="9352e-137">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="9352e-138">A hello **hogyan szeretné tooCisco Webex a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="9352e-138">On hello **How would you like users toosign on tooCisco Webex** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="9352e-139">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="9352e-139">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="9352e-140">A hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="9352e-140">On hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
   <span data-ttu-id="9352e-141">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="9352e-141">![Configure app URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Configure app URL")</span></span>   
   1. <span data-ttu-id="9352e-142">A hello **Sign URL-cím** szövegmező, írja be a Cisco Webex bérlői URL-cím (pl.: *http://contoso.webex.com*).</span><span class="sxs-lookup"><span data-stu-id="9352e-142">In hello **Sing On URL** textbox, type your Cisco Webex tenant URL (e.g.: *http://contoso.webex.com*).</span></span>
   2. <span data-ttu-id="9352e-143">A hello **Cisco Webex válasz URL-CÍMEN** szövegmezőhöz típusa a **Cisco Webex AssertionConsumerService URL-cím** (pl.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).</span><span class="sxs-lookup"><span data-stu-id="9352e-143">In hello **Cisco Webex Reply URL** textbox, type your **Cisco Webex AssertionConsumerService URL** (e.g.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span></span>
4. <span data-ttu-id="9352e-144">A hello **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** lap, toodownload a tanúsítványt, kattintson **tanúsítvánnyal letöltés**, és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9352e-144">On hello **Configure single sign-on at Cisco Webex** page, toodownload your certificate, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
   <span data-ttu-id="9352e-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="9352e-145">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="9352e-146">Egy másik webes böngészőablakban jelentkezzen be a Cisco Webex vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9352e-146">In a different web browser window, log into your Cisco Webex company site as an administrator.</span></span>
6. <span data-ttu-id="9352e-147">Hello hello felső menüben kattintson a **helyfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="9352e-147">In hello menu on hello top, click **Site Administration**.</span></span>
   
   <span data-ttu-id="9352e-148">![Hely felügyelete](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "hely felügyelete")</span><span class="sxs-lookup"><span data-stu-id="9352e-148">![Site Administration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Administration")</span></span>
7. <span data-ttu-id="9352e-149">A hello **kezelése hely** területén kattintson **SSO konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="9352e-149">In hello **Manage Site** section, click **SSO Configuration**.</span></span>
   
   <span data-ttu-id="9352e-150">![Egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="9352e-150">![SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO Configuration")</span></span>
8. <span data-ttu-id="9352e-151">Az összevont webes egyszeri bejelentkezés konfigurációs szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="9352e-151">In hello Federated Web SSO Configuration section, perform hello following steps:</span></span>
   
   <span data-ttu-id="9352e-152">![Összevont egyszeri Bejelentkezéses konfigurációs](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "összevont egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="9352e-152">![Federated SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federated SSO Configuration")</span></span>  
   1. <span data-ttu-id="9352e-153">A hello **összevonási protokoll** listáról válassza ki **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="9352e-153">From hello **Federation Protocol** list, select **SAML 2.0**.</span></span>
   2. <span data-ttu-id="9352e-154">Hozzon létre egy **Base-64 kódolású** fájlt a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="9352e-154">Create a **Base-64 encoded** file from your downloaded certificate.</span></span>  
    >[!TIP]
    ><span data-ttu-id="9352e-155">További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="9352e-155">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
    >  
   3. <span data-ttu-id="9352e-156">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben, és azt a tartalom másolás hello.</span><span class="sxs-lookup"><span data-stu-id="9352e-156">Open your base-64 encoded certificate in notepad, and then copy hello content of it.</span></span>
   4. <span data-ttu-id="9352e-157">Kattintson a **SAML-metaadatok importálása**, majd illessze be a base-64 kódolású tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="9352e-157">Click **Import SAML Metadata**, and then paste your base-64 encoded certificate.</span></span>
   5. <span data-ttu-id="9352e-158">A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás hello **kiállítójának URL-címe** értékét, és illessze be hello **SAML (IdP ID)kiállító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9352e-158">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Issuer URL** value, and then paste it into hello **Issuer for SAML (IdP ID)** textbox.</span></span>
   6. <span data-ttu-id="9352e-159">Hello hello a klasszikus Azure portálra a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás hello **távoli bejelentkezési URL-cím** értékét, és illessze be hello **ügyfél SSO szolgáltatás bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9352e-159">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Remote Login URL** value, and then paste it into hello **Customer SSO Service Login URL** textbox.</span></span>
   7. <span data-ttu-id="9352e-160">A hello **NameID formátum** listáról válassza ki **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="9352e-160">From hello **NameID Format** list, select **Email address**.</span></span>
   8. <span data-ttu-id="9352e-161">A hello **AuthnContextClassRef** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="9352e-161">In hello **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span>
   9. <span data-ttu-id="9352e-162">Hello hello a klasszikus Azure portálra a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás hello **távoli kijelentkezési URL-cím** értékét, és illessze be hello **ügyfél egyszeri bejelentkezési szolgáltatás jelentkezzen ki URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9352e-162">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Remote Logout URL** value, and then paste it into hello **Customer SSO Service Logout URL** textbox.</span></span>
   10. <span data-ttu-id="9352e-163">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="9352e-163">Click **Update**.</span></span>
9. <span data-ttu-id="9352e-164">A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lapon válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="9352e-164">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, select hello single sign-on configuration confirmation, and then click **Complete**.</span></span>
   
   <span data-ttu-id="9352e-165">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="9352e-165">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="9352e-166">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9352e-166">Configure user provisioning</span></span>

<span data-ttu-id="9352e-167">A sorrend tooenable az Azure AD felhasználók toolog Cisco Webex be azok ki kell építenie a Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="9352e-167">In order tooenable Azure AD users toolog into Cisco Webex, they must be provisioned into Cisco Webex.</span></span>  

* <span data-ttu-id="9352e-168">Cisco Webex hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9352e-168">In hello case of Cisco Webex, provisioning is a manual task.</span></span>

<span data-ttu-id="9352e-169">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9352e-169">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="9352e-170">Jelentkezzen be tooyour **Cisco Webex** bérlő.</span><span class="sxs-lookup"><span data-stu-id="9352e-170">Log in tooyour **Cisco Webex** tenant.</span></span>
2. <span data-ttu-id="9352e-171">Nyissa meg túl**felhasználók kezelése \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9352e-171">Go too**Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="9352e-172">![Felhasználók hozzáadása az](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="9352e-172">![Add users](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Add users")</span></span>
3. <span data-ttu-id="9352e-173">Felhasználó hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="9352e-173">On hello Add User section, perform hello following steps:</span></span>
   
   <span data-ttu-id="9352e-174">![Felhasználó hozzáadása](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="9352e-174">![Add user](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Add user")</span></span>   
   1. <span data-ttu-id="9352e-175">Mint **fióktípus**, jelölje be **állomás**.</span><span class="sxs-lookup"><span data-stu-id="9352e-175">As **Account Type**, select **Host**.</span></span>
   2. <span data-ttu-id="9352e-176">Hello információ egy meglévő Azure AD-felhasználó írja be a következő szövegmezők hello: **utónevét, vezetéknevét**, **felhasználónév**, **E-mail**, **jelszó**, **Jelszó megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="9352e-176">Type hello information of an existing Azure AD user into hello following textboxes: **First name, Last name**, **User name**, **Email**, **Password**, **Confirm Password**.</span></span>
   3. <span data-ttu-id="9352e-177">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="9352e-177">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="9352e-178">Bármely más Cisco Webex felhasználói fiók létrehozása eszközök vagy Cisco Webex tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="9352e-178">You can use any other Cisco Webex user account creation tools or APIs provided by Cisco Webex tooprovision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="9352e-179">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9352e-179">Assign users</span></span>
<span data-ttu-id="9352e-180">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9352e-180">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="9352e-181">**tooassign felhasználók tooCisco Webex, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9352e-181">**tooassign users tooCisco Webex, perform hello following steps:**</span></span>

1. <span data-ttu-id="9352e-182">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="9352e-182">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="9352e-183">A hello **Cisco Webex** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="9352e-183">On hello **Cisco Webex** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="9352e-184">![Felhasználók hozzárendelése](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="9352e-184">![Assign users](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Assign users")</span></span>
3. <span data-ttu-id="9352e-185">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="9352e-185">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="9352e-186">![Igen](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="9352e-186">![Yes](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="9352e-187">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="9352e-187">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="9352e-188">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9352e-188">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

