---
title: "Oktatóanyag: Azure Active Directory-integráció Wizergos termelékenység szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a termelékenység szoftver Wizergos között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: cdd186c38c426dde404ed8bb84700faf9c8c1a46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a><span data-ttu-id="141d9-103">Oktatóanyag: Azure Active Directory-integráció Wizergos termelékenység szoftverrel</span><span class="sxs-lookup"><span data-stu-id="141d9-103">Tutorial: Azure Active Directory integration with Wizergos Productivity Software</span></span>
<span data-ttu-id="141d9-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Wizergos termelékenység szoftver az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="141d9-104">hello objective of this tutorial is tooshow you how toointegrate Wizergos Productivity Software  with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="141d9-105">Wizergos termelékenység szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="141d9-105">Integrating Wizergos Productivity Software with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="141d9-106">Megadhatja a hozzáférés tooWizergos termelékenység szoftver rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="141d9-106">You can control in Azure AD who has access tooWizergos Productivity Software</span></span>
* <span data-ttu-id="141d9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWizergos termelékenység szoftver egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="141d9-107">You can enable your users tooautomatically get signed-on tooWizergos Productivity Software single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="141d9-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="141d9-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="141d9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="141d9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="141d9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="141d9-110">Prerequisites</span></span>
<span data-ttu-id="141d9-111">tooconfigure Wizergos termelékenység szoftver az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="141d9-111">tooconfigure Azure AD integration with Wizergos Productivity Software, you need hello following items:</span></span>

* <span data-ttu-id="141d9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="141d9-112">An Azure AD subscription</span></span>
* <span data-ttu-id="141d9-113">Egy Wizergos termelékenység szoftver SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="141d9-113">A Wizergos Productivity Software SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="141d9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="141d9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="141d9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="141d9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="141d9-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="141d9-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="141d9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="141d9-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="141d9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="141d9-118">Scenario description</span></span>
<span data-ttu-id="141d9-119">hello Ez az oktatóanyag célja tooenable meg tootest Azure AD SSO tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="141d9-119">hello objective of this tutorial is tooenable you tootest Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="141d9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="141d9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="141d9-121">Hello gyűjteményből Wizergos termelékenység szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="141d9-121">Adding Wizergos Productivity Software from hello gallery</span></span>
2. <span data-ttu-id="141d9-122">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="141d9-122">Configuring and testing Azure AD SSO</span></span>

## <a name="adding-wizergos-productivity-software-from-hello-gallery"></a><span data-ttu-id="141d9-123">Hello gyűjteményből Wizergos termelékenység szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="141d9-123">Adding Wizergos Productivity Software from hello gallery</span></span>
<span data-ttu-id="141d9-124">tooconfigure hello integrációs Wizergos termelékenység szoftver az Azure AD-be, meg kell tooadd Wizergos termelékenység szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="141d9-124">tooconfigure hello integration of Wizergos Productivity Software into Azure AD, you need tooadd Wizergos Productivity Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="141d9-125">**tooadd Wizergos termelékenység szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="141d9-125">**tooadd Wizergos Productivity Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="141d9-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="141d9-126">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="141d9-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="141d9-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="141d9-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="141d9-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2]
4. <span data-ttu-id="141d9-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="141d9-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="141d9-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="141d9-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4]
6. <span data-ttu-id="141d9-135">Hello keresési mezőbe, írja be a **Wizergos termelékenység szoftver**.</span><span class="sxs-lookup"><span data-stu-id="141d9-135">In hello search box, type **Wizergos Productivity Software**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. <span data-ttu-id="141d9-137">A hello eredmények panelen válassza ki a **Wizergos termelékenység szoftver**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="141d9-137">In hello results panel, select **Wizergos Productivity Software**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Hello katalógusában hello alkalmazás kiválasztása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a><span data-ttu-id="141d9-139">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="141d9-139">Configure and test Azure AD SSO</span></span>
<span data-ttu-id="141d9-140">hello ebben a szakaszban célja tooshow hogyan tooconfigure és tesztelési Azure AD SSO Wizergos termelékenység szoftverrel alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="141d9-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD SSO with Wizergos Productivity Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="141d9-141">Az SSO toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Wizergos termelékenység szoftver tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="141d9-141">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Wizergos Productivity Software tooan user in Azure AD is.</span></span> <span data-ttu-id="141d9-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Wizergos termelékenység szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="141d9-142">In other words, a link relationship between an Azure AD user and hello related user in Wizergos Productivity Software needs toobe established.</span></span>

<span data-ttu-id="141d9-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Wizergos termelékenység szoftverben.</span><span class="sxs-lookup"><span data-stu-id="141d9-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wizergos Productivity Software.</span></span>

<span data-ttu-id="141d9-144">tooconfigure és az Azure AD az egyszeri bejelentkezés BynWizergos termelékenység Softwareder-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="141d9-144">tooconfigure and test Azure AD single sign-on with BynWizergos Productivity Softwareder, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="141d9-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="141d9-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="141d9-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="141d9-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="141d9-147">**[Wizergos termelékenység szoftver tesztfelhasználó létrehozása](#creating-a-wizergos-productivity-software-test-user)**  -toohave Britta Simon Wizergos termelékenység szoftver, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="141d9-147">**[Creating a Wizergos Productivity Software test user](#creating-a-wizergos-productivity-software-test-user)** - toohave a counterpart of Britta Simon in Wizergos Productivity Software that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="141d9-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="141d9-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="141d9-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="141d9-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="141d9-150">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="141d9-150">Configuring Azure AD SSO</span></span>
<span data-ttu-id="141d9-151">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése, és az Wizergos termelékenység szoftver alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="141d9-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Wizergos Productivity Software application.</span></span>

<span data-ttu-id="141d9-152">**Wizergos termelékenység szoftvert, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="141d9-152">**tooconfigure Azure AD single sign-on with Wizergos Productivity Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="141d9-153">Hello klasszikus portál, a hello **Wizergos termelékenység szoftver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="141d9-153">In hello classic portal, on hello **Wizergos Productivity Software** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. <span data-ttu-id="141d9-155">A hello **hogyan szeretné tooWizergos termelékenység szoftvert a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="141d9-155">On hello **How would you like users toosign on tooWizergos Productivity Software** page, select **Azure AD Single Sign-On**, and then click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. <span data-ttu-id="141d9-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lap, kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="141d9-157">On hello **Configure App Settings** dialog page, click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. <span data-ttu-id="141d9-159">A hello **konfigurálhatja az egyszeri bejelentkezés Wizergos termelékenység szoftver** lapján kattintson **tanúsítvánnyal letöltés**, majd mentse hello fájlt a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="141d9-159">On hello **Configure single sign-on at Wizergos Productivity Software** page, click **Download certificate**, and then save hello file on your computer:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. <span data-ttu-id="141d9-161">Egy másik webes böngészőablakban, bejelentkezés tooyour Wizergos termelékenység szoftver Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="141d9-161">In a different web browser window, sign-on tooyour Wizergos Productivity Software tenant as an administrator.</span></span>
6. <span data-ttu-id="141d9-162">Hello Hamburg menüben válassza ki a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="141d9-162">From hello hamburger menu, select **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. <span data-ttu-id="141d9-164">A rendszergazda lap bal oldali menüben válassza ki **hitelesítési** , majd kattintson a **az Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="141d9-164">In Admin page on left hand menu select **AUTHENTICATION** and click on **Azure AD**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. <span data-ttu-id="141d9-166">Hajtsa végre a következő lépéseket hello **hitelesítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="141d9-166">Perform hello following steps on **AUTHENTICATION** section.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. <span data-ttu-id="141d9-168">Kattintson a **FELTÖLTÉSE** gomb tooupload hello letöltött tanúsítvány az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="141d9-168">Click **UPLOAD** button tooupload hello downloaded certificate from Azure AD.</span></span> 
  2. <span data-ttu-id="141d9-169">A hello **kiállítójának URL-címe** szövegmezőbe írja be a hello értéket **kiállítójának URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="141d9-169">In hello **Issuer URL** textbox put hello value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
  3. <span data-ttu-id="141d9-170">A hello **egyszeri bejelentkezési URL-cím** szövegmezőbe írja be a hello értéket **egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="141d9-170">In hello **Single Sign-On URL** textbox put hello value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
  4. <span data-ttu-id="141d9-171">A hello **egyetlen Sign-Out URL-címet** szövegmezőbe írja be a hello értéket **egyetlen Sign-out URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="141d9-171">In hello **Single Sign-Out URL** textbox put hello value of **Single Sign-out Service URL** from Azure AD application configuration wizard.</span></span>
  5. <span data-ttu-id="141d9-172">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="141d9-172">Click **Save** button.</span></span>
9. <span data-ttu-id="141d9-173">A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="141d9-173">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
10. <span data-ttu-id="141d9-175">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="141d9-175">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
    
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="141d9-177">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="141d9-177">Create an Azure AD test user</span></span>
<span data-ttu-id="141d9-178">hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon neve a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="141d9-178">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="141d9-180">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="141d9-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="141d9-181">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="141d9-181">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="141d9-183">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="141d9-183">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="141d9-184">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="141d9-184">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="141d9-186">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="141d9-186">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="141d9-188">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="141d9-188">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="141d9-190">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="141d9-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="141d9-191">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="141d9-191">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="141d9-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="141d9-192">Click **Next**.</span></span>
6. <span data-ttu-id="141d9-193">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="141d9-193">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="141d9-195">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="141d9-195">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="141d9-196">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="141d9-196">In hello **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="141d9-197">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="141d9-197">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="141d9-198">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="141d9-198">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="141d9-199">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="141d9-199">Click **Next**.</span></span>
7. <span data-ttu-id="141d9-200">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="141d9-200">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="141d9-202">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="141d9-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. <span data-ttu-id="141d9-204">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="141d9-204">Write down hello value of hello **New Password**.</span></span>
  2. <span data-ttu-id="141d9-205">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="141d9-205">Click **Complete**.</span></span>   

### <a name="create-a-wizergos-productivity-software-test-user"></a><span data-ttu-id="141d9-206">Wizergos termelékenység szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="141d9-206">Create a Wizergos Productivity Software test user</span></span>
<span data-ttu-id="141d9-207">Ebben a szakaszban egy felhasználó Britta Simon meghívta Wizergos termelékenység szoftver hoz létre.</span><span class="sxs-lookup"><span data-stu-id="141d9-207">In this section, you create a user called Britta Simon in Wizergos Productivity Software.</span></span> <span data-ttu-id="141d9-208">Adjon Wizergos termelékenység szoftver támogatási csoport keresztül együttműködésre [ support@wizergos.com ](emailTo:support@wizergos.com) tooadd hello felhasználók hello Wizergos termelékenység szoftver platform.</span><span class="sxs-lookup"><span data-stu-id="141d9-208">Please work with Wizergos Productivity Software support team via [support@wizergos.com](emailTo:support@wizergos.com) tooadd hello users in hello Wizergos Productivity Software platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="141d9-209">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="141d9-209">Assign hello Azure AD test user</span></span>
<span data-ttu-id="141d9-210">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure SSO saját hozzáférés tooWizergos termelékenység szoftver megadásával.</span><span class="sxs-lookup"><span data-stu-id="141d9-210">hello objective of this section is tooenabling Britta Simon toouse Azure SSO by granting her access tooWizergos Productivity Software.</span></span>

  ![Felhasználó hozzárendelése][200]

<span data-ttu-id="141d9-212">**tooassign Britta Simon tooWizergos termelékenység szoftver, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="141d9-212">**tooassign Britta Simon tooWizergos Productivity Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="141d9-213">Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="141d9-213">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201]
2. <span data-ttu-id="141d9-215">Hello alkalmazások listában válassza ki a **Wizergos termelékenység szoftver**.</span><span class="sxs-lookup"><span data-stu-id="141d9-215">In hello applications list, select **Wizergos Productivity Software**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. <span data-ttu-id="141d9-217">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="141d9-217">In hello menu on hello top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203]
4. <span data-ttu-id="141d9-219">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="141d9-219">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="141d9-220">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="141d9-220">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="141d9-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="141d9-222">Test single sign-on</span></span>
<span data-ttu-id="141d9-223">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="141d9-223">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="141d9-224">Ha a hozzáférési Panel hello hello Wizergos termelékenység szoftver csempe gombra kattint, automatikusan bejelentkezett tooyour Wizergos termelékenység alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="141d9-224">When you click hello Wizergos Productivity Software tile in hello Access Panel, you should get automatically signed-on tooyour Wizergos Productivity Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="141d9-225">További források</span><span class="sxs-lookup"><span data-stu-id="141d9-225">Additional resources</span></span>
* [<span data-ttu-id="141d9-226">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="141d9-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="141d9-227">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="141d9-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
