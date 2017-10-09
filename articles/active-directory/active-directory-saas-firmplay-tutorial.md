---
title: "Oktatóanyag: Azure Active Directoryval integrált FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FirmPlay - a személyzeti osztályon dolgozó tanácsadáson között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f143e0bb8f2a42de880d77e5f033694ce3f09cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a><span data-ttu-id="814c4-103">Oktatóanyag: Azure Active Directoryval integrált FirmPlay - a személyzeti osztályon dolgozó tanácsadáson</span><span class="sxs-lookup"><span data-stu-id="814c4-103">Tutorial: Azure Active Directory integration with FirmPlay - Employee Advocacy for Recruiting</span></span>

<span data-ttu-id="814c4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="814c4-104">In this tutorial, you learn how toointegrate FirmPlay - Employee Advocacy for Recruiting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="814c4-105">FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="814c4-105">Integrating FirmPlay - Employee Advocacy for Recruiting with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="814c4-106">Megadhatja a hozzáférés tooFirmPlay - a személyzeti osztályon dolgozó tanácsadáson rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="814c4-106">You can control in Azure AD who has access tooFirmPlay - Employee Advocacy for Recruiting</span></span>
- <span data-ttu-id="814c4-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFirmPlay - alkalmazott tanácsadáson a személyzeti osztályon (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="814c4-107">You can enable your users tooautomatically get signed-on tooFirmPlay - Employee Advocacy for Recruiting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="814c4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="814c4-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="814c4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="814c4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="814c4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="814c4-110">Prerequisites</span></span>

<span data-ttu-id="814c4-111">a következő elemek hello kell tooconfigure FirmPlay - alkalmazott tanácsadáson személyzeti osztályon, az Azure AD integrálása:</span><span class="sxs-lookup"><span data-stu-id="814c4-111">tooconfigure Azure AD integration with FirmPlay - Employee Advocacy for Recruiting, you need hello following items:</span></span>

- <span data-ttu-id="814c4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="814c4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="814c4-113">Egy FirmPlay - alkalmazott tanácsadáson felvételi egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="814c4-113">A FirmPlay - Employee Advocacy for Recruiting single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="814c4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="814c4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="814c4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="814c4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="814c4-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="814c4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="814c4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="814c4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="814c4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="814c4-118">Scenario description</span></span>
<span data-ttu-id="814c4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="814c4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="814c4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="814c4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="814c4-121">Hozzáadás FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="814c4-121">Adding FirmPlay - Employee Advocacy for Recruiting from hello gallery</span></span>
2. <span data-ttu-id="814c4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="814c4-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-hello-gallery"></a><span data-ttu-id="814c4-123">Hozzáadás FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="814c4-123">Adding FirmPlay - Employee Advocacy for Recruiting from hello gallery</span></span>
<span data-ttu-id="814c4-124">tooconfigure hello integrációja FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure AD-be kell tooadd FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="814c4-124">tooconfigure hello integration of FirmPlay - Employee Advocacy for Recruiting into Azure AD, you need tooadd FirmPlay - Employee Advocacy for Recruiting from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="814c4-125">**tooadd FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="814c4-125">**tooadd FirmPlay - Employee Advocacy for Recruiting from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="814c4-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="814c4-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="814c4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="814c4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="814c4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="814c4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="814c4-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="814c4-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="814c4-133">Hello keresési mezőbe, írja be a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**.</span><span class="sxs-lookup"><span data-stu-id="814c4-133">In hello search box, type **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. <span data-ttu-id="814c4-135">A hello eredmények panelen válassza a **FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="814c4-135">In hello results panel, select **FirmPlay - Employee Advocacy for Recruiting**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="814c4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="814c4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="814c4-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="814c4-138">In this section, you configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="814c4-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="814c4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FirmPlay - Employee Advocacy for Recruiting is tooa user in Azure AD.</span></span> <span data-ttu-id="814c4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FirmPlay - a személyzeti osztályon dolgozó tanácsadáson közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="814c4-140">In other words, a link relationship between an Azure AD user and hello related user in FirmPlay - Employee Advocacy for Recruiting needs toobe established.</span></span>

<span data-ttu-id="814c4-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon.</span><span class="sxs-lookup"><span data-stu-id="814c4-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FirmPlay - Employee Advocacy for Recruiting.</span></span>

<span data-ttu-id="814c4-142">tooconfigure és FirmPlay - alkalmazott tanácsadáson személyzeti osztályon, az Azure AD az egyszeri bejelentkezés teszthez van szüksége a következő építőelemeket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="814c4-142">tooconfigure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="814c4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="814c4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="814c4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="814c4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="814c4-145">**[Egy FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tesztfelhasználó létrehozása](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  -toohave egy megfelelője a Britta Simon a FirmPlay: alkalmazott tanácsadáson felvételi, amely csatolva az Azure AD toohello ábrázolását rá, hogy.</span><span class="sxs-lookup"><span data-stu-id="814c4-145">**[Creating a FirmPlay - Employee Advocacy for Recruiting test user](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)** - toohave a counterpart of Britta Simon in FirmPlay: Employee Advocacy for Recruiting that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="814c4-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="814c4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="814c4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="814c4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="814c4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="814c4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="814c4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a FirmPlay - alkalmazott tanácsadáson személyzeti osztályon alkalmazáshoz az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="814c4-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FirmPlay - Employee Advocacy for Recruiting application.</span></span>

<span data-ttu-id="814c4-150">**FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="814c4-150">**tooconfigure Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, perform hello following steps:**</span></span>

1. <span data-ttu-id="814c4-151">Hello Azure felügyeleti portálon, a hello **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="814c4-151">In hello Azure Management portal, on hello **FirmPlay - Employee Advocacy for Recruiting** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="814c4-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="814c4-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. <span data-ttu-id="814c4-155">A hello **FirmPlay - tartomány felvétele és URL-címek alkalmazott tanácsadáson** című hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<your-subdomain>.firmplay.com/`</span><span class="sxs-lookup"><span data-stu-id="814c4-155">On hello **FirmPlay - Employee Advocacy for Recruiting Domain and URLs** section, in hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<your-subdomain>.firmplay.com/`</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > <span data-ttu-id="814c4-157">Ne feledje, hogy ez a nem hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="814c4-157">Please note that this is not hello real value.</span></span> <span data-ttu-id="814c4-158">Ezt az értéket hello tényleges bejelentkezési URL-cím tooupdate rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="814c4-158">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="814c4-159">Ügyfél [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="814c4-159">Contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) tooget this value.</span></span> 

4. <span data-ttu-id="814c4-160">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="814c4-160">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. <span data-ttu-id="814c4-162">A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="814c4-162">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="814c4-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="814c4-163">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="814c4-165">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="814c4-165">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. <span data-ttu-id="814c4-167">A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="814c4-167">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="814c4-169">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="814c4-169">On hello **SAML Signing Certificate** section, click **Certificate (base64)** and then save hello certificate file on your computer.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. <span data-ttu-id="814c4-171">A hello **FirmPlay - konfiguráció felvételi alkalmazott tanácsadáson** területén kattintson **FirmPlay konfigurálása - a személyzeti osztályon dolgozó tanácsadáson** tooopen **bejelentkezéskonfigurálása**párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="814c4-171">On hello **FirmPlay - Employee Advocacy for Recruiting Configuration** section, click **Configure FirmPlay - Employee Advocacy for Recruiting** tooopen **Configure sign-on** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. <span data-ttu-id="814c4-174">az alkalmazáshoz konfigurált SSO tooget, forduljon a [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) és adja meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="814c4-174">tooget SSO configured for your application, contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) and provide them with hello following:</span></span> 

    <span data-ttu-id="814c4-175">• hello letöltött **tanúsítványfájl**</span><span class="sxs-lookup"><span data-stu-id="814c4-175">•  hello downloaded **Certificate file**</span></span>

    <span data-ttu-id="814c4-176">• hello **SAML-alapú egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="814c4-176">•  hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="814c4-177">• hello **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="814c4-177">•  hello **SAML Entity ID**</span></span>

    <span data-ttu-id="814c4-178">• hello **Sign-Out URL-címe**</span><span class="sxs-lookup"><span data-stu-id="814c4-178">•  hello **Sign-Out URL**</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="814c4-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="814c4-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="814c4-180">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="814c4-180">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="814c4-182">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="814c4-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="814c4-183">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="814c4-183">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="814c4-185">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="814c4-185">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="814c4-187">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="814c4-187">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="814c4-189">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="814c4-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="814c4-191">a.</span><span class="sxs-lookup"><span data-stu-id="814c4-191">a.</span></span> <span data-ttu-id="814c4-192">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="814c4-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="814c4-193">b.</span><span class="sxs-lookup"><span data-stu-id="814c4-193">b.</span></span> <span data-ttu-id="814c4-194">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="814c4-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="814c4-195">c.</span><span class="sxs-lookup"><span data-stu-id="814c4-195">c.</span></span> <span data-ttu-id="814c4-196">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="814c4-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="814c4-197">d.</span><span class="sxs-lookup"><span data-stu-id="814c4-197">d.</span></span> <span data-ttu-id="814c4-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="814c4-198">Click **Create**.</span></span> 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a><span data-ttu-id="814c4-199">Egy FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="814c4-199">Creating a FirmPlay - Employee Advocacy for Recruiting test user</span></span>

<span data-ttu-id="814c4-200">Ebben a szakaszban Britta Simon meghívta FirmPlay - alkalmazott tanácsadáson személyzeti osztályon a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="814c4-200">In this section, you create a user called Britta Simon in FirmPlay - Employee Advocacy for Recruiting.</span></span> <span data-ttu-id="814c4-201">Adjon együttműködve [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) tooadd hello felhasználók hello FirmPlay - alkalmazott tanácsadáson személyzeti osztályon platform.</span><span class="sxs-lookup"><span data-stu-id="814c4-201">Please work with [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) tooadd hello users in hello FirmPlay - Employee Advocacy for Recruiting platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="814c4-202">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="814c4-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="814c4-203">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooFirmPlay - alkalmazott tanácsadáson a személyzeti osztályon megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="814c4-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFirmPlay - Employee Advocacy for Recruiting.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="814c4-205">**tooassign Britta Simon tooFirmPlay - alkalmazott tanácsadáson a személyzeti osztályon, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="814c4-205">**tooassign Britta Simon tooFirmPlay - Employee Advocacy for Recruiting, perform hello following steps:**</span></span>

1. <span data-ttu-id="814c4-206">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="814c4-206">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="814c4-208">Hello alkalmazások listában válassza ki a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**.</span><span class="sxs-lookup"><span data-stu-id="814c4-208">In hello applications list, select **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. <span data-ttu-id="814c4-210">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="814c4-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="814c4-212">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="814c4-212">Click **Add** button.</span></span> <span data-ttu-id="814c4-213">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="814c4-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="814c4-215">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="814c4-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="814c4-216">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="814c4-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="814c4-217">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="814c4-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="814c4-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="814c4-218">Testing single sign-on</span></span>

<span data-ttu-id="814c4-219">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="814c4-219">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="814c4-220">Hello FirmPlay - alkalmazott tanácsadáson személyzeti osztályon csempe a hozzáférési Panel hello kattintva kapja meg automatikusan bejelentkezett tooyour FirmPlay - alkalmazott tanácsadáson személyzeti osztályon alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="814c4-220">When you click hello FirmPlay - Employee Advocacy for Recruiting tile in hello Access Panel, you should get automatically signed-on tooyour FirmPlay - Employee Advocacy for Recruiting application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="814c4-221">További források</span><span class="sxs-lookup"><span data-stu-id="814c4-221">Additional resources</span></span>

* [<span data-ttu-id="814c4-222">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="814c4-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="814c4-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="814c4-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png