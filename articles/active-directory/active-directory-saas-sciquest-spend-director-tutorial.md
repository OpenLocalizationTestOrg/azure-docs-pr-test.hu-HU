---
title: "Oktatóanyag: Azure Active Directoryval integrált SciQuest töltött igazgató |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SciQuest töltött igazgató között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 47c46f1297054fd96b86c1d8c66e1a55ec151497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a><span data-ttu-id="43330-103">Oktatóanyag: Azure Active Directoryval integrált SciQuest töltött igazgató</span><span class="sxs-lookup"><span data-stu-id="43330-103">Tutorial: Azure Active Directory integration with SciQuest Spend Director</span></span>
<span data-ttu-id="43330-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate SciQuest töltött igazgató az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="43330-104">hello objective of this tutorial is tooshow you how toointegrate SciQuest Spend Director with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="43330-105">SciQuest töltött igazgató integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="43330-105">Integrating SciQuest Spend Director with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="43330-106">Megadhatja a hozzáférés tooSciQuest ráfordítás igazgató rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="43330-106">You can control in Azure AD who has access tooSciQuest Spend Director</span></span> 
* <span data-ttu-id="43330-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSciQuest ráfordítás igazgató (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="43330-107">You can enable your users tooautomatically get signed-on tooSciQuest Spend Director (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="43330-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="43330-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="43330-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43330-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43330-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="43330-110">Prerequisites</span></span>
<span data-ttu-id="43330-111">tooconfigure SciQuest töltött igazgató az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="43330-111">tooconfigure Azure AD integration with SciQuest Spend Director, you need hello following items:</span></span>

* <span data-ttu-id="43330-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="43330-112">An Azure AD subscription</span></span>
* <span data-ttu-id="43330-113">Egy SciQuest töltött igazgató egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="43330-113">A SciQuest Spend Director single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="43330-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="43330-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="43330-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="43330-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="43330-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="43330-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="43330-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43330-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="43330-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="43330-118">Scenario Description</span></span>
<span data-ttu-id="43330-119">hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43330-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="43330-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="43330-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43330-121">Hello gyűjteményből SciQuest töltött igazgató hozzáadása</span><span class="sxs-lookup"><span data-stu-id="43330-121">Adding SciQuest Spend Director from hello gallery</span></span> 
2. <span data-ttu-id="43330-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="43330-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciquest-spend-director-from-hello-gallery"></a><span data-ttu-id="43330-123">Hello gyűjteményből SciQuest töltött igazgató hozzáadása</span><span class="sxs-lookup"><span data-stu-id="43330-123">Adding SciQuest Spend Director from hello gallery</span></span>
<span data-ttu-id="43330-124">tooconfigure hello integrációs SciQuest töltött igazgató, az Azure AD-be, meg kell tooadd SciQuest töltött igazgató hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="43330-124">tooconfigure hello integration of SciQuest Spend Director into Azure AD, you need tooadd SciQuest Spend Director from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="43330-125">**tooadd SciQuest töltött igazgató hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="43330-125">**tooadd SciQuest Spend Director from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="43330-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="43330-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="43330-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="43330-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="43330-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="43330-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2]

4. <span data-ttu-id="43330-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="43330-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]

5. <span data-ttu-id="43330-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="43330-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4]

6. <span data-ttu-id="43330-135">Hello keresési mezőbe, írja be a **sciQuest töltött igazgató**.</span><span class="sxs-lookup"><span data-stu-id="43330-135">In hello search box, type **sciQuest spend director**.</span></span>
   
    ![Alkalmazások][5]

7. <span data-ttu-id="43330-137">Hello eredmények ablaktábláján jelöljön ki **SciQuest töltött igazgató**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43330-137">In hello results pane, select **SciQuest Spend Director**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Alkalmazások][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="43330-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="43330-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="43330-140">hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés SciQuest töltött igazgató-teszthez alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="43330-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with SciQuest Spend Director based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="43330-141">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SciQuest töltött igazgató tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="43330-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SciQuest Spend Director tooan user in Azure AD is.</span></span> <span data-ttu-id="43330-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello SciQuest töltött Director közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="43330-142">In other words, a link relationship between an Azure AD user and hello related user in SciQuest Spend Director needs toobe established.</span></span>  
<span data-ttu-id="43330-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** SciQuest töltött Director.</span><span class="sxs-lookup"><span data-stu-id="43330-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SciQuest Spend Director.</span></span>

<span data-ttu-id="43330-144">tooconfigure és az Azure AD az egyszeri bejelentkezés SciQuest töltött igazgató-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="43330-144">tooconfigure and test Azure AD single sign-on with SciQuest Spend Director, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="43330-145">**[Az Azure AD egy egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="43330-145">**[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="43330-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43330-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="43330-147">**[SciQuest töltött igazgató tesztfelhasználó létrehozása](#creating-a-halogen-software-test-user)**  -toohave Britta Simon SciQuest töltött Director, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="43330-147">**[Creating a SciQuest Spend Director test user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in SciQuest Spend Director that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="43330-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="43330-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="43330-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="43330-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-single-sign-on"></a><span data-ttu-id="43330-150">Az Azure AD egy egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43330-150">Configuring Azure AD Single Single Sign-On</span></span>
<span data-ttu-id="43330-151">hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés a klasszikus Azure portálon hello és tooconfigure egyszeri bejelentkezést az SciQuest töltött igazgató alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="43330-151">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your SciQuest Spend Director application.</span></span>

<span data-ttu-id="43330-152">**SciQuest töltött igazgató, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="43330-152">**tooconfigure Azure AD single sign-on with SciQuest Spend Director, perform hello following steps:**</span></span>

1. <span data-ttu-id="43330-153">A klasszikus Azure portálon, a hello hello **SciQuest töltött igazgató** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43330-153">In hello Azure classic portal, on hello **SciQuest Spend Director** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. <span data-ttu-id="43330-155">A hello **hogyan szeretné tooSciQuest ráfordítás igazgató a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="43330-155">On hello **How would you like users toosign on tooSciQuest Spend Director** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. <span data-ttu-id="43330-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="43330-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span> 
   
    ![Alkalmazásbeállítások konfigurálása][10]
   
     <span data-ttu-id="43330-159">a.</span><span class="sxs-lookup"><span data-stu-id="43330-159">a.</span></span> <span data-ttu-id="43330-160">A hello **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók toosign tooyour SciQuest töltött igazgató alkalmazásra mintát a következő hello használata: *https://.* sciquest.com/.**</span><span class="sxs-lookup"><span data-stu-id="43330-160">In hello **Sign On URL** textbox, type your URL used by your users toosign on tooyour SciQuest Spend Director application using hello following pattern: *https://.*sciquest.com/.**</span></span>
   
     <span data-ttu-id="43330-161">b.</span><span class="sxs-lookup"><span data-stu-id="43330-161">b.</span></span> <span data-ttu-id="43330-162">A hello **válasz URL-CÍMEN** szövegmező, hello típusa megegyezik az értékre, amely rendelkezik hello **URL-cím bejelentkezési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="43330-162">In hello **Reply URL** textbox, type hello same value you have typed into hello **Sign On URL** textbox.</span></span> 
   
     <span data-ttu-id="43330-163">c.</span><span class="sxs-lookup"><span data-stu-id="43330-163">c.</span></span> <span data-ttu-id="43330-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="43330-164">Click **Next**.</span></span>

4. <span data-ttu-id="43330-165">A hello **konfigurálhatja az egyszeri bejelentkezés SciQuest töltött igazgató** lapján kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="43330-165">On hello **Configure single sign-on at SciQuest Spend Director** page, click **Download metadata**, and then save hello metadata file locally on your computer.</span></span>
   
    ![Mi az az Azure AD Connect?][11]

5. <span data-ttu-id="43330-167">Lépjen kapcsolatba a SciQuest támogatási tooenable a fent letöltött hello metaadatok segítségével hitelesítési módszerrel.</span><span class="sxs-lookup"><span data-stu-id="43330-167">Contact SciQuest support tooenable this authentication method using hello above downloaded metadata.</span></span>

6. <span data-ttu-id="43330-168">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43330-168">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span> 
   
    ![Mi az az Azure AD Connect?][15]

7. <span data-ttu-id="43330-170">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="43330-170">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="43330-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="43330-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="43330-172">hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="43330-172">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="43330-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="43330-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="43330-174">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="43330-174">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Mi az az Azure AD Connect?][100] 

2. <span data-ttu-id="43330-176">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="43330-176">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="43330-177">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="43330-177">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Mi az az Azure AD Connect?][101] 

4. <span data-ttu-id="43330-179">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="43330-179">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Mi az az Azure AD Connect?][102] 

5. <span data-ttu-id="43330-181">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="43330-181">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Mi az az Azure AD Connect?][103] 
   
    <span data-ttu-id="43330-183">a.</span><span class="sxs-lookup"><span data-stu-id="43330-183">a.</span></span> <span data-ttu-id="43330-184">Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="43330-184">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="43330-185">b.</span><span class="sxs-lookup"><span data-stu-id="43330-185">b.</span></span> <span data-ttu-id="43330-186">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43330-186">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="43330-187">c.</span><span class="sxs-lookup"><span data-stu-id="43330-187">c.</span></span> <span data-ttu-id="43330-188">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="43330-188">Click **Next**.</span></span>

6. <span data-ttu-id="43330-189">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="43330-189">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Mi az az Azure AD Connect?][104] 
   
    <span data-ttu-id="43330-191">a.</span><span class="sxs-lookup"><span data-stu-id="43330-191">a.</span></span> <span data-ttu-id="43330-192">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="43330-192">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="43330-193">b.</span><span class="sxs-lookup"><span data-stu-id="43330-193">b.</span></span> <span data-ttu-id="43330-194">A hello **Vezetéknév** txtbox, típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="43330-194">In hello **Last Name** txtbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="43330-195">c.</span><span class="sxs-lookup"><span data-stu-id="43330-195">c.</span></span> <span data-ttu-id="43330-196">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="43330-196">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="43330-197">d.</span><span class="sxs-lookup"><span data-stu-id="43330-197">d.</span></span> <span data-ttu-id="43330-198">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="43330-198">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="43330-199">e.</span><span class="sxs-lookup"><span data-stu-id="43330-199">e.</span></span> <span data-ttu-id="43330-200">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="43330-200">Click **Next**.</span></span>

7. <span data-ttu-id="43330-201">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="43330-201">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Mi az az Azure AD Connect?][105]  

8. <span data-ttu-id="43330-203">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="43330-203">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Mi az az Azure AD Connect?][106]   
   
    <span data-ttu-id="43330-205">a.</span><span class="sxs-lookup"><span data-stu-id="43330-205">a.</span></span> <span data-ttu-id="43330-206">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="43330-206">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="43330-207">b.</span><span class="sxs-lookup"><span data-stu-id="43330-207">b.</span></span> <span data-ttu-id="43330-208">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43330-208">Click **Complete**.</span></span>   

### <a name="creating-a-sciquest-spend-director-test-user"></a><span data-ttu-id="43330-209">SciQuest töltött igazgató tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="43330-209">Creating a SciQuest Spend Director test user</span></span>
<span data-ttu-id="43330-210">hello ebben a szakaszban célja toocreate SciQuest töltött igazgató Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="43330-210">hello objective of this section is toocreate a user called Britta Simon in SciQuest Spend Director.</span></span>

<span data-ttu-id="43330-211">Toocontact a SciQuest töltött igazgató támogatási csoportjához kell, és adja a teszt fiók tooget létrehozást kapcsolatos hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="43330-211">You need toocontact your SciQuest Spend Director support team and provide them with hello details about your test account tooget it created.</span></span>

<span data-ttu-id="43330-212">Másik lehetőségként is kihasználhatja just-in-time kiépítés, egyetlen bejelentkezés szolgáltatásként SciQuest töltött igazgató által támogatott.</span><span class="sxs-lookup"><span data-stu-id="43330-212">Alternatively, you can also leverage just-in-time provisioning, a single sign-on feature that is supported by SciQuest Spend Director.</span></span>  
<span data-ttu-id="43330-213">Ha közvetlenül az időponthoz kötött kiépítés engedélyezve van, felhasználók automatikusan létrejönnek SciQuest töltött igazgató egy egyszeri bejelentkezési kísérlet során, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="43330-213">If just-in-time provisioning is enabled, users are automatically created by SciQuest Spend Director during a single sign-on attempt if they don't exist.</span></span> <span data-ttu-id="43330-214">Ez a szolgáltatás hello szükségtelenné teszi toomanually egyszeri bejelentkezés tartozó felhasználók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="43330-214">This feature eliminates hello need toomanually create single sign-on counterpart users.</span></span>

<span data-ttu-id="43330-215">tooget just-in-time kiépítés, akkor szükséges toocontact még a SciQuest töltött igazgató támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="43330-215">tooget just-in-time provisioning enabled, you need toocontact your your SciQuest Spend Director support team.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="43330-216">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="43330-216">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="43330-217">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooSciQuest ráfordítás igazgató megadásával.</span><span class="sxs-lookup"><span data-stu-id="43330-217">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSciQuest Spend Director.</span></span>

![Mi az az Azure AD Connect?][200]

<span data-ttu-id="43330-219">**tooassign Britta Simon tooSciQuest ráfordítás igazgató, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="43330-219">**tooassign Britta Simon tooSciQuest Spend Director, perform hello following steps:**</span></span>

1. <span data-ttu-id="43330-220">A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="43330-220">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Mi az az Azure AD Connect?][201]

2. <span data-ttu-id="43330-222">Hello alkalmazások listában válassza ki a **SciQuest töltött igazgató**.</span><span class="sxs-lookup"><span data-stu-id="43330-222">In hello applications list, select **SciQuest Spend Director**.</span></span>
   
    ![Mi az az Azure AD Connect?][202]

3. <span data-ttu-id="43330-224">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="43330-224">In hello menu on hello top, click **Users**.</span></span>
   
    ![Mi az az Azure AD Connect?][203]

4. <span data-ttu-id="43330-226">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="43330-226">In hello Users list, select **Britta Simon**.</span></span>
   
    ![Mi az az Azure AD Connect?][204]

5. <span data-ttu-id="43330-228">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="43330-228">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Mi az az Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="43330-230">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="43330-230">Testing Single Sign-On</span></span>
<span data-ttu-id="43330-231">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="43330-231">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="43330-232">Ha a hozzáférési Panel hello hello SciQuest töltött igazgató csempe gombra kattint, automatikusan bejelentkezett tooyour SciQuest töltött igazgató alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="43330-232">When you click hello SciQuest Spend Director tile in hello Access Panel, you should get automatically signed-on tooyour SciQuest Spend Director application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43330-233">További források</span><span class="sxs-lookup"><span data-stu-id="43330-233">Additional Resources</span></span>
* [<span data-ttu-id="43330-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="43330-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="43330-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="43330-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

