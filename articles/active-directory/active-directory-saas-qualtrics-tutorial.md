---
title: "Oktatóanyag: Azure Active Directoryval integrált Qualtrics |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Qualtrics az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 941642e74b90bb13a5ce37ce6665cfa32cd86016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a><span data-ttu-id="31453-103">Oktatóanyag: Azure Active Directoryval integrált Qualtrics</span><span class="sxs-lookup"><span data-stu-id="31453-103">Tutorial: Azure Active Directory integration with Qualtrics</span></span>
<span data-ttu-id="31453-104">hello Ez az oktatóanyag célja az Azure és Qualtrics tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="31453-104">hello objective of this tutorial is tooshow hello integration of Azure and Qualtrics.</span></span>  

<span data-ttu-id="31453-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="31453-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="31453-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="31453-106">A valid Azure subscription</span></span>
* <span data-ttu-id="31453-107">Egy Qualtrics egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="31453-107">A Qualtrics single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="31453-108">Ez az oktatóanyag befejezése után a tooQualtrics hozzárendelt hello Azure AD felhasználók fognak képes toosingle jelentkezzen be a Qualtrics vállalati helyen (szolgáltatás szolgáltató által kezdeményezett bejelentkezés), vagy hello segítségével hello alkalmazás [toohello bemutatása Hozzáférési Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="31453-108">After completing this tutorial, hello Azure AD users you have assigned tooQualtrics will be able toosingle sign into hello application at your Qualtrics company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="31453-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="31453-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="31453-110">Qualtrics hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="31453-110">Enabling hello application integration for Qualtrics</span></span>
2. <span data-ttu-id="31453-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="31453-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="31453-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="31453-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="31453-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="31453-113">Assigning users</span></span>

<span data-ttu-id="31453-114">![A forgatókönyv](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="31453-114">![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-qualtrics"></a><span data-ttu-id="31453-115">Qualtrics hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="31453-115">Enabling hello application integration for Qualtrics</span></span>
<span data-ttu-id="31453-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="31453-116">hello objective of this section is toooutline how tooenable hello application integration for Qualtrics.</span></span>

<span data-ttu-id="31453-117">**tooenable hello alkalmazásintegráció Qualtrics, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="31453-117">**tooenable hello application integration for Qualtrics, perform hello following steps:**</span></span>

1. <span data-ttu-id="31453-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="31453-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="31453-119">![Az Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="31453-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="31453-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="31453-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="31453-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="31453-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="31453-122">![Alkalmazások](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="31453-122">![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="31453-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="31453-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="31453-124">![Alkalmazás hozzáadása](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="31453-124">![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="31453-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="31453-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="31453-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="31453-126">![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="31453-127">A hello **keresőmezőbe**, típus **Qualtrics**.</span><span class="sxs-lookup"><span data-stu-id="31453-127">In hello **search box**, type **Qualtrics**.</span></span>
   
   <span data-ttu-id="31453-128">![Alkalmazáskatalógusában](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="31453-128">![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")</span></span>
7. <span data-ttu-id="31453-129">Hello eredmények ablaktábláján jelöljön ki **Qualtrics**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="31453-129">In hello results pane, select **Qualtrics**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="31453-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span><span class="sxs-lookup"><span data-stu-id="31453-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="31453-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="31453-131">Configure single sign-on</span></span>

<span data-ttu-id="31453-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooQualtrics fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="31453-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooQualtrics with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="31453-133">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="31453-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="31453-134">A klasszikus Azure portálon, a hello hello **Qualtrics** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="31453-134">In hello Azure classic portal, on hello **Qualtrics** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="31453-135">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="31453-135">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="31453-136">A hello **hogyan szeretné tooQualtrics a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="31453-136">On hello **How would you like users toosign on tooQualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="31453-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="31453-137">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="31453-138">A hello **alkalmazás URL-cím konfigurálása** lap hello **Qualtrics bejelentkezési URL-cím** szövegmező, írja be az URL-cím (pl.: "*https://ssotest2ut1.qualtrics.com*"), majd **Következő**.</span><span class="sxs-lookup"><span data-stu-id="31453-138">On hello **Configure App URL** page, in hello **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.</span></span>
   
   <span data-ttu-id="31453-139">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="31453-139">![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")</span></span>
4. <span data-ttu-id="31453-140">A hello **konfigurálhatja az egyszeri bejelentkezés Qualtrics** lapján kattintson **metaadatok letöltése**, és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="31453-140">On hello **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save hello metadata file on your computer.</span></span>
   
   <span data-ttu-id="31453-141">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="31453-141">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="31453-142">Küldés hello metaadatok fájl toohello Qualtrics támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="31453-142">Send hello metadata file toohello Qualtrics support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="31453-143">hello SSO konfiguráció hello Qualtrics támogatási csapat által elvégzett toobe tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="31453-143">hello SSO configuration has toobe performed by hello Qualtrics support team.</span></span> <span data-ttu-id="31453-144">Értesítést kap, amint hello konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="31453-144">You will get a notification as soon as hello configuration has been completed.</span></span>
   > 
   > 
6. <span data-ttu-id="31453-145">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="31453-145">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="31453-146">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="31453-146">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="31453-147">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="31453-147">Configure user provisioning</span></span>

<span data-ttu-id="31453-148">Nincs a akkor tooconfigure felhasználók átadásához tooQualtrics művelet elem.</span><span class="sxs-lookup"><span data-stu-id="31453-148">There is no action item for you tooconfigure user provisioning tooQualtrics.</span></span> <span data-ttu-id="31453-149">Ha egy hozzárendelt felhasználó megpróbál toolog Qualtrics hello hozzáférési panelen történő, Qualtrics ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="31453-149">When an assigned user tries toolog into Qualtrics using hello access panel, Qualtrics checks whether hello user exists.</span></span>  

<span data-ttu-id="31453-150">Nincs még nincs felhasználói fiók érhető el, ha a Qualtrics automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="31453-150">If there is no user account available yet, it is automatically created by Qualtrics.</span></span>

## <a name="assign-users"></a><span data-ttu-id="31453-151">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="31453-151">Assign users</span></span>
<span data-ttu-id="31453-152">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="31453-152">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="31453-153">**tooassign felhasználók tooQualtrics, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="31453-153">**tooassign users tooQualtrics, perform hello following steps:**</span></span>

1. <span data-ttu-id="31453-154">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="31453-154">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="31453-155">A hello **Qualtrics** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="31453-155">On hello **Qualtrics** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="31453-156">![Felhasználók hozzárendelése](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="31453-156">![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")</span></span>
3. <span data-ttu-id="31453-157">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="31453-157">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="31453-158">![Igen](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="31453-158">![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="31453-159">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="31453-159">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="31453-160">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="31453-160">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

