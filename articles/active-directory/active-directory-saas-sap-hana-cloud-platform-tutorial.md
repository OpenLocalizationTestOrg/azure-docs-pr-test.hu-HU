---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP HANA Cloud Platform |} Microsoft Docs"
description: "Útmutató SAP HANA Cloud Platform az Azure Active Directoryval az egyszeri bejelentkezés, automatikus kiépítésének, és több!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: e03bc2410a8d57363c558f723b3bfd0e69b3f4c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a><span data-ttu-id="be11c-103">Oktatóanyag: Azure Active Directory-integráció az SAP HANA Cloud Platform megoldással</span><span class="sxs-lookup"><span data-stu-id="be11c-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform</span></span>
<span data-ttu-id="be11c-104">Ez az oktatóanyag célja az Azure és az SAP HANA Cloud Platform integrációját megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="be11c-104">The objective of this tutorial is to show the integration of Azure and SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="be11c-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="be11c-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="be11c-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="be11c-106">A valid Azure subscription</span></span>
* <span data-ttu-id="be11c-107">Egy SAP HANA-Felhőplatform-fiók</span><span class="sxs-lookup"><span data-stu-id="be11c-107">A SAP HANA Cloud Platform account</span></span>

<span data-ttu-id="be11c-108">Ez az oktatóanyag befejezése után az Azure AD felhasználók SAP HANA Cloud Platform rendelt tudják az alkalmazás használatával történő egyszeri bejelentkezéshez a [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="be11c-108">After completing this tutorial, the Azure AD users you have assigned to SAP HANA Cloud Platform will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

>[!IMPORTANT]
><span data-ttu-id="be11c-109">A saját-alkalmazás központi telepítése, illetve fizethetnek elő az egyszeri bejelentkezés tesztelése SAP HANA Cloud Platform fiókja kérelmet kell.</span><span class="sxs-lookup"><span data-stu-id="be11c-109">You need to deploy your own application or subscribe to an application on your SAP HANA Cloud Platform account to test single sign on.</span></span> <span data-ttu-id="be11c-110">Ebben az oktatóanyagban egy alkalmazás lett telepítve a fiókban.</span><span class="sxs-lookup"><span data-stu-id="be11c-110">In this tutorial, an application is deployed in the account.</span></span>
> 
> 

<span data-ttu-id="be11c-111">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="be11c-111">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="be11c-112">Az alkalmazás-integráció SAP HANA Cloud Platform engedélyezése</span><span class="sxs-lookup"><span data-stu-id="be11c-112">Enabling the application integration for SAP HANA Cloud Platform</span></span>
2. <span data-ttu-id="be11c-113">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="be11c-113">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="be11c-114">Szerepkör hozzárendelése felhasználóhoz</span><span class="sxs-lookup"><span data-stu-id="be11c-114">Assigning a role to a user</span></span>
4. <span data-ttu-id="be11c-115">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="be11c-115">Assigning users</span></span>

<span data-ttu-id="be11c-116">![A forgatókönyv](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="be11c-116">![Scenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a><span data-ttu-id="be11c-117">Az alkalmazás-integráció SAP HANA Cloud Platform engedélyezése</span><span class="sxs-lookup"><span data-stu-id="be11c-117">Enabling the application integration for SAP HANA Cloud Platform</span></span>
<span data-ttu-id="be11c-118">Ez a szakasz célja felvázoló SAP HANA Cloud Platform az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="be11c-118">The objective of this section is to outline how to enable the application integration for SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="be11c-119">**Ahhoz, hogy az alkalmazás-integráció SAP HANA Cloud Platform, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be11c-119">**To enable the application integration for SAP HANA Cloud Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="be11c-120">Az Azure felügyeleti portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="be11c-120">In the Azure Management Portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="be11c-121">![Az Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="be11c-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="be11c-122">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="be11c-122">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="be11c-123">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="be11c-123">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="be11c-124">![Alkalmazások](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="be11c-124">![Applications](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="be11c-125">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="be11c-125">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="be11c-126">![Alkalmazás hozzáadása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="be11c-126">![Add application](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="be11c-127">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="be11c-127">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="be11c-128">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="be11c-128">![Add an application from gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="be11c-129">Az a **keresőmezőbe**, típus **SAP HANA Cloud Platform**.</span><span class="sxs-lookup"><span data-stu-id="be11c-129">In the **search box**, type **SAP HANA Cloud Platform**.</span></span>
   
    <span data-ttu-id="be11c-130">![Alkalmazáskatalógusában](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="be11c-130">![Application Gallery](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")</span></span>
7. <span data-ttu-id="be11c-131">Az eredmények ablaktáblájában válassza **SAP HANA Cloud Platform**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="be11c-131">In the results pane, select **SAP HANA Cloud Platform**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="be11c-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span><span class="sxs-lookup"><span data-stu-id="be11c-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="be11c-133">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="be11c-133">Configure single sign-on</span></span>

<span data-ttu-id="be11c-134">Ez a szakasz célja felvázoló engedélyezése a felhasználók a fiókjukkal SAP HANA Cloud platform hitelesítést az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="be11c-134">The objective of this section is to outline how to enable users to authenticate to SAP HANA Cloud Platform with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="be11c-135">Ez az eljárás részeként kötelesek base-64 kódolású tanúsítvány feltöltéséhez az SAP HANA-Felhőplatform-bérlőjéhez tartozik.</span><span class="sxs-lookup"><span data-stu-id="be11c-135">As part of this procedure, you are required to upload a base-64 encoded certificate to your SAP HANA Cloud Platform tenant.</span></span>  

<span data-ttu-id="be11c-136">Ha nem ismeri ezt az eljárást, tekintse meg a [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="be11c-136">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="be11c-137">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be11c-137">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="be11c-138">A klasszikus Azure portálon a a **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="be11c-138">In the Azure classic portal, on the **SAP HANA Cloud Platform** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="be11c-139">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="be11c-139">![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="be11c-140">Az a **hová felhasználók bejelentkezhetnek a SAP HANA felhő Platform** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="be11c-140">On the **How would you like users to sign on to SAP HANA Cloud Platform** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="be11c-141">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="be11c-141">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="be11c-142">Egy másik webes böngészőablakban jelentkezzen be az SAP HANA felhő Platform vezérlőpultot https://account. \<fekvő állomás\>.ondemand.com/cockpit (pl.: *https://account.hanatrial.ondemand.com/cockpit*).</span><span class="sxs-lookup"><span data-stu-id="be11c-142">In a different web browser window, sign on to the SAP HANA Cloud Platform Cockpit at https://account.\<landscape host\>.ondemand.com/cockpit (e.g.: *https://account.hanatrial.ondemand.com/cockpit*).</span></span>
4. <span data-ttu-id="be11c-143">Kattintson a **megbízható** fülre.</span><span class="sxs-lookup"><span data-stu-id="be11c-143">Click the **Trust** tab.</span></span>
   
    <span data-ttu-id="be11c-144">![Megbízható](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="be11c-144">![Trust](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")</span></span>
5. <span data-ttu-id="be11c-145">Megbízhatósági felügyeleti csoportjában hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="be11c-145">In trust management section, perform the following steps:</span></span>
   
    <span data-ttu-id="be11c-146">![Metaadatok beolvasása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "metaadatot beszerezni")</span><span class="sxs-lookup"><span data-stu-id="be11c-146">![Get Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")</span></span>
   
   1. <span data-ttu-id="be11c-147">Kattintson a **helyi szolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="be11c-147">Click the **Local Service Provider** tab.</span></span>
   2. <span data-ttu-id="be11c-148">Az SAP HANA Cloud Platform metaadatait tartalmazó fájl letöltéséhez kattintson **metaadatok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="be11c-148">To download the SAP HANA Cloud Platform metadata file, click **Get Metadata**.</span></span>
6. <span data-ttu-id="be11c-149">Az Azure Active klasszikus portálon a a **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="be11c-149">In the Azure Active classic portal, on the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
    <span data-ttu-id="be11c-150">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="be11c-150">![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")</span></span>
   
   1. <span data-ttu-id="be11c-151">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-cím segítségével a felhasználók jelentkezzen be a **SAP HANA Cloud Platform** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="be11c-151">In the **Sign On URL** textbox, type the URL used by your users to sign into your **SAP HANA Cloud Platform** application.</span></span> <span data-ttu-id="be11c-152">Ez az a fiók-specifikus az SAP HANA Cloud Platform alkalmazásban védett erőforrás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="be11c-152">This is the account-specific URL of a protected resource in your SAP HANA Cloud Platform application.</span></span> <span data-ttu-id="be11c-153">Az URL-cím alapján a következő mintát: *https://\<applicationName\>\<accountName\>.\< fekvő állomás\>.ondemand.com/\<elérési\_való\_védett\_erőforrás\>*  (pl.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span><span class="sxs-lookup"><span data-stu-id="be11c-153">The URL is based on the following pattern: *https://\<applicationName\>\<accountName\>.\<landscape host\>.ondemand.com/\<path\_to\_protected\_resource\>* (e.g.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="be11c-154">Ez az URL-CÍMÉT, amelyhez a felhasználó hitelesítésére az SAP HANA Cloud Platform alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="be11c-154">This is the URL in your SAP HANA Cloud Platform application that requires the user to authenticate.</span></span>
     > 

   2. <span data-ttu-id="be11c-155">Nyissa meg a letöltött SAP HANA Cloud Platform metaadat-fájlt, és keresse meg a **ns3:AssertionConsumerService** címke.</span><span class="sxs-lookup"><span data-stu-id="be11c-155">Open the downloaded SAP HANA Cloud Platform metadata file, and then locate the **ns3:AssertionConsumerService** tag.</span></span>
   3. <span data-ttu-id="be11c-156">Másolja a értékének a **hely** attribútumot, és illessze be azt a **SAP HANA felhő Platform válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="be11c-156">Copy the value of the **Location** attribute, and then paste it into the **SAP HANA Cloud Platform Reply URL** textbox.</span></span>

7. <span data-ttu-id="be11c-157">Az a **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** töltse le a metaadatokat, kattintson **metaadatok letöltése**, majd mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="be11c-157">On the **Configure single sign-on at SAP HANA Cloud Platform** page, to download your metadata, click **Download metadata**, and then save the file on your computer.</span></span>
   
    <span data-ttu-id="be11c-158">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="be11c-158">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")</span></span>
8. <span data-ttu-id="be11c-159">Az SAP HANA felhő Platform vezérlőpultot a a **helyi szolgáltató** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="be11c-159">On the SAP HANA Cloud Platform Cockpit, in the **Local Service Provider** section, perform the following steps:</span></span>
   
    <span data-ttu-id="be11c-160">![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "felügyeleti megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="be11c-160">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")</span></span>
   
  1. <span data-ttu-id="be11c-161">Kattintson a **Szerkesztés** gombra.</span><span class="sxs-lookup"><span data-stu-id="be11c-161">Click **Edit**.</span></span>
  2. <span data-ttu-id="be11c-162">Mint **konfigurációtípus**, jelölje be **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="be11c-162">As **Configuration Type**, select **Custom**.</span></span>
  3. <span data-ttu-id="be11c-163">Mint **helyi szolgáltatónevet**, hagyja meg az alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="be11c-163">As **Local Provider Name**, leave the default value.</span></span>
  4. <span data-ttu-id="be11c-164">Létrehozásához egy **aláírási kulcs** és egy **aláíró tanúsítvány** pár billentyűt, kattintson a **kulcspár létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="be11c-164">To generate a **Signing Key** and a **Signing Certificate** key pair, click **Generate Key Pair**.</span></span>
  5. <span data-ttu-id="be11c-165">Mint **egyszerű propagálás**, jelölje be **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="be11c-165">As **Principal Propagation**, select **Disabled**.</span></span>
  6. <span data-ttu-id="be11c-166">Mint **kényszerített hitelesítési**, jelölje be **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="be11c-166">As **Force Authentication**, select **Disabled**.</span></span>
  7. <span data-ttu-id="be11c-167">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="be11c-167">Click **Save**.</span></span>

9. <span data-ttu-id="be11c-168">Kattintson a **megbízható identitásszolgáltató** fülre, majd **megbízható identitásszolgáltató hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="be11c-168">Click the **Trusted Identity Provider** tab, and then click **Add Trusted Identity Provider**.</span></span>
   
    <span data-ttu-id="be11c-169">![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "felügyeleti megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="be11c-169">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="be11c-170">A megbízható identitás-szolgáltatóktól kezelését, az egyéni konfigurációs típus választotta, a helyi szolgáltató szakasz kell.</span><span class="sxs-lookup"><span data-stu-id="be11c-170">To manage the list of trusted identity providers, you need to have chosen the Custom configuration type in the Local Service Provider section.</span></span> <span data-ttu-id="be11c-171">Az alapértelmezett típus hogy egy nem szerkeszthető és implicit megbízhatóságot az SAP-azonosító szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="be11c-171">For Default configuration type, you have a non-editable and implicit trust to the SAP ID Service.</span></span> <span data-ttu-id="be11c-172">Sem a megbízhatóság beállításokat nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="be11c-172">For None, you don't have any trust settings.</span></span>
    > 
    > 

10. <span data-ttu-id="be11c-173">Kattintson a **általános** fülre, majd **Tallózás** feltölteni a fájlt a letöltött metaadat.</span><span class="sxs-lookup"><span data-stu-id="be11c-173">Click the **General** tab, and then click **Browse** to upload the downloaded metadata file.</span></span>
    
    <span data-ttu-id="be11c-174">![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "felügyeleti megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="be11c-174">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="be11c-175">A metaadatfájl, értékeit feltöltése után **egyszeri bejelentkezési URL-cím**, **egyetlen kijelentkezési URL-címet** és **aláíró tanúsítvány** automatikusan fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="be11c-175">After uploading the metadata file, the values for **Single Sign-on URL**, **Single Logout URL** and **Signing Certificate** are populated automatically.</span></span>
    > 
    > 

11. <span data-ttu-id="be11c-176">Kattintson az **Attribútumok** fülre.</span><span class="sxs-lookup"><span data-stu-id="be11c-176">Click the **Attributes** tab.</span></span>
12. <span data-ttu-id="be11c-177">Az a **attribútumok** lapra, hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="be11c-177">On the **Attributes** tab, perform the following step:</span></span>
    
    <span data-ttu-id="be11c-178">![Attribútumok](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="be11c-178">![Attributes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")</span></span> 
  * <span data-ttu-id="be11c-179">Kattintson a **Add Assertion-Based attribútum**, majd adja hozzá a következő állítás alapú attribútumok:</span><span class="sxs-lookup"><span data-stu-id="be11c-179">Click **Add Assertion-Based Attribute**, and then add the following assertion-based attributes:</span></span>
       
    | <span data-ttu-id="be11c-180">Helyességi feltétel attribútum</span><span class="sxs-lookup"><span data-stu-id="be11c-180">Assertion Attribute</span></span> | <span data-ttu-id="be11c-181">Egyszerű attribútum</span><span class="sxs-lookup"><span data-stu-id="be11c-181">Principal Attribute</span></span> |
    | --- | --- |
    | <span data-ttu-id="be11c-182">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName</span><span class="sxs-lookup"><span data-stu-id="be11c-182">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname</span></span> |<span data-ttu-id="be11c-183">Utónév</span><span class="sxs-lookup"><span data-stu-id="be11c-183">firstname</span></span> |
    | <span data-ttu-id="be11c-184">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname</span><span class="sxs-lookup"><span data-stu-id="be11c-184">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname</span></span> |<span data-ttu-id="be11c-185">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="be11c-185">lastname</span></span> |
    | <span data-ttu-id="be11c-186">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress</span><span class="sxs-lookup"><span data-stu-id="be11c-186">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress</span></span> |<span data-ttu-id="be11c-187">E-mailek</span><span class="sxs-lookup"><span data-stu-id="be11c-187">email</span></span> 
   
     >[!NOTE]
     ><span data-ttu-id="be11c-188">A konfiguráció az attribútumok attól függ, hogyan HCP a alkalmazás(ok) fejlesztett, azaz mely attribútum(ok) a SAML-válasz várható, és milyen néven (egyszerű attribútum) hozzáféréskor ezt az attribútumot a kódot.</span><span class="sxs-lookup"><span data-stu-id="be11c-188">The configuration of the Attributes depends on how the application(s) on HCP are developed, i.e. which attribute(s) they expect in the SAML response and under which name (Principal Attribute) they access this attribute in the code.</span></span>
     > 
     >  

    1.  <span data-ttu-id="be11c-189">A **alapértelmezett attribútum** ezen a képernyőfelvételen a folyamat csak illusztrációs célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="be11c-189">The **Default Attribute** in the screenshot is just for illustration purposes.</span></span> <span data-ttu-id="be11c-190">Nem működik a forgatókönyv végrehajtásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="be11c-190">It is not required to make the scenario work.</span></span>   
    2.  <span data-ttu-id="be11c-191">A nevek és értékek **egyszerű attribútum** a képernyőfelvételen látható módon az alkalmazás fejlesztése hogyan függ.</span><span class="sxs-lookup"><span data-stu-id="be11c-191">The names and values for **Principal Attribute** shown in the screenshot depend on how the application is developed.</span></span> <span data-ttu-id="be11c-192">Akkor lehet, hogy az alkalmazás által igényelt különböző leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="be11c-192">It is possible that your application requires different mappings.</span></span>
     
13. <span data-ttu-id="be11c-193">A klasszikus Azure portálon a a **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** párbeszéd lapra, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="be11c-193">In the Azure classic portal, on the **Configure single sign-on at SAP HANA Cloud Platform** dialogue page, select the single sign-on configuration confirmation, and then click **Complete**.</span></span>
    
    <span data-ttu-id="be11c-194">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="be11c-194">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")</span></span>

###<a name="assertion-based-groups"></a><span data-ttu-id="be11c-195">Csoportok helyességi feltétel alapján</span><span class="sxs-lookup"><span data-stu-id="be11c-195">Assertion-based groups</span></span>
<span data-ttu-id="be11c-196">Egy nem kötelező lépés konfigurálható az Azure Active Directory identitásszolgáltató csoportok helyességi feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="be11c-196">As an optional step, you can configure assertion-based groups for your Azure Active Directory Identity Provider.</span></span>

<span data-ttu-id="be11c-197">Csoportok használata a SAP HANA Cloud Platform lehetővé teszi egy vagy több felhasználó dinamikusan hozzárendelése egy vagy több szerepkör az SAP HANA Cloud Platform alkalmazásokban, határozza meg a SAML 2.0 helyességi feltétel attribútumok értékek.</span><span class="sxs-lookup"><span data-stu-id="be11c-197">Using groups on SAP HANA Cloud Platform allows you to dynamically assign one or more users to one or more roles in your SAP HANA Cloud Platform applications, determined by values of attributes in the SAML 2.0 assertion.</span></span> 

<span data-ttu-id="be11c-198">Például, ha a helyességi feltételt tartalmaz az attribútum "*szerződés = ideiglenes*", érdemes lehet a csoporthoz lehet hozzáadni az összes érintett felhasználók"*ideiglenes*".</span><span class="sxs-lookup"><span data-stu-id="be11c-198">For example, if the assertion contains the attribute "*contract=temporary*", you may want all affected users to be added to the group "*TEMPORARY*".</span></span> <span data-ttu-id="be11c-199">A csoport "*ideiglenes*" tartalmazhat egy vagy több szerepkör az SAP HANA-Felhőplatform-fiókban telepített egy vagy több alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="be11c-199">The group "*TEMPORARY*" may contain one or more roles from one or more applications deployed in your SAP HANA Cloud Platform account.</span></span>
 
<span data-ttu-id="be11c-200">Csoportok helyességi feltétel alapú használható, ha egyszerre sok felhasználó hozzárendelése egy vagy több szerepkör az alkalmazások az SAP HANA-Felhőplatform-fiókban.</span><span class="sxs-lookup"><span data-stu-id="be11c-200">Use assertion-based groups when you want to simultaneously assign many users to one or more roles of applications in your SAP HANA Cloud Platform account.</span></span> <span data-ttu-id="be11c-201">Ha azt szeretné, csak egyetlen vagy kis számú felhasználók hozzárendelése adott szerepkörök, ajánlott közvetlenül a hozzárendelése a "**engedélyek**" az SAP HANA Cloud Platform vezérlőpultot lapján.</span><span class="sxs-lookup"><span data-stu-id="be11c-201">If you want to assign only a single or small number of users to specific roles, we recommend assigning them directly in the “**Authorizations**” tab of the SAP HANA Cloud Platform cockpit.</span></span>

## <a name="assign-a-role-to-a-user"></a><span data-ttu-id="be11c-202">A szerepkör hozzárendelése felhasználóhoz</span><span class="sxs-lookup"><span data-stu-id="be11c-202">Assign a role to a user</span></span>
<span data-ttu-id="be11c-203">Ahhoz, hogy az Azure AD-felhasználók SAP HANA Cloud Platform bejelentkezni, meg kell hozzájuk rendelhet szerepköröket a SAP HANA felhő platform.</span><span class="sxs-lookup"><span data-stu-id="be11c-203">In order to enable Azure AD users to log into SAP HANA Cloud Platform, you must assign roles in the SAP HANA Cloud Platform to them.</span></span>

<span data-ttu-id="be11c-204">**A szerepkör hozzárendelése felhasználóhoz, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be11c-204">**To assign a role to a user, perform the following steps:**</span></span>

1. <span data-ttu-id="be11c-205">Jelentkezzen be a **SAP HANA Cloud Platform** vezérlőpultot.</span><span class="sxs-lookup"><span data-stu-id="be11c-205">Log in to your **SAP HANA Cloud Platform** cockpit.</span></span>
2. <span data-ttu-id="be11c-206">Hajtsa végre a következő:</span><span class="sxs-lookup"><span data-stu-id="be11c-206">Perform the following:</span></span>
   
   <span data-ttu-id="be11c-207">![Engedélyek](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "engedélyek")</span><span class="sxs-lookup"><span data-stu-id="be11c-207">![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")</span></span>
   
  1. <span data-ttu-id="be11c-208">Kattintson a **engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="be11c-208">Click **Authorization**.</span></span>
  2. <span data-ttu-id="be11c-209">Kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="be11c-209">Click the **Users** tab.</span></span>
  3. <span data-ttu-id="be11c-210">Az a **felhasználói** szövegmező, írja be a felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="be11c-210">In the **User** textbox, type the user’s email address.</span></span>
  4. <span data-ttu-id="be11c-211">Kattintson a **hozzárendelése** a felhasználó hozzárendelése egy szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="be11c-211">Click **Assign** to assign the user to a role.</span></span>
  5. <span data-ttu-id="be11c-212">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="be11c-212">Click **Save**.</span></span>

## <a name="assign-users"></a><span data-ttu-id="be11c-213">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="be11c-213">Assign users</span></span>
<span data-ttu-id="be11c-214">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="be11c-214">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="be11c-215">**Felhasználók hozzárendelése SAP HANA Cloud Platform, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be11c-215">**To assign users to SAP HANA Cloud Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="be11c-216">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="be11c-216">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="be11c-217">Az a **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="be11c-217">On the **SAP HANA Cloud Platform** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="be11c-218">![Felhasználók hozzárendelése](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="be11c-218">![Assign Users](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")</span></span>
3. <span data-ttu-id="be11c-219">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="be11c-219">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="be11c-220">![Igen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="be11c-220">![Yes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="be11c-221">Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="be11c-221">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="be11c-222">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="be11c-222">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

