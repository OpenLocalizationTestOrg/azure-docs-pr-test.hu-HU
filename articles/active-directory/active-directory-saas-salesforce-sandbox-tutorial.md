---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Útmutató a Salesforce védőfal használata az Azure Active Directoryval az egyszeri bejelentkezés, automatikus üzembe helyezést, és több engedélyezéséhez!."
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
ms.openlocfilehash: 32835e79188806bb2ff319eea23b1b52ab585ab1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="21ef2-103">Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal</span><span class="sxs-lookup"><span data-stu-id="21ef2-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="21ef2-104">Ez az oktatóanyag célja az Azure és a Salesforce védőfal integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="21ef2-104">The objective of this tutorial is to show the integration of Azure and Salesforce Sandbox.</span></span>  

>[!TIP]
><span data-ttu-id="21ef2-105">Visszajelzés, tekintse meg a [az Azure támogatási lap](http://go.microsoft.com/fwlink/?LinkId=521878).</span><span class="sxs-lookup"><span data-stu-id="21ef2-105">For feedback, see the [Azure support page](http://go.microsoft.com/fwlink/?LinkId=521878).</span></span> 
> 

<span data-ttu-id="21ef2-106">Védőfalak biztosítanak a szervezet több példánya létre külön környezetekben a számos célra, például a fejlesztői, tesztelési, és a képzési, az adatok és alkalmazások Salesforce éles vállalati veszélyeztetése nélkül.</span><span class="sxs-lookup"><span data-stu-id="21ef2-106">Sandboxes give you the ability to create multiple copies of your organization in separate environments for a variety of purposes, such as development, testing, and training, without compromising the data and applications in your Salesforce production organization.</span></span>  

<span data-ttu-id="21ef2-107">További részletekért lásd: [védőfal – áttekintés](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span><span class="sxs-lookup"><span data-stu-id="21ef2-107">For more details, see [Sandbox Overview](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span></span>

<span data-ttu-id="21ef2-108">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="21ef2-108">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="21ef2-109">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="21ef2-109">A valid Azure subscription</span></span>
* <span data-ttu-id="21ef2-110">A védőfal a Salesforce.com-on</span><span class="sxs-lookup"><span data-stu-id="21ef2-110">A sandbox in Salesforce.com</span></span>

<span data-ttu-id="21ef2-111">Ha egy érvényes védőfal még nem rendelkezik a Salesforce.com-on, akkor lépjen kapcsolatba a Salesforce.</span><span class="sxs-lookup"><span data-stu-id="21ef2-111">If you don’t have a valid sandbox in Salesforce.com yet, you need to contact Salesforce.</span></span>

<span data-ttu-id="21ef2-112">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="21ef2-112">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="21ef2-113">A Salesforce védőfal alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="21ef2-113">Enabling the application integration for Salesforce Sandbox</span></span>
2. <span data-ttu-id="21ef2-114">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21ef2-114">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="21ef2-115">A tartomány engedélyezése</span><span class="sxs-lookup"><span data-stu-id="21ef2-115">Enabling your domain</span></span>
4. <span data-ttu-id="21ef2-116">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="21ef2-116">Configuring user provisioning</span></span>
5. <span data-ttu-id="21ef2-117">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="21ef2-117">Assigning users</span></span>

<span data-ttu-id="21ef2-118">![A forgatókönyv](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="21ef2-118">![Scenario](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-salesforce-sandbox"></a><span data-ttu-id="21ef2-119">A Salesforce védőfal alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="21ef2-119">Enable the application integration for Salesforce Sandbox</span></span>
<span data-ttu-id="21ef2-120">Ez a szakasz célja felvázoló Salesforce védőfal az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="21ef2-120">The objective of this section is to outline how to enable the application integration for Salesforce sandbox.</span></span>

<span data-ttu-id="21ef2-121">**Ahhoz, hogy az alkalmazás integráció Salesforce védőfal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21ef2-121">**To enable the application integration for Salesforce sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="21ef2-122">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-122">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="21ef2-123">![Az Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="21ef2-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="21ef2-124">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="21ef2-124">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="21ef2-125">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="21ef2-125">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="21ef2-126">![Alkalmazások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="21ef2-126">![Applications](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="21ef2-127">Lehetőségre a **Alkalmazáskatalógusában**, kattintson a **alkalmazás hozzáadása**, és kattintson a **hozzáadhat egy alkalmazást a saját szervezet által használható**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-127">To open the **Application Gallery**, click **Add An App**, and then click **Add an application for my organization to use**.</span></span>
   
   <span data-ttu-id="21ef2-128">![Választható? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Mi történjen a teendő?")</span><span class="sxs-lookup"><span data-stu-id="21ef2-128">![What do you want to do?](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want to do?")</span></span>
5. <span data-ttu-id="21ef2-129">Az a **keresőmezőbe**, típus **Salesforce védőfal**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-129">In the **search box**, type **Salesforce Sandbox**.</span></span>
   
   <span data-ttu-id="21ef2-130">![Alkalmazáskatalógusában](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="21ef2-130">![Application Gallery](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")</span></span>
6. <span data-ttu-id="21ef2-131">Az eredmények ablaktáblájában válassza **Salesforce védőfal**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="21ef2-131">In the results pane, select **Salesforce Sandbox**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="21ef2-132">![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce védőfal")</span><span class="sxs-lookup"><span data-stu-id="21ef2-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span></span>
   
## <a name="configur-single-sign-on-sso"></a><span data-ttu-id="21ef2-133">Configur egyszeri bejelentkezés (SSO)</span><span class="sxs-lookup"><span data-stu-id="21ef2-133">Configur single sign-on (SSO)</span></span>

<span data-ttu-id="21ef2-134">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Salesforce fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="21ef2-134">The objective of this section is to outline how to enable users to authenticate to Salesforce with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="21ef2-135">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21ef2-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="21ef2-136">A klasszikus Azure portálon a a **Salesforce védőfal** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21ef2-136">In the Azure classic portal, on the **Salesforce Sandbox** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="21ef2-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="21ef2-137">![Configure single sign-on](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="21ef2-138">A a **hová bejelentkezni Salesforce védőfal felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-138">On the **How would you like users to sign on to Salesforce Sandbox** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="21ef2-139">![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce védőfal")</span><span class="sxs-lookup"><span data-stu-id="21ef2-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span></span>
3. <span data-ttu-id="21ef2-140">Az a **alkalmazás URL-cím konfigurálása** lap a **URL-cím bejelentkezési** szövegmező, írja be az URL-CÍMÉT a következő mintát `http://company.my.salesforce.com`, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-140">On the **Configure App URL** page, in the **Sign On URL** textbox, type your URL using the following pattern `http://company.my.salesforce.com`, and then click **Next**.</span></span>
   
   <span data-ttu-id="21ef2-141">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="21ef2-141">![Configure App URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")</span></span>
4. <span data-ttu-id="21ef2-142">Ha már beállította egyszeri bejelentkezés Salesforce védőfal egy másik példány a könyvtárban, majd konfigurálnia kell a **azonosító** ugyanazt az értéket, hogy a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-142">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure the **Identifier** to have the same value as the **Sign on URL**.</span></span> 
 * <span data-ttu-id="21ef2-143">A **azonosító** mező található ellenőrzésével a **megjelenítése speciális beállítások** jelölőnégyzetet a **alkalmazás URL-cím konfigurálása** párbeszédpanel oldalán.</span><span class="sxs-lookup"><span data-stu-id="21ef2-143">The **Identifier** field can be found by checking the **Show advanced settings** checkbox on the **Configure App URL** page of the dialog.</span></span>
5. <span data-ttu-id="21ef2-144">A a **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** lapján kattintson **tanúsítvánnyal letöltés**, majd mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="21ef2-144">On the **Configure single sign-on at Salesforce Sandbox** page, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
   <span data-ttu-id="21ef2-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="21ef2-145">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="21ef2-146">Egy másik webes böngészőablakban jelentkezzen be a Salesforce védőfal rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="21ef2-146">In a different web browser window, log into your Salesforce sandbox as an administrator.</span></span>
7. <span data-ttu-id="21ef2-147">Kattintson a felső menüben **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-147">In the menu on the top, click **Setup**.</span></span>
   
   <span data-ttu-id="21ef2-148">![A telepítő](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="21ef2-148">![Setup](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")</span></span>
8. <span data-ttu-id="21ef2-149">A bal oldali navigációs ablaktábláján kattintson **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-149">In the navigation pane on the left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="21ef2-150">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="21ef2-150">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")</span></span>
9. <span data-ttu-id="21ef2-151">Egyszeri bejelentkezés beállítások csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21ef2-151">On the Single Sign-On Settings section, perform the following steps:</span></span>
   
   <span data-ttu-id="21ef2-152">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="21ef2-152">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")</span></span>  
 1.  <span data-ttu-id="21ef2-153">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-153">Select **SAML Enabled**.</span></span> 
 2.  <span data-ttu-id="21ef2-154">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="21ef2-154">Click **New**.</span></span>
10. <span data-ttu-id="21ef2-155">A SAML egyszeri bejelentkezés beállítások szakaszban hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21ef2-155">On the SAML Single Sign-On Settings section, perform the following steps:</span></span>
    
    <span data-ttu-id="21ef2-156">![SAML-alapú egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML-alapú egyszeri bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="21ef2-156">![SAML Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")</span></span>  
 1. <span data-ttu-id="21ef2-157">A szövegmezőben, írja be a konfiguráció nevét (pl.: *SPSSOWAAD\_teszt*).</span><span class="sxs-lookup"><span data-stu-id="21ef2-157">In the Name textbox, type the name of the configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 
 2. <span data-ttu-id="21ef2-158">A klasszikus Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** párbeszéd lap, a Másolás a **kiállítójának URL-címe** értékét, és illessze be azt a **kibocsátó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21ef2-158">In the Azure classic portal, on the **Configure single sign-on at Salesforce Sandbox** dialogue page, copy the **Issuer URL** value, and then paste it into the **Issuer** textbox.</span></span>
 3. <span data-ttu-id="21ef2-159">Az a **entitásazonosító** szövegmezőhöz típus **https://test.salesforce.com** Ha ez az első Salesforce védőfal példány, hogy a címtárban ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="21ef2-159">In the **Entity Id** textbox, type **https://test.salesforce.com** if this is the first Salesforce Sandbox instance that you are adding to your directory.</span></span> <span data-ttu-id="21ef2-160">Ha már van egy példánya Salesforce védőfal, majd a a **Entitásazonosító** írja be a **URL-cím bejelentkezési**, amely a következő formátumban kell lennie:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="21ef2-160">If you have already added an instance of Salesforce Sandbox, then for the **Entity ID** type in the **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>   
 4. <span data-ttu-id="21ef2-161">Kattintson a **Tallózás** a letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21ef2-161">Click **Browse** to upload the downloaded certificate.</span></span>  
 5. <span data-ttu-id="21ef2-162">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz az összevonási azonosító felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-162">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span> 
 6. <span data-ttu-id="21ef2-163">Mint **SAML-alapú identitás hely**, jelölje be **identitás a tulajdonos utasítás NameIdentifier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-163">As **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>
 7. <span data-ttu-id="21ef2-164">A klasszikus Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** párbeszéd lap, a Másolás a **távoli bejelentkezési URL-cím** értékét, és illessze be azt a **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21ef2-164">In the Azure classic portal, on the **Configure single sign-on at Salesforce Sandbox** dialogue page, copy the **Remote Login URL** value, and then paste it into the **Identity Provider Login URL** textbox.</span></span> 
 8. <span data-ttu-id="21ef2-165">SFDC nem támogatja az SAML jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="21ef2-165">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="21ef2-166">A probléma megoldásához, illessze be a "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" be azt a **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21ef2-166">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into the **Identity Provider Logout URL** textbox.</span></span>
 9. <span data-ttu-id="21ef2-167">Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-167">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 
 10. <span data-ttu-id="21ef2-168">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="21ef2-168">Click **Save**.</span></span>
11. <span data-ttu-id="21ef2-169">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21ef2-169">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="21ef2-170">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="21ef2-170">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")</span></span>

## <a name="enable-your-domain"></a><span data-ttu-id="21ef2-171">A tartomány</span><span class="sxs-lookup"><span data-stu-id="21ef2-171">Enable your domain</span></span>
<span data-ttu-id="21ef2-172">Jelen szakaszban feltételezzük, hogy már létrehozta a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="21ef2-172">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="21ef2-173">További részletekért lásd: [meghatározása saját tartomány neve](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="21ef2-173">For more details, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="21ef2-174">**Ahhoz, hogy a tartomány, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21ef2-174">**To enable your domain, perform the following steps:**</span></span>

1. <span data-ttu-id="21ef2-175">A bal oldali navigációs ablaktáblán kattintson **tartományok**, és kattintson a **saját tartomány.**</span><span class="sxs-lookup"><span data-stu-id="21ef2-175">In the left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
   <span data-ttu-id="21ef2-176">![Saját tartomány](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="21ef2-176">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="21ef2-177">Győződjön meg arról, hogy a tartomány megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="21ef2-177">Please make sure that your domain has been configured correctly.</span></span> 
   > 
2. <span data-ttu-id="21ef2-178">Az a **bejelentkezési oldal beállításainak** területen kattintson **szerkesztése**, majd, mint **hitelesítési szolgáltatás**, válassza ki a nevét a SAML egyszeri bejelentkezés beállítása a fenti szakaszban leírt, és végül kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-178">In the **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select the name of the SAML Single Sign-On Setting from the previous section, and finally click **Save**.</span></span>
   
   <span data-ttu-id="21ef2-179">![Saját tartomány](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="21ef2-179">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")</span></span>

<span data-ttu-id="21ef2-180">Amint egy tartományhoz, konfigurálva van, a felhasználók használjon bejelentkezni a Salesforce védőfal tartomány URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="21ef2-180">As soon as you have a domain configured, your users should use the domain URL to login to the Salesforce sandbox.</span></span>  

<span data-ttu-id="21ef2-181">Ahhoz, hogy az URL-cím értékét, kattintson az előző szakaszban létrehozott SSO profilra.</span><span class="sxs-lookup"><span data-stu-id="21ef2-181">To get the value of the URL, click the SSO profile you have created in the previous section.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="21ef2-182">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21ef2-182">Configure user provisioning</span></span>
<span data-ttu-id="21ef2-183">Ez a szakasz célja felvázoló engedélyezése a felhasználók átadása, az Active Directory felhasználói fiókoknak Salesforce védőfal felé.</span><span class="sxs-lookup"><span data-stu-id="21ef2-183">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce Sandbox.</span></span>

<span data-ttu-id="21ef2-184">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21ef2-184">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="21ef2-185">A Salesforce-portálon, a felső navigációs sávon válassza ki a nevét, bontsa ki a felhasználó menüben:</span><span class="sxs-lookup"><span data-stu-id="21ef2-185">In the Salesforce portal, in the top navigation bar, select your name to expand your user menu:</span></span>
   
   <span data-ttu-id="21ef2-186">![A beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="21ef2-186">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")</span></span>
2. <span data-ttu-id="21ef2-187">A felhasználó menüből válassza ki a **saját beállítások** megnyitásához a **saját beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="21ef2-187">From your user menu, select **My Settings** to open your **My Settings** page.</span></span>
3. <span data-ttu-id="21ef2-188">Kattintson a bal oldali ablaktáblában **személyes** bontsa ki a személyes szakaszt, és kattintson **alaphelyzetbe állítani a biztonsági jogkivonat**:</span><span class="sxs-lookup"><span data-stu-id="21ef2-188">In the left pane, click **Personal** to expand the Personal section, and then click **Reset My Security Token**:</span></span>
   
   <span data-ttu-id="21ef2-189">![A beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="21ef2-189">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")</span></span>
4. <span data-ttu-id="21ef2-190">A a **alaphelyzetbe állítani a biztonsági jogkivonat** kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** kérjen a Salesforce.com biztonsági jogkivonatot tartalmazó e-maileket.</span><span class="sxs-lookup"><span data-stu-id="21ef2-190">On the **Reset My Security Token** page, click **Reset Security Token** to request an email that contains your Salesforce.com security token.</span></span>
   
   <span data-ttu-id="21ef2-191">![Új jogkivonat](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "új jogkivonat")</span><span class="sxs-lookup"><span data-stu-id="21ef2-191">![New Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")</span></span>
5. <span data-ttu-id="21ef2-192">Ellenőrizze a Salesforce.com az e-mailt a beérkezett e-mail "**esetbejegyzéseinek biztonsági megerősítő**" tulajdonos szerint.</span><span class="sxs-lookup"><span data-stu-id="21ef2-192">Check your email inbox for an email from Salesforce.com with “**salesforce.com.com security confirmation**” as subject.</span></span>
6. <span data-ttu-id="21ef2-193">Tekintse át az e-mailt, és másolja a biztonsági token értékét.</span><span class="sxs-lookup"><span data-stu-id="21ef2-193">Review this email and copy the security token value.</span></span>
7. <span data-ttu-id="21ef2-194">A klasszikus Azure portálon a a **salesforce védőfal** alkalmazás integráció lapján, kattintson a **konfigurálja, a felhasználók átadása** megnyitásához a **konfigurálhatja a felhasználók átadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21ef2-194">In the Azure classic portal, on the **salesforce Sandbox** application integration page, click **Configure user provisioning** to open the **Configure User Provisioning** dialog.</span></span>
   
   <span data-ttu-id="21ef2-195">![Konfigurálja a felhasználók átadása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "felhasználólétesítés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="21ef2-195">![Configure user provisioning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")</span></span>
8. <span data-ttu-id="21ef2-196">Az a **hitelesítő adatait ahhoz, hogy a felhasználók automatikus átadása Salesforce védőfal** lapján adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="21ef2-196">On the **Enter your Salesforce Sandbox credentials to enable automatic user provisioning** page, provide the following configuration settings:</span></span>
   
   <span data-ttu-id="21ef2-197">![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce védőfal")</span><span class="sxs-lookup"><span data-stu-id="21ef2-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span></span>   
 1. <span data-ttu-id="21ef2-198">Az a **Salesforce védőfal rendszergazda felhasználóneve** szövegmezőhöz Salesforce védőfalat a fióknevet, amelynek típusa a **rendszergazda** Salesforce.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="21ef2-198">In the **Salesforce Sandbox Admin User Name** textbox, type a Salesforce sandbox account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
 2. <span data-ttu-id="21ef2-199">Az a **Salesforce védőfal rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="21ef2-199">In the **Salesforce Sandbox Admin Password** textbox, type the password for this account.</span></span>
 3. <span data-ttu-id="21ef2-200">Az a **felhasználói biztonsági jogkivonatot** szövegmezőhöz illessze be a biztonsági token értékét.</span><span class="sxs-lookup"><span data-stu-id="21ef2-200">In the **User Security Token** textbox, paste the security token value.</span></span>
 4. <span data-ttu-id="21ef2-201">Kattintson a **ellenőrzése** a konfiguráció ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="21ef2-201">Click **Validate** to verify your configuration.</span></span>
 5. <span data-ttu-id="21ef2-202">Kattintson a **következő** gombra kattintva nyissa meg a **megerősítő** lap.</span><span class="sxs-lookup"><span data-stu-id="21ef2-202">Click the **Next** button to open the **Confirmation** page.</span></span>
9. <span data-ttu-id="21ef2-203">Az a **megerősítő** kattintson **Complete** a konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ef2-203">On the **Confirmation** page, click **Complete** to save your configuration.</span></span>
   
## <a name="assigning-users"></a><span data-ttu-id="21ef2-204">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="21ef2-204">Assigning users</span></span>

<span data-ttu-id="21ef2-205">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="21ef2-205">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="21ef2-206">**Felhasználók hozzárendelése Salesforce védőfal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21ef2-206">**To assign users to Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="21ef2-207">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="21ef2-207">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="21ef2-208">Az a ** Salesforce védőfal ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="21ef2-208">On the **Salesforce Sandbox **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="21ef2-209">![Felhasználók hozzárendelése](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="21ef2-209">![Assign users](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")</span></span>
3. <span data-ttu-id="21ef2-210">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21ef2-210">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="21ef2-211">![Igen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="21ef2-211">![Yes](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="21ef2-212">Most Várjon 10 percet, és ellenőrizze, hogy a szinkronizált fiókkal Salesforce védőfal felé.</span><span class="sxs-lookup"><span data-stu-id="21ef2-212">You should now wait for 10 minutes and verify that the account has been synchronized to Salesforce Sandbox.</span></span>

<span data-ttu-id="21ef2-213">Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="21ef2-213">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="21ef2-214">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="21ef2-214">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

