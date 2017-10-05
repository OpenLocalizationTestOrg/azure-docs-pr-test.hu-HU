---
title: "Oktatóanyag: Azure Active Directoryval integrált Coupa |} Microsoft Docs"
description: "Megtudhatja, hogyan Coupa használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
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
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a><span data-ttu-id="2302c-103">Oktatóanyag: Azure Active Directoryval integrált Coupa</span><span class="sxs-lookup"><span data-stu-id="2302c-103">Tutorial: Azure Active Directory integration with Coupa</span></span>
<span data-ttu-id="2302c-104">Ez az oktatóanyag célja az Azure és Coupa integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="2302c-104">The objective of this tutorial is to show the integration of Azure and Coupa.</span></span>  
<span data-ttu-id="2302c-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="2302c-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="2302c-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="2302c-106">A valid Azure subscription</span></span>
* <span data-ttu-id="2302c-107">Egy Coupa egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2302c-107">A Coupa single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="2302c-108">Ez az oktatóanyag befejezése után az Azure AD felhasználók Coupa rendelt tudják az alkalmazás használatával történő egyszeri bejelentkezéshez a [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2302c-108">After completing this tutorial, the Azure AD users you have assigned to Coupa will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="2302c-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2302c-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="2302c-110">Az alkalmazás Coupa-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2302c-110">Enabling the application integration for Coupa</span></span>
* <span data-ttu-id="2302c-111">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2302c-111">Configuring single sign-on</span></span>
* <span data-ttu-id="2302c-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="2302c-112">Configuring user provisioning</span></span>
* <span data-ttu-id="2302c-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2302c-113">Assigning users</span></span>

<span data-ttu-id="2302c-114">![A forgatókönyv](./media/active-directory-saas-coupa-tutorial/IC791897.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="2302c-114">![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-coupa"></a><span data-ttu-id="2302c-115">Az alkalmazás Coupa-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2302c-115">Enable the application integration for Coupa</span></span>
<span data-ttu-id="2302c-116">Ez a szakasz célja felvázoló Coupa az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2302c-116">The objective of this section is to outline how to enable the application integration for Coupa.</span></span>

<span data-ttu-id="2302c-117">**Ahhoz, hogy az alkalmazás-integráció Coupa, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2302c-117">**To enable the application integration for Coupa, perform the following steps:**</span></span>

1. <span data-ttu-id="2302c-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2302c-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="2302c-119">![Az Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="2302c-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="2302c-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="2302c-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="2302c-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="2302c-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="2302c-122">![Alkalmazások](./media/active-directory-saas-coupa-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="2302c-122">![Applications](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="2302c-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="2302c-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="2302c-124">![Alkalmazás hozzáadása](./media/active-directory-saas-coupa-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="2302c-124">![Add application](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="2302c-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="2302c-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="2302c-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="2302c-126">![Add an application from gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="2302c-127">Az a **keresőmezőbe**, típus **Coupa**.</span><span class="sxs-lookup"><span data-stu-id="2302c-127">In the **search box**, type **Coupa**.</span></span>
   
   <span data-ttu-id="2302c-128">![Alkalmazáskatalógusában](./media/active-directory-saas-coupa-tutorial/IC791898.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="2302c-128">![Application Gallery](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")</span></span>
7. <span data-ttu-id="2302c-129">Az eredmények ablaktáblájában válassza **Coupa**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2302c-129">In the results pane, select **Coupa**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="2302c-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span><span class="sxs-lookup"><span data-stu-id="2302c-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="2302c-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2302c-131">Configure single sign-on</span></span>

<span data-ttu-id="2302c-132">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Coupa fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="2302c-132">The objective of this section is to outline how to enable users to authenticate to Coupa with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="2302c-133">Egyszeri bejelentkezés az Coupa konfigurálásával meg kell ujjlenyomat értéket beolvasni egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2302c-133">Configuring single sign-on for Coupa requires you to retrieve a thumbprint value from a certificate.</span></span> <span data-ttu-id="2302c-134">Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan lehet lekérni egy tanúsítvány-ujjlenyomat értékének](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="2302c-134">If you are not familiar with this procedure, see [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span>

<span data-ttu-id="2302c-135">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2302c-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="2302c-136">Jelentkezzen be rendszergazdaként a Coupa vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="2302c-136">Sign on to your Coupa company site as an administrator.</span></span>
2. <span data-ttu-id="2302c-137">Ugrás a **telepítő \> biztonsági ellenőrzést**.</span><span class="sxs-lookup"><span data-stu-id="2302c-137">Go to **Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="2302c-138">![Biztonsági vezérlők](./media/active-directory-saas-coupa-tutorial/IC791900.png "biztonsági vezérlőket")</span><span class="sxs-lookup"><span data-stu-id="2302c-138">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
3. <span data-ttu-id="2302c-139">A számítógépre a Coupa metaadatait tartalmazó fájl letöltéséhez kattintson **letöltési és SP metaadatok importálása**.</span><span class="sxs-lookup"><span data-stu-id="2302c-139">To download the Coupa metadata file to your computer, click **Download and import SP metadata**.</span></span>
   
   <span data-ttu-id="2302c-140">![Coupa SP metaadatok](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metaadatok")</span><span class="sxs-lookup"><span data-stu-id="2302c-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span></span>
4. <span data-ttu-id="2302c-141">Egy másik böngészőablakban jelentkezzen be a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="2302c-141">In a different browser window, sign on to the Azure classic portal.</span></span>
5. <span data-ttu-id="2302c-142">Az a **Coupa** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2302c-142">On the **Coupa** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="2302c-143">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791902.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="2302c-143">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="2302c-144">Az a **hová bejelentkezni Coupa felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2302c-144">On the **How would you like users to sign on to Coupa** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="2302c-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791903.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="2302c-145">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="2302c-146">Az a **alkalmazás URL-cím konfigurálása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="2302c-146">On the **Configure App URL** page, perform the following steps:</span></span>
   
   <span data-ttu-id="2302c-147">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791904.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="2302c-147">![Configure App URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")</span></span>   
   1. <span data-ttu-id="2302c-148">Az a **URL-cím bejelentkezési** szövegmező, jelentkezzen be a Coupa alkalmazás a felhasználók által használt típus URL-cím (pl.: "*http://company.Coupa.com*").</span><span class="sxs-lookup"><span data-stu-id="2302c-148">In the **Sign On URL** textbox, type URL used by your users to sign on to your Coupa application (e.g.: “*http://company.Coupa.com*”).</span></span>
   2. <span data-ttu-id="2302c-149">Nyissa meg a letöltött Coupa metaadat-fájlt, és másolja a **AssertionConsumerService index/URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="2302c-149">Open your downloaded Coupa metadata file, and then copy the **AssertionConsumerService index/URL**.</span></span>
   3. <span data-ttu-id="2302c-150">Az a **Coupa válasz URL-CÍMEN** szövegmező, illessze be a **AssertionConsumerService index/URL-cím** érték.</span><span class="sxs-lookup"><span data-stu-id="2302c-150">In the **Coupa Reply URL** textbox, paste the **AssertionConsumerService index/URL** value.</span></span>
   4. <span data-ttu-id="2302c-151">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2302c-151">Click **Next**.</span></span>
8. <span data-ttu-id="2302c-152">A a **konfigurálhatja az egyszeri bejelentkezés Coupa** töltse le a metaadat-fájlt, kattintson **metaadatok letöltése**, és mentse helyileg a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2302c-152">On the **Configure single sign-on at Coupa** page, to download your metadata file, click **Download metadata**, and then save the file locally on your computer.</span></span>
   
   <span data-ttu-id="2302c-153">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791905.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="2302c-153">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")</span></span>
9. <span data-ttu-id="2302c-154">Lépjen a Coupa vállalati helyen **telepítő \> biztonsági ellenőrzést**.</span><span class="sxs-lookup"><span data-stu-id="2302c-154">On the Coupa company site, go to **Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="2302c-155">![Biztonsági vezérlők](./media/active-directory-saas-coupa-tutorial/IC791900.png "biztonsági vezérlőket")</span><span class="sxs-lookup"><span data-stu-id="2302c-155">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
10. <span data-ttu-id="2302c-156">Az a **jelentkezzen be Coupa hitelesítő adatok használatával** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2302c-156">In the **Log in using Coupa credentials** section, perform the following steps:</span></span>  

   <span data-ttu-id="2302c-157">![Jelentkezzen be Coupa hitelesítő adatok használatával](./media/active-directory-saas-coupa-tutorial/IC791906.png "jelentkezzen be Coupa hitelesítő adatok használatával")</span><span class="sxs-lookup"><span data-stu-id="2302c-157">![Log in using Coupa credentials](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")</span></span> 
   1. <span data-ttu-id="2302c-158">Válassza ki **jelentkezzen be SAML**.</span><span class="sxs-lookup"><span data-stu-id="2302c-158">Select **Log in using SAML**.</span></span>
   2. <span data-ttu-id="2302c-159">Kattintson a **Tallózás** fel kell töltenie a letöltött Azure Active metaadat-fájlt.</span><span class="sxs-lookup"><span data-stu-id="2302c-159">Click **Browse** to upload your downloaded Azure Active metadata file.</span></span>
   3. <span data-ttu-id="2302c-160">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2302c-160">Click **Save**.</span></span>
11. <span data-ttu-id="2302c-161">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2302c-161">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
   <span data-ttu-id="2302c-162">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791907.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="2302c-162">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="2302c-163">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2302c-163">Configure user provisioning</span></span>

<span data-ttu-id="2302c-164">Ahhoz, hogy az Azure AD-felhasználók Coupa bejelentkezni, akkor ki kell építenie Coupa be.</span><span class="sxs-lookup"><span data-stu-id="2302c-164">In order to enable Azure AD users to log into Coupa, they must be provisioned into Coupa.</span></span>  

* <span data-ttu-id="2302c-165">Coupa, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2302c-165">In the case of Coupa, provisioning is a manual task.</span></span>

<span data-ttu-id="2302c-166">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2302c-166">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="2302c-167">Jelentkezzen be a **Coupa** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2302c-167">Log in to your **Coupa** company site as administrator.</span></span>
2. <span data-ttu-id="2302c-168">Kattintson a felső menüben **telepítő**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="2302c-168">In the menu on the top, click **Setup**, and then click **Users**.</span></span>
   
   <span data-ttu-id="2302c-169">![Felhasználók](./media/active-directory-saas-coupa-tutorial/IC791908.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="2302c-169">![Users](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")</span></span>
3. <span data-ttu-id="2302c-170">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2302c-170">Click **Create**.</span></span>
   
   <span data-ttu-id="2302c-171">![Felhasználók létrehozása](./media/active-directory-saas-coupa-tutorial/IC791909.png "felhasználók létrehozása")</span><span class="sxs-lookup"><span data-stu-id="2302c-171">![Create Users](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")</span></span>
4. <span data-ttu-id="2302c-172">Az a **felhasználó hozhat létre** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2302c-172">In the **User Create** section, perform the following steps:</span></span>
   
   <span data-ttu-id="2302c-173">![Felhasználó adatait](./media/active-directory-saas-coupa-tutorial/IC791910.png "felhasználó részletei")</span><span class="sxs-lookup"><span data-stu-id="2302c-173">![User Details](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")</span></span>
   
   1. <span data-ttu-id="2302c-174">Típus a **bejelentkezési**, **Utónév**, **Vezetéknév**, **egyszeri bejelentkezési azonosító**, **E-mail** egy érvényes Azure Active Directory-fiókot szeretné azokat a kapcsolódó szövegmezők rendelkezés attribútumait.</span><span class="sxs-lookup"><span data-stu-id="2302c-174">Type the **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** attributes of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   2. <span data-ttu-id="2302c-175">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2302c-175">Click **Create**.</span></span>   
   >[!NOTE]
   ><span data-ttu-id="2302c-176">Az Azure Active Directory fióktulajdonos kap egy e-mailt egy hivatkozás, mielőtt aktívvá válik, győződjön meg arról, hogy a fiók.</span><span class="sxs-lookup"><span data-stu-id="2302c-176">The Azure Active Directory account holder will get an email with a link to confirm the account before it becomes active.</span></span> 
   > 

>[!NOTE]
><span data-ttu-id="2302c-177">Bármely más Coupa felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Coupa által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="2302c-177">You can use any other Coupa user account creation tools or APIs provided by Coupa to provision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="2302c-178">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2302c-178">Assign users</span></span>
<span data-ttu-id="2302c-179">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="2302c-179">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="2302c-180">**Felhasználók hozzárendelése Coupa, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2302c-180">**To assign users to Coupa, perform the following steps:**</span></span>

1. <span data-ttu-id="2302c-181">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="2302c-181">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="2302c-182">Az a ** Coupa ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="2302c-182">On the **Coupa **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="2302c-183">![Felhasználók hozzárendelése](./media/active-directory-saas-coupa-tutorial/IC791911.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="2302c-183">![Assign Users](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")</span></span>
3. <span data-ttu-id="2302c-184">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2302c-184">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="2302c-185">![Igen](./media/active-directory-saas-coupa-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="2302c-185">![Yes](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="2302c-186">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="2302c-186">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="2302c-187">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2302c-187">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

