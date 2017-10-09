---
title: "Oktatóanyag: Azure Active Directory-integráció Questetra BPM Suite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Questetra BPM Suite között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 4907e3b5751cd79f994fbd2ebcb7faec4eac34e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a><span data-ttu-id="d4a73-103">Oktatóanyag: Azure Active Directory-integráció Questetra BPM csomaggal</span><span class="sxs-lookup"><span data-stu-id="d4a73-103">Tutorial: Azure Active Directory integration with Questetra BPM Suite</span></span>
<span data-ttu-id="d4a73-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Questetra BPM Suite az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d4a73-104">hello objective of this tutorial is tooshow you how toointegrate Questetra BPM Suite with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="d4a73-105">Questetra BPM Suite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-105">Integrating Questetra BPM Suite with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="d4a73-106">Megadhatja a hozzáférés tooQuestetra BPM csomaggal rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d4a73-106">You can control in Azure AD who has access tooQuestetra BPM Suite</span></span> 
* <span data-ttu-id="d4a73-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooQuestetra BPM Suite (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="d4a73-107">You can enable your users tooautomatically get signed-on tooQuestetra BPM Suite (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="d4a73-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="d4a73-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="d4a73-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d4a73-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4a73-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d4a73-110">Prerequisites</span></span>
<span data-ttu-id="d4a73-111">tooconfigure az Azure AD-integrációs Questetra BPM csomaggal, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="d4a73-111">tooconfigure Azure AD integration with Questetra BPM Suite, you need hello following items:</span></span>

* <span data-ttu-id="d4a73-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d4a73-112">An Azure AD subscription</span></span>
* <span data-ttu-id="d4a73-113">Egy [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d4a73-113">An [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d4a73-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d4a73-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="d4a73-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d4a73-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="d4a73-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d4a73-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="d4a73-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d4a73-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="d4a73-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d4a73-118">Scenario Description</span></span>
<span data-ttu-id="d4a73-119">hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d4a73-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="d4a73-120">hello forgatókönyv ebben az oktatóanyagban leírt három fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d4a73-120">hello scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="d4a73-121">Hello gyűjteményből Questetra BPM csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d4a73-121">Adding Questetra BPM Suite from hello gallery</span></span> 
2. <span data-ttu-id="d4a73-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d4a73-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-questetra-bpm-suite-from-hello-gallery"></a><span data-ttu-id="d4a73-123">Hello gyűjteményből Questetra BPM csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d4a73-123">Adding Questetra BPM Suite from hello gallery</span></span>
<span data-ttu-id="d4a73-124">tooconfigure hello integrációja Questetra BPM Suite az Azure AD-be, meg kell tooadd Questetra BPM Suite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d4a73-124">tooconfigure hello integration of Questetra BPM Suite into Azure AD, you need tooadd Questetra BPM Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d4a73-125">**tooadd Questetra BPM Suite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d4a73-125">**tooadd Questetra BPM Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4a73-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="d4a73-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="d4a73-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="d4a73-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="d4a73-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2]

4. <span data-ttu-id="d4a73-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="d4a73-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]

5. <span data-ttu-id="d4a73-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4]

6. <span data-ttu-id="d4a73-135">Hello keresési mezőbe, írja be a **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-135">In hello search box, type **Questetra BPM Suite**.</span></span>
   
    ![Alkalmazások][5]

7. <span data-ttu-id="d4a73-137">Hello eredmények ablaktábláján jelöljön ki **Questetra BPM Suite**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d4a73-137">In hello results pane, select **Questetra BPM Suite**, and then click **Complete** tooadd hello application.</span></span>

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d4a73-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d4a73-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d4a73-139">hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Questetra BPM Suite alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d4a73-139">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with Questetra BPM Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d4a73-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Questetra BPM Suite tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d4a73-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Questetra BPM Suite tooan user in Azure AD is.</span></span> <span data-ttu-id="d4a73-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Questetra BPM csomagban található hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="d4a73-141">In other words, a link relationship between an Azure AD user and hello related user in Questetra BPM Suite needs toobe established.</span></span>  
<span data-ttu-id="d4a73-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Questetra BPM csomagban található.</span><span class="sxs-lookup"><span data-stu-id="d4a73-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Questetra BPM Suite.</span></span>

<span data-ttu-id="d4a73-143">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Questetra BPM csomaggal, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d4a73-143">tooconfigure and test Azure AD single sign-on with Questetra BPM Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d4a73-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d4a73-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d4a73-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4a73-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d4a73-146">**[Questetra BPM Suite tesztfelhasználó létrehozása](#creating-a-questetra-bpm-suite-test-user)**  -toohave Britta Simon Questetra BPM Suite, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="d4a73-146">**[Creating a Questetra BPM Suite test user](#creating-a-questetra-bpm-suite-test-user)** - toohave a counterpart of Britta Simon in Questetra BPM Suite that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="d4a73-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d4a73-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d4a73-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d4a73-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d4a73-149">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d4a73-149">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="d4a73-150">hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés a klasszikus Azure portálon hello és tooconfigure egyszeri bejelentkezést az Questetra BPM Suite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d4a73-150">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your Questetra BPM Suite application.</span></span>

<span data-ttu-id="d4a73-151">**az Azure AD az egyszeri bejelentkezés tooconfigure Questetra BPM csomaggal, hajtsa végre a hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d4a73-151">**tooconfigure Azure AD single sign-on with Questetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4a73-152">A klasszikus Azure portálon, a hello hello **Questetra BPM Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d4a73-152">In hello Azure classic portal, on hello **Questetra BPM Suite** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. <span data-ttu-id="d4a73-154">A hello **hogyan szeretné tooQuestetra BPM Suite a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-154">On hello **How would you like users toosign on tooQuestetra BPM Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. <span data-ttu-id="d4a73-156">Egy másik webes böngészőablakban, jelentkezzen be a **Questetra BPM Suite** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d4a73-156">In a different web browser window, log into your **Questetra BPM Suite** company site as an administrator.</span></span>

4. <span data-ttu-id="d4a73-157">Hello hello felső menüben kattintson a **rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-157">In hello menu on hello top, click **System Settings**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

5. <span data-ttu-id="d4a73-159">tooopen hello **SingleSignOnSAML** kattintson **egyszeri Bejelentkezést (SAML)**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-159">tooopen hello **SingleSignOnSAML** page, click **SSO (SAML)**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

6. <span data-ttu-id="d4a73-161">A klasszikus Azure portálon, a hello hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-161">In hello Azure classic portal, on hello **Configure App Settings** dialog page, perform hello following steps:</span></span> 
   
    ![Alkalmazásbeállítások konfigurálása][13]
   
    <span data-ttu-id="d4a73-163">a.</span><span class="sxs-lookup"><span data-stu-id="d4a73-163">a.</span></span> <span data-ttu-id="d4a73-164">Az Ön **Questetra BPM Suite** hello SP információk szakasza vállalati webhely, a Másolás hello **ACS URL-cím**, és illessze be hello **URL-cím bejelentkezési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-164">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **ACS URL**, and then paste it into hello **Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="d4a73-165">b.</span><span class="sxs-lookup"><span data-stu-id="d4a73-165">b.</span></span> <span data-ttu-id="d4a73-166">Az Ön **Questetra BPM Suite** hello SP információk szakasza vállalati webhely, a Másolás hello **Entitásazonosító**, és illessze be hello **kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-166">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **Entity ID**, and then paste it into hello **Issuer URL** textbox.</span></span>
   
    <span data-ttu-id="d4a73-167">c.</span><span class="sxs-lookup"><span data-stu-id="d4a73-167">c.</span></span> <span data-ttu-id="d4a73-168">Az Ön **Questetra BPM Suite** hello SP információk szakasza vállalati webhely, a Másolás hello **ACS URL-cím**, és illessze be hello **válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-168">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **ACS URL**, and then paste it into hello **Reply URL** textbox.</span></span>
   
    <span data-ttu-id="d4a73-169">d.</span><span class="sxs-lookup"><span data-stu-id="d4a73-169">d.</span></span> <span data-ttu-id="d4a73-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="d4a73-170">Click **Next**.</span></span>

7. <span data-ttu-id="d4a73-171">A hello **konfigurálhatja az egyszeri bejelentkezés Questetra BPM Suite** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="d4a73-171">On hello **Configure single sign-on at Questetra BPM Suite** page, click **Download certificate**, and then save hello certificate file locally on your computer.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][14]

8. <span data-ttu-id="d4a73-173">Az Ön **Questetra BPM Suite** a vállalati webhely, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-173">On you **Questetra BPM Suite** company site, perform hello following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][15]
   
    <span data-ttu-id="d4a73-175">a.</span><span class="sxs-lookup"><span data-stu-id="d4a73-175">a.</span></span> <span data-ttu-id="d4a73-176">Válassza ki **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-176">Select **Enable Single Sign-On**.</span></span>
   
    <span data-ttu-id="d4a73-177">b.</span><span class="sxs-lookup"><span data-stu-id="d4a73-177">b.</span></span> <span data-ttu-id="d4a73-178">A klasszikus Azure portálon hello, másolja a hello **kiállítójának URL-címe** értékét, és illessze be hello **Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-178">On hello Azure classic portal, copy hello **Issuer URL** value, and then paste it into hello **Entity ID** textbox.</span></span>
   
    <span data-ttu-id="d4a73-179">c.</span><span class="sxs-lookup"><span data-stu-id="d4a73-179">c.</span></span> <span data-ttu-id="d4a73-180">A klasszikus Azure portálon hello, másolja a hello **egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **bejelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-180">On hello Azure classic portal, copy hello **Single Sign-On Service URL** value, and then paste it into hello **Sign-in page URL** textbox.</span></span>
   
    <span data-ttu-id="d4a73-181">d.</span><span class="sxs-lookup"><span data-stu-id="d4a73-181">d.</span></span> <span data-ttu-id="d4a73-182">A klasszikus Azure portálon hello, másolja a hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **kijelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-182">On hello Azure classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Sign-out page URL** textbox.</span></span>
   
    <span data-ttu-id="d4a73-183">e.</span><span class="sxs-lookup"><span data-stu-id="d4a73-183">e.</span></span> <span data-ttu-id="d4a73-184">A hello **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-184">In hello **NameID format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="d4a73-185">f.</span><span class="sxs-lookup"><span data-stu-id="d4a73-185">f.</span></span> <span data-ttu-id="d4a73-186">Hozzon létre egy base-64 kódolású fájlt a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d4a73-186">Create a base-64 encoded file from your downloaded certificate.</span></span> 

    >[!TIP] 
    ><span data-ttu-id="d4a73-187">További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="d4a73-187">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

    <span data-ttu-id="d4a73-188">g.</span><span class="sxs-lookup"><span data-stu-id="d4a73-188">g.</span></span> <span data-ttu-id="d4a73-189">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be hello **érvényesítési tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d4a73-189">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it into hello **Validation certificate** textbox.</span></span> 

    <span data-ttu-id="d4a73-190">h.</span><span class="sxs-lookup"><span data-stu-id="d4a73-190">h.</span></span> <span data-ttu-id="d4a73-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d4a73-191">Click **Save**.</span></span>

1. <span data-ttu-id="d4a73-192">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-192">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Mi az az Azure AD Connect?][17]

2. <span data-ttu-id="d4a73-194">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-194">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Mi az az Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d4a73-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4a73-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="d4a73-197">hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d4a73-197">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="d4a73-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d4a73-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4a73-199">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-199">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][100] 

2. <span data-ttu-id="d4a73-201">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="d4a73-201">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="d4a73-202">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-202">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][101] 

4. <span data-ttu-id="d4a73-204">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-204">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása][102] 

5. <span data-ttu-id="d4a73-206">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-206">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][103]
   
    <span data-ttu-id="d4a73-208">a.</span><span class="sxs-lookup"><span data-stu-id="d4a73-208">a.</span></span> <span data-ttu-id="d4a73-209">Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-209">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="d4a73-210">b.</span><span class="sxs-lookup"><span data-stu-id="d4a73-210">b.</span></span> <span data-ttu-id="d4a73-211">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-211">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="d4a73-212">c.</span><span class="sxs-lookup"><span data-stu-id="d4a73-212">c.</span></span> <span data-ttu-id="d4a73-213">Kattintson a Next (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="d4a73-213">Click Next.</span></span>

6. <span data-ttu-id="d4a73-214">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-214">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása][104] 
   
    <span data-ttu-id="d4a73-216">a.</span><span class="sxs-lookup"><span data-stu-id="d4a73-216">a.</span></span> <span data-ttu-id="d4a73-217">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-217">In hello **First Name** textbox, type **Britta**.</span></span> 
   
    <span data-ttu-id="d4a73-218">b.</span><span class="sxs-lookup"><span data-stu-id="d4a73-218">b.</span></span> <span data-ttu-id="d4a73-219">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-219">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="d4a73-220">c.</span><span class="sxs-lookup"><span data-stu-id="d4a73-220">c.</span></span> <span data-ttu-id="d4a73-221">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-221">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="d4a73-222">d.</span><span class="sxs-lookup"><span data-stu-id="d4a73-222">d.</span></span> <span data-ttu-id="d4a73-223">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-223">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="d4a73-224">e.</span><span class="sxs-lookup"><span data-stu-id="d4a73-224">e.</span></span> <span data-ttu-id="d4a73-225">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="d4a73-225">Click **Next**.</span></span>

7. <span data-ttu-id="d4a73-226">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-226">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][105]  

8. <span data-ttu-id="d4a73-228">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-228">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][106]   
   
    <span data-ttu-id="d4a73-230">a.</span><span class="sxs-lookup"><span data-stu-id="d4a73-230">a.</span></span> <span data-ttu-id="d4a73-231">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-231">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="d4a73-232">b.</span><span class="sxs-lookup"><span data-stu-id="d4a73-232">b.</span></span> <span data-ttu-id="d4a73-233">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="d4a73-233">Click **Complete**.</span></span>   

### <a name="creating-a-questetra-bpm-suite-test-user"></a><span data-ttu-id="d4a73-234">Questetra BPM Suite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4a73-234">Creating a Questetra BPM Suite test user</span></span>
<span data-ttu-id="d4a73-235">hello ebben a szakaszban célja toocreate Britta Simon Questetra BPM Suite nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d4a73-235">hello objective of this section is toocreate a user called Britta Simon in Questetra BPM Suite.</span></span>

<span data-ttu-id="d4a73-236">**toocreate Britta Simon meghívta Questetra BPM Suite, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d4a73-236">**toocreate a user called Britta Simon in Questetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4a73-237">Bejelentkezés tooyour Questetra BPM Suite vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d4a73-237">Sign-on tooyour Questetra BPM Suite company site as an administrator.</span></span>
2. <span data-ttu-id="d4a73-238">Nyissa meg túl**Rendszerbeállítások > Felhasználólista > Új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-238">Go too**System Settings > User List > New User**.</span></span> 
3. <span data-ttu-id="d4a73-239">Hello új felhasználó párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="d4a73-239">On hello New User dialog, perform hello following steps:</span></span> 
   
    ![Tesztfelhasználó létrehozása][300] 
   
    <span data-ttu-id="d4a73-241">a.</span><span class="sxs-lookup"><span data-stu-id="d4a73-241">a.</span></span> <span data-ttu-id="d4a73-242">A hello **neve** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d4a73-242">In hello **Name** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="d4a73-243">b.</span><span class="sxs-lookup"><span data-stu-id="d4a73-243">b.</span></span> <span data-ttu-id="d4a73-244">A hello **E-mail** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d4a73-244">In hello **Email** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="d4a73-245">c.</span><span class="sxs-lookup"><span data-stu-id="d4a73-245">c.</span></span> <span data-ttu-id="d4a73-246">A hello **jelszó** szövegmező, a jelszót írja be.</span><span class="sxs-lookup"><span data-stu-id="d4a73-246">In hello **Password** textbox, type a password.</span></span>

4. <span data-ttu-id="d4a73-247">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-247">Click **Add new user**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d4a73-248">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d4a73-248">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="d4a73-249">hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooQuestetra BPM Suite megadásával.</span><span class="sxs-lookup"><span data-stu-id="d4a73-249">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooQuestetra BPM Suite.</span></span>

![Mi az az Azure AD Connect?][200]

<span data-ttu-id="d4a73-251">**tooassign Britta Simon tooQuestetra BPM Suite, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d4a73-251">**tooassign Britta Simon tooQuestetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4a73-252">A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="d4a73-252">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Mi az az Azure AD Connect?][201]
2. <span data-ttu-id="d4a73-254">Hello alkalmazások listában válassza ki a **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-254">In hello applications list, select **Questetra BPM Suite**.</span></span>
   
    ![Mi az az Azure AD Connect?][205]
3. <span data-ttu-id="d4a73-256">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-256">In hello menu on hello top, click **Users**.</span></span>
   
    ![Mi az az Azure AD Connect?][202]
4. <span data-ttu-id="d4a73-258">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-258">In hello Users list, select **Britta Simon**.</span></span>
   
    ![Mi az az Azure AD Connect?][203]
5. <span data-ttu-id="d4a73-260">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="d4a73-260">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Mi az az Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a><span data-ttu-id="d4a73-262">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d4a73-262">Testing Single Sign-On</span></span>
<span data-ttu-id="d4a73-263">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="d4a73-263">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="d4a73-264">Ha a hozzáférési Panel hello hello Questetra BPM Suite csempe gombra kattint, automatikusan bejelentkezett tooyour Questetra BPM Suite alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d4a73-264">When you click hello Questetra BPM Suite tile in hello Access Panel, you should get automatically signed-on tooyour Questetra BPM Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4a73-265">További források</span><span class="sxs-lookup"><span data-stu-id="d4a73-265">Additional Resources</span></span>
* [<span data-ttu-id="d4a73-266">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d4a73-266">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d4a73-267">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d4a73-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
