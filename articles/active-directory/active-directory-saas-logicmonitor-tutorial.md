---
title: "Oktatóanyag: Azure Active Directoryval integrált LogicMonitor |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LogicMonitor között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: ea5cb8b574d763cb114286e3b2a5c94ab5546756
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a><span data-ttu-id="7a652-103">Oktatóanyag: Azure Active Directoryval integrált LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="7a652-103">Tutorial: Azure Active Directory integration with LogicMonitor</span></span>

<span data-ttu-id="7a652-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LogicMonitor az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7a652-104">In this tutorial, you learn how toointegrate LogicMonitor with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7a652-105">LogicMonitor integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7a652-105">Integrating LogicMonitor with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7a652-106">Megadhatja a hozzáférés tooLogicMonitor rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7a652-106">You can control in Azure AD who has access tooLogicMonitor</span></span>
- <span data-ttu-id="7a652-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLogicMonitor (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7a652-107">You can enable your users tooautomatically get signed-on tooLogicMonitor (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7a652-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7a652-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7a652-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7a652-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a652-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7a652-110">Prerequisites</span></span>

<span data-ttu-id="7a652-111">az Azure AD integrálása LogicMonitor tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7a652-111">tooconfigure Azure AD integration with LogicMonitor, you need hello following items:</span></span>

- <span data-ttu-id="7a652-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7a652-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7a652-113">Egy LogicMonitor egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7a652-113">A LogicMonitor single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7a652-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7a652-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7a652-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7a652-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7a652-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7a652-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7a652-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a652-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7a652-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7a652-118">Scenario description</span></span>
<span data-ttu-id="7a652-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7a652-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7a652-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7a652-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7a652-121">Hello gyűjteményből LogicMonitor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a652-121">Adding LogicMonitor from hello gallery</span></span>
2. <span data-ttu-id="7a652-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7a652-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-logicmonitor-from-hello-gallery"></a><span data-ttu-id="7a652-123">Hello gyűjteményből LogicMonitor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7a652-123">Adding LogicMonitor from hello gallery</span></span>
<span data-ttu-id="7a652-124">tooconfigure hello integrációja LogicMonitor az Azure AD-be, meg kell tooadd LogicMonitor hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7a652-124">tooconfigure hello integration of LogicMonitor into Azure AD, you need tooadd LogicMonitor from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7a652-125">**tooadd LogicMonitor hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7a652-125">**tooadd LogicMonitor from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a652-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7a652-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7a652-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7a652-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7a652-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7a652-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7a652-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7a652-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7a652-133">Hello keresési mezőbe, írja be a **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="7a652-133">In hello search box, type **LogicMonitor**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. <span data-ttu-id="7a652-135">A hello eredmények panelen válassza ki a **LogicMonitor**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7a652-135">In hello results panel, select **LogicMonitor**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7a652-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7a652-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7a652-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="7a652-138">In this section, you configure and test Azure AD single sign-on with LogicMonitor based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7a652-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LogicMonitor tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7a652-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LogicMonitor is tooa user in Azure AD.</span></span> <span data-ttu-id="7a652-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LogicMonitor közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7a652-140">In other words, a link relationship between an Azure AD user and hello related user in LogicMonitor needs toobe established.</span></span>

<span data-ttu-id="7a652-141">LogicMonitor, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7a652-141">In LogicMonitor, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7a652-142">tooconfigure és az Azure AD az egyszeri bejelentkezés LogicMonitor-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7a652-142">tooconfigure and test Azure AD single sign-on with LogicMonitor, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7a652-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7a652-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7a652-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a652-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7a652-145">**[LogicMonitor tesztfelhasználó létrehozása](#creating-a-logicmonitor-test-user)**  -toohave egy megfelelője a Britta Simon a LogicMonitor, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7a652-145">**[Creating a LogicMonitor test user](#creating-a-logicmonitor-test-user)** - toohave a counterpart of Britta Simon in LogicMonitor that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7a652-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7a652-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7a652-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7a652-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7a652-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a652-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7a652-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az LogicMonitor alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7a652-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LogicMonitor application.</span></span>

<span data-ttu-id="7a652-150">**az Azure AD tooconfigure egyszeri bejelentkezést a LogicMonitor, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7a652-150">**tooconfigure Azure AD single sign-on with LogicMonitor, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a652-151">Az Azure portál, a hello hello **LogicMonitor** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7a652-151">In hello Azure portal, on hello **LogicMonitor** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7a652-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7a652-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. <span data-ttu-id="7a652-155">A hello **LogicMonitor tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a652-155">On hello **LogicMonitor Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    <span data-ttu-id="7a652-157">a.</span><span class="sxs-lookup"><span data-stu-id="7a652-157">a.</span></span> <span data-ttu-id="7a652-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="7a652-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    <span data-ttu-id="7a652-159">b.</span><span class="sxs-lookup"><span data-stu-id="7a652-159">b.</span></span> <span data-ttu-id="7a652-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="7a652-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7a652-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7a652-161">These values are not real.</span></span> <span data-ttu-id="7a652-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="7a652-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7a652-163">Ügyfél [LogicMonitor ügyfél-támogatási csoport](https://www.logicmonitor.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7a652-163">Contact [LogicMonitor Client support team](https://www.logicmonitor.com/contact/) tooget these values.</span></span> 
 


4. <span data-ttu-id="7a652-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7a652-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. <span data-ttu-id="7a652-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a652-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7a652-168">Jelentkezzen be tooyour **LogicMonitor** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7a652-168">Log in tooyour **LogicMonitor** company site as an administrator.</span></span>

7. <span data-ttu-id="7a652-169">Hello hello felső menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7a652-169">In hello menu on hello top, click **Settings**.</span></span>
   
   <span data-ttu-id="7a652-170">![Beállítások](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="7a652-170">![Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "Settings")</span></span>

8. <span data-ttu-id="7a652-171">Hello navigációs bat hello bal oldalon, kattintson **egyszeri bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="7a652-171">In hello navigation bat on hello left side, click **Single Sign On**</span></span>
   
   <span data-ttu-id="7a652-172">![Egyszeri bejelentkezés](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="7a652-172">![Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "Single Sign-On")</span></span>

9. <span data-ttu-id="7a652-173">A hello **egyszeri bejelentkezés (SSO) beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a652-173">In hello **Single Sign-on (SSO) settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="7a652-174">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="7a652-174">![Single Sign-On Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="7a652-175">a.</span><span class="sxs-lookup"><span data-stu-id="7a652-175">a.</span></span> <span data-ttu-id="7a652-176">Válassza ki **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="7a652-176">Select **Enable Single Sign-on**.</span></span>

   <span data-ttu-id="7a652-177">b.</span><span class="sxs-lookup"><span data-stu-id="7a652-177">b.</span></span> <span data-ttu-id="7a652-178">Mint **alapértelmezett szerepkör-hozzárendelés**, jelölje be **readonly**.</span><span class="sxs-lookup"><span data-stu-id="7a652-178">As **Default Role Assignment**, select **readonly**.</span></span>
   
   <span data-ttu-id="7a652-179">c.</span><span class="sxs-lookup"><span data-stu-id="7a652-179">c.</span></span> <span data-ttu-id="7a652-180">Nyissa meg a letöltött hello metaadat-fájlt a Jegyzettömbben, és illessze be hello hello fájl tartalma **Identity Provider metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7a652-180">Open hello downloaded metadata file in notepad, and then paste content of hello file into hello **Identity Provider Metadata** textbox.</span></span>
   
   <span data-ttu-id="7a652-181">d.</span><span class="sxs-lookup"><span data-stu-id="7a652-181">d.</span></span> <span data-ttu-id="7a652-182">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="7a652-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="7a652-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7a652-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7a652-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7a652-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7a652-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7a652-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7a652-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a652-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="7a652-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7a652-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7a652-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7a652-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a652-190">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7a652-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7a652-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7a652-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7a652-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a652-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7a652-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a652-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7a652-198">a.</span><span class="sxs-lookup"><span data-stu-id="7a652-198">a.</span></span> <span data-ttu-id="7a652-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7a652-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7a652-200">b.</span><span class="sxs-lookup"><span data-stu-id="7a652-200">b.</span></span> <span data-ttu-id="7a652-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7a652-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7a652-202">c.</span><span class="sxs-lookup"><span data-stu-id="7a652-202">c.</span></span> <span data-ttu-id="7a652-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7a652-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7a652-204">d.</span><span class="sxs-lookup"><span data-stu-id="7a652-204">d.</span></span> <span data-ttu-id="7a652-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7a652-205">Click **Create**.</span></span>
 
### <a name="creating-a-logicmonitor-test-user"></a><span data-ttu-id="7a652-206">LogicMonitor tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a652-206">Creating a LogicMonitor test user</span></span>

<span data-ttu-id="7a652-207">Az aad-ben felhasználók toobe képes toosign a kiosztott toohello LogicMonitor alkalmazást az Azure Active Directory felhasználói neveket kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="7a652-207">For AAD users toobe able toosign in, they must be provisioned toohello LogicMonitor application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="7a652-208">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7a652-208">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a652-209">Jelentkezzen be tooyour LogicMonitor vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7a652-209">Log in tooyour LogicMonitor company site as an administrator.</span></span>

2. <span data-ttu-id="7a652-210">Hello hello felső menüben kattintson a **beállítások**, és kattintson a **szerepkörök és a felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="7a652-210">In hello menu on hello top, click **Settings**, and then click **Roles and Users**.</span></span>
   
   <span data-ttu-id="7a652-211">![Szerepkörök és a felhasználók](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "szerepkörök és a felhasználók")</span><span class="sxs-lookup"><span data-stu-id="7a652-211">![Roles and Users](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "Roles and Users")</span></span>

3. <span data-ttu-id="7a652-212">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="7a652-212">Click **Add**.</span></span>

4. <span data-ttu-id="7a652-213">A hello **vegyen fel egy fiókot** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7a652-213">In hello **Add an account** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="7a652-214">![Vegyen fel egy fiókot](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "fiók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="7a652-214">![Add an account](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Add an account")</span></span>
   
   <span data-ttu-id="7a652-215">a.</span><span class="sxs-lookup"><span data-stu-id="7a652-215">a.</span></span> <span data-ttu-id="7a652-216">Típus hello **felhasználónév**, **E-mail**, **jelszó**, és **írja be újra jelszó** hello Azure Active Directory-felhasználók kívánt értékeket a hello tooprovision kapcsolatos szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="7a652-216">Type hello **Username**, **Email**, **Password**, and **Retype password** values of hello Azure Active Directory user you want tooprovision into hello related textboxes.</span></span>
   
   <span data-ttu-id="7a652-217">b.</span><span class="sxs-lookup"><span data-stu-id="7a652-217">b.</span></span> <span data-ttu-id="7a652-218">Válassza ki **szerepkörök**, **engedélyek megtekintése**, és hello **állapot**.</span><span class="sxs-lookup"><span data-stu-id="7a652-218">Select **Roles**, **View Permissions**, and hello **Status**.</span></span>
   
   <span data-ttu-id="7a652-219">c.</span><span class="sxs-lookup"><span data-stu-id="7a652-219">c.</span></span> <span data-ttu-id="7a652-220">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="7a652-220">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="7a652-221">Bármely más LogicMonitor felhasználói fiók létrehozása eszközök vagy LogicMonitor tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="7a652-221">You can use any other LogicMonitor user account creation tools or APIs provided by LogicMonitor tooprovision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7a652-222">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7a652-222">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7a652-223">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLogicMonitor megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7a652-223">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLogicMonitor.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7a652-225">**tooassign Britta Simon tooLogicMonitor, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7a652-225">**tooassign Britta Simon tooLogicMonitor, perform hello following steps:**</span></span>

1. <span data-ttu-id="7a652-226">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7a652-226">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7a652-228">Hello alkalmazások listában válassza ki a **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="7a652-228">In hello applications list, select **LogicMonitor**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. <span data-ttu-id="7a652-230">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7a652-230">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7a652-232">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a652-232">Click **Add** button.</span></span> <span data-ttu-id="7a652-233">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a652-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7a652-235">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7a652-235">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7a652-236">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a652-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7a652-237">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a652-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7a652-238">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7a652-238">Testing single sign-on</span></span>

<span data-ttu-id="7a652-239">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="7a652-239">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="7a652-240">Ha a hozzáférési Panel hello hello LogicMonitor csempe gombra kattint, automatikusan bejelentkezett tooyour LogicMonitor alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7a652-240">When you click hello LogicMonitor tile in hello Access Panel, you should get automatically signed-on tooyour LogicMonitor application.</span></span>
<span data-ttu-id="7a652-241">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a652-241">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7a652-242">További források</span><span class="sxs-lookup"><span data-stu-id="7a652-242">Additional resources</span></span>

* [<span data-ttu-id="7a652-243">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7a652-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7a652-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7a652-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

