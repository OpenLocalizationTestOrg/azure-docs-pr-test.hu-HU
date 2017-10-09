---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Salesforce védőfal az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: deccd6a07b57c8ba56b7e1c3f3ab311813ca017b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="7ba50-103">Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal</span><span class="sxs-lookup"><span data-stu-id="7ba50-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="7ba50-104">hello Ez az oktatóanyag célja az Azure és a Salesforce védőfal tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="7ba50-104">hello objective of this tutorial is tooshow hello integration of Azure and Salesforce Sandbox.</span></span>  

>[!TIP]
><span data-ttu-id="7ba50-105">Visszajelzés, lásd: hello [az Azure támogatási lap](http://go.microsoft.com/fwlink/?LinkId=521878).</span><span class="sxs-lookup"><span data-stu-id="7ba50-105">For feedback, see hello [Azure support page](http://go.microsoft.com/fwlink/?LinkId=521878).</span></span> 
> 

<span data-ttu-id="7ba50-106">Védőfalak adjon meg hello képességét toocreate számos célra, fejlesztési, például a különböző környezetekben a szervezet több példányát tesztelése, és a képzési, nélkül megőrzése hello adatokhoz és alkalmazásokhoz a Salesforce éles környezetben szervezet.</span><span class="sxs-lookup"><span data-stu-id="7ba50-106">Sandboxes give you hello ability toocreate multiple copies of your organization in separate environments for a variety of purposes, such as development, testing, and training, without compromising hello data and applications in your Salesforce production organization.</span></span>  

<span data-ttu-id="7ba50-107">További részletekért lásd: [védőfal – áttekintés](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span><span class="sxs-lookup"><span data-stu-id="7ba50-107">For more details, see [Sandbox Overview](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span></span>

<span data-ttu-id="7ba50-108">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7ba50-108">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="7ba50-109">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="7ba50-109">A valid Azure subscription</span></span>
* <span data-ttu-id="7ba50-110">A védőfal a Salesforce.com-on</span><span class="sxs-lookup"><span data-stu-id="7ba50-110">A sandbox in Salesforce.com</span></span>

<span data-ttu-id="7ba50-111">Ha egy érvényes védőfal még nem rendelkezik a Salesforce.com-on, toocontact Salesforce kell.</span><span class="sxs-lookup"><span data-stu-id="7ba50-111">If you don’t have a valid sandbox in Salesforce.com yet, you need toocontact Salesforce.</span></span>

<span data-ttu-id="7ba50-112">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="7ba50-112">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="7ba50-113">Alkalmazások integrálása hello Salesforce védőfal engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7ba50-113">Enabling hello application integration for Salesforce Sandbox</span></span>
2. <span data-ttu-id="7ba50-114">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7ba50-114">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="7ba50-115">A tartomány engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7ba50-115">Enabling your domain</span></span>
4. <span data-ttu-id="7ba50-116">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="7ba50-116">Configuring user provisioning</span></span>
5. <span data-ttu-id="7ba50-117">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7ba50-117">Assigning users</span></span>

<span data-ttu-id="7ba50-118">![A forgatókönyv](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="7ba50-118">![Scenario](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-salesforce-sandbox"></a><span data-ttu-id="7ba50-119">Alkalmazások integrálása hello Salesforce védőfal engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7ba50-119">Enable hello application integration for Salesforce Sandbox</span></span>
<span data-ttu-id="7ba50-120">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Salesforce védőfal.</span><span class="sxs-lookup"><span data-stu-id="7ba50-120">hello objective of this section is toooutline how tooenable hello application integration for Salesforce sandbox.</span></span>

<span data-ttu-id="7ba50-121">**tooenable hello alkalmazásintegráció Salesforce védőfal, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7ba50-121">**tooenable hello application integration for Salesforce sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ba50-122">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-122">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="7ba50-123">![Az Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="7ba50-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="7ba50-124">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="7ba50-124">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="7ba50-125">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="7ba50-125">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="7ba50-126">![Alkalmazások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="7ba50-126">![Applications](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="7ba50-127">tooopen hello **Alkalmazáskatalógusában**, kattintson a **alkalmazás hozzáadása**, és kattintson a **hozzáadhat egy alkalmazást a saját szervezet toouse**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-127">tooopen hello **Application Gallery**, click **Add An App**, and then click **Add an application for my organization toouse**.</span></span>
   
   <span data-ttu-id="7ba50-128">![Miről szeretne toodo? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Miről szeretne toodo?")</span><span class="sxs-lookup"><span data-stu-id="7ba50-128">![What do you want toodo?](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want toodo?")</span></span>
5. <span data-ttu-id="7ba50-129">A hello **keresőmezőbe**, típus **Salesforce védőfal**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-129">In hello **search box**, type **Salesforce Sandbox**.</span></span>
   
   <span data-ttu-id="7ba50-130">![Alkalmazáskatalógusában](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="7ba50-130">![Application Gallery](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")</span></span>
6. <span data-ttu-id="7ba50-131">Hello eredmények ablaktábláján jelöljön ki **Salesforce védőfal**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7ba50-131">In hello results pane, select **Salesforce Sandbox**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="7ba50-132">![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce védőfal")</span><span class="sxs-lookup"><span data-stu-id="7ba50-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span></span>
   
## <a name="configur-single-sign-on-sso"></a><span data-ttu-id="7ba50-133">Configur egyszeri bejelentkezés (SSO)</span><span class="sxs-lookup"><span data-stu-id="7ba50-133">Configur single sign-on (SSO)</span></span>

<span data-ttu-id="7ba50-134">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooSalesforce fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="7ba50-134">hello objective of this section is toooutline how tooenable users tooauthenticate tooSalesforce with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="7ba50-135">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="7ba50-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ba50-136">A klasszikus Azure portálon, a hello hello **Salesforce védőfal** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7ba50-136">In hello Azure classic portal, on hello **Salesforce Sandbox** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="7ba50-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7ba50-137">![Configure single sign-on](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="7ba50-138">A hello **hogyan szeretné a védőfal tooSalesforce felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-138">On hello **How would you like users toosign on tooSalesforce Sandbox** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="7ba50-139">![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce védőfal")</span><span class="sxs-lookup"><span data-stu-id="7ba50-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span></span>
3. <span data-ttu-id="7ba50-140">A hello **alkalmazás URL-cím konfigurálása** lap hello **URL-cím bejelentkezési** szövegmező, írja be az URL-CÍMÉT a következő mintát hello `http://company.my.salesforce.com`, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-140">On hello **Configure App URL** page, in hello **Sign On URL** textbox, type your URL using hello following pattern `http://company.my.salesforce.com`, and then click **Next**.</span></span>
   
   <span data-ttu-id="7ba50-141">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7ba50-141">![Configure App URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")</span></span>
4. <span data-ttu-id="7ba50-142">Ha már beállította egyszeri bejelentkezés Salesforce védőfal egy másik példány a könyvtárban, akkor is konfigurálnia kell hello **azonosító** toohave hello hello megegyező értékűnek **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-142">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure hello **Identifier** toohave hello same value as hello **Sign on URL**.</span></span> 
 * <span data-ttu-id="7ba50-143">Hello **azonosító** mező található hello ellenőrzésével **megjelenítése speciális beállítások** hello jelölőnégyzet **alkalmazás URL-cím konfigurálása** hello párbeszédpanel oldalán.</span><span class="sxs-lookup"><span data-stu-id="7ba50-143">hello **Identifier** field can be found by checking hello **Show advanced settings** checkbox on hello **Configure App URL** page of hello dialog.</span></span>
5. <span data-ttu-id="7ba50-144">A hello **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** kattintson **tanúsítvánnyal letöltés**, és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7ba50-144">On hello **Configure single sign-on at Salesforce Sandbox** page, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
   <span data-ttu-id="7ba50-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7ba50-145">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="7ba50-146">Egy másik webes böngészőablakban jelentkezzen be a Salesforce védőfal rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7ba50-146">In a different web browser window, log into your Salesforce sandbox as an administrator.</span></span>
7. <span data-ttu-id="7ba50-147">Hello hello felső menüben kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-147">In hello menu on hello top, click **Setup**.</span></span>
   
   <span data-ttu-id="7ba50-148">![A telepítő](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="7ba50-148">![Setup](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")</span></span>
8. <span data-ttu-id="7ba50-149">Hello bal oldali hello navigációs ablaktábláján kattintson **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-149">In hello navigation pane on hello left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="7ba50-150">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="7ba50-150">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")</span></span>
9. <span data-ttu-id="7ba50-151">Hello egyszeri bejelentkezési beállítások szakaszban hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="7ba50-151">On hello Single Sign-On Settings section, perform hello following steps:</span></span>
   
   <span data-ttu-id="7ba50-152">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="7ba50-152">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")</span></span>  
 1.  <span data-ttu-id="7ba50-153">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-153">Select **SAML Enabled**.</span></span> 
 2.  <span data-ttu-id="7ba50-154">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7ba50-154">Click **New**.</span></span>
10. <span data-ttu-id="7ba50-155">Hello SAML-alapú egyszeri bejelentkezés beállítások szakaszban hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="7ba50-155">On hello SAML Single Sign-On Settings section, perform hello following steps:</span></span>
    
    <span data-ttu-id="7ba50-156">![SAML-alapú egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML-alapú egyszeri bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="7ba50-156">![SAML Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")</span></span>  
 1. <span data-ttu-id="7ba50-157">A hello szövegmezőben írja be a hello konfigurációs hello nevét (pl.: *SPSSOWAAD\_teszt*).</span><span class="sxs-lookup"><span data-stu-id="7ba50-157">In hello Name textbox, type hello name of hello configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 
 2. <span data-ttu-id="7ba50-158">A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** párbeszéd lap, a Másolás hello **kiállítójának URL-címe** értékét, és illessze be hello **kibocsátó**szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7ba50-158">In hello Azure classic portal, on hello **Configure single sign-on at Salesforce Sandbox** dialogue page, copy hello **Issuer URL** value, and then paste it into hello **Issuer** textbox.</span></span>
 3. <span data-ttu-id="7ba50-159">A hello **entitásazonosító** szövegmezőhöz típus **https://test.salesforce.com** Ha hello első védőfal Salesforce-példány, hogy tooyour directory ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="7ba50-159">In hello **Entity Id** textbox, type **https://test.salesforce.com** if this is hello first Salesforce Sandbox instance that you are adding tooyour directory.</span></span> <span data-ttu-id="7ba50-160">Ha már hozzáadott egy példányát, majd a Salesforce védőfal hello **Entitásazonosító** hello típusának **URL-cím bejelentkezési**, amely a következő formátumban kell lennie:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="7ba50-160">If you have already added an instance of Salesforce Sandbox, then for hello **Entity ID** type in hello **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>   
 4. <span data-ttu-id="7ba50-161">Kattintson a **Tallózás** tooupload hello letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="7ba50-161">Click **Browse** tooupload hello downloaded certificate.</span></span>  
 5. <span data-ttu-id="7ba50-162">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-162">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span> 
 6. <span data-ttu-id="7ba50-163">Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-163">As **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>
 7. <span data-ttu-id="7ba50-164">A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** párbeszéd lap, a Másolás hello **távoli bejelentkezési URL-cím** értékét, és illessze be hello **identitásszolgáltató Bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7ba50-164">In hello Azure classic portal, on hello **Configure single sign-on at Salesforce Sandbox** dialogue page, copy hello **Remote Login URL** value, and then paste it into hello **Identity Provider Login URL** textbox.</span></span> 
 8. <span data-ttu-id="7ba50-165">SFDC nem támogatja az SAML jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="7ba50-165">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="7ba50-166">A probléma megoldásához, illessze be a "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" hello be azt **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7ba50-166">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into hello **Identity Provider Logout URL** textbox.</span></span>
 9. <span data-ttu-id="7ba50-167">Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-167">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 
 10. <span data-ttu-id="7ba50-168">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7ba50-168">Click **Save**.</span></span>
11. <span data-ttu-id="7ba50-169">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7ba50-169">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="7ba50-170">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7ba50-170">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")</span></span>

## <a name="enable-your-domain"></a><span data-ttu-id="7ba50-171">A tartomány</span><span class="sxs-lookup"><span data-stu-id="7ba50-171">Enable your domain</span></span>
<span data-ttu-id="7ba50-172">Jelen szakaszban feltételezzük, hogy már létrehozta a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="7ba50-172">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="7ba50-173">További részletekért lásd: [meghatározása saját tartomány neve](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="7ba50-173">For more details, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="7ba50-174">**tooenable a tartományba, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="7ba50-174">**tooenable your domain, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ba50-175">Hello bal oldali navigációs ablaktábláján kattintson **tartományok**, és kattintson a **saját tartomány.**</span><span class="sxs-lookup"><span data-stu-id="7ba50-175">In hello left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
   <span data-ttu-id="7ba50-176">![Saját tartomány](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="7ba50-176">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="7ba50-177">Győződjön meg arról, hogy a tartomány megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7ba50-177">Please make sure that your domain has been configured correctly.</span></span> 
   > 
2. <span data-ttu-id="7ba50-178">A hello **bejelentkezési lap beállításai** területen kattintson **szerkesztése**, később, **hitelesítési szolgáltatás**, jelölje be az előző hello SAML-alapú egyszeri bejelentkezés beállítása hello hello neve szakaszt, és végül kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-178">In hello **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select hello name of hello SAML Single Sign-On Setting from hello previous section, and finally click **Save**.</span></span>
   
   <span data-ttu-id="7ba50-179">![Saját tartomány](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="7ba50-179">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")</span></span>

<span data-ttu-id="7ba50-180">Amint egy tartományhoz, konfigurálva van, a felhasználók hello tartomány URL-cím toologin toohello Salesforce védőfal használja.</span><span class="sxs-lookup"><span data-stu-id="7ba50-180">As soon as you have a domain configured, your users should use hello domain URL toologin toohello Salesforce sandbox.</span></span>  

<span data-ttu-id="7ba50-181">tooget hello értékének hello URL-t, kattintson a hello egyszeri bejelentkezési profil hello előző szakaszban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="7ba50-181">tooget hello value of hello URL, click hello SSO profile you have created in hello previous section.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="7ba50-182">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7ba50-182">Configure user provisioning</span></span>
<span data-ttu-id="7ba50-183">hello ebben a szakaszban célja toooutline hogyan tooSalesforce védőfal tooenable a felhasználók átadása az Active Directory felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="7ba50-183">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooSalesforce Sandbox.</span></span>

<span data-ttu-id="7ba50-184">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7ba50-184">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ba50-185">Hello Salesforce portal hello felső navigációs sávon válassza ki a név tooexpand a felhasználó menüben:</span><span class="sxs-lookup"><span data-stu-id="7ba50-185">In hello Salesforce portal, in hello top navigation bar, select your name tooexpand your user menu:</span></span>
   
   <span data-ttu-id="7ba50-186">![A beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="7ba50-186">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")</span></span>
2. <span data-ttu-id="7ba50-187">A felhasználó menüből válassza ki a **saját beállítások** tooopen a **saját beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="7ba50-187">From your user menu, select **My Settings** tooopen your **My Settings** page.</span></span>
3. <span data-ttu-id="7ba50-188">Hello bal oldali ablaktáblában kattintson **személyes** tooexpand hello személyes szakaszt, és kattintson a **alaphelyzetbe állítani a biztonsági jogkivonat**:</span><span class="sxs-lookup"><span data-stu-id="7ba50-188">In hello left pane, click **Personal** tooexpand hello Personal section, and then click **Reset My Security Token**:</span></span>
   
   <span data-ttu-id="7ba50-189">![A beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="7ba50-189">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")</span></span>
4. <span data-ttu-id="7ba50-190">A hello **alaphelyzetbe állítani a biztonsági jogkivonat** kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** toorequest a Salesforce.com biztonsági jogkivonatot tartalmazó e-maileket.</span><span class="sxs-lookup"><span data-stu-id="7ba50-190">On hello **Reset My Security Token** page, click **Reset Security Token** toorequest an email that contains your Salesforce.com security token.</span></span>
   
   <span data-ttu-id="7ba50-191">![Új jogkivonat](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "új jogkivonat")</span><span class="sxs-lookup"><span data-stu-id="7ba50-191">![New Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")</span></span>
5. <span data-ttu-id="7ba50-192">Ellenőrizze a Salesforce.com az e-mailt a beérkezett e-mail "**esetbejegyzéseinek biztonsági megerősítő**" tulajdonos szerint.</span><span class="sxs-lookup"><span data-stu-id="7ba50-192">Check your email inbox for an email from Salesforce.com with “**salesforce.com.com security confirmation**” as subject.</span></span>
6. <span data-ttu-id="7ba50-193">Tekintse át az e-mailek és a példány hello biztonsági jogkivonat érték.</span><span class="sxs-lookup"><span data-stu-id="7ba50-193">Review this email and copy hello security token value.</span></span>
7. <span data-ttu-id="7ba50-194">A klasszikus Azure portálon, a hello hello **salesforce védőfal** alkalmazás integráció lapján, kattintson a **konfigurálhatja a felhasználók átadása** tooopen hello **konfigurálhatja a felhasználók átadása**párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7ba50-194">In hello Azure classic portal, on hello **salesforce Sandbox** application integration page, click **Configure user provisioning** tooopen hello **Configure User Provisioning** dialog.</span></span>
   
   <span data-ttu-id="7ba50-195">![Konfigurálja a felhasználók átadása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "felhasználólétesítés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7ba50-195">![Configure user provisioning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")</span></span>
8. <span data-ttu-id="7ba50-196">A hello **adja meg a Salesforce védőfal hitelesítő adatok tooenable automatikus felhasználólétesítés** lapján adja meg a következő konfigurációs beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="7ba50-196">On hello **Enter your Salesforce Sandbox credentials tooenable automatic user provisioning** page, provide hello following configuration settings:</span></span>
   
   <span data-ttu-id="7ba50-197">![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce védőfal")</span><span class="sxs-lookup"><span data-stu-id="7ba50-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span></span>   
 1. <span data-ttu-id="7ba50-198">A hello **Salesforce védőfal rendszergazda felhasználóneve** szövegmezőhöz Salesforce védőfalat fióknév rendelkezik hello típus **rendszergazda** Salesforce.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="7ba50-198">In hello **Salesforce Sandbox Admin User Name** textbox, type a Salesforce sandbox account name that has hello **System Administrator** profile in Salesforce.com assigned.</span></span>
 2. <span data-ttu-id="7ba50-199">A hello **Salesforce védőfal rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="7ba50-199">In hello **Salesforce Sandbox Admin Password** textbox, type hello password for this account.</span></span>
 3. <span data-ttu-id="7ba50-200">A hello **felhasználói biztonsági jogkivonatot** szövegmezőhöz Beillesztés hello biztonsági token értékét.</span><span class="sxs-lookup"><span data-stu-id="7ba50-200">In hello **User Security Token** textbox, paste hello security token value.</span></span>
 4. <span data-ttu-id="7ba50-201">Kattintson a **ellenőrzése** tooverify a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7ba50-201">Click **Validate** tooverify your configuration.</span></span>
 5. <span data-ttu-id="7ba50-202">Kattintson a hello **következő** gomb tooopen hello **megerősítő** lap.</span><span class="sxs-lookup"><span data-stu-id="7ba50-202">Click hello **Next** button tooopen hello **Confirmation** page.</span></span>
9. <span data-ttu-id="7ba50-203">A hello **megerősítő** lapján kattintson **Complete** toosave a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7ba50-203">On hello **Confirmation** page, click **Complete** toosave your configuration.</span></span>
   
## <a name="assigning-users"></a><span data-ttu-id="7ba50-204">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7ba50-204">Assigning users</span></span>

<span data-ttu-id="7ba50-205">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="7ba50-205">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="7ba50-206">**tooassign felhasználók tooSalesforce védőfal, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7ba50-206">**tooassign users tooSalesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ba50-207">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="7ba50-207">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="7ba50-208">A hello ** Salesforce védőfal ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="7ba50-208">On hello **Salesforce Sandbox **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="7ba50-209">![Felhasználók hozzárendelése](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="7ba50-209">![Assign users](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")</span></span>
3. <span data-ttu-id="7ba50-210">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="7ba50-210">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="7ba50-211">![Igen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="7ba50-211">![Yes](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="7ba50-212">Most Várjon 10 percet, és ellenőrizze, hogy a hello fiók szinkronizált tooSalesforce védőfal megtörtént.</span><span class="sxs-lookup"><span data-stu-id="7ba50-212">You should now wait for 10 minutes and verify that hello account has been synchronized tooSalesforce Sandbox.</span></span>

<span data-ttu-id="7ba50-213">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="7ba50-213">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="7ba50-214">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="7ba50-214">For more details about hello Access Panel, see [Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

