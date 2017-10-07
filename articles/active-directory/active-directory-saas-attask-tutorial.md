---
title: "Oktatóanyag: Azure Active Directoryval integrált @Task|} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory közötti és @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 0840763622086a02a27cfafff3b741bc66cec498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a><span data-ttu-id="ff246-103">Oktatóanyag: Azure Active Directory integrációja@Task</span><span class="sxs-lookup"><span data-stu-id="ff246-103">Tutorial: Azure Active Directory integration with @Task</span></span>
<span data-ttu-id="ff246-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate @Task az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ff246-104">hello objective of this tutorial is tooshow you how toointegrate @Task with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="ff246-105">Integrálása @Task az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ff246-105">Integrating @Task with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="ff246-106">Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-bentoo@Task</span><span class="sxs-lookup"><span data-stu-id="ff246-106">You can control in Azure AD who has access too@Task</span></span>
* <span data-ttu-id="ff246-107">Engedélyezheti a felhasználóknak tooautomatically bejelentkezett too@Task (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ff246-107">You can enable your users tooautomatically get signed-on too@Task (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="ff246-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="ff246-108">You can manage your accounts in one central location - hello Azure classic Portal</span></span>

<span data-ttu-id="ff246-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff246-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff246-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff246-110">Prerequisites</span></span>
<span data-ttu-id="ff246-111">integráció az Azure AD tooconfigure @Task, a következő elemek hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="ff246-111">tooconfigure Azure AD integration with @Task, you need hello following items:</span></span>

* <span data-ttu-id="ff246-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ff246-112">An Azure AD subscription</span></span>
* <span data-ttu-id="ff246-113">Egy @Task egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ff246-113">An @Task single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff246-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ff246-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="ff246-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ff246-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="ff246-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ff246-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="ff246-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff246-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="ff246-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ff246-118">Scenario Description</span></span>
<span data-ttu-id="ff246-119">hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ff246-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="ff246-120">hello forgatókönyv ebben az oktatóanyagban leírt három fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ff246-120">hello scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="ff246-121">Hozzáadás @Task hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ff246-121">Adding @Task from hello gallery</span></span> 
2. <span data-ttu-id="ff246-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ff246-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-task-from-hello-gallery"></a><span data-ttu-id="ff246-123">Hozzáadás @Task hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ff246-123">Adding @Task from hello gallery</span></span>
<span data-ttu-id="ff246-124">tooconfigure hello integrációja @Task az Azure AD-be kell tooadd @Task hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="ff246-124">tooconfigure hello integration of @Task into Azure AD, you need tooadd @Task from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ff246-125">**tooadd @Task hello gyűjteményből, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ff246-125">**tooadd @Task from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff246-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ff246-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1] 
2. <span data-ttu-id="ff246-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="ff246-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="ff246-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="ff246-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2] 
4. <span data-ttu-id="ff246-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="ff246-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3] 
5. <span data-ttu-id="ff246-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ff246-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4] 
6. <span data-ttu-id="ff246-135">Hello keresési mezőbe, írja be a  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="ff246-135">In hello search box, type **@Task**.</span></span>
   
    ![Alkalmazások][5] 
7. <span data-ttu-id="ff246-137">Hello eredmények ablaktábláján jelöljön ki  **@Task** , és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ff246-137">In hello results pane, select **@Task**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Alkalmazások][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff246-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ff246-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff246-140">hello ebben a szakaszban célja az, hogyan tooconfigure és tesztelése az Azure AD egyszeri bejelentkezés az tooshow @Task "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="ff246-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with @Task based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff246-141">Az egyszeri bejelentkezés toowork, az Azure AD kell tooknow milyen hello megfelelőjére felhasználó @Task tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ff246-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in @Task tooan user in Azure AD is.</span></span> <span data-ttu-id="ff246-142">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello közötti kapcsolat kapcsolatot @Task létrehozott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="ff246-142">In other words, a link relationship between an Azure AD user and hello related user in @Task needs toobe established.</span></span>   
<span data-ttu-id="ff246-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a @Task.</span><span class="sxs-lookup"><span data-stu-id="ff246-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in @Task.</span></span>

<span data-ttu-id="ff246-144">tooconfigure és tesztelése az Azure AD-egyszeri bejelentkezés az @Task, a következő építőelemeket toocomplete hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="ff246-144">tooconfigure and test Azure AD single sign-on with @Task, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ff246-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ff246-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ff246-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff246-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff246-147">**[Létrehozása egy @Tasktest felhasználói](#creating-a-halogen-software-test-user)**  -Britta Simon a megfelelője a toohave @Taskthat rá, hogy az Azure AD csatolt toohello megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ff246-147">**[Creating a @Tasktest user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in @Taskthat is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="ff246-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ff246-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff246-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ff246-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff246-150">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ff246-150">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="ff246-151">hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés a klasszikus Azure portálon hello és tooconfigure egyszeri bejelentkezést a a @Task alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ff246-151">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your @Task application.</span></span>

<span data-ttu-id="ff246-152">**az Azure AD az egyszeri bejelentkezés tooconfigure @Task, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="ff246-152">**tooconfigure Azure AD single sign-on with @Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff246-153">A klasszikus Azure portálon, a hello hello  **@Task**  alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ff246-153">In hello Azure classic portal, on hello **@Task** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. <span data-ttu-id="ff246-155">A hello **hogyan szeretné felhasználók toosign a too@Task**  lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ff246-155">On hello **How would you like users toosign on too@Task** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][7] 
3. <span data-ttu-id="ff246-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ff246-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>
   
    ![Alkalmazásbeállítások konfigurálása][8] 
   
     <span data-ttu-id="ff246-159">a.</span><span class="sxs-lookup"><span data-stu-id="ff246-159">a.</span></span> <span data-ttu-id="ff246-160">A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címet használják a felhasználók toosign a tooyour @Task alkalmazás (pl.:*https://<Tenant name>.attask-ondemand.com*).</span><span class="sxs-lookup"><span data-stu-id="ff246-160">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour @Task application (e.g.:*https://<Tenant name>.attask-ondemand.com*).</span></span>
   
     <span data-ttu-id="ff246-161">b.</span><span class="sxs-lookup"><span data-stu-id="ff246-161">b.</span></span> <span data-ttu-id="ff246-162">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ff246-162">Click **Next**.</span></span>
4. <span data-ttu-id="ff246-163">A hello **konfigurálása egyszeri bejelentkezéshez, @Task**  kattintson **metaadatok letöltése**, mentse helyileg a számítógépen hello metaadatok fájlt, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ff246-163">On hello **Configure single sign-on at @Task** page, click **Download metadata**, save hello metadata file locally on your computer, and then click **Next**.</span></span>
   
    ![Mi az az Azure AD Connect?][9] 
5. <span data-ttu-id="ff246-165">Bejelentkezés tooyour @Task vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ff246-165">Sign-on tooyour @Task company site as administrator.</span></span>
6. <span data-ttu-id="ff246-166">Nyissa meg túl**egyszeri bejelentkezést a konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="ff246-166">Go too**Single Sign On Configuration**.</span></span>
7. <span data-ttu-id="ff246-167">A hello **egyszeri bejelentkezés** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello</span><span class="sxs-lookup"><span data-stu-id="ff246-167">On hello **Single Sign-On** dialog, perform hello following steps</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    <span data-ttu-id="ff246-169">a.</span><span class="sxs-lookup"><span data-stu-id="ff246-169">a.</span></span> <span data-ttu-id="ff246-170">Mint **típus**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ff246-170">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="ff246-171">b.</span><span class="sxs-lookup"><span data-stu-id="ff246-171">b.</span></span> <span data-ttu-id="ff246-172">Válassza ki **Szolgáltatóazonosító szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="ff246-172">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="ff246-173">c.</span><span class="sxs-lookup"><span data-stu-id="ff246-173">c.</span></span> <span data-ttu-id="ff246-174">A klasszikus Azure portálon hello, másolja a hello **távoli bejelentkezési URL-cím**, és illessze be hello **bejelentkezési portál URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ff246-174">On hello Azure classic portal, copy hello **Remote Login URL**, and then paste it into hello **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="ff246-175">d.</span><span class="sxs-lookup"><span data-stu-id="ff246-175">d.</span></span> <span data-ttu-id="ff246-176">A klasszikus Azure portálon hello, másolja a hello **egyetlen Sign-Out URL-címe**, majd illessze be hello **Sign-Out URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ff246-176">On hello Azure classic portal, copy hello **Single Sign-Out Service URL**, and then paste it into hello **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="ff246-177">e.</span><span class="sxs-lookup"><span data-stu-id="ff246-177">e.</span></span> <span data-ttu-id="ff246-178">A klasszikus Azure portálon hello, másolja a hello **jelszó URL-cím módosítása**, majd illessze be hello **jelszó URL-cím módosítása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ff246-178">On hello Azure classic portal, copy hello **Change Password URL**, and then paste it into hello **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="ff246-179">f.</span><span class="sxs-lookup"><span data-stu-id="ff246-179">f.</span></span> <span data-ttu-id="ff246-180">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ff246-180">Click **Save**.</span></span>
8. <span data-ttu-id="ff246-181">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ff246-181">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Mi az az Azure AD Connect?][10]
9. <span data-ttu-id="ff246-183">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="ff246-183">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Mi az az Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff246-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff246-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff246-186">hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ff246-186">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>  

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="ff246-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ff246-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff246-189">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ff246-189">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. <span data-ttu-id="ff246-191">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="ff246-191">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="ff246-192">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="ff246-192">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. <span data-ttu-id="ff246-194">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ff246-194">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. <span data-ttu-id="ff246-196">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ff246-196">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="ff246-198">a.</span><span class="sxs-lookup"><span data-stu-id="ff246-198">a.</span></span> <span data-ttu-id="ff246-199">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="ff246-199">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="ff246-200">b.</span><span class="sxs-lookup"><span data-stu-id="ff246-200">b.</span></span> <span data-ttu-id="ff246-201">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff246-201">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="ff246-202">c.</span><span class="sxs-lookup"><span data-stu-id="ff246-202">c.</span></span> <span data-ttu-id="ff246-203">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ff246-203">Click **Next**.</span></span>
6. <span data-ttu-id="ff246-204">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ff246-204">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    <span data-ttu-id="ff246-206">a.</span><span class="sxs-lookup"><span data-stu-id="ff246-206">a.</span></span> <span data-ttu-id="ff246-207">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ff246-207">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="ff246-208">b.</span><span class="sxs-lookup"><span data-stu-id="ff246-208">b.</span></span> <span data-ttu-id="ff246-209">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ff246-209">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="ff246-210">c.</span><span class="sxs-lookup"><span data-stu-id="ff246-210">c.</span></span> <span data-ttu-id="ff246-211">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ff246-211">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="ff246-212">d.</span><span class="sxs-lookup"><span data-stu-id="ff246-212">d.</span></span> <span data-ttu-id="ff246-213">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="ff246-213">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="ff246-214">e.</span><span class="sxs-lookup"><span data-stu-id="ff246-214">e.</span></span> <span data-ttu-id="ff246-215">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ff246-215">Click **Next**.</span></span>

7. <span data-ttu-id="ff246-216">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ff246-216">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. <span data-ttu-id="ff246-218">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ff246-218">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="ff246-220">a.</span><span class="sxs-lookup"><span data-stu-id="ff246-220">a.</span></span> <span data-ttu-id="ff246-221">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ff246-221">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="ff246-222">b.</span><span class="sxs-lookup"><span data-stu-id="ff246-222">b.</span></span> <span data-ttu-id="ff246-223">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ff246-223">Click **Complete**.</span></span>   

### <a name="creating-an-task-test-user"></a><span data-ttu-id="ff246-224">Létrehozása egy @Task tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="ff246-224">Creating an @Task test user</span></span>
<span data-ttu-id="ff246-225">hello ebben a szakaszban célja toocreate Britta Simon nevű felhasználó @Task.</span><span class="sxs-lookup"><span data-stu-id="ff246-225">hello objective of this section is toocreate a user called Britta Simon in @Task.</span></span>

<span data-ttu-id="ff246-226">**toocreate Britta Simon nevű felhasználó @Task, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="ff246-226">**toocreate a user called Britta Simon in @Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff246-227">Jelentkezzen be tooyour @Task vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ff246-227">Sign on tooyour @Task company site as administrator.</span></span>
2. <span data-ttu-id="ff246-228">Hello hello felső menüben kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="ff246-228">In hello menu on hello top, click **People**.</span></span>
3. <span data-ttu-id="ff246-229">Kattintson a **új személy**.</span><span class="sxs-lookup"><span data-stu-id="ff246-229">Click **New Person**.</span></span> 
4. <span data-ttu-id="ff246-230">Hello új személy párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="ff246-230">On hello New Person dialog, perform hello following steps:</span></span>
   
    ![Hozzon létre egy @Task tesztfelhasználó számára][21] 
   
    <span data-ttu-id="ff246-232">a.</span><span class="sxs-lookup"><span data-stu-id="ff246-232">a.</span></span> <span data-ttu-id="ff246-233">A hello **Keresztnév** szövegmező, írja be a "Britta".</span><span class="sxs-lookup"><span data-stu-id="ff246-233">In hello **First Name** textbox, type "Britta".</span></span>
   
    <span data-ttu-id="ff246-234">b.</span><span class="sxs-lookup"><span data-stu-id="ff246-234">b.</span></span> <span data-ttu-id="ff246-235">A hello **Vezetéknév** szövegmező, írja be a "Simon".</span><span class="sxs-lookup"><span data-stu-id="ff246-235">In hello **Last Name** textbox, type "Simon".</span></span>
   
    <span data-ttu-id="ff246-236">c.</span><span class="sxs-lookup"><span data-stu-id="ff246-236">c.</span></span> <span data-ttu-id="ff246-237">A hello **E-mail cím** szövegmező, írja be a Britta Simon e-mail címét az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="ff246-237">In hello **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="ff246-238">d.</span><span class="sxs-lookup"><span data-stu-id="ff246-238">d.</span></span> <span data-ttu-id="ff246-239">Kattintson a **személy hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ff246-239">Click **Add Person**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ff246-240">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ff246-240">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="ff246-241">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezés nyújtó saját too@Task.</span><span class="sxs-lookup"><span data-stu-id="ff246-241">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access too@Task.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ff246-243">**tooassign Britta Simon too@Task, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="ff246-243">**tooassign Britta Simon too@Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff246-244">A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="ff246-244">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201] 
2. <span data-ttu-id="ff246-246">Hello alkalmazások listában válassza ki a  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="ff246-246">In hello applications list, select **@Task**.</span></span>
   
    ![Felhasználó hozzárendelése][202] 
3. <span data-ttu-id="ff246-248">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="ff246-248">In hello menu on hello top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203] 
4. <span data-ttu-id="ff246-250">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ff246-250">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="ff246-251">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="ff246-251">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="ff246-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ff246-253">Testing Single Sign-On</span></span>
<span data-ttu-id="ff246-254">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="ff246-254">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="ff246-255">Amikor rákattint hello @Task csempe az hello hozzáférési panelre, akkor kapja meg automatikusan bejelentkezett tooyour @Task alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ff246-255">When you click hello @Task tile in hello Access Panel, you should get automatically signed-on tooyour @Task application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff246-256">További források</span><span class="sxs-lookup"><span data-stu-id="ff246-256">Additional Resources</span></span>
* [<span data-ttu-id="ff246-257">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ff246-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff246-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ff246-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






