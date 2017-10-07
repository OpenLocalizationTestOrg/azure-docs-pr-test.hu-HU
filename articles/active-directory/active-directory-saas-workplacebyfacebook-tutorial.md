---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f71a59527394730757d501a973251dc293fd3683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="64f0a-103">Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="64f0a-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="64f0a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate munkahelyi által Facebook az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="64f0a-104">In this tutorial, you learn how toointegrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="64f0a-105">Munkahelyi által Facebook integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="64f0a-105">Integrating Workplace by Facebook with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="64f0a-106">Megadhatja a hozzáférés tooWorkplace által Facebook rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="64f0a-106">You can control in Azure AD who has access tooWorkplace by Facebook</span></span>
- <span data-ttu-id="64f0a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkplace Facebook (egyszeri bejelentkezés) által az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="64f0a-107">You can enable your users tooautomatically get signed-on tooWorkplace by Facebook (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="64f0a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="64f0a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="64f0a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="64f0a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64f0a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="64f0a-110">Prerequisites</span></span>

<span data-ttu-id="64f0a-111">tooconfigure az Azure AD-integráció a munkahely által Facebook, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="64f0a-111">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="64f0a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="64f0a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="64f0a-113">A munkahelyi Facebook egyszeri bejelentkezés által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="64f0a-113">A Workplace by Facebook single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="64f0a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="64f0a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="64f0a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="64f0a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="64f0a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="64f0a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="64f0a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64f0a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="64f0a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="64f0a-118">Scenario description</span></span>
<span data-ttu-id="64f0a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="64f0a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="64f0a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="64f0a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="64f0a-121">Munkahelyi által Facebook hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="64f0a-121">Adding Workplace by Facebook from hello gallery</span></span>
2. <span data-ttu-id="64f0a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="64f0a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workplace-by-facebook-from-hello-gallery"></a><span data-ttu-id="64f0a-123">Munkahelyi által Facebook hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="64f0a-123">Adding Workplace by Facebook from hello gallery</span></span>
<span data-ttu-id="64f0a-124">tooconfigure hello integrációs munkahely Facebook által az Azure AD-be, meg kell tooadd által Facebook munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="64f0a-124">tooconfigure hello integration of Workplace by Facebook into Azure AD, you need tooadd Workplace by Facebook from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="64f0a-125">**tooadd által hello gyűjteményből Facebook munkahelyi hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="64f0a-125">**tooadd Workplace by Facebook from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="64f0a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="64f0a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="64f0a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="64f0a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="64f0a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="64f0a-133">Hello keresési mezőbe, írja be a **által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-133">In hello search box, type **Workplace by Facebook**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. <span data-ttu-id="64f0a-135">Hello eredmények panelen, jelölje ki a **által Facebook munkahelyi**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-135">In hello results panel, select **Workplace by Facebook**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="64f0a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="64f0a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="64f0a-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés a munkahelyi által Facebook "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="64f0a-138">In this section, you configure and test Azure AD single sign-on with Workplace by Facebook based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="64f0a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói által Facebook munkahelyi tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="64f0a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workplace by Facebook is tooa user in Azure AD.</span></span> <span data-ttu-id="64f0a-140">Ez azt jelenti hello kapcsolódó felhasználói által Facebook munkahelyi és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="64f0a-140">In other words, a link relationship between an Azure AD user and hello related user in Workplace by Facebook needs toobe established.</span></span>

<span data-ttu-id="64f0a-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** munkahelyi Facebook által.</span><span class="sxs-lookup"><span data-stu-id="64f0a-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workplace by Facebook.</span></span>

<span data-ttu-id="64f0a-142">tooconfigure és az Azure AD az egyszeri bejelentkezés munkahelyi által Facebook-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="64f0a-142">tooconfigure and test Azure AD single sign-on with Workplace by Facebook, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="64f0a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="64f0a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="64f0a-144">**[Újrahitelesítés gyakoriságának beállítása](#configuring-reauthentication-frequency)**  -tooconfigure munkahelyi tooprompt egy SAML-ellenőrzés céljából.</span><span class="sxs-lookup"><span data-stu-id="64f0a-144">**[Configuring Reauthentication Frequency](#configuring-reauthentication-frequency)** - tooconfigure Workplace tooprompt for a SAML check.</span></span>
3. <span data-ttu-id="64f0a-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64f0a-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="64f0a-146">**[A munkahelyi Facebook teszt felhasználó létrehozása](#creating-a-workplace-by-facebook-test-user)**  -toohave egy megfelelője a Facebook, a felhasználó ábrázolása csatolt toohello az Azure AD által munkahelyi Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64f0a-146">**[Creating a Workplace by Facebook test user](#creating-a-workplace-by-facebook-test-user)** - toohave a counterpart of Britta Simon in Workplace by Facebook that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="64f0a-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="64f0a-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="64f0a-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="64f0a-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="64f0a-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="64f0a-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="64f0a-150">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az egyszeri bejelentkezés konfigurálása a munkahelyi Facebook-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="64f0a-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workplace by Facebook application.</span></span>

<span data-ttu-id="64f0a-151">**az Azure AD tooconfigure egyszeri bejelentkezést a munkahely által Facebook, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="64f0a-151">**tooconfigure Azure AD single sign-on with Workplace by Facebook, perform hello following steps:**</span></span>

1. <span data-ttu-id="64f0a-152">Az Azure portál, a hello hello **által Facebook munkahelyi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-152">In hello Azure portal, on hello **Workplace by Facebook** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="64f0a-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="64f0a-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="64f0a-156">A hello **Facebook-tartomány és az URL-címek munkahelyi** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="64f0a-156">On hello **Workplace by Facebook Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    <span data-ttu-id="64f0a-158">a.</span><span class="sxs-lookup"><span data-stu-id="64f0a-158">a.</span></span> <span data-ttu-id="64f0a-159">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="64f0a-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.facebook.com`</span></span>

    <span data-ttu-id="64f0a-160">b.</span><span class="sxs-lookup"><span data-stu-id="64f0a-160">b.</span></span> <span data-ttu-id="64f0a-161">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.facebook.com/company/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="64f0a-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.facebook.com/company/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="64f0a-162">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="64f0a-162">These values are not hello real.</span></span> <span data-ttu-id="64f0a-163">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="64f0a-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="64f0a-164">Ügyfél [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="64f0a-164">Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) tooget these values.</span></span> 

4. <span data-ttu-id="64f0a-165">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="64f0a-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="64f0a-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="64f0a-169">A hello **Facebook konfigurációja munkahelyi** területén kattintson **munkahelyi konfigurálása által a Facebook** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="64f0a-169">On hello **Workplace by Facebook Configuration** section, click **Configure Workplace by Facebook** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="64f0a-170">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="64f0a-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. <span data-ttu-id="64f0a-172">Egy másik webes böngészőablakban, bejelentkezési tooyour munkahelyi Facebook vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="64f0a-172">In a different web browser window, login tooyour Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="64f0a-173">Hello SAML-alapú hitelesítési folyamat részeként munkahelyi rendelés toopass paraméterek tooAzure AD mérete kilobájtban too2.5 be a lekérdezési karakterláncok is használják.</span><span class="sxs-lookup"><span data-stu-id="64f0a-173">As part of hello SAML authentication process, Workplace may utilize query strings of up too2.5 kilobytes in size in order toopass parameters tooAzure AD.</span></span>

8. <span data-ttu-id="64f0a-174">A hello **vállalati irányítópult**, nyissa meg toohello **hitelesítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="64f0a-174">In hello **Company Dashboard**, go toohello **Authentication** tab.</span></span>

9. <span data-ttu-id="64f0a-175">A **SAML-alapú hitelesítés**, jelölje be **SSO csak** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="64f0a-175">Under **SAML Authentication**, select **SSO Only** from hello drop-down list.</span></span>

10. <span data-ttu-id="64f0a-176">Bemeneti hello értékek átmásolva **Facebook konfigurációja munkahelyi** hello Azure-portálon hello megfelelő mezőkbe szakaszában:</span><span class="sxs-lookup"><span data-stu-id="64f0a-176">Input hello values copied from **Workplace by Facebook Configuration** section of hello Azure portal into hello corresponding fields:</span></span>

    *   <span data-ttu-id="64f0a-177">A **SAML-alapú URL-cím** szövegmezőhöz Beillesztés hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="64f0a-177">In **SAML URL** textbox, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="64f0a-178">A **SAML kibocsátó URL-cím beviteli mező**, illessze be a hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="64f0a-178">In **SAML Issuer URL textbox**, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="64f0a-179">A **SAML kijelentkezési átirányítási** (nem kötelező), illessze be a hello értékének **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="64f0a-179">In **SAML Logout Redirect** (Optional), paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="64f0a-180">Nyissa meg a **base-64 kódolású tanúsítvány** a Jegyzettömbben az Azure portálról letöltött hello tartalmát, másolja a vágólapra és toothe beillesztési **SAML tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="64f0a-180">Open your **base-64 encoded certificate** in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toothe **SAML Certificate** textbox.</span></span>

11. <span data-ttu-id="64f0a-181">Előfordulhat, hogy tooenter hello célközönség URL-címe, címzett URL-címet, és ACS (helyességi feltétel fogyasztói szolgáltatás) URL-cím alatt hello feltüntetve **SAML-alapú konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="64f0a-181">You may need tooenter hello Audience URL, Recipient URL, and ACS (Assertion Consumer Service) URL listed under hello **SAML Configuration** section.</span></span>

12. <span data-ttu-id="64f0a-182">Görgessen hello szakasz toohello aljára, és kattintson a hello **teszt SSO** gombra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-182">Scroll toohello bottom of hello section and click hello **Test SSO** button.</span></span> <span data-ttu-id="64f0a-183">Ennek eredményeképp a egy előugró ablakban jelenik meg az Azure AD bejelentkezési oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="64f0a-183">This results in a popup window appearing with Azure AD login page presented.</span></span> <span data-ttu-id="64f0a-184">Normál tooauthenticate adja meg a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="64f0a-184">Enter your credentials in as normal tooauthenticate.</span></span> 

    <span data-ttu-id="64f0a-185">**Hibaelhárítás:** ellenőrizze, hogy hello e-mail címet ad vissza az Azure AD vissza az hello megegyeznek a hello munkahelyi fiókkal jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="64f0a-185">**Troubleshooting:** Ensure hello email address being returned back from Azure AD is hello same as hello Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="64f0a-186">Miután hello teszt sikeresen befejeződött, toohello a hello lap alján görgessen, majd kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-186">Once hello test has been completed successfully, scroll toohello bottom of hello page and click hello **Save** button.</span></span>

14. <span data-ttu-id="64f0a-187">Minden felhasználó használja a munkahelyi most számára jelenik meg az Azure AD bejelentkezési oldalt a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="64f0a-187">All users using Workplace will now be presented with Azure AD login page for authentication.</span></span>

15. <span data-ttu-id="64f0a-188">**SAML kijelentkezési átirányítási (nem kötelező)** -</span><span class="sxs-lookup"><span data-stu-id="64f0a-188">**SAML Logout Redirect (optional)** -</span></span> 

    <span data-ttu-id="64f0a-189">Választhat toooptionally konfigurálása egy SAML kijelentkezési URL-címet, amely az Azure AD kijelentkezési oldalon használt toopoint lehet.</span><span class="sxs-lookup"><span data-stu-id="64f0a-189">You can choose toooptionally configure a SAML Logout Url, which can be used toopoint at Azure AD's logout page.</span></span> <span data-ttu-id="64f0a-190">Ha ezt a beállítást engedélyezve és konfigurálva van, a hello felhasználó már nem irányított toohello munkahelyi kijelentkezési lap.</span><span class="sxs-lookup"><span data-stu-id="64f0a-190">When this setting is enabled and configured, hello user will no longer be directed toohello Workplace logout page.</span></span> <span data-ttu-id="64f0a-191">Ehelyett a hello felhasználó hozzá lett adva a hello SAML kijelentkezési átirányítási beállítás átirányított toohello URL-cím lesz.</span><span class="sxs-lookup"><span data-stu-id="64f0a-191">Instead, hello user will be redirected toohello url that was added in hello SAML Logout Redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="64f0a-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="64f0a-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="64f0a-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="64f0a-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="64f0a-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="64f0a-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="configuring-reauthentication-frequency"></a><span data-ttu-id="64f0a-195">Újrahitelesítés gyakoriság beállítása</span><span class="sxs-lookup"><span data-stu-id="64f0a-195">Configuring Reauthentication Frequency</span></span>

<span data-ttu-id="64f0a-196">Beállíthatja a munkahelyi tooprompt egy SAML-ellenőrzés minden nap, három nap, hét, két hét, hónap vagy egyáltalán nem.</span><span class="sxs-lookup"><span data-stu-id="64f0a-196">You can configure Workplace tooprompt for a SAML check every day, three days, week, two weeks, month or never.</span></span>

> [!NOTE] 
><span data-ttu-id="64f0a-197">hello minimális hello SAML-ellenőrzése mobilalkalmazás értéke tooone hét.</span><span class="sxs-lookup"><span data-stu-id="64f0a-197">hello minimum value for hello SAML check on mobile applications is set tooone week.</span></span>

<span data-ttu-id="64f0a-198">Is beállíthatja, hogy egy SAML hello gomb segítségével minden felhasználó visszaállítása: szükséges SAML-hitelesítés az összes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="64f0a-198">You can also force a SAML reset for all users using hello button: Require SAML authentication for all users now.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="64f0a-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="64f0a-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="64f0a-200">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="64f0a-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="64f0a-202">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="64f0a-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="64f0a-203">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="64f0a-205">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="64f0a-207">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="64f0a-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="64f0a-209">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="64f0a-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="64f0a-211">a.</span><span class="sxs-lookup"><span data-stu-id="64f0a-211">a.</span></span> <span data-ttu-id="64f0a-212">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="64f0a-213">b.</span><span class="sxs-lookup"><span data-stu-id="64f0a-213">b.</span></span> <span data-ttu-id="64f0a-214">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="64f0a-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="64f0a-215">c.</span><span class="sxs-lookup"><span data-stu-id="64f0a-215">c.</span></span> <span data-ttu-id="64f0a-216">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="64f0a-217">d.</span><span class="sxs-lookup"><span data-stu-id="64f0a-217">d.</span></span> <span data-ttu-id="64f0a-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-218">Click **Create**.</span></span>
 
### <a name="creating-a-workplace-by-facebook-test-user"></a><span data-ttu-id="64f0a-219">A munkahelyi Facebook teszt felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="64f0a-219">Creating a Workplace by Facebook test user</span></span>

<span data-ttu-id="64f0a-220">Ebben a szakaszban Britta Simon nevű felhasználó létrehozta a munkaterületen Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="64f0a-220">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="64f0a-221">Munkahelyi Facebook által támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="64f0a-221">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="64f0a-222">Nincs olyan művelet, ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="64f0a-222">There is no action for you in this section.</span></span> <span data-ttu-id="64f0a-223">Ha a felhasználó nem létezik a munkahely által Facebook, egy új hozta létre tooaccess munkahelyi tett kísérlet során Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="64f0a-223">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt tooaccess Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="64f0a-224">Ha a felhasználó manuálisan, forduljon a toocreate kell [munkahelyi Facebook ügyfél-támogatási csoport szerint](https://workplace.fb.com/faq/)</span><span class="sxs-lookup"><span data-stu-id="64f0a-224">If you need toocreate a user manually, Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/)</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="64f0a-225">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="64f0a-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="64f0a-226">Ebben a szakaszban Britta Simon toouse Azure egyszeri bejelentkezés Facebook által hozzáférés tooWorkplace megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="64f0a-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkplace by Facebook.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="64f0a-228">**tooassign Britta Simon tooWorkplace által Facebook, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="64f0a-228">**tooassign Britta Simon tooWorkplace by Facebook, perform hello following steps:**</span></span>

1. <span data-ttu-id="64f0a-229">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="64f0a-231">Hello alkalmazások listában válassza ki a **által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-231">In hello applications list, select **Workplace by Facebook**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="64f0a-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="64f0a-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="64f0a-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="64f0a-235">Click **Add** button.</span></span> <span data-ttu-id="64f0a-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="64f0a-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="64f0a-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="64f0a-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="64f0a-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="64f0a-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="64f0a-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="64f0a-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="64f0a-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="64f0a-241">Testing single sign-on</span></span>

<span data-ttu-id="64f0a-242">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="64f0a-242">If you want tootest your single sign-on settings, open hello Access Panel.</span></span>
<span data-ttu-id="64f0a-243">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="64f0a-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="64f0a-244">További források</span><span class="sxs-lookup"><span data-stu-id="64f0a-244">Additional resources</span></span>

* [<span data-ttu-id="64f0a-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="64f0a-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="64f0a-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="64f0a-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="64f0a-247">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="64f0a-247">Configure User Provisioning</span></span>](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

