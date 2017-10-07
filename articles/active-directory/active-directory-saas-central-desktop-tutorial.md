---
title: "Oktatóanyag: Azure Active Directoryval integrált központi asztali |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse központi asztali az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
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
ms.openlocfilehash: 93036ae801c446ce476288c00579931ba10a843b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a><span data-ttu-id="8d13b-103">Oktatóanyag: Azure Active Directory-integráció központi asztal</span><span class="sxs-lookup"><span data-stu-id="8d13b-103">Tutorial: Azure Active Directory integration with Central Desktop</span></span>
<span data-ttu-id="8d13b-104">hello Ez az oktatóanyag célja az Azure és a központi asztali tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="8d13b-104">hello objective of this tutorial is tooshow hello integration of Azure and Central Desktop.</span></span> <span data-ttu-id="8d13b-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8d13b-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="8d13b-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="8d13b-106">A valid Azure subscription</span></span>
* <span data-ttu-id="8d13b-107">Egy központi asztali az egyszeri bejelentkezés engedélyezett előfizetés / központi asztali bérlői</span><span class="sxs-lookup"><span data-stu-id="8d13b-107">A Central desktop single sign on enabled subscription / Central desktop tenant</span></span>

<span data-ttu-id="8d13b-108">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="8d13b-108">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="8d13b-109">Központi asztali hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8d13b-109">Enabling hello application integration for Central Desktop</span></span>
* <span data-ttu-id="8d13b-110">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8d13b-110">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="8d13b-111">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="8d13b-111">Configuring user provisioning</span></span>
* <span data-ttu-id="8d13b-112">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8d13b-112">Assigning users</span></span>

<span data-ttu-id="8d13b-113">![A forgatókönyv](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="8d13b-113">![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-central-desktop"></a><span data-ttu-id="8d13b-114">A központi asztal hello alkalmazás integrációjának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8d13b-114">Enable hello application integration for Central Desktop</span></span>
<span data-ttu-id="8d13b-115">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció központi asztali verzióját.</span><span class="sxs-lookup"><span data-stu-id="8d13b-115">hello objective of this section is toooutline how tooenable hello application integration for Central Desktop.</span></span>

<span data-ttu-id="8d13b-116">**tooenable hello alkalmazás-integráció központi asztali, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8d13b-116">**tooenable hello application integration for Central Desktop, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d13b-117">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-117">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="8d13b-118">![Az Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="8d13b-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="8d13b-119">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="8d13b-119">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="8d13b-120">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="8d13b-120">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="8d13b-121">![Alkalmazások](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="8d13b-121">![Applications](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="8d13b-122">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="8d13b-122">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="8d13b-123">![Alkalmazás hozzáadása](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-123">![Add application](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="8d13b-124">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-124">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="8d13b-125">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-125">![Add an application from gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="8d13b-126">A hello **keresőmezőbe**, típus **központi asztali**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-126">In hello **search box**, type **Central Desktop**.</span></span>
   
   <span data-ttu-id="8d13b-127">![Alkalmazáskatalógusában](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="8d13b-127">![Application gallery](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")</span></span>
7. <span data-ttu-id="8d13b-128">Hello eredmények ablaktábláján jelöljön ki **központi asztali**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d13b-128">In hello results pane, select **Central Desktop**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="8d13b-129">![Központi asztali](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "központi asztali")</span><span class="sxs-lookup"><span data-stu-id="8d13b-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="8d13b-130">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8d13b-130">Configure single sign-on</span></span>

<span data-ttu-id="8d13b-131">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooCentral asztali fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="8d13b-131">hello objective of this section is toooutline how tooenable users tooauthenticate tooCentral Desktop with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="8d13b-132">Ez az eljárás részeként szükség tooupload a base-64 kódolású tanúsítvány tooyour központi asztali bérlő.</span><span class="sxs-lookup"><span data-stu-id="8d13b-132">As part of this procedure, you are required tooupload a base-64 encoded certificate tooyour Central Desktop tenant.</span></span>  
<span data-ttu-id="8d13b-133">Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="8d13b-133">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

<span data-ttu-id="8d13b-134">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="8d13b-134">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d13b-135">A klasszikus Azure portálon, a hello hello **központi asztali** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8d13b-135">In hello Azure classic portal, on hello **Central Desktop** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
   <span data-ttu-id="8d13b-136">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-136">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="8d13b-137">A hello **hogyan szeretné a tooCentral asztali felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-137">On hello **How would you like users toosign on tooCentral Desktop** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="8d13b-138">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-138">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="8d13b-139">A hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="8d13b-139">On hello **Configure App URL** page, perform hello following steps, and then click **Next**:</span></span> 
   
   1. <span data-ttu-id="8d13b-140">A hello **központi asztali bejelentkezési az URL-címet** szövegmezőhöz központi asztali bérlője típus hello URL-cím (pl.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="8d13b-140">In hello **Central Desktop Sign In URL** textbox, type hello URL of your Central Desktop tenant (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   2. <span data-ttu-id="8d13b-141">Hello központi asztali válasz URL-címet szövegmezőben, írja be a központi asztali AssertionConsumerService URL-címet (pl.: https://contoso.centraldesktop.com/saml2-assertion.php).</span><span class="sxs-lookup"><span data-stu-id="8d13b-141">In hello Central  Desktop Reply URL textbox, type your Central Desktop AssertionConsumerService URL (e.g.:  https://contoso.centraldesktop.com/saml2-assertion.php).</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="8d13b-142">Hello érték letölthető hello központi asztali metaadatok (pl.: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="8d13b-142">You can get hello value from hello central desktop metadata (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   >  
   
   <span data-ttu-id="8d13b-143">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-143">![Configure app URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")</span></span>
4. <span data-ttu-id="8d13b-144">A hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, toodownload a tanúsítványt, kattintson **tanúsítvánnyal letöltés**, és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8d13b-144">On hello **Configure single sign-on at Central Desktop** page, toodownload your certificate, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
  <span data-ttu-id="8d13b-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-145">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="8d13b-146">Jelentkezzen be tooyour **központi asztali** bérlő.</span><span class="sxs-lookup"><span data-stu-id="8d13b-146">Log in tooyour **Central Desktop** tenant.</span></span>
6. <span data-ttu-id="8d13b-147">Nyissa meg túl**beállítások**, kattintson a **speciális**, és kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-147">Go too**Settings**, click **Advanced**, and then click **Single Sign On**.</span></span>
   
  <span data-ttu-id="8d13b-148">![A telepítő - speciális](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "telepítő – speciális")</span><span class="sxs-lookup"><span data-stu-id="8d13b-148">![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")</span></span>
7. <span data-ttu-id="8d13b-149">A hello **egyetlen bejelentkezést a beállítások** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8d13b-149">On hello **Single Sign On Settings** page, perform hello following steps:</span></span>
   
  <span data-ttu-id="8d13b-150">![Egyszeri bejelentkezés beállításai](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="8d13b-150">![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")</span></span>
   
  1. <span data-ttu-id="8d13b-151">Válassza ki **engedélyezése SAML v2 az egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-151">Select **Enable SAML v2 Single Sign On**.</span></span>
  2. <span data-ttu-id="8d13b-152">A klasszikus Azure portálon, a hello hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, a Másolás hello **kiállítójának URL-címe** értékét, és illessze be hello **egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8d13b-152">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Issuer URL** value, and then paste it into hello **SSO URL** textbox.</span></span>
  3. <span data-ttu-id="8d13b-153">A klasszikus Azure portálon, a hello hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, a Másolás hello **távoli bejelentkezési URL-címet** értékét, és illessze be hello **Egyszeri bejelentkezési URL-cím**szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8d13b-153">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Remote Login URL** value, and then paste it into hello **SSO Login URL** textbox.</span></span>
  4. <span data-ttu-id="8d13b-154">A klasszikus Azure portálon, a hello hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, a Másolás hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **SSO kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8d13b-154">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **SSO Logout URL** textbox.</span></span>
8. <span data-ttu-id="8d13b-155">A hello **üzenet aláírás-ellenőrzési módszer** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8d13b-155">In hello **Message Signature Verification Method** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="8d13b-156">![Üzenet-aláírást ellenőrzési módszer](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "üzenet-aláírást ellenőrzési módszer")</span><span class="sxs-lookup"><span data-stu-id="8d13b-156">![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")</span></span>
   
  1. <span data-ttu-id="8d13b-157">Válassza ki **tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-157">Select **Certificate**.</span></span>
  2. <span data-ttu-id="8d13b-158">A hello **SSO tanúsítvány** listáról válassza ki **RSH SHA256**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-158">From hello **SSO Certificate** list, select **RSH SHA256**.</span></span>
  3. <span data-ttu-id="8d13b-159">Hozzon létre egy szövegfájlt letöltött hello tanúsítványból másolási hello tartalom hello szövegfájl, és illessze be hello **SSO tanúsítvány** mező.</span><span class="sxs-lookup"><span data-stu-id="8d13b-159">Create a text file from hello downloaded certificate, copy hello content of hello text file, and then paste it into hello **SSO Certificate** field.</span></span>  
     >[!TIP]
     ><span data-ttu-id="8d13b-160">További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="8d13b-160">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
      >  
   4. <span data-ttu-id="8d13b-161">Válassza ki **hivatkozás tooyour SAMLv2 bejelentkezési oldal megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-161">Select **Display a link tooyour SAMLv2 login page**.</span></span>
9. <span data-ttu-id="8d13b-162">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-162">Click **Update**.</span></span>
10. <span data-ttu-id="8d13b-163">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8d13b-163">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="8d13b-164">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-164">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="8d13b-165">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8d13b-165">Configure user provisioning</span></span>

<span data-ttu-id="8d13b-166">Az aad-ben felhasználók toobe képes toosign a kiosztott toohello központi asztali alkalmazások kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="8d13b-166">For AAD users toobe able toosign in, they must be provisioned toohello Central Desktop application.</span></span> <span data-ttu-id="8d13b-167">Ez a szakasz ismerteti, hogyan toocreate AAD felhasználói fiókot, a központi asztali.</span><span class="sxs-lookup"><span data-stu-id="8d13b-167">This section describes how toocreate AAD user accounts in Central Desktop.</span></span>

<span data-ttu-id="8d13b-168">**tooprovision felhasználói fiókok tooCentral asztali:**</span><span class="sxs-lookup"><span data-stu-id="8d13b-168">**tooprovision user accounts tooCentral Desktop:**</span></span>
1. <span data-ttu-id="8d13b-169">Jelentkezzen be tooyour központi asztali bérlő.</span><span class="sxs-lookup"><span data-stu-id="8d13b-169">Log in tooyour Central Desktop tenant.</span></span>
2. <span data-ttu-id="8d13b-170">Nyissa meg túl**személyek \> belső tagok**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-170">Go too**People \> Internal Members**.</span></span>
3. <span data-ttu-id="8d13b-171">Kattintson a **belső tagok felvétele**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-171">Click **Add Internal Members**.</span></span>
   
  <span data-ttu-id="8d13b-172">![Személyek](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="8d13b-172">![People](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")</span></span>
4. <span data-ttu-id="8d13b-173">A hello **E-mail címet az új tagok** szövegmező, írja be a kívánt tooprovision, és kattintson egy AAD-fiókba **következő**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-173">In hello **Email Address of New Members** textbox, type an AAD account you want tooprovision, and then click **Next**.</span></span>
   
  <span data-ttu-id="8d13b-174">![E-mail-címeket az új tagok](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mail címet az új tagok")</span><span class="sxs-lookup"><span data-stu-id="8d13b-174">![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")</span></span>
5. <span data-ttu-id="8d13b-175">Kattintson a **hozzáadása belső tag**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-175">Click **Add Internal member(s)**.</span></span>
   
  <span data-ttu-id="8d13b-176">![Adja hozzá a belső tag](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "belső tag hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8d13b-176">![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="8d13b-177">hello felhasználók hozzáadott tooclick tooactivate hello fiók szükséges megerősítési hivatkozást tartalmazó e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="8d13b-177">hello users you have added will receive an email that includes a confirmation link they need tooclick tooactivate hello account.</span></span>
   > 

>[!NOTE]
><span data-ttu-id="8d13b-178">Bármely más központi asztali felhasználói fiók létrehozása eszközök vagy központi asztali tooprovision által nyújtott API-k AAD-felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="8d13b-178">You can use any other Central Desktop user account creation tools or APIs provided by Central Desktop tooprovision AAD user accounts</span></span>
>  

## <a name="assign-users"></a><span data-ttu-id="8d13b-179">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8d13b-179">Assign users</span></span>
<span data-ttu-id="8d13b-180">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="8d13b-180">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="8d13b-181">**tooassign felhasználók tooCentral asztali, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8d13b-181">**tooassign users tooCentral Desktop, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d13b-182">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="8d13b-182">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="8d13b-183">A hello **központi asztali** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="8d13b-183">On hello **Central Desktop** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="8d13b-184">![Felhasználók hozzárendelése](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="8d13b-184">![Assign users](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")</span></span>
3. <span data-ttu-id="8d13b-185">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="8d13b-185">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="8d13b-186">![Igen](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="8d13b-186">![Yes](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="8d13b-187">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="8d13b-187">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="8d13b-188">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8d13b-188">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

