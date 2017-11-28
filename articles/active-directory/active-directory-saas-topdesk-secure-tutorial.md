---
title: "Oktatóanyag: Azure Active Directoryval integrált TOPdesk - biztonságos |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse TOPdesk - Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további biztonságos!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 10fe420d1691c2845b89c779486ffd6fcd736432
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a><span data-ttu-id="119b4-103">Oktatóanyag: Azure Active Directoryval integrált TOPdesk - biztonságos</span><span class="sxs-lookup"><span data-stu-id="119b4-103">Tutorial: Azure Active Directory integration with TOPdesk - Secure</span></span>
<span data-ttu-id="119b4-104">hello Ez az oktatóanyag célja tooshow hello integrálása az Azure és TOPdesk - biztonságos.</span><span class="sxs-lookup"><span data-stu-id="119b4-104">hello objective of this tutorial is tooshow hello integration of Azure and TOPdesk - Secure.</span></span>  
<span data-ttu-id="119b4-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="119b4-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="119b4-106">A valid Azure subscription</span></span>
* <span data-ttu-id="119b4-107">A TOPdesk - biztonságos egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="119b4-107">A TOPdesk - Secure single sign-on enabled subscription</span></span>

<span data-ttu-id="119b4-108">Ez az oktatóanyag, hello Azure AD-felhasználók befejezése után rendelt tooTOPdesk - biztonságos lesz képes toosingle jelentkezzen be a TOPdesk - biztonságos vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési) hello alkalmazást, vagy hello segítségével [bemutatása Hozzáférési Panel toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="119b4-108">After completing this tutorial, hello Azure AD users you have assigned tooTOPdesk - Secure will be able toosingle sign into hello application at your TOPdesk - Secure company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="119b4-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="119b4-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="119b4-110">TOPdesk - biztonságos hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="119b4-110">Enabling hello application integration for TOPdesk - Secure</span></span>
2. <span data-ttu-id="119b4-111">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="119b4-111">Configuring single sign-on</span></span>
3. <span data-ttu-id="119b4-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="119b4-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="119b4-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="119b4-113">Assigning users</span></span>

<span data-ttu-id="119b4-114">![A forgatókönyv](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="119b4-114">![Scenario](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-topdesk---secure"></a><span data-ttu-id="119b4-115">TOPdesk - biztonságos hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="119b4-115">Enabling hello application integration for TOPdesk - Secure</span></span>
<span data-ttu-id="119b4-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció TOPdesk - biztonságos.</span><span class="sxs-lookup"><span data-stu-id="119b4-116">hello objective of this section is toooutline how tooenable hello application integration for TOPdesk - Secure.</span></span>

### <a name="tooenable-hello-application-integration-for-topdesk---secure-perform-hello-following-steps"></a><span data-ttu-id="119b4-117">tooenable hello alkalmazásintegráció TOPdesk - biztonságos, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-117">tooenable hello application integration for TOPdesk - Secure, perform hello following steps:</span></span>
1. <span data-ttu-id="119b4-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="119b4-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="119b4-119">![Az Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="119b4-119">![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span></span>

2. <span data-ttu-id="119b4-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="119b4-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="119b4-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="119b4-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="119b4-122">![Alkalmazások](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="119b4-122">![Applications](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applications")</span></span>

4. <span data-ttu-id="119b4-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="119b4-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="119b4-124">![Alkalmazás hozzáadása](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="119b4-124">![Add application](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Add application")</span></span>

5. <span data-ttu-id="119b4-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="119b4-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="119b4-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="119b4-126">![Add an application from gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Add an application from gallerry")</span></span>

6. <span data-ttu-id="119b4-127">A hello **keresőmezőbe**, típus **TOPdesk - biztonságos**.</span><span class="sxs-lookup"><span data-stu-id="119b4-127">In hello **search box**, type **TOPdesk - Secure**.</span></span>
   
    <span data-ttu-id="119b4-128">![Alkalmazáskatalógusában](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="119b4-128">![Application Gallery](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Application Gallery")</span></span>

7. <span data-ttu-id="119b4-129">Hello eredmények ablaktábláján jelöljön ki **TOPdesk - biztonságos**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="119b4-129">In hello results pane, select **TOPdesk - Secure**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="119b4-130">![TOPdesk - biztonságos](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - biztonságos")</span><span class="sxs-lookup"><span data-stu-id="119b4-130">![TOPdesk - Secure](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Secure")</span></span>

## <a name="configuring-single-sign-on"></a><span data-ttu-id="119b4-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="119b4-131">Configuring single sign-on</span></span>
<span data-ttu-id="119b4-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooTOPdesk - biztonságos fiókkal az Azure AD összevonási alapján hello SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="119b4-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooTOPdesk - Secure with their account in Azure AD using federation based on hello SAML protocol.</span></span>  
<span data-ttu-id="119b4-133">Konfigurálása egyszeri bejelentkezéshez az TOPdesk - biztonságos meg tooupload egy embléma ikonfájlt.</span><span class="sxs-lookup"><span data-stu-id="119b4-133">Configuring single sign-on for TOPdesk - Secure requires you tooupload a logo icon file.</span></span> <span data-ttu-id="119b4-134">tooget hello ikonfájlját, kapcsolattartási hello TOPdesk támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="119b4-134">tooget hello icon file, contact hello TOPdesk support team.</span></span>

### <a name="tooconfigure-single-sign-on-perform-hello-following-steps"></a><span data-ttu-id="119b4-135">tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-135">tooconfigure single sign-on, perform hello following steps:</span></span>
1. <span data-ttu-id="119b4-136">Jelentkezzen be tooyour **TOPdesk - biztonságos** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="119b4-136">Sign on tooyour **TOPdesk - Secure** company site as an administrator.</span></span>
2. <span data-ttu-id="119b4-137">A hello **TOPdesk** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="119b4-137">In hello **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="119b4-138">![Beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="119b4-138">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

3. <span data-ttu-id="119b4-139">Kattintson a **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="119b4-139">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="119b4-140">![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="119b4-140">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

4. <span data-ttu-id="119b4-141">Bontsa ki a hello **bejelentkezési beállítások** menüben, majd kattintson **általános**.</span><span class="sxs-lookup"><span data-stu-id="119b4-141">Expand hello **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="119b4-142">![Általános](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "általános")</span><span class="sxs-lookup"><span data-stu-id="119b4-142">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

5. <span data-ttu-id="119b4-143">A hello **biztonságos** hello szakasza **SAML bejelentkezési** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-143">In hello **Secure** section of hello **SAML login** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="119b4-144">![Műszaki beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "műszaki beállításai")</span><span class="sxs-lookup"><span data-stu-id="119b4-144">![Technical Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technical Settings")</span></span>
   
    <span data-ttu-id="119b4-145">a.</span><span class="sxs-lookup"><span data-stu-id="119b4-145">a.</span></span> <span data-ttu-id="119b4-146">Kattintson a **letöltése** toodownload hello nyilvános metaadat-fájlt, és mentse helyileg a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="119b4-146">Click **Download** toodownload hello public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="119b4-147">b.</span><span class="sxs-lookup"><span data-stu-id="119b4-147">b.</span></span> <span data-ttu-id="119b4-148">Nyissa meg a hello metaadat-fájlt, és keresse a hello **AssertionConsumerService** csomópont.</span><span class="sxs-lookup"><span data-stu-id="119b4-148">Open hello metadata file, and then locate hello **AssertionConsumerService** node.</span></span>
    
    <span data-ttu-id="119b4-149">![Helyességi feltétel fogyasztói szolgáltatás](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "helyességi feltétel fogyasztói szolgáltatás")</span><span class="sxs-lookup"><span data-stu-id="119b4-149">![Assertion Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer Service")</span></span>
   
    <span data-ttu-id="119b4-150">c.</span><span class="sxs-lookup"><span data-stu-id="119b4-150">c.</span></span> <span data-ttu-id="119b4-151">Másolás hello **AssertionConsumerService** érték.</span><span class="sxs-lookup"><span data-stu-id="119b4-151">Copy hello **AssertionConsumerService** value.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="119b4-152">Akkor lesz szüksége hello hello értéket **alkalmazás URL-cím konfigurálása** szakasz az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="119b4-152">You will need hello value in hello **Configure App URL** section later in this tutorial.</span></span>
    > 
    > 

6. <span data-ttu-id="119b4-153">Egy másik webes böngészőablakban, jelentkezzen be a **a klasszikus Azure portálon** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="119b4-153">In a different web browser window, log into your **Azure classic portal** as an administrator.</span></span>

7. <span data-ttu-id="119b4-154">A hello **TOPdesk - biztonságos** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="119b4-154">On hello **TOPdesk - Secure** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="119b4-155">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="119b4-155">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Configure Single Sign-On")</span></span>

8. <span data-ttu-id="119b4-156">A hello **hogyan valóban tooTOPdesk – a felhasználók toosign például biztonságos** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="119b4-156">On hello **How would you like users toosign on tooTOPdesk - Secure** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="119b4-157">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="119b4-157">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Configure Single Sign-On")</span></span>

9. <span data-ttu-id="119b4-158">A hello **alkalmazás URL-cím konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-158">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="119b4-159">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="119b4-159">![Configure App URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Configure App URL")</span></span>
   
    <span data-ttu-id="119b4-160">a.</span><span class="sxs-lookup"><span data-stu-id="119b4-160">a.</span></span> <span data-ttu-id="119b4-161">A hello **TOPdesk - bejelentkezési biztonságos az URL-cím** szövegmezőhöz a TOPdesk - biztonságos alkalmazás azokat a felhasználók toosign által használt típus hello URL (pl.: "*https://qssolutions.topdesk.net*").</span><span class="sxs-lookup"><span data-stu-id="119b4-161">In hello **TOPdesk - Secure Sign On URL** textbox, type hello URL used by your users toosign into your TOPdesk - Secure application (e.g.: "*https://qssolutions.topdesk.net*").</span></span>
   
    <span data-ttu-id="119b4-162">b.</span><span class="sxs-lookup"><span data-stu-id="119b4-162">b.</span></span> <span data-ttu-id="119b4-163">A hello **TOPdesk – válasz URL-címe** szövegmezőhöz Beillesztés hello **TOPdesk - biztonságos AssertionConsumerService URL-** (pl.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span><span class="sxs-lookup"><span data-stu-id="119b4-163">In hello **TOPdesk – Public Reply URL** textbox, paste hello **TOPdesk - Secure AssertionConsumerService URL** (e.g.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span></span>
   
    <span data-ttu-id="119b4-164">c.</span><span class="sxs-lookup"><span data-stu-id="119b4-164">c.</span></span> <span data-ttu-id="119b4-165">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="119b4-165">Click **Next**.</span></span>

10. <span data-ttu-id="119b4-166">A hello **konfigurálhatja az egyszeri bejelentkezés TOPdesk - biztonságos** lap, toodownload a metaadatfájl kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="119b4-166">On hello **Configure single sign-on at TOPdesk - Secure** page, toodownload your metadata file, click **Download metadata**, and then save hello file locally on your computer.</span></span>
    
    <span data-ttu-id="119b4-167">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="119b4-167">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Configure Single Sign-On")</span></span>

11. <span data-ttu-id="119b4-168">toocreate a tanúsítványfájlt, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-168">toocreate a certificate file, perform hello following steps:</span></span>
    
    <span data-ttu-id="119b4-169">![Tanúsítvány](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "tanúsítvány")</span><span class="sxs-lookup"><span data-stu-id="119b4-169">![Certificate](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certificate")</span></span>
    
    <span data-ttu-id="119b4-170">a.</span><span class="sxs-lookup"><span data-stu-id="119b4-170">a.</span></span> <span data-ttu-id="119b4-171">Nyissa meg hello letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="119b4-171">Open hello downloaded metadata file.</span></span>
    <span data-ttu-id="119b4-172">b.</span><span class="sxs-lookup"><span data-stu-id="119b4-172">b.</span></span> <span data-ttu-id="119b4-173">Bontsa ki a hello **RoleDescriptor** csomópont egy **xsi: type** a **táplált: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="119b4-173">Expand hello **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    <span data-ttu-id="119b4-174">c.</span><span class="sxs-lookup"><span data-stu-id="119b4-174">c.</span></span> <span data-ttu-id="119b4-175">Másolja a hello hello értékének **x.509** csomópont.</span><span class="sxs-lookup"><span data-stu-id="119b4-175">Copy hello value of hello **X509Certificate** node.</span></span>
    <span data-ttu-id="119b4-176">d.</span><span class="sxs-lookup"><span data-stu-id="119b4-176">d.</span></span> <span data-ttu-id="119b4-177">Mentés hello másolt **x.509** érték helyileg a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="119b4-177">Save hello copied **X509Certificate** value locally on your computer in a file.</span></span>

12. <span data-ttu-id="119b4-178">A TOPdesk - a vállalati webhely hello biztonságos **TOPdesk** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="119b4-178">On your TOPdesk - Secure company site, in hello **TOPdesk** menu, click **Settings**.</span></span>
    
    <span data-ttu-id="119b4-179">![Beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="119b4-179">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

13. <span data-ttu-id="119b4-180">Kattintson a **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="119b4-180">Click **Login Settings**.</span></span>
    
    <span data-ttu-id="119b4-181">![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="119b4-181">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

14. <span data-ttu-id="119b4-182">Bontsa ki a hello **bejelentkezési beállítások** menüben, majd kattintson **általános**.</span><span class="sxs-lookup"><span data-stu-id="119b4-182">Expand hello **Login Settings** menu, and then click **General**.</span></span>
    
    <span data-ttu-id="119b4-183">![Általános](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "általános")</span><span class="sxs-lookup"><span data-stu-id="119b4-183">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

15. <span data-ttu-id="119b4-184">A hello **nyilvános** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="119b4-184">In hello **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="119b4-185">![Adja hozzá](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="119b4-185">![Add](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Add")</span></span>

16. <span data-ttu-id="119b4-186">A hello **SAML-alapú konfigurációs Segéd** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-186">On hello **SAML configuration assistant** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="119b4-187">![SAML-alapú konfigurációs Segéd](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML-alapú konfigurációs Segéd")</span><span class="sxs-lookup"><span data-stu-id="119b4-187">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="119b4-188">a.</span><span class="sxs-lookup"><span data-stu-id="119b4-188">a.</span></span> <span data-ttu-id="119b4-189">a letöltött metaadat-fájlt, a tooupload **összevonási metaadatok**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="119b4-189">tooupload your downloaded metadata file, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="119b4-190">b.</span><span class="sxs-lookup"><span data-stu-id="119b4-190">b.</span></span> <span data-ttu-id="119b4-191">a tanúsítvány a fájl tooupload **tanúsítvány (RSA)**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="119b4-191">tooupload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="119b4-192">c.</span><span class="sxs-lookup"><span data-stu-id="119b4-192">c.</span></span> <span data-ttu-id="119b4-193">során kapott azonosítóértékeket hello TOPdesk támogatási csoport, a fájl tooupload hello **embléma ikon**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="119b4-193">tooupload hello logo file you got from hello TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="119b4-194">d.</span><span class="sxs-lookup"><span data-stu-id="119b4-194">d.</span></span> <span data-ttu-id="119b4-195">A hello **felhasználói név attribútum** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="119b4-195">In hello **User name attribute** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="119b4-196">e.</span><span class="sxs-lookup"><span data-stu-id="119b4-196">e.</span></span> <span data-ttu-id="119b4-197">A hello **megjelenített név** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="119b4-197">In hello **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="119b4-198">f.</span><span class="sxs-lookup"><span data-stu-id="119b4-198">f.</span></span> <span data-ttu-id="119b4-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="119b4-199">Click **Save**.</span></span>

17. <span data-ttu-id="119b4-200">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="119b4-200">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="119b4-201">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="119b4-201">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Configure Single Sign-On")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="119b4-202">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="119b4-202">Configuring user provisioning</span></span>
<span data-ttu-id="119b4-203">Rendelés tooenable az Azure AD felhasználók toolog TOPdesk - be a biztonságos, azok ki kell építenie a TOPdesk - biztonságos.</span><span class="sxs-lookup"><span data-stu-id="119b4-203">In order tooenable Azure AD users toolog into TOPdesk - Secure, they must be provisioned into TOPdesk - Secure.</span></span>  
<span data-ttu-id="119b4-204">Hello esetében TOPdesk - biztonságos, egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="119b4-204">In hello case of TOPdesk - Secure, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="119b4-205">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-205">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="119b4-206">Jelentkezzen be tooyour **TOPdesk - biztonságos** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="119b4-206">Sign on tooyour **TOPdesk - Secure** company site as administrator.</span></span>
2. <span data-ttu-id="119b4-207">Hello hello felső menüben kattintson a **TOPdesk \> új \> támogatófájljait \> operátor**.</span><span class="sxs-lookup"><span data-stu-id="119b4-207">In hello menu on hello top, click **TOPdesk \> New \> Support Files \> Operator**.</span></span>
   
    <span data-ttu-id="119b4-208">![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "operátor")</span><span class="sxs-lookup"><span data-stu-id="119b4-208">![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")</span></span>

3. <span data-ttu-id="119b4-209">A hello **új operátor** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-209">On hello **New Operator** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="119b4-210">![Új operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "új operátor")</span><span class="sxs-lookup"><span data-stu-id="119b4-210">![New Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New Operator")</span></span>
   
    <span data-ttu-id="119b4-211">a.</span><span class="sxs-lookup"><span data-stu-id="119b4-211">a.</span></span> <span data-ttu-id="119b4-212">Kattintson a hello Általános fülre.</span><span class="sxs-lookup"><span data-stu-id="119b4-212">Click hello General tab.</span></span>
   
    <span data-ttu-id="119b4-213">b.</span><span class="sxs-lookup"><span data-stu-id="119b4-213">b.</span></span> <span data-ttu-id="119b4-214">A hello **vezetékneve** hello a szövegmező **általános** szakaszban, hello utolsó nevét egy érvényes Azure Active Directory-fiókot szeretne tooprovision.</span><span class="sxs-lookup"><span data-stu-id="119b4-214">In hello **Surname** textbox of hello **General** section, type hello last name of a valid Azure Active Directory account you want tooprovision.</span></span>
   
    <span data-ttu-id="119b4-215">c.</span><span class="sxs-lookup"><span data-stu-id="119b4-215">c.</span></span> <span data-ttu-id="119b4-216">Válassza ki a **hely** hello hello fiók **hely** szakasz.</span><span class="sxs-lookup"><span data-stu-id="119b4-216">Select a **Site** for hello account in hello **Location** section.</span></span>
   
    <span data-ttu-id="119b4-217">d.</span><span class="sxs-lookup"><span data-stu-id="119b4-217">d.</span></span> <span data-ttu-id="119b4-218">A hello **bejelentkezési név** hello a szövegmező **TOPdesk bejelentkezési** területen írja be a felhasználó bejelentkezési nevét.</span><span class="sxs-lookup"><span data-stu-id="119b4-218">In hello **Login Name** textbox of hello **TOPdesk Login** section, type a login name for your user.</span></span>
   
    <span data-ttu-id="119b4-219">e.</span><span class="sxs-lookup"><span data-stu-id="119b4-219">e.</span></span> <span data-ttu-id="119b4-220">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="119b4-220">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="119b4-221">Bármely más TOPdesk - biztonságos felhasználói fiók létrehozása eszközök vagy TOPdesk - által nyújtott API-kat biztonságos tooprovision AAD felhasználói fiókokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="119b4-221">You can use any other TOPdesk - Secure user account creation tools or APIs provided by TOPdesk - Secure tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assigning-users"></a><span data-ttu-id="119b4-222">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="119b4-222">Assigning users</span></span>
<span data-ttu-id="119b4-223">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="119b4-223">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

### <a name="tooassign-users-tootopdesk---secure-perform-hello-following-steps"></a><span data-ttu-id="119b4-224">tooassign felhasználók tooTOPdesk - biztonságos, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="119b4-224">tooassign users tooTOPdesk - Secure, perform hello following steps:</span></span>
1. <span data-ttu-id="119b4-225">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="119b4-225">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="119b4-226">A hello ** TOPdesk - biztonságos ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="119b4-226">On hello **TOPdesk - Secure **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="119b4-227">![Felhasználók hozzárendelése](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="119b4-227">![Assign Users](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Assign Users")</span></span>

3. <span data-ttu-id="119b4-228">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="119b4-228">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="119b4-229">![Igen](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="119b4-229">![Yes](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="119b4-230">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="119b4-230">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="119b4-231">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="119b4-231">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

