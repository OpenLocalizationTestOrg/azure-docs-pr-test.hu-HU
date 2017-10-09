---
title: "Oktatóanyag: Azure Active Directoryval integrált SuccessFactors |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SuccessFactors az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f7895d7d5e26fda27f555ae2f14a1645b50dcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a><span data-ttu-id="dfdb4-103">Oktatóanyag: Azure Active Directoryval integrált SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="dfdb4-103">Tutorial: Azure Active Directory integration with SuccessFactors</span></span>
<span data-ttu-id="dfdb4-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate SuccessFactors az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dfdb4-104">hello objective of this tutorial is tooshow you how toointegrate SuccessFactors with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dfdb4-105">SuccessFactors integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-105">Integrating SuccessFactors with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="dfdb4-106">Megadhatja a hozzáférés tooSuccessFactors rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="dfdb4-106">You can control in Azure AD who has access tooSuccessFactors</span></span>
* <span data-ttu-id="dfdb4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSuccessFactors (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="dfdb4-107">You can enable your users tooautomatically get signed-on tooSuccessFactors (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="dfdb4-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="dfdb4-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="dfdb4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dfdb4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfdb4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dfdb4-110">Prerequisites</span></span>
<span data-ttu-id="dfdb4-111">az Azure AD integrálása SuccessFactors tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-111">tooconfigure Azure AD integration with SuccessFactors, you need hello following items:</span></span>

* <span data-ttu-id="dfdb4-112">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="dfdb4-112">A valid Azure subscription</span></span>
* <span data-ttu-id="dfdb4-113">A bérlő által SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="dfdb4-113">A tenant in SuccessFactors</span></span>

> [!NOTE]
> <span data-ttu-id="dfdb4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="dfdb4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="dfdb4-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="dfdb4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dfdb4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dfdb4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dfdb4-118">Scenario description</span></span>
<span data-ttu-id="dfdb4-119">hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="dfdb4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dfdb4-121">Hello gyűjteményből SuccessFactors hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfdb4-121">Adding SuccessFactors from hello gallery</span></span>
2. <span data-ttu-id="dfdb4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dfdb4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-successfactors-from-hello-gallery"></a><span data-ttu-id="dfdb4-123">Hello gyűjteményből SuccessFactors hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfdb4-123">Adding SuccessFactors from hello gallery</span></span>
<span data-ttu-id="dfdb4-124">tooconfigure hello integrációja SuccessFactors az Azure AD-be, meg kell tooadd SuccessFactors hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-124">tooconfigure hello integration of SuccessFactors into Azure AD, you need tooadd SuccessFactors from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dfdb4-125">**tooadd SuccessFactors hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="dfdb4-125">**tooadd SuccessFactors from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="dfdb4-126">Hello hello bal oldali navigációs panelen, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-126">In hello Azure classic portal, on hello left navigation panel, click **Active Directory**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][1]
2. <span data-ttu-id="dfdb4-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="dfdb4-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][2]
4. <span data-ttu-id="dfdb4-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="dfdb4-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][4]
6. <span data-ttu-id="dfdb4-135">A hello **keresőmezőbe**, típus **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-135">In hello **search box**, type **SuccessFactors**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][5]
7. <span data-ttu-id="dfdb4-137">A hello eredmények panelen válassza ki a **SuccessFactors**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-137">In hello results panel, select **SuccessFactors**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dfdb4-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dfdb4-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dfdb4-140">hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés SuccessFactors-teszthez alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with SuccessFactors based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dfdb4-141">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SuccessFactors tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SuccessFactors tooan user in Azure AD is.</span></span> <span data-ttu-id="dfdb4-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SuccessFactors közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-142">In other words, a link relationship between an Azure AD user and hello related user in SuccessFactors needs toobe established.</span></span>

<span data-ttu-id="dfdb4-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SuccessFactors.</span></span>

<span data-ttu-id="dfdb4-144">tooconfigure és az Azure AD az egyszeri bejelentkezés SuccessFactors-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-144">tooconfigure and test Azure AD single sign-on with SuccessFactors, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="dfdb4-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="dfdb4-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dfdb4-147">**[SuccessFactors tesztfelhasználó létrehozása](#creating-a-successfactors-test-user)**  -toohave Britta Simon SuccessFactors, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-147">**[Creating a SuccessFactors test user](#creating-a-successfactors-test-user)** - toohave a counterpart of Britta Simon in SuccessFactors that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="dfdb4-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dfdb4-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dfdb4-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dfdb4-150">Configuring Azure AD single sign-on</span></span>
<span data-ttu-id="dfdb4-151">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése, és az SuccessFactors alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your SuccessFactors application.</span></span>

<span data-ttu-id="dfdb4-152">**az Azure AD tooconfigure egyszeri bejelentkezést a SuccessFactors, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="dfdb4-152">**tooconfigure Azure AD single sign-on with SuccessFactors, perform hello following steps:**</span></span>

1. <span data-ttu-id="dfdb4-153">A klasszikus Azure portálon, a hello hello **SuccessFactors** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-153">In hello Azure classic portal, on hello **SuccessFactors** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][7]
2. <span data-ttu-id="dfdb4-155">A hello **hogyan szeretné tooSuccessFactors a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-155">On hello **How would you like users toosign on tooSuccessFactors** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][8]
3. <span data-ttu-id="dfdb4-157">A hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-157">On hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][9]
   
    <span data-ttu-id="dfdb4-159">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-159">a.</span></span> <span data-ttu-id="dfdb4-160">A hello **URL-cím bejelentkezési** szövegmező, adja meg a következő minták hello egyikével URL-címe:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-160">In hello **Sign On URL** textbox, type a URL using one of hello following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    <span data-ttu-id="dfdb4-161">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-161">b.</span></span> <span data-ttu-id="dfdb4-162">A hello **válasz URL-CÍMEN** szövegmező, adja meg a következő minták hello egyikével URL-címe:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-162">In hello **Reply URL** textbox, type a URL using one of hello following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    <span data-ttu-id="dfdb4-163">c.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-163">c.</span></span> <span data-ttu-id="dfdb4-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-164">Click **Next**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="dfdb4-165">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-165">Please note that these are not hello real values.</span></span> <span data-ttu-id="dfdb4-166">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges URL-cím bejelentkezési és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-166">You have tooupdate these values with hello actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="dfdb4-167">tooget ezeket az értékeket, forduljon a [SuccessFactors támogatási csoport](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="dfdb4-167">tooget these values, contact [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

1. <span data-ttu-id="dfdb4-168">A hello **konfigurálhatja az egyszeri bejelentkezés SuccessFactors** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-168">On hello **Configure single sign-on at SuccessFactors** page, click **Download certificate**, and then save hello certificate file locally on your computer.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][10]

2. <span data-ttu-id="dfdb4-170">Egy másik webes böngészőablakban, jelentkezzen be a **SuccessFactors felügyeleti portál** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-170">In a different web browser window, log into your **SuccessFactors admin portal** as an administrator.</span></span>

3. <span data-ttu-id="dfdb4-171">Látogasson el **alkalmazásbiztonsági** és natív túl**egyszeri bejelentkezést a szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-171">Visit **Application Security** and native too**Single Sign On Feature**.</span></span> 

4. <span data-ttu-id="dfdb4-172">Bármely érték helyez hello **Token alaphelyzetbe** kattintson **Token mentése** tooenable SAML SSO.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-172">Place any value in hello **Reset Token** and click **Save Token** tooenable SAML SSO.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][11]

    > [!NOTE] 
    > <span data-ttu-id="dfdb4-174">Ez az érték csak be-és kikapcsolása kapcsoló hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-174">This value is just used as hello on/off switch.</span></span> <span data-ttu-id="dfdb4-175">Ha bármely érték menti, hello SAML SSO ON értékre állítva.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-175">If any value is saved, hello SAML SSO is ON.</span></span> <span data-ttu-id="dfdb4-176">Ha üres menti a SAML SSO hello értéke OFF.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-176">If a blank value is saved hello SAML SSO is OFF.</span></span>

1. <span data-ttu-id="dfdb4-177">Natív toobelow képernyőképe, és végezze el a következő műveletek hello.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-177">Native toobelow screenshot and perform hello following actions.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][12]
   
    <span data-ttu-id="dfdb4-179">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-179">a.</span></span> <span data-ttu-id="dfdb4-180">Jelölje be hello **SAML v2 SSO** választógomb</span><span class="sxs-lookup"><span data-stu-id="dfdb4-180">Select hello **SAML v2 SSO** Radio Button</span></span>
   
    <span data-ttu-id="dfdb4-181">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-181">b.</span></span> <span data-ttu-id="dfdb4-182">Hello SAML-fél Name(e.g. SAml issuer + company name) amely azt mutatja be.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-182">Set hello SAML Asserting Party Name(e.g. SAml issuer + company name).</span></span>
   
    <span data-ttu-id="dfdb4-183">c.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-183">c.</span></span> <span data-ttu-id="dfdb4-184">A hello **SAML kibocsátó** szövegmezőbe írja be a hello értéket **kiállítójának URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-184">In hello **SAML Issuer** textbox put hello value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="dfdb4-185">d.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-185">d.</span></span> <span data-ttu-id="dfdb4-186">Válassza ki **választ (felhasználói generált/IdP/AP)** , **kötelező kötelező aláírás**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-186">Select **Response(Customer Generated/IdP/AP)** as **Require Mandatory Signature**.</span></span>
   
    <span data-ttu-id="dfdb4-187">e.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-187">e.</span></span> <span data-ttu-id="dfdb4-188">Válassza ki **engedélyezett** , **SAML jelző engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-188">Select **Enabled** as **Enable SAML Flag**.</span></span>
   
    <span data-ttu-id="dfdb4-189">f.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-189">f.</span></span> <span data-ttu-id="dfdb4-190">Válassza ki **nem** , **bejelentkezési kérelem-aláírás (KB generált/SP/RP)**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-190">Select **No** as **Login Request Signature(SF Generated/SP/RP)**.</span></span>
   
    <span data-ttu-id="dfdb4-191">g.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-191">g.</span></span> <span data-ttu-id="dfdb4-192">Válassza ki **böngésző/utólagos profil** , **SAML profil**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-192">Select **Browser/Post Profile** as **SAML Profile**.</span></span>
   
    <span data-ttu-id="dfdb4-193">h.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-193">h.</span></span> <span data-ttu-id="dfdb4-194">Válassza ki **nem** , **kényszerítése a tanúsítvány érvényes időtartama**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-194">Select **No** as **Enforce Certificate Valid Period**.</span></span>
   
    <span data-ttu-id="dfdb4-195">i.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-195">i.</span></span> <span data-ttu-id="dfdb4-196">Másolja a letöltött tanúsítványfájl hello hello tartalmat, és illessze be hello **SAML ellenőrzése tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-196">Copy hello content of hello downloaded certificate file, and then paste it into hello **SAML Verifying Certificate** textbox.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dfdb4-197">hello tanúsítványok tartalmát kell kezdődniük tanúsítványt és a záró tanúsítvány címkék.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-197">hello certificate content must have begin certificate and end certificate tags.</span></span>

1. <span data-ttu-id="dfdb4-198">Keresse meg a tooSAML V2, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-198">Navigate tooSAML V2, and then perform hello following steps:</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][13]
   
    <span data-ttu-id="dfdb4-200">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-200">a.</span></span> <span data-ttu-id="dfdb4-201">Válassza ki **Igen** , **támogatja a Szolgáltató által kezdeményezett globális kijelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-201">Select **Yes** as **Support SP-initiated Global Logout**.</span></span>
   
    <span data-ttu-id="dfdb4-202">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-202">b.</span></span> <span data-ttu-id="dfdb4-203">A hello **globális kijelentkezési URL-címe (LogoutRequest cél)** szövegmezőbe írja be a hello értéket **távoli kijelentkezési URL-cím** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-203">In hello **Global Logout Service URL (LogoutRequest destination)** textbox put hello value of **Remote Logout URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="dfdb4-204">c.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-204">c.</span></span> <span data-ttu-id="dfdb4-205">Válassza ki **nem** , **sp titkosítani kell az összes NameID elem szükséges**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-205">Select **No** as **Require sp must encrypt all NameID element**.</span></span>
   
    <span data-ttu-id="dfdb4-206">d.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-206">d.</span></span> <span data-ttu-id="dfdb4-207">Válassza ki **meghatározatlan** , **NameID formátum**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-207">Select **unspecified** as **NameID Format**.</span></span>
   
    <span data-ttu-id="dfdb4-208">e.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-208">e.</span></span> <span data-ttu-id="dfdb4-209">Válassza ki **Igen** , **engedélyezése sp kezdeményezett (AuthnRequest) bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-209">Select **Yes** as **Enable sp initiated login (AuthnRequest)**.</span></span>
   
    <span data-ttu-id="dfdb4-210">f.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-210">f.</span></span> <span data-ttu-id="dfdb4-211">A hello **küldési kérelmek vállalati szintű kiállítóként** szövegmezőbe írja be a hello értéket **távoli bejelentkezési URL-cím** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-211">In hello **Send request as Company-Wide issuer** textbox put hello value of **Remote Login URL** from Azure AD application configuration wizard.</span></span>
2. <span data-ttu-id="dfdb4-212">Hajtsa végre ezeket a lépéseket, ha azt szeretné, hogy toomake hello bejelentkezési felhasználónevek-és nagybetűket,.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-212">Perform these steps if you want toomake hello login usernames Case Insensitive, .</span></span>
   
    <span data-ttu-id="dfdb4-213">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-213">a.</span></span> <span data-ttu-id="dfdb4-214">Látogasson el **vállalati beállítások**(közelében hello lent).</span><span class="sxs-lookup"><span data-stu-id="dfdb4-214">Visit **Company Settings**(near hello bottom).</span></span>
   
    <span data-ttu-id="dfdb4-215">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-215">b.</span></span> <span data-ttu-id="dfdb4-216">Jelölje be a jelölőnégyzetet közelében **engedélyezése nem-nagybetűk felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-216">select checkbox near **Enable Non-Case-Sensitive Username**.</span></span>
   
    <span data-ttu-id="dfdb4-217">c.Click **mentése**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-217">c.Click **Save**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][29]

    > [!NOTE] 
    > <span data-ttu-id="dfdb4-219">Ha megpróbál tooenable ez, hello rendszer ellenőrzi, ha ismétlődő SAML-bejelentkezési nevet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-219">If you try tooenable this, hello system checks if it will create a duplicate SAML login name.</span></span> <span data-ttu-id="dfdb4-220">Például ha hello vevő felhasználónevek felhasználó1, mind a felhasználó1.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-220">For example if hello customer has usernames User1 and user1.</span></span> <span data-ttu-id="dfdb4-221">Ezeket az ismétlődéseket véve számítógépnél nagybetűk teszi.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-221">Taking away case sensitivity makes these duplicates.</span></span> <span data-ttu-id="dfdb4-222">hello rendszer kap, egy hibaüzenet, és hello szolgáltatást teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-222">hello system will give you an error message and will not enable hello feature.</span></span> <span data-ttu-id="dfdb4-223">hello ügyfél kell toochange hello felhasználónevek egyik, a ténylegesen hibátlanul különböző.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-223">hello customer will need toochange one of hello usernames so it’s actually spelled different.</span></span> 

1. <span data-ttu-id="dfdb4-224">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-224">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    ![Alkalmazások][14]
2. <span data-ttu-id="dfdb4-226">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-226">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    ![Alkalmazások][15]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dfdb4-228">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dfdb4-228">Creating an Azure AD test user</span></span>
<span data-ttu-id="dfdb4-229">hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon neve a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-229">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][16]

<span data-ttu-id="dfdb4-231">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="dfdb4-231">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="dfdb4-232">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-232">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][17]
2. <span data-ttu-id="dfdb4-234">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-234">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="dfdb4-235">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-235">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][18]
4. <span data-ttu-id="dfdb4-237">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-237">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][19]
5. <span data-ttu-id="dfdb4-239">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-239">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][20]
   
    <span data-ttu-id="dfdb4-241">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-241">a.</span></span> <span data-ttu-id="dfdb4-242">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-242">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="dfdb4-243">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-243">b.</span></span> <span data-ttu-id="dfdb4-244">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-244">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="dfdb4-245">c.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-245">c.</span></span> <span data-ttu-id="dfdb4-246">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-246">Click **Next**.</span></span>
6. <span data-ttu-id="dfdb4-247">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-247">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][21]
   
    <span data-ttu-id="dfdb4-249">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-249">a.</span></span> <span data-ttu-id="dfdb4-250">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-250">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="dfdb4-251">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-251">b.</span></span> <span data-ttu-id="dfdb4-252">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-252">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="dfdb4-253">c.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-253">c.</span></span> <span data-ttu-id="dfdb4-254">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-254">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="dfdb4-255">d.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-255">d.</span></span> <span data-ttu-id="dfdb4-256">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-256">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="dfdb4-257">e.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-257">e.</span></span> <span data-ttu-id="dfdb4-258">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-258">Click **Next**.</span></span>
7. <span data-ttu-id="dfdb4-259">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-259">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][22]
8. <span data-ttu-id="dfdb4-261">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="dfdb4-261">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][23]
   
    <span data-ttu-id="dfdb4-263">a.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-263">a.</span></span> <span data-ttu-id="dfdb4-264">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-264">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="dfdb4-265">b.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-265">b.</span></span> <span data-ttu-id="dfdb4-266">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-266">Click **Complete**.</span></span>  

### <a name="creating-a-successfactors-test-user"></a><span data-ttu-id="dfdb4-267">SuccessFactors tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dfdb4-267">Creating a SuccessFactors test user</span></span>
<span data-ttu-id="dfdb4-268">A sorrend tooenable az Azure AD felhasználók toolog SuccessFactors be azok ki kell építenie SuccessFactors be.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-268">In order tooenable Azure AD users toolog into SuccessFactors, they must be provisioned into SuccessFactors.</span></span>  
<span data-ttu-id="dfdb4-269">SuccessFactors hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-269">In hello case of SuccessFactors, provisioning is a manual task.</span></span>

<span data-ttu-id="dfdb4-270">tooget felhasználó hozott létre az SuccessFactors, kell toocontact hello [SuccessFactors támogatási csoport](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="dfdb4-270">tooget users created in SuccessFactors, you need toocontact hello [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="dfdb4-271">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dfdb4-271">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="dfdb4-272">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooSuccessFactors megadásával.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-272">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSuccessFactors.</span></span>

![Felhasználó hozzárendelése][24]

<span data-ttu-id="dfdb4-274">**tooassign Britta Simon tooSuccessFactors, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="dfdb4-274">**tooassign Britta Simon tooSuccessFactors, perform hello following steps:**</span></span>

1. <span data-ttu-id="dfdb4-275">Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-275">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Felhasználó hozzárendelése][25]
2. <span data-ttu-id="dfdb4-277">Hello alkalmazások listában válassza ki a **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-277">In hello applications list, select **SuccessFactors**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][26]
3. <span data-ttu-id="dfdb4-279">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-279">In hello menu on hello top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][27]
4. <span data-ttu-id="dfdb4-281">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-281">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="dfdb4-282">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-282">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][28]

### <a name="testing-single-sign-on"></a><span data-ttu-id="dfdb4-284">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dfdb4-284">Testing single sign-on</span></span>
<span data-ttu-id="dfdb4-285">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-285">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="dfdb4-286">Hello SuccessFactors hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour SuccessFactors alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dfdb4-286">When you click hello SuccessFactors tile in hello Access Panel, you should get automatically signed-on tooyour SuccessFactors application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dfdb4-287">További források</span><span class="sxs-lookup"><span data-stu-id="dfdb4-287">Additional resources</span></span>
* [<span data-ttu-id="dfdb4-288">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="dfdb4-288">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dfdb4-289">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dfdb4-289">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
