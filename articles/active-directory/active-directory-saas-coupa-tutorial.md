---
title: "Oktatóanyag: Azure Active Directoryval integrált Coupa |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Coupa az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 87e98573718d27d408c886466a374a987f58faa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a><span data-ttu-id="a387c-103">Oktatóanyag: Azure Active Directoryval integrált Coupa</span><span class="sxs-lookup"><span data-stu-id="a387c-103">Tutorial: Azure Active Directory integration with Coupa</span></span>
<span data-ttu-id="a387c-104">hello Ez az oktatóanyag célja az Azure és Coupa tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="a387c-104">hello objective of this tutorial is tooshow hello integration of Azure and Coupa.</span></span>  
<span data-ttu-id="a387c-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a387c-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="a387c-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="a387c-106">A valid Azure subscription</span></span>
* <span data-ttu-id="a387c-107">Egy Coupa egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a387c-107">A Coupa single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="a387c-108">Ez az oktatóanyag befejezése után tooCoupa hozzárendelt hello Azure AD felhasználók fognak tudni toosingle jelentkezzen be a hello alkalmazást hello [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a387c-108">After completing this tutorial, hello Azure AD users you have assigned tooCoupa will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="a387c-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="a387c-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="a387c-110">Coupa hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a387c-110">Enabling hello application integration for Coupa</span></span>
* <span data-ttu-id="a387c-111">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a387c-111">Configuring single sign-on</span></span>
* <span data-ttu-id="a387c-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="a387c-112">Configuring user provisioning</span></span>
* <span data-ttu-id="a387c-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a387c-113">Assigning users</span></span>

<span data-ttu-id="a387c-114">![A forgatókönyv](./media/active-directory-saas-coupa-tutorial/IC791897.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="a387c-114">![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-coupa"></a><span data-ttu-id="a387c-115">Coupa hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a387c-115">Enable hello application integration for Coupa</span></span>
<span data-ttu-id="a387c-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Coupa.</span><span class="sxs-lookup"><span data-stu-id="a387c-116">hello objective of this section is toooutline how tooenable hello application integration for Coupa.</span></span>

<span data-ttu-id="a387c-117">**tooenable hello alkalmazásintegráció Coupa, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a387c-117">**tooenable hello application integration for Coupa, perform hello following steps:**</span></span>

1. <span data-ttu-id="a387c-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a387c-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="a387c-119">![Az Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="a387c-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="a387c-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="a387c-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="a387c-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="a387c-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="a387c-122">![Alkalmazások](./media/active-directory-saas-coupa-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="a387c-122">![Applications](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="a387c-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="a387c-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="a387c-124">![Alkalmazás hozzáadása](./media/active-directory-saas-coupa-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a387c-124">![Add application](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="a387c-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a387c-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="a387c-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a387c-126">![Add an application from gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="a387c-127">A hello **keresőmezőbe**, típus **Coupa**.</span><span class="sxs-lookup"><span data-stu-id="a387c-127">In hello **search box**, type **Coupa**.</span></span>
   
   <span data-ttu-id="a387c-128">![Alkalmazáskatalógusában](./media/active-directory-saas-coupa-tutorial/IC791898.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="a387c-128">![Application Gallery](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")</span></span>
7. <span data-ttu-id="a387c-129">Hello eredmények ablaktábláján jelöljön ki **Coupa**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a387c-129">In hello results pane, select **Coupa**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="a387c-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span><span class="sxs-lookup"><span data-stu-id="a387c-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="a387c-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a387c-131">Configure single sign-on</span></span>

<span data-ttu-id="a387c-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooCoupa fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="a387c-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooCoupa with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="a387c-133">Egyszeri bejelentkezés az Coupa konfigurálása olyan tooretrieve egy tanúsítvány ujjlenyomata értéket.</span><span class="sxs-lookup"><span data-stu-id="a387c-133">Configuring single sign-on for Coupa requires you tooretrieve a thumbprint value from a certificate.</span></span> <span data-ttu-id="a387c-134">Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooretrieve egy tanúsítvány-ujjlenyomat értékének](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="a387c-134">If you are not familiar with this procedure, see [How tooretrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span>

<span data-ttu-id="a387c-135">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="a387c-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="a387c-136">Bejelentkezés tooyour Coupa vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a387c-136">Sign on tooyour Coupa company site as an administrator.</span></span>
2. <span data-ttu-id="a387c-137">Nyissa meg túl**telepítő \> biztonsági ellenőrzést**.</span><span class="sxs-lookup"><span data-stu-id="a387c-137">Go too**Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="a387c-138">![Biztonsági vezérlők](./media/active-directory-saas-coupa-tutorial/IC791900.png "biztonsági vezérlőket")</span><span class="sxs-lookup"><span data-stu-id="a387c-138">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
3. <span data-ttu-id="a387c-139">toodownload hello Coupa metaadatok fájl tooyour számítógép **töltse le és SP metaadatok importálása**.</span><span class="sxs-lookup"><span data-stu-id="a387c-139">toodownload hello Coupa metadata file tooyour computer, click **Download and import SP metadata**.</span></span>
   
   <span data-ttu-id="a387c-140">![Coupa SP metaadatok](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metaadatok")</span><span class="sxs-lookup"><span data-stu-id="a387c-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span></span>
4. <span data-ttu-id="a387c-141">Egy másik böngészőablakban bejelentkezéskor toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a387c-141">In a different browser window, sign on toohello Azure classic portal.</span></span>
5. <span data-ttu-id="a387c-142">A hello **Coupa** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a387c-142">On hello **Coupa** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="a387c-143">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791902.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a387c-143">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="a387c-144">A hello **hogyan szeretné tooCoupa a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a387c-144">On hello **How would you like users toosign on tooCoupa** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="a387c-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791903.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a387c-145">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="a387c-146">A hello **alkalmazás URL-cím konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a387c-146">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
   <span data-ttu-id="a387c-147">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791904.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a387c-147">![Configure App URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")</span></span>   
   1. <span data-ttu-id="a387c-148">A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign a tooyour Coupa alkalmazás által használt típus URL-cím (pl.: "*http://company.Coupa.com*").</span><span class="sxs-lookup"><span data-stu-id="a387c-148">In hello **Sign On URL** textbox, type URL used by your users toosign on tooyour Coupa application (e.g.: “*http://company.Coupa.com*”).</span></span>
   2. <span data-ttu-id="a387c-149">Nyissa meg a letöltött Coupa metaadat-fájlt, és másolja hello **AssertionConsumerService index/URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="a387c-149">Open your downloaded Coupa metadata file, and then copy hello **AssertionConsumerService index/URL**.</span></span>
   3. <span data-ttu-id="a387c-150">A hello **Coupa válasz URL-CÍMEN** szövegmezőhöz Beillesztés hello **AssertionConsumerService index/URL-cím** érték.</span><span class="sxs-lookup"><span data-stu-id="a387c-150">In hello **Coupa Reply URL** textbox, paste hello **AssertionConsumerService index/URL** value.</span></span>
   4. <span data-ttu-id="a387c-151">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a387c-151">Click **Next**.</span></span>
8. <span data-ttu-id="a387c-152">A hello **konfigurálhatja az egyszeri bejelentkezés Coupa** lap, toodownload a metaadatfájl kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="a387c-152">On hello **Configure single sign-on at Coupa** page, toodownload your metadata file, click **Download metadata**, and then save hello file locally on your computer.</span></span>
   
   <span data-ttu-id="a387c-153">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791905.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a387c-153">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")</span></span>
9. <span data-ttu-id="a387c-154">Hello Coupa vállalati webhelyen, lépjen túl**telepítő \> biztonsági ellenőrzést**.</span><span class="sxs-lookup"><span data-stu-id="a387c-154">On hello Coupa company site, go too**Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="a387c-155">![Biztonsági vezérlők](./media/active-directory-saas-coupa-tutorial/IC791900.png "biztonsági vezérlőket")</span><span class="sxs-lookup"><span data-stu-id="a387c-155">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
10. <span data-ttu-id="a387c-156">A hello **jelentkezzen be Coupa hitelesítő adatok használatával** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a387c-156">In hello **Log in using Coupa credentials** section, perform hello following steps:</span></span>  

   <span data-ttu-id="a387c-157">![Jelentkezzen be Coupa hitelesítő adatok használatával](./media/active-directory-saas-coupa-tutorial/IC791906.png "jelentkezzen be Coupa hitelesítő adatok használatával")</span><span class="sxs-lookup"><span data-stu-id="a387c-157">![Log in using Coupa credentials](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")</span></span> 
   1. <span data-ttu-id="a387c-158">Válassza ki **jelentkezzen be SAML**.</span><span class="sxs-lookup"><span data-stu-id="a387c-158">Select **Log in using SAML**.</span></span>
   2. <span data-ttu-id="a387c-159">Kattintson a **Tallózás** tooupload a letöltött Azure Active metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="a387c-159">Click **Browse** tooupload your downloaded Azure Active metadata file.</span></span>
   3. <span data-ttu-id="a387c-160">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a387c-160">Click **Save**.</span></span>
11. <span data-ttu-id="a387c-161">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a387c-161">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
   <span data-ttu-id="a387c-162">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791907.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a387c-162">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="a387c-163">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a387c-163">Configure user provisioning</span></span>

<span data-ttu-id="a387c-164">A sorrend tooenable az Azure AD felhasználók toolog Coupa be azok ki kell építenie Coupa be.</span><span class="sxs-lookup"><span data-stu-id="a387c-164">In order tooenable Azure AD users toolog into Coupa, they must be provisioned into Coupa.</span></span>  

* <span data-ttu-id="a387c-165">Coupa hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="a387c-165">In hello case of Coupa, provisioning is a manual task.</span></span>

<span data-ttu-id="a387c-166">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a387c-166">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="a387c-167">Jelentkezzen be tooyour **Coupa** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a387c-167">Log in tooyour **Coupa** company site as administrator.</span></span>
2. <span data-ttu-id="a387c-168">Hello hello felső menüben kattintson a **telepítő**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="a387c-168">In hello menu on hello top, click **Setup**, and then click **Users**.</span></span>
   
   <span data-ttu-id="a387c-169">![Felhasználók](./media/active-directory-saas-coupa-tutorial/IC791908.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="a387c-169">![Users](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")</span></span>
3. <span data-ttu-id="a387c-170">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a387c-170">Click **Create**.</span></span>
   
   <span data-ttu-id="a387c-171">![Felhasználók létrehozása](./media/active-directory-saas-coupa-tutorial/IC791909.png "felhasználók létrehozása")</span><span class="sxs-lookup"><span data-stu-id="a387c-171">![Create Users](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")</span></span>
4. <span data-ttu-id="a387c-172">A hello **felhasználó hozhat létre** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a387c-172">In hello **User Create** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="a387c-173">![Felhasználó adatait](./media/active-directory-saas-coupa-tutorial/IC791910.png "felhasználó részletei")</span><span class="sxs-lookup"><span data-stu-id="a387c-173">![User Details](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")</span></span>
   
   1. <span data-ttu-id="a387c-174">Típus hello **bejelentkezési**, **Utónév**, **Vezetéknév**, **egyszeri bejelentkezési azonosító**, **E-mail** attribútumait a a kívánt tooprovision hello érvényes Azure Active Directory-fiókkal kapcsolatos szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="a387c-174">Type hello **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** attributes of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   2. <span data-ttu-id="a387c-175">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a387c-175">Click **Create**.</span></span>   
   >[!NOTE]
   ><span data-ttu-id="a387c-176">hello Azure Active Directory fióktulajdonos kap e-mailben található egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="a387c-176">hello Azure Active Directory account holder will get an email with a link tooconfirm hello account before it becomes active.</span></span> 
   > 

>[!NOTE]
><span data-ttu-id="a387c-177">Bármely más Coupa felhasználói fiók létrehozása eszközök vagy Coupa tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="a387c-177">You can use any other Coupa user account creation tools or APIs provided by Coupa tooprovision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="a387c-178">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a387c-178">Assign users</span></span>
<span data-ttu-id="a387c-179">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="a387c-179">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="a387c-180">**tooassign felhasználók tooCoupa, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="a387c-180">**tooassign users tooCoupa, perform hello following steps:**</span></span>

1. <span data-ttu-id="a387c-181">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="a387c-181">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="a387c-182">A hello ** Coupa ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="a387c-182">On hello **Coupa **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="a387c-183">![Felhasználók hozzárendelése](./media/active-directory-saas-coupa-tutorial/IC791911.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="a387c-183">![Assign Users](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")</span></span>
3. <span data-ttu-id="a387c-184">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="a387c-184">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="a387c-185">![Igen](./media/active-directory-saas-coupa-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="a387c-185">![Yes](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="a387c-186">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="a387c-186">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="a387c-187">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a387c-187">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

