---
title: "Oktatóanyag: Azure Active Directoryval integrált Halosys |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Halosys az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18043ed1b6f7ab45c59cfd36252bef1621618e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a><span data-ttu-id="2c18f-103">Oktatóanyag: Azure Active Directoryval integrált Halosys</span><span class="sxs-lookup"><span data-stu-id="2c18f-103">Tutorial: Azure Active Directory integration with Halosys</span></span>

<span data-ttu-id="2c18f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Halosys az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2c18f-104">In this tutorial, you learn how toointegrate Halosys with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2c18f-105">Halosys integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2c18f-105">Integrating Halosys with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2c18f-106">Megadhatja a hozzáférés tooHalosys rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2c18f-106">You can control in Azure AD who has access tooHalosys</span></span>
- <span data-ttu-id="2c18f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHalosys (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2c18f-107">You can enable your users tooautomatically get signed-on tooHalosys (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c18f-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="2c18f-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="2c18f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c18f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c18f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2c18f-110">Prerequisites</span></span>

<span data-ttu-id="2c18f-111">az Azure AD integrálása Halosys tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="2c18f-111">tooconfigure Azure AD integration with Halosys, you need hello following items:</span></span>

- <span data-ttu-id="2c18f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2c18f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2c18f-113">Egy Halosys egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="2c18f-113">A Halosys single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="2c18f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2c18f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="2c18f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2c18f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c18f-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2c18f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="2c18f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c18f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="2c18f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2c18f-118">Scenario description</span></span>
<span data-ttu-id="2c18f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2c18f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="2c18f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2c18f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c18f-121">Hello gyűjteményből Halosys hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2c18f-121">Adding Halosys from hello gallery</span></span>
2. <span data-ttu-id="2c18f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2c18f-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-halosys-from-hello-gallery"></a><span data-ttu-id="2c18f-123">Hello gyűjteményből Halosys hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2c18f-123">Adding Halosys from hello gallery</span></span>
<span data-ttu-id="2c18f-124">tooconfigure hello integrációja Halosys az Azure AD-be, meg kell tooadd Halosys hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2c18f-124">tooconfigure hello integration of Halosys into Azure AD, you need tooadd Halosys from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2c18f-125">**tooadd Halosys hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2c18f-125">**tooadd Halosys from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c18f-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]
2. <span data-ttu-id="2c18f-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="2c18f-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="2c18f-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="2c18f-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Alkalmazások][2]

4. <span data-ttu-id="2c18f-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="2c18f-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Alkalmazások][3]

5. <span data-ttu-id="2c18f-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>

    ![Alkalmazások][4]

6. <span data-ttu-id="2c18f-135">Hello keresési mezőbe, írja be a **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-135">In hello search box, type **Halosys**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. <span data-ttu-id="2c18f-137">Hello eredmények ablaktábláján jelöljön ki **Halosys**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2c18f-137">In hello results pane, select **Halosys**, and then click **Complete** tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2c18f-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2c18f-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2c18f-140">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Halosys.</span><span class="sxs-lookup"><span data-stu-id="2c18f-140">In this section, you configure and test Azure AD single sign-on with Halosys based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2c18f-141">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Halosys tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2c18f-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Halosys is tooa user in Azure AD.</span></span> <span data-ttu-id="2c18f-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Halosys közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2c18f-142">In other words, a link relationship between an Azure AD user and hello related user in Halosys needs toobe established.</span></span>

<span data-ttu-id="2c18f-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Halosys.</span><span class="sxs-lookup"><span data-stu-id="2c18f-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Halosys.</span></span>

<span data-ttu-id="2c18f-144">tooconfigure és az Azure AD az egyszeri bejelentkezés Halosys-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2c18f-144">tooconfigure and test Azure AD single sign-on with Halosys, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2c18f-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2c18f-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2c18f-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c18f-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c18f-147">**[Halosys tesztfelhasználó létrehozása](#creating-a-halosys-test-user)**  -toohave Britta Simon Halosys, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="2c18f-147">**[Creating a Halosys test user](#creating-a-halosys-test-user)** - toohave a counterpart of Britta Simon in Halosys that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="2c18f-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2c18f-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c18f-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2c18f-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2c18f-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2c18f-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2c18f-151">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése, és az Halosys alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="2c18f-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Halosys application.</span></span>


<span data-ttu-id="2c18f-152">**az Azure AD tooconfigure egyszeri bejelentkezést a Halosys, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2c18f-152">**tooconfigure Azure AD single sign-on with Halosys, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c18f-153">Hello klasszikus portál, a hello **Halosys** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2c18f-153">In hello classic portal, on hello **Halosys** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
     
    ![Egyszeri bejelentkezés konfigurálása][6] 

2. <span data-ttu-id="2c18f-155">A hello **hogyan szeretné tooHalosys a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-155">On hello **How would you like users toosign on tooHalosys** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. <span data-ttu-id="2c18f-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2c18f-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    <span data-ttu-id="2c18f-159">a.</span><span class="sxs-lookup"><span data-stu-id="2c18f-159">a.</span></span> <span data-ttu-id="2c18f-160">A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello használt URL-cím a felhasználók toosign tooyour Halosys alkalmazás hello mintát a következő használatával: `https://<company-name>.Halosys.com/client-api/api`.</span><span class="sxs-lookup"><span data-stu-id="2c18f-160">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Halosys application using hello following pattern: `https://<company-name>.Halosys.com/client-api/api`.</span></span>

    <span data-ttu-id="2c18f-161">b.In hello **azonosító URL-t** szövegmezőhöz típus hello URL-címet a következő mintát hello: `https://<company-name>.Halosys.com`.</span><span class="sxs-lookup"><span data-stu-id="2c18f-161">b.In hello **Identifier URL** textbox, type hello URL in hello following pattern: `https://<company-name>.Halosys.com`.</span></span> 
         
4. <span data-ttu-id="2c18f-162">A hello **konfigurálhatja az egyszeri bejelentkezés Halosys** lapján kattintson **metaadatok letöltése**, majd mentse hello fájlt a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="2c18f-162">On hello **Configure single sign-on at Halosys** page, click **Download metadata**, and then save hello file on your computer:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. <span data-ttu-id="2c18f-164">az alkalmazáshoz konfigurált SSO tooget Halosys támogatási csoportjához, és adja meg hello következő:</span><span class="sxs-lookup"><span data-stu-id="2c18f-164">tooget SSO configured for your application, contact Halosys support team and provide them with hello following:</span></span>

    <span data-ttu-id="2c18f-165">• hello letöltött **metaadatait tartalmazó fájl**</span><span class="sxs-lookup"><span data-stu-id="2c18f-165">• hello downloaded **metadata file**</span></span>
    
    <span data-ttu-id="2c18f-166">• hello **SAML SSO URL-címe**</span><span class="sxs-lookup"><span data-stu-id="2c18f-166">• hello **SAML SSO URL**</span></span>
    

6. <span data-ttu-id="2c18f-167">A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-167">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Az Azure AD-egyszeri bejelentkezés][10]

7. <span data-ttu-id="2c18f-169">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-169">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Az Azure AD-egyszeri bejelentkezés][11]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2c18f-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c18f-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="2c18f-172">Ebben a szakaszban a tesztfelhasználó hello Britta Simon neve a klasszikus portálon létrehozta.</span><span class="sxs-lookup"><span data-stu-id="2c18f-172">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>


![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="2c18f-174">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2c18f-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c18f-175">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-175">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="2c18f-177">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="2c18f-177">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="2c18f-178">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-178">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c18f-180">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-180">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="2c18f-182">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: ![létrehozása az Azure AD-teszt felhasználó](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span><span class="sxs-lookup"><span data-stu-id="2c18f-182">On hello **Tell us about this user** dialog page, perform hello following steps:  ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span></span> 

    <span data-ttu-id="2c18f-183">a.</span><span class="sxs-lookup"><span data-stu-id="2c18f-183">a.</span></span> <span data-ttu-id="2c18f-184">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="2c18f-184">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="2c18f-185">b.</span><span class="sxs-lookup"><span data-stu-id="2c18f-185">b.</span></span> <span data-ttu-id="2c18f-186">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-186">In hello User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2c18f-187">c.</span><span class="sxs-lookup"><span data-stu-id="2c18f-187">c.</span></span> <span data-ttu-id="2c18f-188">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c18f-188">Click **Next**.</span></span>

6.  <span data-ttu-id="2c18f-189">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: ![létrehozása az Azure AD-teszt felhasználó](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span><span class="sxs-lookup"><span data-stu-id="2c18f-189">On hello **User Profile** dialog page, perform hello following steps: ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span></span> 

    <span data-ttu-id="2c18f-190">a.</span><span class="sxs-lookup"><span data-stu-id="2c18f-190">a.</span></span> <span data-ttu-id="2c18f-191">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-191">In hello **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="2c18f-192">b.</span><span class="sxs-lookup"><span data-stu-id="2c18f-192">b.</span></span> <span data-ttu-id="2c18f-193">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-193">In hello **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="2c18f-194">c.</span><span class="sxs-lookup"><span data-stu-id="2c18f-194">c.</span></span> <span data-ttu-id="2c18f-195">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-195">In hello **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="2c18f-196">d.</span><span class="sxs-lookup"><span data-stu-id="2c18f-196">d.</span></span> <span data-ttu-id="2c18f-197">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-197">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="2c18f-198">e.</span><span class="sxs-lookup"><span data-stu-id="2c18f-198">e.</span></span> <span data-ttu-id="2c18f-199">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c18f-199">Click **Next**.</span></span>

7. <span data-ttu-id="2c18f-200">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-200">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="2c18f-202">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2c18f-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="2c18f-204">a.</span><span class="sxs-lookup"><span data-stu-id="2c18f-204">a.</span></span> <span data-ttu-id="2c18f-205">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-205">Write down hello value of hello **New Password**.</span></span>

    <span data-ttu-id="2c18f-206">b.</span><span class="sxs-lookup"><span data-stu-id="2c18f-206">b.</span></span> <span data-ttu-id="2c18f-207">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c18f-207">Click **Complete**.</span></span>   



### <a name="creating-a-halosys-test-user"></a><span data-ttu-id="2c18f-208">Halosys tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c18f-208">Creating a Halosys test user</span></span>

<span data-ttu-id="2c18f-209">Ebben a szakaszban egy Halosys Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2c18f-209">In this section, you create a user called Britta Simon in Halosys.</span></span> <span data-ttu-id="2c18f-210">Adjon együttműködve Halosys támogatási csapatának tooadd hello felhasználók hello Halosys platform.</span><span class="sxs-lookup"><span data-stu-id="2c18f-210">Please work with Halosys support team tooadd hello users in hello Halosys platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2c18f-211">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2c18f-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2c18f-212">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooHalosys megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2c18f-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooHalosys.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2c18f-214">**tooassign Britta Simon tooHalosys, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2c18f-214">**tooassign Britta Simon tooHalosys, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c18f-215">Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="2c18f-215">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2c18f-217">Hello alkalmazások listában válassza ki a **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-217">In hello applications list, select **Halosys**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. <span data-ttu-id="2c18f-219">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-219">In hello menu on hello top, click **Users**.</span></span>

    ![Felhasználó hozzárendelése][203]

4. <span data-ttu-id="2c18f-221">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-221">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="2c18f-222">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="2c18f-222">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a><span data-ttu-id="2c18f-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2c18f-224">Testing single sign-on</span></span>

<span data-ttu-id="2c18f-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2c18f-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2c18f-226">Hello Halosys hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Halosys alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2c18f-226">When you click hello Halosys tile in hello Access Panel, you should get automatically signed-on tooyour Halosys application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2c18f-227">További források</span><span class="sxs-lookup"><span data-stu-id="2c18f-227">Additional resources</span></span>

* [<span data-ttu-id="2c18f-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2c18f-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c18f-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2c18f-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
