---
title: "Oktatóanyag: Azure Active Directory-integráció SCC életciklusával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SCC életciklusa az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: c10c313c5fc157ed70d2ccecfb930a8a765f8444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="a576b-103">Oktatóanyag: Azure Active Directoryval integrált SCC életciklusa</span><span class="sxs-lookup"><span data-stu-id="a576b-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>
<span data-ttu-id="a576b-104">hello Ez az oktatóanyag célja az Azure és SCC életciklus tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="a576b-104">hello objective of this tutorial is tooshow hello integration of Azure and SCC LifeCycle.</span></span>  

<span data-ttu-id="a576b-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a576b-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="a576b-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="a576b-106">A valid Azure subscription</span></span>
* <span data-ttu-id="a576b-107">Egy SCC életciklusa az egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a576b-107">A SCC LifeCycle single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="a576b-108">Ez az oktatóanyag befejezése után a hozzárendelt tooSCC életciklus lesz képes toosingle bejelentkezési hello alkalmazásba a SCC életciklus vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési) Azure AD-felhasználók hello, vagy a hello segítségével [bemutatása Hozzáférési Panel toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a576b-108">After completing this tutorial, hello Azure AD users you have assigned tooSCC LifeCycle will be able toosingle sign into hello application at your SCC LifeCycle company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="a576b-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="a576b-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="a576b-110">Hello alkalmazás SCC életciklus-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a576b-110">Enabling hello application integration for SCC LifeCycle</span></span>
2. <span data-ttu-id="a576b-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a576b-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="a576b-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="a576b-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="a576b-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a576b-113">Assigning users</span></span>

<span data-ttu-id="a576b-114">![A forgatókönyv](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="a576b-114">![Scenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-scc-lifecycle"></a><span data-ttu-id="a576b-115">Engedélyezze a hello alkalmazás integrációját SCC életciklusa</span><span class="sxs-lookup"><span data-stu-id="a576b-115">Enable hello application integration for SCC LifeCycle</span></span>
<span data-ttu-id="a576b-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció SCC életciklusát.</span><span class="sxs-lookup"><span data-stu-id="a576b-116">hello objective of this section is toooutline how tooenable hello application integration for SCC LifeCycle.</span></span>

<span data-ttu-id="a576b-117">**tooenable hello alkalmazásintegráció SCC életciklusát, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a576b-117">**tooenable hello application integration for SCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="a576b-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a576b-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="a576b-119">![Az Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="a576b-119">![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="a576b-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="a576b-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="a576b-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="a576b-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="a576b-122">![Alkalmazások](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="a576b-122">![Applications](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="a576b-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="a576b-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="a576b-124">![Alkalmazás hozzáadása](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a576b-124">![Add application](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="a576b-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a576b-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="a576b-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="a576b-126">![Add an application from gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="a576b-127">A hello **keresőmezőbe**, típus **SCC életciklus**.</span><span class="sxs-lookup"><span data-stu-id="a576b-127">In hello **search box**, type **SCC LifeCycle**.</span></span>
   
    <span data-ttu-id="a576b-128">![Alkalmazáskatalógusában](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="a576b-128">![Application Gallery](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")</span></span>
7. <span data-ttu-id="a576b-129">Hello eredmények ablaktábláján jelöljön ki **SCC életciklus**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a576b-129">In hello results pane, select **SCC LifeCycle**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="a576b-130">![SCC életciklus](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC életciklusa")</span><span class="sxs-lookup"><span data-stu-id="a576b-130">![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="a576b-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a576b-131">Configure single sign-on</span></span>

<span data-ttu-id="a576b-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooSCC életciklus fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="a576b-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooSCC LifeCycle with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="a576b-133">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="a576b-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="a576b-134">A klasszikus Azure portálon, a hello hello **SCC életciklus** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a576b-134">In hello Azure classic portal, on hello **SCC LifeCycle** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="a576b-135">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a576b-135">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="a576b-136">A hello **hogyan szeretné tooSCC életciklusa a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a576b-136">On hello **How would you like users toosign on tooSCC LifeCycle** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="a576b-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a576b-137">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="a576b-138">A hello **alkalmazás URL-cím konfigurálása** lap hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címet használják a felhasználók toosign tooyour a SCC életciklus-alkalmazást a következő mintát hello "*https:// bs1.SCC.com/lc7/Welcome/Customer/PICTtest.aspx*", és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a576b-138">On hello **Configure App URL** page, in hello **Sign On URL** textbox, type hello URL used by your users toosign on tooyour SCC LifeCycle application using hello following pattern "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", and then click **Next**.</span></span>
   
    <span data-ttu-id="a576b-139">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a576b-139">![Configure App URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")</span></span>
4. <span data-ttu-id="a576b-140">A hello **konfigurálhatja az egyszeri bejelentkezés SCC életciklus** lapján kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="a576b-140">On hello **Configure single sign-on at SCC LifeCycle** page, click **Download metadata**, and then save metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="a576b-141">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a576b-141">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="a576b-142">A metaadatok fájl tooSCC életciklus támogatási csoport továbbítja.</span><span class="sxs-lookup"><span data-stu-id="a576b-142">Forward that Metadata file tooSCC LifeCycle Support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="a576b-143">Egyszeri bejelentkezés hello SCC életciklus támogatási csoport által engedélyezett toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a576b-143">Single sign-on has toobe enabled by hello SCC LifeCycle support team.</span></span>
   > 
   > 

6. <span data-ttu-id="a576b-144">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a576b-144">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="a576b-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="a576b-145">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="a576b-146">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a576b-146">Configure user provisioning</span></span>

<span data-ttu-id="a576b-147">A sorrend tooenable az Azure AD felhasználók toolog SCC életciklus be azok kell üzembe SCC életciklus be.</span><span class="sxs-lookup"><span data-stu-id="a576b-147">In order tooenable Azure AD users toolog into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="a576b-148">Nincs művelet elem a akkor tooconfigure felhasználók átadásához tooSCC életciklusát.</span><span class="sxs-lookup"><span data-stu-id="a576b-148">There is no action item for you tooconfigure user provisioning tooSCC LifeCycle.</span></span>

<span data-ttu-id="a576b-149">Amikor egy hozzárendelt felhasználó záma toolog SCC életciklus be, SCC életciklus fiók automatikusan létrejön, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="a576b-149">When an assigned user tries toolog into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="a576b-150">Bármely más SCC életciklus felhasználói fiók létrehozása eszközök vagy SCC életciklus tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="a576b-150">You can use any other SCC LifeCycle user account creation tools or APIs provided by SCC LifeCycle tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="a576b-151">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a576b-151">Assign users</span></span>
<span data-ttu-id="a576b-152">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="a576b-152">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="a576b-153">**tooassign felhasználók tooSCC életciklusát, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a576b-153">**tooassign users tooSCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="a576b-154">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="a576b-154">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="a576b-155">A hello ** SCC életciklus ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="a576b-155">On hello **SCC LifeCycle **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="a576b-156">![Felhasználók hozzárendelése](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="a576b-156">![Assign Users](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")</span></span>
3. <span data-ttu-id="a576b-157">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="a576b-157">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="a576b-158">![Igen](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="a576b-158">![Yes](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="a576b-159">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="a576b-159">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="a576b-160">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a576b-160">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

