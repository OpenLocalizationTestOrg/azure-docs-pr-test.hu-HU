---
title: "Oktatóanyag: Azure Active Directoryval integrált Bynder |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Bynder között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: a2a8477580d28fe422f2836f483dff286bc71c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a><span data-ttu-id="69bea-103">Oktatóanyag: Azure Active Directoryval integrált Bynder</span><span class="sxs-lookup"><span data-stu-id="69bea-103">Tutorial: Azure Active Directory integration with Bynder</span></span>
<span data-ttu-id="69bea-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Bynder az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="69bea-104">hello objective of this tutorial is tooshow you how toointegrate Bynder with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="69bea-105">Bynder integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="69bea-105">Integrating Bynder with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="69bea-106">Megadhatja a hozzáférés tooBynder rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="69bea-106">You can control in Azure AD who has access tooBynder</span></span>
* <span data-ttu-id="69bea-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBynder egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="69bea-107">You can enable your users tooautomatically get signed-on tooBynder single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="69bea-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="69bea-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="69bea-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="69bea-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69bea-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69bea-110">Prerequisites</span></span>
<span data-ttu-id="69bea-111">az Azure AD integrálása Bynder tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="69bea-111">tooconfigure Azure AD integration with Bynder, you need hello following items:</span></span>

* <span data-ttu-id="69bea-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="69bea-112">An Azure AD subscription</span></span>
* <span data-ttu-id="69bea-113">Egy Bynder egyszeri bejelentkezést (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="69bea-113">A Bynder single-sign on (SSO) enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="69bea-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="69bea-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="69bea-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="69bea-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="69bea-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="69bea-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="69bea-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="69bea-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="69bea-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="69bea-118">Scenario description</span></span>
<span data-ttu-id="69bea-119">hello Ez az oktatóanyag célja tooenable meg tootest Microsoft Azure AD SSO tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="69bea-119">hello objective of this tutorial is tooenable you tootest Microsoft Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="69bea-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="69bea-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="69bea-121">Hello gyűjteményből Bynder hozzáadása</span><span class="sxs-lookup"><span data-stu-id="69bea-121">Adding Bynder from hello gallery</span></span>
2. <span data-ttu-id="69bea-122">A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69bea-122">Configuring and testing Microsoft Azure AD SSO</span></span>

## <a name="add-bynder-from-hello-gallery"></a><span data-ttu-id="69bea-123">Adja hozzá a Bynder hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="69bea-123">Add Bynder from hello gallery</span></span>
<span data-ttu-id="69bea-124">tooconfigure hello integrációja Bynder az Azure AD-be, meg kell tooadd Bynder hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="69bea-124">tooconfigure hello integration of Bynder into Azure AD, you need tooadd Bynder from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="69bea-125">**tooadd Bynder hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="69bea-125">**tooadd Bynder from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="69bea-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="69bea-126">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="69bea-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="69bea-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="69bea-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="69bea-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2]
4. <span data-ttu-id="69bea-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="69bea-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="69bea-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="69bea-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4]
6. <span data-ttu-id="69bea-135">Hello keresési mezőbe, írja be a **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="69bea-135">In hello search box, type **Bynder**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. <span data-ttu-id="69bea-137">A hello eredmények panelen válassza ki a **Bynder**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="69bea-137">In hello results panel, select **Bynder**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Hello katalógusában hello alkalmazás kiválasztása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a><span data-ttu-id="69bea-139">A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69bea-139">Configure and test Microsoft Azure AD SSO</span></span>
<span data-ttu-id="69bea-140">hello ebben a szakaszban célja tooshow hogyan tooconfigure és tesztelése a Microsoft Azure AD SSO Bynder alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="69bea-140">hello objective of this section is tooshow you how tooconfigure and test Microsoft Azure AD SSO with Bynder based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="69bea-141">Az SSO toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Bynder tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="69bea-141">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Bynder tooan user in Azure AD is.</span></span> <span data-ttu-id="69bea-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Bynder közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="69bea-142">In other words, a link relationship between an Azure AD user and hello related user in Bynder needs toobe established.</span></span>

<span data-ttu-id="69bea-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Bynder.</span><span class="sxs-lookup"><span data-stu-id="69bea-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Bynder.</span></span>

<span data-ttu-id="69bea-144">tooconfigure és tesztelése a Microsoft Azure AD Bynder egyszeri bejelentkezés, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="69bea-144">tooconfigure and test Microsoft Azure AD SSO with Bynder, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="69bea-145">**[Microsoft Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="69bea-145">**[Configuring Microsoft Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="69bea-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest Microsoft Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="69bea-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Microsoft Azure AD Single Sign-On with Britta Simon.</span></span>
3. <span data-ttu-id="69bea-147">**[Bynder tesztfelhasználó létrehozása](#creating-a-bynder-test-user)**  -toohave Britta Simon Bynder, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="69bea-147">**[Creating a Bynder test user](#creating-a-bynder-test-user)** - toohave a counterpart of Britta Simon in Bynder that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="69bea-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Microsoft Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="69bea-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Microsoft Azure AD Single Sign-On.</span></span>
5. <span data-ttu-id="69bea-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="69bea-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-microsoft-azure-ad-sso"></a><span data-ttu-id="69bea-150">A Microsoft Azure AD egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69bea-150">Configuring Microsoft Azure AD SSO</span></span>
<span data-ttu-id="69bea-151">Ebben a szakaszban engedélyezze a Microsoft Azure AD SSO hello klasszikus portál, és konfigurálja a SSO Bynder alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="69bea-151">In this section, you enable Microsoft Azure AD SSO in hello classic portal and configure SSO in your Bynder application.</span></span>

<span data-ttu-id="69bea-152">**tooconfigure Microsoft Azure AD SSO Bynder, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="69bea-152">**tooconfigure Microsoft Azure AD SSO with Bynder, perform hello following steps:**</span></span>

1. <span data-ttu-id="69bea-153">Hello klasszikus portál, a hello **Bynder** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="69bea-153">In hello classic portal, on hello **Bynder** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. <span data-ttu-id="69bea-155">A hello **hogyan szeretné tooBynder a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="69bea-155">On hello **How would you like users toosign on tooBynder** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. <span data-ttu-id="69bea-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="69bea-157">On hello **Configure App Settings** dialog page, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps and click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. <span data-ttu-id="69bea-159">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.getbynder.com/sso/SAML/authenticate/`</span><span class="sxs-lookup"><span data-stu-id="69bea-159">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.getbynder.com/sso/SAML/authenticate/`</span></span>
  2. <span data-ttu-id="69bea-160">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="69bea-160">Click **Next**.</span></span>
4. <span data-ttu-id="69bea-161">Ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód** a hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapra, majd kattintson a hello **"Show speciális (nem kötelező) beállítások"**és írja be a hello **URL-cím bejelentkezési** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="69bea-161">If you wish tooconfigure hello application in **SP initiated mode** on hello **Configure App Settings** dialog page, then click on hello **“Show advanced settings (optional)”** and then enter hello **Sign On URL** and click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. <span data-ttu-id="69bea-163">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.getbynder.com/login/`</span><span class="sxs-lookup"><span data-stu-id="69bea-163">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company name>.getbynder.com/login/`</span></span>
  2. <span data-ttu-id="69bea-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="69bea-164">Click **Next**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="69bea-165">hello hello URL-cím bejelentkezési ebben az oktatóanyagban értéke csak egy placeholfer.</span><span class="sxs-lookup"><span data-stu-id="69bea-165">hello value for hello Sign On URL in this tutorial is just a placeholfer.</span></span> <span data-ttu-id="69bea-166">tooget hello tényleges vlaue környezetre Bynder Forduljon.</span><span class="sxs-lookup"><span data-stu-id="69bea-166">tooget hello actual vlaue for your environment, contact Bynder.</span></span>
   >

5. <span data-ttu-id="69bea-167">A hello **konfigurálhatja az egyszeri bejelentkezés Bynder** lapon hajtsa végre az alábbi lépésekkel hello, majd kattintson **következő**:</span><span class="sxs-lookup"><span data-stu-id="69bea-167">On hello **Configure single sign-on at Bynder** page, perform hello following steps and click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. <span data-ttu-id="69bea-169">Kattintson a **metaadatok letöltése**, majd mentse hello fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="69bea-169">Click **Download metadata**, and then save hello file on your computer.</span></span>
  2. <span data-ttu-id="69bea-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="69bea-170">Click **Next**.</span></span>
6. <span data-ttu-id="69bea-171">az alkalmazáshoz konfigurált SSO tooget a Bynder támogatási csapattól.</span><span class="sxs-lookup"><span data-stu-id="69bea-171">tooget SSO configured for your application, contact your Bynder support team.</span></span> <span data-ttu-id="69bea-172">Hello letöltött metaadatfájl csatolja, és azt megosztása Bynder team tooset be egyszeri Bejelentkezést az oldalon.</span><span class="sxs-lookup"><span data-stu-id="69bea-172">Attach hello downloaded metadata file and share it with Bynder team tooset up SSO on their side.</span></span>
7. <span data-ttu-id="69bea-173">A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="69bea-173">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
8. <span data-ttu-id="69bea-175">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="69bea-175">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="69bea-177">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="69bea-177">Create an Azure AD test user</span></span>
<span data-ttu-id="69bea-178">hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon neve a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="69bea-178">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="69bea-180">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="69bea-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="69bea-181">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="69bea-181">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="69bea-183">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="69bea-183">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="69bea-184">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="69bea-184">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="69bea-186">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="69bea-186">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="69bea-188">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="69bea-188">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. <span data-ttu-id="69bea-190">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="69bea-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="69bea-191">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="69bea-191">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="69bea-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="69bea-192">Click **Next**.</span></span>
6. <span data-ttu-id="69bea-193">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="69bea-193">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="69bea-195">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="69bea-195">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="69bea-196">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="69bea-196">In hello **Last Name** textbox, type, **Simon**.</span></span> 
  3. <span data-ttu-id="69bea-197">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="69bea-197">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="69bea-198">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="69bea-198">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="69bea-199">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="69bea-199">Click **Next**.</span></span>
7. <span data-ttu-id="69bea-200">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="69bea-200">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="69bea-202">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="69bea-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. <span data-ttu-id="69bea-204">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="69bea-204">Write down hello value of hello **New Password**.</span></span>
   2. <span data-ttu-id="69bea-205">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="69bea-205">Click **Complete**.</span></span>   

### <a name="create-a-bynder-test-user"></a><span data-ttu-id="69bea-206">Bynder tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="69bea-206">Create a Bynder test user</span></span>
<span data-ttu-id="69bea-207">hello ebben a szakaszban célja toocreate Bynder Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="69bea-207">hello objective of this section is toocreate a user called Britta Simon in Bynder.</span></span> <span data-ttu-id="69bea-208">Bynder támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="69bea-208">Bynder supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="69bea-209">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="69bea-209">There is no action item for you in this section.</span></span> <span data-ttu-id="69bea-210">Új felhasználó létrejön egy kísérlet tooaccess Bynder során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="69bea-210">A new user will be created during an attempt tooaccess Bynder if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="69bea-211">Ha egy felhasználó toocreate manuálisan kell, kell toocontact hello Bynder támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="69bea-211">If you need toocreate an user manually, you need toocontact hello Bynder support team.</span></span> 
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="69bea-212">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="69bea-212">Assign hello Azure AD test user</span></span>
<span data-ttu-id="69bea-213">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure SSO saját hozzáférés tooBynder megadásával.</span><span class="sxs-lookup"><span data-stu-id="69bea-213">hello objective of this section is tooenabling Britta Simon toouse Azure SSO by granting her access tooBynder.</span></span>

   ![Felhasználó hozzárendelése][200]

<span data-ttu-id="69bea-215">**tooassign Britta Simon tooBynder, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="69bea-215">**tooassign Britta Simon tooBynder, perform hello following steps:**</span></span>

1. <span data-ttu-id="69bea-216">Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="69bea-216">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201]
2. <span data-ttu-id="69bea-218">Hello alkalmazások listában válassza ki a **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="69bea-218">In hello applications list, select **Bynder**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. <span data-ttu-id="69bea-220">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="69bea-220">In hello menu on hello top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203]
4. <span data-ttu-id="69bea-222">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="69bea-222">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="69bea-223">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="69bea-223">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="69bea-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="69bea-225">Test single sign-on</span></span>
<span data-ttu-id="69bea-226">hello ebben a szakaszban célja tootest hello a Microsoft Azure AD SSO konfiguráció segítségével a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="69bea-226">hello objective of this section is tootest your Microsoft Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="69bea-227">Ha a hozzáférési Panel hello hello Bynder csempe gombra kattint, automatikusan bejelentkezett tooyour Bynder alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="69bea-227">When you click hello Bynder tile in hello Access Panel, you should get automatically signed-on tooyour Bynder application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69bea-228">További források</span><span class="sxs-lookup"><span data-stu-id="69bea-228">Additional resources</span></span>
* [<span data-ttu-id="69bea-229">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="69bea-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="69bea-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="69bea-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
