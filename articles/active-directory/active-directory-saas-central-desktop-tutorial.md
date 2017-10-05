---
title: "Oktatóanyag: Azure Active Directoryval integrált központi asztali |} Microsoft Docs"
description: "Megtudhatja, hogyan központi asztal használata az Azure Active Directoryval az egyszeri bejelentkezés, automatikus kiépítésének, és több!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a><span data-ttu-id="3fb0d-103">Oktatóanyag: Azure Active Directory-integráció központi asztal</span><span class="sxs-lookup"><span data-stu-id="3fb0d-103">Tutorial: Azure Active Directory integration with Central Desktop</span></span>
<span data-ttu-id="3fb0d-104">Ez az oktatóanyag célja az integráció az Azure és a központi asztal megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-104">The objective of this tutorial is to show the integration of Azure and Central Desktop.</span></span> <span data-ttu-id="3fb0d-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="3fb0d-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="3fb0d-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="3fb0d-106">A valid Azure subscription</span></span>
* <span data-ttu-id="3fb0d-107">Egy központi asztali az egyszeri bejelentkezés engedélyezett előfizetés / központi asztali bérlői</span><span class="sxs-lookup"><span data-stu-id="3fb0d-107">A Central desktop single sign on enabled subscription / Central desktop tenant</span></span>

<span data-ttu-id="3fb0d-108">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3fb0d-108">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="3fb0d-109">A központi asztali alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3fb0d-109">Enabling the application integration for Central Desktop</span></span>
* <span data-ttu-id="3fb0d-110">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3fb0d-110">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="3fb0d-111">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="3fb0d-111">Configuring user provisioning</span></span>
* <span data-ttu-id="3fb0d-112">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3fb0d-112">Assigning users</span></span>

<span data-ttu-id="3fb0d-113">![A forgatókönyv](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-113">![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-central-desktop"></a><span data-ttu-id="3fb0d-114">A központi asztali alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3fb0d-114">Enable the application integration for Central Desktop</span></span>
<span data-ttu-id="3fb0d-115">Ez a szakasz célja helyzeteit vázolják fel, az alkalmazás központi asztal-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-115">The objective of this section is to outline how to enable the application integration for Central Desktop.</span></span>

<span data-ttu-id="3fb0d-116">**Ahhoz, hogy az alkalmazás-integráció központi asztali, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3fb0d-116">**To enable the application integration for Central Desktop, perform the following steps:**</span></span>

1. <span data-ttu-id="3fb0d-117">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-117">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="3fb0d-118">![Az Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="3fb0d-119">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-119">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="3fb0d-120">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-120">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="3fb0d-121">![Alkalmazások](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-121">![Applications](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="3fb0d-122">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-122">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="3fb0d-123">![Alkalmazás hozzáadása](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-123">![Add application](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="3fb0d-124">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-124">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="3fb0d-125">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-125">![Add an application from gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="3fb0d-126">Az a **keresőmezőbe**, típus **központi asztali**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-126">In the **search box**, type **Central Desktop**.</span></span>
   
   <span data-ttu-id="3fb0d-127">![Alkalmazáskatalógusában](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-127">![Application gallery](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")</span></span>
7. <span data-ttu-id="3fb0d-128">Az eredmények ablaktáblájában válassza **központi asztali**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-128">In the results pane, select **Central Desktop**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="3fb0d-129">![Központi asztali](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "központi asztali")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="3fb0d-130">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3fb0d-130">Configure single sign-on</span></span>

<span data-ttu-id="3fb0d-131">Ez a szakasz célja felvázoló engedélyezése a felhasználók a fiókjukkal központi asztalra hitelesítést az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-131">The objective of this section is to outline how to enable users to authenticate to Central Desktop with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="3fb0d-132">Ez az eljárás részeként kötelesek base-64 kódolású tanúsítvány feltöltése a központi asztal-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-132">As part of this procedure, you are required to upload a base-64 encoded certificate to your Central Desktop tenant.</span></span>  
<span data-ttu-id="3fb0d-133">Ha nem ismeri ezt az eljárást, tekintse meg a [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="3fb0d-133">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

<span data-ttu-id="3fb0d-134">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3fb0d-134">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="3fb0d-135">A klasszikus Azure portálon a a **központi asztali** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-135">In the Azure classic portal, on the **Central Desktop** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
   <span data-ttu-id="3fb0d-136">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-136">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="3fb0d-137">Az a **hová felhasználók bejelentkezhetnek a központi asztalra** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-137">On the **How would you like users to sign on to Central Desktop** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="3fb0d-138">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-138">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="3fb0d-139">Az a **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="3fb0d-139">On the **Configure App URL** page, perform the following steps, and then click **Next**:</span></span> 
   
   1. <span data-ttu-id="3fb0d-140">Az a **központi asztali bejelentkezési az URL-címet** szövegmező, a központi asztali bérlői URL-címét (pl.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="3fb0d-140">In the **Central Desktop Sign In URL** textbox, type the URL of your Central Desktop tenant (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   2. <span data-ttu-id="3fb0d-141">Központi asztali válasz URL-cím szövegmezőbe írja be a központi asztali AssertionConsumerService URL-címet (pl.: https://contoso.centraldesktop.com/saml2-assertion.php).</span><span class="sxs-lookup"><span data-stu-id="3fb0d-141">In the Central  Desktop Reply URL textbox, type your Central Desktop AssertionConsumerService URL (e.g.:  https://contoso.centraldesktop.com/saml2-assertion.php).</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="3fb0d-142">Az érték beszerezni a központi asztali metaadatok (pl.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="3fb0d-142">You can get the value from the central desktop metadata (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   >  
   
   <span data-ttu-id="3fb0d-143">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-143">![Configure app URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")</span></span>
4. <span data-ttu-id="3fb0d-144">A a **konfigurálása egyszeri bejelentkezéshez központi asztali** töltse le a tanúsítványt, kattintson **tanúsítvánnyal letöltés**, majd mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-144">On the **Configure single sign-on at Central Desktop** page, to download your certificate, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
  <span data-ttu-id="3fb0d-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-145">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="3fb0d-146">Jelentkezzen be a **központi asztali** bérlő.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-146">Log in to your **Central Desktop** tenant.</span></span>
6. <span data-ttu-id="3fb0d-147">Nyissa meg a **beállítások**, kattintson a **speciális**, és kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-147">Go to **Settings**, click **Advanced**, and then click **Single Sign On**.</span></span>
   
  <span data-ttu-id="3fb0d-148">![A telepítő - speciális](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "telepítő – speciális")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-148">![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")</span></span>
7. <span data-ttu-id="3fb0d-149">Az a **egyetlen bejelentkezést a beállítások** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3fb0d-149">On the **Single Sign On Settings** page, perform the following steps:</span></span>
   
  <span data-ttu-id="3fb0d-150">![Egyszeri bejelentkezés beállításai](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-150">![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")</span></span>
   
  1. <span data-ttu-id="3fb0d-151">Válassza ki **engedélyezése SAML v2 az egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-151">Select **Enable SAML v2 Single Sign On**.</span></span>
  2. <span data-ttu-id="3fb0d-152">A klasszikus Azure portálon a a **konfigurálása egyszeri bejelentkezéshez központi asztali** lapon, másolja a **kiállítójának URL-címe** értékét, és illessze be azt a **egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-152">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Issuer URL** value, and then paste it into the **SSO URL** textbox.</span></span>
  3. <span data-ttu-id="3fb0d-153">A klasszikus Azure portálon a a **konfigurálása egyszeri bejelentkezéshez központi asztali** lapon, másolja a **távoli bejelentkezési URL-cím** értékét, és illessze be azt a **Egyszeri bejelentkezési URL-cím** szövegmező .</span><span class="sxs-lookup"><span data-stu-id="3fb0d-153">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Remote Login URL** value, and then paste it into the **SSO Login URL** textbox.</span></span>
  4. <span data-ttu-id="3fb0d-154">A klasszikus Azure portálon a a **konfigurálása egyszeri bejelentkezéshez központi asztali** lapon, másolja a **egyetlen Sign-Out URL-címe** értékét, és illessze be azt a **SSO kijelentkezési URL-cím**szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-154">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Single Sign-Out Service URL** value, and then paste it into the **SSO Logout URL** textbox.</span></span>
8. <span data-ttu-id="3fb0d-155">Az a **üzenet aláírás-ellenőrzési módszer** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3fb0d-155">In the **Message Signature Verification Method** section, perform the following steps:</span></span>
   
   <span data-ttu-id="3fb0d-156">![Üzenet-aláírást ellenőrzési módszer](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "üzenet-aláírást ellenőrzési módszer")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-156">![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")</span></span>
   
  1. <span data-ttu-id="3fb0d-157">Válassza ki **tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-157">Select **Certificate**.</span></span>
  2. <span data-ttu-id="3fb0d-158">Az a **SSO tanúsítvány** listáról válassza ki **RSH SHA256**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-158">From the **SSO Certificate** list, select **RSH SHA256**.</span></span>
  3. <span data-ttu-id="3fb0d-159">Hozzon létre egy szövegfájlt a letöltött tanúsítvány, másolja a tartalmat a szöveges fájl, és illessze be azt a **SSO tanúsítvány** mező.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-159">Create a text file from the downloaded certificate, copy the content of the text file, and then paste it into the **SSO Certificate** field.</span></span>  
     >[!TIP]
     ><span data-ttu-id="3fb0d-160">További részletekért lásd: [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="3fb0d-160">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
      >  
   4. <span data-ttu-id="3fb0d-161">Válassza ki **jeleníti meg a hivatkozást a SAMLv2 bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-161">Select **Display a link to your SAMLv2 login page**.</span></span>
9. <span data-ttu-id="3fb0d-162">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-162">Click **Update**.</span></span>
10. <span data-ttu-id="3fb0d-163">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-163">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="3fb0d-164">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-164">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="3fb0d-165">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3fb0d-165">Configure user provisioning</span></span>

<span data-ttu-id="3fb0d-166">Az AAD-felhasználókat kell jelentkezhetnek be akkor ki kell építenie a központi asztali alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-166">For AAD users to be able to sign in, they must be provisioned to the Central Desktop application.</span></span> <span data-ttu-id="3fb0d-167">Ez a szakasz ismerteti, hogyan AAD felhasználói fiókokat hozhat létre a központi asztali.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-167">This section describes how to create AAD user accounts in Central Desktop.</span></span>

<span data-ttu-id="3fb0d-168">**Felhasználói fiókok központi asztalra telepítéséhez:**</span><span class="sxs-lookup"><span data-stu-id="3fb0d-168">**To provision user accounts to Central Desktop:**</span></span>
1. <span data-ttu-id="3fb0d-169">Jelentkezzen be a központi asztali bérlő.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-169">Log in to your Central Desktop tenant.</span></span>
2. <span data-ttu-id="3fb0d-170">Ugrás a **személyek \> belső tagok**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-170">Go to **People \> Internal Members**.</span></span>
3. <span data-ttu-id="3fb0d-171">Kattintson a **belső tagok felvétele**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-171">Click **Add Internal Members**.</span></span>
   
  <span data-ttu-id="3fb0d-172">![Személyek](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-172">![People](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")</span></span>
4. <span data-ttu-id="3fb0d-173">Az a **E-mail címet az új tagok** szövegmező, írja be egy AAD-fiókba rendelkezés szeretne, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-173">In the **Email Address of New Members** textbox, type an AAD account you want to provision, and then click **Next**.</span></span>
   
  <span data-ttu-id="3fb0d-174">![E-mail-címeket az új tagok](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mail címet az új tagok")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-174">![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")</span></span>
5. <span data-ttu-id="3fb0d-175">Kattintson a **hozzáadása belső tag**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-175">Click **Add Internal member(s)**.</span></span>
   
  <span data-ttu-id="3fb0d-176">![Adja hozzá a belső tag](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "belső tag hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-176">![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="3fb0d-177">A hozzáadott felhasználók kattintson a fiók aktiválásához szükséges megerősítési hivatkozást tartalmazó e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-177">The users you have added will receive an email that includes a confirmation link they need to click to activate the account.</span></span>
   > 

>[!NOTE]
><span data-ttu-id="3fb0d-178">Bármely más központi asztali felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz központi asztali által nyújtott API-k</span><span class="sxs-lookup"><span data-stu-id="3fb0d-178">You can use any other Central Desktop user account creation tools or APIs provided by Central Desktop to provision AAD user accounts</span></span>
>  

## <a name="assign-users"></a><span data-ttu-id="3fb0d-179">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3fb0d-179">Assign users</span></span>
<span data-ttu-id="3fb0d-180">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-180">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="3fb0d-181">**Felhasználók hozzárendelése központi asztali, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3fb0d-181">**To assign users to Central Desktop, perform the following steps:**</span></span>

1. <span data-ttu-id="3fb0d-182">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-182">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="3fb0d-183">Az a **központi asztali** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-183">On the **Central Desktop** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="3fb0d-184">![Felhasználók hozzárendelése](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-184">![Assign users](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")</span></span>
3. <span data-ttu-id="3fb0d-185">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-185">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="3fb0d-186">![Igen](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="3fb0d-186">![Yes](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="3fb0d-187">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="3fb0d-187">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="3fb0d-188">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3fb0d-188">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

