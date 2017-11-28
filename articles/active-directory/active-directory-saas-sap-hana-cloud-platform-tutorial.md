---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP HANA Cloud Platform |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SAP HANA Cloud Platform az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
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
ms.openlocfilehash: cc6610969b1c7b08f776e6969a5977fc75eb9ab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a><span data-ttu-id="70f57-103">Oktatóanyag: Azure Active Directory-integráció az SAP HANA Cloud Platform megoldással</span><span class="sxs-lookup"><span data-stu-id="70f57-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform</span></span>
<span data-ttu-id="70f57-104">hello Ez az oktatóanyag célja az Azure és az SAP HANA Cloud Platform tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="70f57-104">hello objective of this tutorial is tooshow hello integration of Azure and SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="70f57-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="70f57-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="70f57-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="70f57-106">A valid Azure subscription</span></span>
* <span data-ttu-id="70f57-107">Egy SAP HANA-Felhőplatform-fiók</span><span class="sxs-lookup"><span data-stu-id="70f57-107">A SAP HANA Cloud Platform account</span></span>

<span data-ttu-id="70f57-108">Ez az oktatóanyag befejezése után a hozzárendelt tooSAP HANA felhő platform képes toosingle bejelentkezési hello alkalmazásba hello használata az Azure AD-felhasználók hello [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="70f57-108">After completing this tutorial, hello Azure AD users you have assigned tooSAP HANA Cloud Platform will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

>[!IMPORTANT]
><span data-ttu-id="70f57-109">Tooan alkalmazás a SAP HANA Cloud Platform egyetlen fiók tootest jelentkezzen be az előfizetés vagy toodeploy saját alkalmazás szükséges.</span><span class="sxs-lookup"><span data-stu-id="70f57-109">You need toodeploy your own application or subscribe tooan application on your SAP HANA Cloud Platform account tootest single sign on.</span></span> <span data-ttu-id="70f57-110">Ebben az oktatóanyagban egy alkalmazás hello fiók van telepítve.</span><span class="sxs-lookup"><span data-stu-id="70f57-110">In this tutorial, an application is deployed in hello account.</span></span>
> 
> 

<span data-ttu-id="70f57-111">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="70f57-111">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="70f57-112">Alkalmazások integrálása hello SAP HANA felhő platform engedélyezése</span><span class="sxs-lookup"><span data-stu-id="70f57-112">Enabling hello application integration for SAP HANA Cloud Platform</span></span>
2. <span data-ttu-id="70f57-113">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="70f57-113">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="70f57-114">Egy szerepkör tooa felhasználói hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70f57-114">Assigning a role tooa user</span></span>
4. <span data-ttu-id="70f57-115">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70f57-115">Assigning users</span></span>

<span data-ttu-id="70f57-116">![A forgatókönyv](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="70f57-116">![Scenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-sap-hana-cloud-platform"></a><span data-ttu-id="70f57-117">Alkalmazások integrálása hello SAP HANA felhő platform engedélyezése</span><span class="sxs-lookup"><span data-stu-id="70f57-117">Enabling hello application integration for SAP HANA Cloud Platform</span></span>
<span data-ttu-id="70f57-118">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció SAP HANA felhő platform.</span><span class="sxs-lookup"><span data-stu-id="70f57-118">hello objective of this section is toooutline how tooenable hello application integration for SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="70f57-119">**tooenable hello alkalmazásintegráció SAP HANA felhő platform, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="70f57-119">**tooenable hello application integration for SAP HANA Cloud Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="70f57-120">Hello Azure felügyeleti portálon, a hello bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="70f57-120">In hello Azure Management Portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="70f57-121">![Az Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="70f57-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="70f57-122">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="70f57-122">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="70f57-123">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="70f57-123">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="70f57-124">![Alkalmazások](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="70f57-124">![Applications](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="70f57-125">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="70f57-125">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="70f57-126">![Alkalmazás hozzáadása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="70f57-126">![Add application](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="70f57-127">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="70f57-127">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="70f57-128">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="70f57-128">![Add an application from gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="70f57-129">A hello **keresőmezőbe**, típus **SAP HANA Cloud Platform**.</span><span class="sxs-lookup"><span data-stu-id="70f57-129">In hello **search box**, type **SAP HANA Cloud Platform**.</span></span>
   
    <span data-ttu-id="70f57-130">![Alkalmazáskatalógusában](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="70f57-130">![Application Gallery](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")</span></span>
7. <span data-ttu-id="70f57-131">Hello eredmények ablaktábláján jelöljön ki **SAP HANA Cloud Platform**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="70f57-131">In hello results pane, select **SAP HANA Cloud Platform**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="70f57-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span><span class="sxs-lookup"><span data-stu-id="70f57-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="70f57-133">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="70f57-133">Configure single sign-on</span></span>

<span data-ttu-id="70f57-134">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooSAP HANA Cloud Platform fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="70f57-134">hello objective of this section is toooutline how tooenable users tooauthenticate tooSAP HANA Cloud Platform with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="70f57-135">Ez az eljárás részeként szükség tooupload a base-64 kódolású tanúsítvány tooyour SAP HANA Cloud Platform bérlő.</span><span class="sxs-lookup"><span data-stu-id="70f57-135">As part of this procedure, you are required tooupload a base-64 encoded certificate tooyour SAP HANA Cloud Platform tenant.</span></span>  

<span data-ttu-id="70f57-136">Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="70f57-136">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="70f57-137">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="70f57-137">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="70f57-138">A klasszikus Azure portálon, a hello hello **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **egyszeribejelentkezéskonfigurálása**párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="70f57-138">In hello Azure classic portal, on hello **SAP HANA Cloud Platform** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="70f57-139">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="70f57-139">![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="70f57-140">A hello **hogyan szeretné tooSAP HANA Cloud Platform a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="70f57-140">On hello **How would you like users toosign on tooSAP HANA Cloud Platform** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="70f57-141">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="70f57-141">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="70f57-142">Egy másik webes böngészőablakban toohello SAP HANA felhő Platform Vezérlőpult https://account a bejelentkezéskor. \<fekvő állomás\>.ondemand.com/cockpit (pl.: *https://account.hanatrial.ondemand.com/cockpit*).</span><span class="sxs-lookup"><span data-stu-id="70f57-142">In a different web browser window, sign on toohello SAP HANA Cloud Platform Cockpit at https://account.\<landscape host\>.ondemand.com/cockpit (e.g.: *https://account.hanatrial.ondemand.com/cockpit*).</span></span>
4. <span data-ttu-id="70f57-143">Kattintson a hello **megbízható** fülre.</span><span class="sxs-lookup"><span data-stu-id="70f57-143">Click hello **Trust** tab.</span></span>
   
    <span data-ttu-id="70f57-144">![Megbízható](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="70f57-144">![Trust](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")</span></span>
5. <span data-ttu-id="70f57-145">Megbízhatósági felügyeleti csoportjában hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="70f57-145">In trust management section, perform hello following steps:</span></span>
   
    <span data-ttu-id="70f57-146">![Metaadatok beolvasása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "metaadatot beszerezni")</span><span class="sxs-lookup"><span data-stu-id="70f57-146">![Get Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")</span></span>
   
   1. <span data-ttu-id="70f57-147">Kattintson a hello **helyi szolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="70f57-147">Click hello **Local Service Provider** tab.</span></span>
   2. <span data-ttu-id="70f57-148">Kattintson a toodownload hello SAP HANA Cloud Platform metaadatfájl, **metaadatok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="70f57-148">toodownload hello SAP HANA Cloud Platform metadata file, click **Get Metadata**.</span></span>
6. <span data-ttu-id="70f57-149">Hello Azure Active klasszikus portál a hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="70f57-149">In hello Azure Active classic portal, on hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
    <span data-ttu-id="70f57-150">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="70f57-150">![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")</span></span>
   
   1. <span data-ttu-id="70f57-151">A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címet használják a felhasználók toosign azokat a **SAP HANA Cloud Platform** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="70f57-151">In hello **Sign On URL** textbox, type hello URL used by your users toosign into your **SAP HANA Cloud Platform** application.</span></span> <span data-ttu-id="70f57-152">Ez egy SAP HANA Cloud Platform alkalmazásában védett erőforrás hello fiókspecifikus URL-címet.</span><span class="sxs-lookup"><span data-stu-id="70f57-152">This is hello account-specific URL of a protected resource in your SAP HANA Cloud Platform application.</span></span> <span data-ttu-id="70f57-153">hello URL-címe alapján a következő mintát hello: *https://\<applicationName\>\<accountName\>.\< fekvő állomás\>.ondemand.com/\<elérési\_való\_védett\_erőforrás\>*  (pl.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span><span class="sxs-lookup"><span data-stu-id="70f57-153">hello URL is based on hello following pattern: *https://\<applicationName\>\<accountName\>.\<landscape host\>.ondemand.com/\<path\_to\_protected\_resource\>* (e.g.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="70f57-154">Ez az az SAP HANA Cloud Platform alkalmazás hello felhasználói tooauthenticate igénylő hello URL-címet.</span><span class="sxs-lookup"><span data-stu-id="70f57-154">This is hello URL in your SAP HANA Cloud Platform application that requires hello user tooauthenticate.</span></span>
     > 

   2. <span data-ttu-id="70f57-155">Nyissa meg a letöltött hello SAP HANA Cloud Platform metaadat-fájlt, és keresse a hello **ns3:AssertionConsumerService** címke.</span><span class="sxs-lookup"><span data-stu-id="70f57-155">Open hello downloaded SAP HANA Cloud Platform metadata file, and then locate hello **ns3:AssertionConsumerService** tag.</span></span>
   3. <span data-ttu-id="70f57-156">Másolja a hello hello értékének **hely** attribútumot, és illessze be hello **SAP HANA felhő Platform válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="70f57-156">Copy hello value of hello **Location** attribute, and then paste it into hello **SAP HANA Cloud Platform Reply URL** textbox.</span></span>

7. <span data-ttu-id="70f57-157">A hello **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** lap, toodownload a metaadatok kattintson **metaadatok letöltése**, majd mentse hello fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="70f57-157">On hello **Configure single sign-on at SAP HANA Cloud Platform** page, toodownload your metadata, click **Download metadata**, and then save hello file on your computer.</span></span>
   
    <span data-ttu-id="70f57-158">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="70f57-158">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")</span></span>
8. <span data-ttu-id="70f57-159">Az SAP HANA felhő Platform Vezérlőpult hello a hello **helyi szolgáltató** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="70f57-159">On hello SAP HANA Cloud Platform Cockpit, in hello **Local Service Provider** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="70f57-160">![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "felügyeleti megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="70f57-160">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")</span></span>
   
  1. <span data-ttu-id="70f57-161">Kattintson a **Szerkesztés** gombra.</span><span class="sxs-lookup"><span data-stu-id="70f57-161">Click **Edit**.</span></span>
  2. <span data-ttu-id="70f57-162">Mint **konfigurációtípus**, jelölje be **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="70f57-162">As **Configuration Type**, select **Custom**.</span></span>
  3. <span data-ttu-id="70f57-163">Mint **helyi szolgáltatónevet**, hagyja hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="70f57-163">As **Local Provider Name**, leave hello default value.</span></span>
  4. <span data-ttu-id="70f57-164">toogenerate egy **aláírási kulcs** és egy **aláíró tanúsítvány** pár billentyűt, kattintson a **kulcspár létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="70f57-164">toogenerate a **Signing Key** and a **Signing Certificate** key pair, click **Generate Key Pair**.</span></span>
  5. <span data-ttu-id="70f57-165">Mint **egyszerű propagálás**, jelölje be **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="70f57-165">As **Principal Propagation**, select **Disabled**.</span></span>
  6. <span data-ttu-id="70f57-166">Mint **kényszerített hitelesítési**, jelölje be **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="70f57-166">As **Force Authentication**, select **Disabled**.</span></span>
  7. <span data-ttu-id="70f57-167">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="70f57-167">Click **Save**.</span></span>

9. <span data-ttu-id="70f57-168">Kattintson a hello **megbízható identitásszolgáltató** fülre, majd **megbízható identitásszolgáltató hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="70f57-168">Click hello **Trusted Identity Provider** tab, and then click **Add Trusted Identity Provider**.</span></span>
   
    <span data-ttu-id="70f57-169">![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "felügyeleti megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="70f57-169">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="70f57-170">megbízható identitás-szolgáltatóktól toomanage hello listája, hello egyéni konfigurációs típus hello helyi szolgáltató szakasz a kiválasztott toohave kell.</span><span class="sxs-lookup"><span data-stu-id="70f57-170">toomanage hello list of trusted identity providers, you need toohave chosen hello Custom configuration type in hello Local Service Provider section.</span></span> <span data-ttu-id="70f57-171">Az alapértelmezett típus hogy egy nem szerkeszthető és implicit megbízhatósági toohello SAP ID szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="70f57-171">For Default configuration type, you have a non-editable and implicit trust toohello SAP ID Service.</span></span> <span data-ttu-id="70f57-172">Sem a megbízhatóság beállításokat nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="70f57-172">For None, you don't have any trust settings.</span></span>
    > 
    > 

10. <span data-ttu-id="70f57-173">Kattintson a hello **általános** fülre, majd **Tallózás** tooupload hello letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="70f57-173">Click hello **General** tab, and then click **Browse** tooupload hello downloaded metadata file.</span></span>
    
    <span data-ttu-id="70f57-174">![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "felügyeleti megbízhatóság")</span><span class="sxs-lookup"><span data-stu-id="70f57-174">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="70f57-175">Hello metaadatok fájl feltöltése után hello értékei **egyszeri bejelentkezési URL-cím**, **egyetlen kijelentkezési URL-címet** és **aláíró tanúsítvány** automatikusan fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="70f57-175">After uploading hello metadata file, hello values for **Single Sign-on URL**, **Single Logout URL** and **Signing Certificate** are populated automatically.</span></span>
    > 
    > 

11. <span data-ttu-id="70f57-176">Kattintson a hello **attribútumok** fülre.</span><span class="sxs-lookup"><span data-stu-id="70f57-176">Click hello **Attributes** tab.</span></span>
12. <span data-ttu-id="70f57-177">A hello **attribútumok** lapra, hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="70f57-177">On hello **Attributes** tab, perform hello following step:</span></span>
    
    <span data-ttu-id="70f57-178">![Attribútumok](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="70f57-178">![Attributes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")</span></span> 
  * <span data-ttu-id="70f57-179">Kattintson a **Add Assertion-Based attribútum**, majd adja hozzá a következő attribútumok helyességi feltétel alapú hello:</span><span class="sxs-lookup"><span data-stu-id="70f57-179">Click **Add Assertion-Based Attribute**, and then add hello following assertion-based attributes:</span></span>
       
    | <span data-ttu-id="70f57-180">Helyességi feltétel attribútum</span><span class="sxs-lookup"><span data-stu-id="70f57-180">Assertion Attribute</span></span> | <span data-ttu-id="70f57-181">Egyszerű attribútum</span><span class="sxs-lookup"><span data-stu-id="70f57-181">Principal Attribute</span></span> |
    | --- | --- |
    | <span data-ttu-id="70f57-182">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName</span><span class="sxs-lookup"><span data-stu-id="70f57-182">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname</span></span> |<span data-ttu-id="70f57-183">Utónév</span><span class="sxs-lookup"><span data-stu-id="70f57-183">firstname</span></span> |
    | <span data-ttu-id="70f57-184">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname</span><span class="sxs-lookup"><span data-stu-id="70f57-184">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname</span></span> |<span data-ttu-id="70f57-185">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="70f57-185">lastname</span></span> |
    | <span data-ttu-id="70f57-186">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress</span><span class="sxs-lookup"><span data-stu-id="70f57-186">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress</span></span> |<span data-ttu-id="70f57-187">E-mailek</span><span class="sxs-lookup"><span data-stu-id="70f57-187">email</span></span> 
   
     >[!NOTE]
     ><span data-ttu-id="70f57-188">hello konfiguráció hello attribútumok attól függ, hogyan hello alkalmazás(ok) a HCP fejlesztett, azaz mely attribútum(ok) hello SAML-válasz a várható, és milyen néven (egyszerű attribútum) hozzáféréskor hello kód ezt az attribútumot.</span><span class="sxs-lookup"><span data-stu-id="70f57-188">hello configuration of hello Attributes depends on how hello application(s) on HCP are developed, i.e. which attribute(s) they expect in hello SAML response and under which name (Principal Attribute) they access this attribute in hello code.</span></span>
     > 
     >  

    1.  <span data-ttu-id="70f57-189">Hello **alapértelmezett attribútum** hello a képernyőkép a folyamat csak illusztrációs célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="70f57-189">hello **Default Attribute** in hello screenshot is just for illustration purposes.</span></span> <span data-ttu-id="70f57-190">Nem szükséges toomake hello forgatókönyv munka.</span><span class="sxs-lookup"><span data-stu-id="70f57-190">It is not required toomake hello scenario work.</span></span>   
    2.  <span data-ttu-id="70f57-191">hello és tartozó értékek **egyszerű attribútum** hello látható képernyőfelvétel, attól függ, hogyan hello alkalmazás fejlesztése.</span><span class="sxs-lookup"><span data-stu-id="70f57-191">hello names and values for **Principal Attribute** shown in hello screenshot depend on how hello application is developed.</span></span> <span data-ttu-id="70f57-192">Akkor lehet, hogy az alkalmazás által igényelt különböző leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="70f57-192">It is possible that your application requires different mappings.</span></span>
     
13. <span data-ttu-id="70f57-193">A klasszikus Azure portálon, a hello hello **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** párbeszéd lap, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, kattintson a **Complete**.</span><span class="sxs-lookup"><span data-stu-id="70f57-193">In hello Azure classic portal, on hello **Configure single sign-on at SAP HANA Cloud Platform** dialogue page, select hello single sign-on configuration confirmation, and then click **Complete**.</span></span>
    
    <span data-ttu-id="70f57-194">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="70f57-194">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")</span></span>

###<a name="assertion-based-groups"></a><span data-ttu-id="70f57-195">Csoportok helyességi feltétel alapján</span><span class="sxs-lookup"><span data-stu-id="70f57-195">Assertion-based groups</span></span>
<span data-ttu-id="70f57-196">Egy nem kötelező lépés konfigurálható az Azure Active Directory identitásszolgáltató csoportok helyességi feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="70f57-196">As an optional step, you can configure assertion-based groups for your Azure Active Directory Identity Provider.</span></span>

<span data-ttu-id="70f57-197">Csoportok használata a SAP HANA Cloud Platform lehetővé teszi toodynamically hozzárendelése egy vagy több felhasználók tooone vagy a SAP HANA Cloud Platform alkalmazások, a további szerepkörök határozza meg értékek attribútumok hello SAML 2.0 helyességi feltétel.</span><span class="sxs-lookup"><span data-stu-id="70f57-197">Using groups on SAP HANA Cloud Platform allows you toodynamically assign one or more users tooone or more roles in your SAP HANA Cloud Platform applications, determined by values of attributes in hello SAML 2.0 assertion.</span></span> 

<span data-ttu-id="70f57-198">Például akkor, ha hello helyességi feltételt tartalmaz hello attribútum "*szerződés = ideiglenes*", érdemes lehet az összes érintett felhasználók toobe hozzáadott toohello csoport"*ideiglenes*".</span><span class="sxs-lookup"><span data-stu-id="70f57-198">For example, if hello assertion contains hello attribute "*contract=temporary*", you may want all affected users toobe added toohello group "*TEMPORARY*".</span></span> <span data-ttu-id="70f57-199">hello csoport "*ideiglenes*" tartalmazhat egy vagy több szerepkör az SAP HANA-Felhőplatform-fiókban telepített egy vagy több alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="70f57-199">hello group "*TEMPORARY*" may contain one or more roles from one or more applications deployed in your SAP HANA Cloud Platform account.</span></span>
 
<span data-ttu-id="70f57-200">Előfeltétel-alapú csoportok esetén célszerű használni toosimultaneously hozzárendelése számos felhasználók tooone vagy a további szerepkörök az alkalmazások az SAP HANA-Felhőplatform-fiókban.</span><span class="sxs-lookup"><span data-stu-id="70f57-200">Use assertion-based groups when you want toosimultaneously assign many users tooone or more roles of applications in your SAP HANA Cloud Platform account.</span></span> <span data-ttu-id="70f57-201">Ha azt szeretné, hogy csak egyetlen vagy kis számú felhasználók toospecific szerepkörök tooassign, ajánlott közvetlenül a hello hozzárendelése "**engedélyek**" hello SAP HANA Cloud Platform Vezérlőpult lapján.</span><span class="sxs-lookup"><span data-stu-id="70f57-201">If you want tooassign only a single or small number of users toospecific roles, we recommend assigning them directly in hello “**Authorizations**” tab of hello SAP HANA Cloud Platform cockpit.</span></span>

## <a name="assign-a-role-tooa-user"></a><span data-ttu-id="70f57-202">A szerepkör tooa felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70f57-202">Assign a role tooa user</span></span>
<span data-ttu-id="70f57-203">A sorrend tooenable az Azure AD felhasználók toolog az SAP HANA Cloud Platform kell rendelnie a szerepkörök az hello SAP HANA Cloud Platform toothem.</span><span class="sxs-lookup"><span data-stu-id="70f57-203">In order tooenable Azure AD users toolog into SAP HANA Cloud Platform, you must assign roles in hello SAP HANA Cloud Platform toothem.</span></span>

<span data-ttu-id="70f57-204">**tooassign szerepkör tooa felhasználója, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="70f57-204">**tooassign a role tooa user, perform hello following steps:**</span></span>

1. <span data-ttu-id="70f57-205">Jelentkezzen be tooyour **SAP HANA Cloud Platform** vezérlőpultot.</span><span class="sxs-lookup"><span data-stu-id="70f57-205">Log in tooyour **SAP HANA Cloud Platform** cockpit.</span></span>
2. <span data-ttu-id="70f57-206">Hello következőket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="70f57-206">Perform hello following:</span></span>
   
   <span data-ttu-id="70f57-207">![Engedélyek](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "engedélyek")</span><span class="sxs-lookup"><span data-stu-id="70f57-207">![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")</span></span>
   
  1. <span data-ttu-id="70f57-208">Kattintson a **engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="70f57-208">Click **Authorization**.</span></span>
  2. <span data-ttu-id="70f57-209">Kattintson a hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="70f57-209">Click hello **Users** tab.</span></span>
  3. <span data-ttu-id="70f57-210">A hello **felhasználói** szövegmezőhöz típus hello felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="70f57-210">In hello **User** textbox, type hello user’s email address.</span></span>
  4. <span data-ttu-id="70f57-211">Kattintson a **hozzárendelése** tooassign hello tooa szerepkörben.</span><span class="sxs-lookup"><span data-stu-id="70f57-211">Click **Assign** tooassign hello user tooa role.</span></span>
  5. <span data-ttu-id="70f57-212">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="70f57-212">Click **Save**.</span></span>

## <a name="assign-users"></a><span data-ttu-id="70f57-213">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70f57-213">Assign users</span></span>
<span data-ttu-id="70f57-214">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="70f57-214">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="70f57-215">**tooassign felhasználók tooSAP HANA Cloud Platform, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="70f57-215">**tooassign users tooSAP HANA Cloud Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="70f57-216">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="70f57-216">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="70f57-217">A hello **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="70f57-217">On hello **SAP HANA Cloud Platform** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="70f57-218">![Felhasználók hozzárendelése](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="70f57-218">![Assign Users](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")</span></span>
3. <span data-ttu-id="70f57-219">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="70f57-219">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="70f57-220">![Igen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="70f57-220">![Yes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="70f57-221">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="70f57-221">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="70f57-222">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="70f57-222">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

