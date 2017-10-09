---
title: "Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Soonr munkahelyi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: f950b45d0beceab2fa17b7690c9de81ec6603089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a><span data-ttu-id="26cd7-103">Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi</span><span class="sxs-lookup"><span data-stu-id="26cd7-103">Tutorial: Azure Active Directory integration with Soonr Workplace</span></span>

<span data-ttu-id="26cd7-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Soonr munkahelyi Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="26cd7-104">hello objective of this tutorial is tooshow you how toointegrate Soonr Workplace with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="26cd7-105">Soonr munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="26cd7-105">Integrating Soonr Workplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="26cd7-106">Megadhatja a munkahelyi hozzáférés tooSoonr rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="26cd7-106">You can control in Azure AD who has access tooSoonr Workplace</span></span>
- <span data-ttu-id="26cd7-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSoonr munkahelyi (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="26cd7-107">You can enable your users tooautomatically get signed-on tooSoonr Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="26cd7-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="26cd7-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="26cd7-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="26cd7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26cd7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="26cd7-110">Prerequisites</span></span>

<span data-ttu-id="26cd7-111">az Azure AD integrálása Soonr munkahelyi tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="26cd7-111">tooconfigure Azure AD integration with Soonr Workplace, you need hello following items:</span></span>

- <span data-ttu-id="26cd7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="26cd7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="26cd7-113">Egy Soonr munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="26cd7-113">A Soonr Workplace single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="26cd7-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="26cd7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="26cd7-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="26cd7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="26cd7-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="26cd7-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="26cd7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="26cd7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="26cd7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="26cd7-118">Scenario description</span></span>
<span data-ttu-id="26cd7-119">hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="26cd7-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="26cd7-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="26cd7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="26cd7-121">Hello gyűjteményből Soonr munkahelyi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="26cd7-121">Adding Soonr Workplace from hello gallery</span></span>
2. <span data-ttu-id="26cd7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="26cd7-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-soonr-workplace-from-hello-gallery"></a><span data-ttu-id="26cd7-123">Hello gyűjteményből Soonr munkahelyi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="26cd7-123">Adding Soonr Workplace from hello gallery</span></span>
<span data-ttu-id="26cd7-124">tooconfigure hello integrációs Soonr munkahely, az Azure AD-be, meg kell tooadd Soonr munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="26cd7-124">tooconfigure hello integration of Soonr Workplace into Azure AD, you need tooadd Soonr Workplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="26cd7-125">**tooadd Soonr munkahelyi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="26cd7-125">**tooadd Soonr Workplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="26cd7-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="26cd7-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="26cd7-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="26cd7-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="26cd7-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Alkalmazások][2]

4. <span data-ttu-id="26cd7-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="26cd7-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Alkalmazások][3]

5. <span data-ttu-id="26cd7-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
 
    ![Alkalmazások][4]

6. <span data-ttu-id="26cd7-135">Hello keresési mezőbe, írja be a **Soonr munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-135">In hello search box, type **Soonr Workplace**.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. <span data-ttu-id="26cd7-137">Hello eredmények ablaktábláján jelöljön ki **Soonr munkahelyi**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26cd7-137">In hello results pane, select **Soonr Workplace**, and then click **Complete** tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="26cd7-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="26cd7-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="26cd7-140">hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés Soonr munkahelyi-teszthez alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="26cd7-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with Soonr Workplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="26cd7-141">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Soonr munkahelyi tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="26cd7-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Soonr Workplace tooan user in Azure AD is.</span></span> <span data-ttu-id="26cd7-142">Ez azt jelenti hello kapcsolódó felhasználói Soonr munkahelyi és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="26cd7-142">In other words, a link relationship between an Azure AD user and hello related user in Soonr Workplace needs toobe established.</span></span>  

<span data-ttu-id="26cd7-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Soonr munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="26cd7-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Soonr Workplace.</span></span>

<span data-ttu-id="26cd7-144">tooconfigure és az Azure AD az egyszeri bejelentkezés Soonr munkahelyi-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="26cd7-144">tooconfigure and test Azure AD single sign-on with Soonr Workplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="26cd7-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="26cd7-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="26cd7-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="26cd7-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="26cd7-147">**[Soonr munkahelyi tesztfelhasználó létrehozása](#creating-a-soonr-workplace-test-user)**  -toohave Britta Simon Soonr munkahelyi, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="26cd7-147">**[Creating a Soonr Workplace test user](#creating-a-soonr-workplace-test-user)** - toohave a counterpart of Britta Simon in Soonr Workplace that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="26cd7-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="26cd7-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="26cd7-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="26cd7-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="26cd7-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26cd7-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="26cd7-151">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az Soonr munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26cd7-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Soonr Workplace application.</span></span>


<span data-ttu-id="26cd7-152">**tooconfigure az Azure AD egyszeri bejelentkezést a Soonr munkahelyi, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="26cd7-152">**tooconfigure Azure AD single sign-on with Soonr Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="26cd7-153">A klasszikus Azure portálon, a hello hello **Soonr munkahelyi** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="26cd7-153">In hello Azure classic portal, on hello **Soonr Workplace** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][6] 

2. <span data-ttu-id="26cd7-155">A hello **hogyan szeretné a munkahelyi tooSoonr felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-155">On hello **How would you like users toosign on tooSoonr Workplace** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. <span data-ttu-id="26cd7-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:.</span><span class="sxs-lookup"><span data-stu-id="26cd7-157">On hello **Configure App Settings** dialog page, perform hello following steps:.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    <span data-ttu-id="26cd7-159">a.</span><span class="sxs-lookup"><span data-stu-id="26cd7-159">a.</span></span> <span data-ttu-id="26cd7-160">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span><span class="sxs-lookup"><span data-stu-id="26cd7-160">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span></span>

    <span data-ttu-id="26cd7-161">b.</span><span class="sxs-lookup"><span data-stu-id="26cd7-161">b.</span></span> <span data-ttu-id="26cd7-162">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="26cd7-162">Click **Next**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="26cd7-163">Ne feledje, hogy ez a nem hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="26cd7-163">Please note that this is not hello real value.</span></span> <span data-ttu-id="26cd7-164">Ezt az értéket hello tényleges bejelentkezési URL-cím tooupdate rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="26cd7-164">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="26cd7-165">Kapcsolattartási Soonr munkahelyi team tooget támogatják ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="26cd7-165">Contact Soonr Workplace support team tooget this value.</span></span>

4. <span data-ttu-id="26cd7-166">A hello **Soonr munkahelyi egyszeri bejelentkezés konfigurálása** kattintson **metaadatok letöltése** , és mentse a hello fájlt a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="26cd7-166">On hello **Configure single sign-on at Soonr Workplace** page, click **Download metadata** and then save hello file on your computer:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. <span data-ttu-id="26cd7-168">az alkalmazáshoz konfigurált SSO tooget a Soonr munkahelyi támogatási csoportjához, és adja meg hello következő:</span><span class="sxs-lookup"><span data-stu-id="26cd7-168">tooget SSO configured for your application, contact your Soonr Workplace support team and provide them with hello following:</span></span> 

    <span data-ttu-id="26cd7-169">• hello letöltött **metaadatok** fájl</span><span class="sxs-lookup"><span data-stu-id="26cd7-169">•  hello downloaded **Metadata** file</span></span>

    <span data-ttu-id="26cd7-170">• hello **kiállítójának URL-címe**</span><span class="sxs-lookup"><span data-stu-id="26cd7-170">•  hello **Issuer URL**</span></span>

    <span data-ttu-id="26cd7-171">• hello **SAML SSO URL-címe**</span><span class="sxs-lookup"><span data-stu-id="26cd7-171">•  hello **SAML SSO URL**</span></span>

    <span data-ttu-id="26cd7-172">• hello **egyetlen Sign-Out URL-címe**</span><span class="sxs-lookup"><span data-stu-id="26cd7-172">•  hello **Single Sign-Out Service URL**</span></span>

    >[!NOTE]
    ><span data-ttu-id="26cd7-173">Ezt az alkalmazást felváltotta <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask munkahelyi</a> és olvassa el a <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">ez</a> oktatóanyag hello-alkalmazások konfigurálása az Azure AD számára.</span><span class="sxs-lookup"><span data-stu-id="26cd7-173">This application is superseded by <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask Workplace</a> and you can refer <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">this</a> tutorial for configuring hello application with Azure AD.</span></span>
   
6. <span data-ttu-id="26cd7-174">Hello a klasszikus Azure portálon, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-174">In hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>

    ![Az Azure AD-egyszeri bejelentkezés][10]

7. <span data-ttu-id="26cd7-176">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-176">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
  
    ![Az Azure AD-egyszeri bejelentkezés][11]



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="26cd7-178">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="26cd7-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="26cd7-179">hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="26cd7-179">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>  

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="26cd7-181">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="26cd7-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="26cd7-182">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-182">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="26cd7-184">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="26cd7-184">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="26cd7-185">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-185">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="26cd7-187">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-187">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="26cd7-189">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26cd7-189">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    <span data-ttu-id="26cd7-191">a.</span><span class="sxs-lookup"><span data-stu-id="26cd7-191">a.</span></span> <span data-ttu-id="26cd7-192">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="26cd7-192">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="26cd7-193">b.</span><span class="sxs-lookup"><span data-stu-id="26cd7-193">b.</span></span> <span data-ttu-id="26cd7-194">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-194">In hello User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="26cd7-195">c.</span><span class="sxs-lookup"><span data-stu-id="26cd7-195">c.</span></span> <span data-ttu-id="26cd7-196">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="26cd7-196">Click **Next**.</span></span>

6.  <span data-ttu-id="26cd7-197">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26cd7-197">On hello **User Profile** dialog page, perform hello following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    <span data-ttu-id="26cd7-199">a.</span><span class="sxs-lookup"><span data-stu-id="26cd7-199">a.</span></span> <span data-ttu-id="26cd7-200">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-200">In hello **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="26cd7-201">b.</span><span class="sxs-lookup"><span data-stu-id="26cd7-201">b.</span></span> <span data-ttu-id="26cd7-202">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-202">In hello **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="26cd7-203">c.</span><span class="sxs-lookup"><span data-stu-id="26cd7-203">c.</span></span> <span data-ttu-id="26cd7-204">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-204">In hello **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="26cd7-205">d.</span><span class="sxs-lookup"><span data-stu-id="26cd7-205">d.</span></span> <span data-ttu-id="26cd7-206">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-206">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="26cd7-207">e.</span><span class="sxs-lookup"><span data-stu-id="26cd7-207">e.</span></span> <span data-ttu-id="26cd7-208">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="26cd7-208">Click **Next**.</span></span>

7. <span data-ttu-id="26cd7-209">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-209">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="26cd7-211">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="26cd7-211">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="26cd7-213">a.</span><span class="sxs-lookup"><span data-stu-id="26cd7-213">a.</span></span> <span data-ttu-id="26cd7-214">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-214">Write down hello value of hello **New Password**.</span></span>

    <span data-ttu-id="26cd7-215">b.</span><span class="sxs-lookup"><span data-stu-id="26cd7-215">b.</span></span> <span data-ttu-id="26cd7-216">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="26cd7-216">Click **Complete**.</span></span>   



### <a name="creating-a-soonr-workplace-test-user"></a><span data-ttu-id="26cd7-217">Soonr munkahelyi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="26cd7-217">Creating a Soonr Workplace test user</span></span>

<span data-ttu-id="26cd7-218">hello ebben a szakaszban célja toocreate Soonr munkahelyi Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="26cd7-218">hello objective of this section is toocreate a user called Britta Simon in Soonr Workplace.</span></span> <span data-ttu-id="26cd7-219">Adjon együttműködve Soonr munkahelyi támogatási csapatának toocreate hello platform-felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="26cd7-219">Please work with Soonr Workplace support team toocreate a user in hello platform.</span></span> <span data-ttu-id="26cd7-220">Is növelheti a Soonr a hello támogatási jegy <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">Itt</a>.</span><span class="sxs-lookup"><span data-stu-id="26cd7-220">You can raise hello support ticket with Soonr from <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">here</a>.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="26cd7-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="26cd7-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="26cd7-222">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooSoonr munkahelyi megadásával.</span><span class="sxs-lookup"><span data-stu-id="26cd7-222">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSoonr Workplace.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="26cd7-224">**tooassign Britta Simon tooSoonr munkahelyi, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="26cd7-224">**tooassign Britta Simon tooSoonr Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="26cd7-225">A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="26cd7-225">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="26cd7-227">Hello alkalmazások listában válassza ki a **Soonr munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-227">In hello applications list, select **Soonr Workplace**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. <span data-ttu-id="26cd7-229">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-229">In hello menu on hello top, click **Users**.</span></span>

    ![Felhasználó hozzárendelése][203] 

1. <span data-ttu-id="26cd7-231">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-231">In hello Users list, select **Britta Simon**.</span></span>

2. <span data-ttu-id="26cd7-232">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="26cd7-232">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a><span data-ttu-id="26cd7-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="26cd7-234">Testing single sign-on</span></span>

<span data-ttu-id="26cd7-235">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="26cd7-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="26cd7-236">Hello Soonr munkahelyi hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Soonr munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26cd7-236">When you click hello Soonr Workplace tile in hello Access Panel, you should get automatically signed-on tooyour Soonr Workplace application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="26cd7-237">További források</span><span class="sxs-lookup"><span data-stu-id="26cd7-237">Additional resources</span></span>

* [<span data-ttu-id="26cd7-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="26cd7-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="26cd7-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="26cd7-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
