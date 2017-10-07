---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: fd19b3f178a2aee7dd2f204d6d3cf6df8fe6b444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="43e70-103">Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="43e70-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="43e70-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate munkahelyi által Facebook az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="43e70-104">In this tutorial, you learn how toointegrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="43e70-105">Munkahelyi által Facebook integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="43e70-105">Integrating Workplace by Facebook with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="43e70-106">Az Azure AD hozzáférési tooWorkplace által Facebook rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="43e70-106">You can control in Azure AD who has access tooWorkplace by Facebook.</span></span>
- <span data-ttu-id="43e70-107">Engedélyezheti a felhasználók tooautomatically beolvasása feliratkozva a tooWorkplace Facebook (egyszeri bejelentkezés) által az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="43e70-107">You can enable your users tooautomatically get signed on tooWorkplace by Facebook (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="43e70-108">A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="43e70-108">You can manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="43e70-109">Szoftver egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatos további tudnivalókért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43e70-109">For more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43e70-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="43e70-110">Prerequisites</span></span>

<span data-ttu-id="43e70-111">tooconfigure az Azure AD-integráció a munkahely által Facebook, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="43e70-111">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="43e70-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="43e70-112">An Azure AD subscription</span></span>
- <span data-ttu-id="43e70-113">A munkahelyi Facebook egyszeri bejelentkezés (SSO) által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="43e70-113">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="43e70-114">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="43e70-114">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="43e70-115">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="43e70-115">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="43e70-116">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43e70-116">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="43e70-117">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="43e70-117">Scenario description</span></span>
<span data-ttu-id="43e70-118">Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni.</span><span class="sxs-lookup"><span data-stu-id="43e70-118">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="43e70-119">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="43e70-119">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43e70-120">Adja hozzá a munkahelyi által Facebook hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="43e70-120">Add Workplace by Facebook from hello gallery.</span></span>
2. <span data-ttu-id="43e70-121">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="43e70-121">Configure and test Azure AD single sign-on.</span></span>

## <a name="add-workplace-by-facebook-from-hello-gallery"></a><span data-ttu-id="43e70-122">Adja hozzá a munkahelyi által Facebook hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="43e70-122">Add Workplace by Facebook from hello gallery</span></span>
<span data-ttu-id="43e70-123">tooconfigure hello integrálása a munkahelyi Facebook által az Azure AD által Facebook munkahelyi hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="43e70-123">tooconfigure hello integration of Workplace by Facebook into Azure AD, add Workplace by Facebook from hello gallery tooyour list of managed SaaS apps.</span></span>

1. <span data-ttu-id="43e70-124">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, jelölje ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="43e70-124">In hello [Azure portal](https://portal.azure.com), in hello left pane, select **Azure Active Directory**.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="43e70-126">Keresse meg a túl**vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="43e70-126">Browse too**Enterprise applications** > **All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="43e70-128">tooadd hello új alkalmazást, jelölje be **új alkalmazás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43e70-128">tooadd hello new application, select **New application** on hello top of hello dialog box.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="43e70-130">Hello keresési mezőbe, írja be a **munkahelyi Facebook által**, és válassza ki **által Facebook munkahelyi** eredmények.</span><span class="sxs-lookup"><span data-stu-id="43e70-130">In hello search box, type **Workplace by Facebook**, and select **Workplace by Facebook** from results.</span></span> <span data-ttu-id="43e70-131">Válassza ki **Hozzáadás**, tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43e70-131">Then select **Add**, tooadd hello application.</span></span>

    ![Munkahelyi által Facebook hello eredmények listájában](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="43e70-133">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43e70-133">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="43e70-134">Ebben a szakaszban konfigurálása és tesztelése a Facebook, a "Britta Simon." nevű tesztfelhasználó alapján által munkahelyi Azure AD egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="43e70-134">In this section, you configure and test Azure AD SSO with Workplace by Facebook, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="43e70-135">Az SSO toowork az Azure AD kell tooknow milyen hello tartozó felhasználói által Facebook munkahelyi tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="43e70-135">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Workplace by Facebook is tooa user in Azure AD.</span></span> <span data-ttu-id="43e70-136">Más szóval csatolt hello kapcsolódó felhasználói által Facebook munkahelyi és az Azure AD-felhasználó közötti kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="43e70-136">In other words, a linked relationship between an Azure AD user and hello related user in Workplace by Facebook should be established.</span></span>

<span data-ttu-id="43e70-137">Ezt a kapcsolatot létesíteni hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** munkahelyi Facebook által.</span><span class="sxs-lookup"><span data-stu-id="43e70-137">Establish this relationship by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workplace by Facebook.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="43e70-138">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43e70-138">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="43e70-139">Ebben a szakaszban hello Azure-portálon az Azure AD SSO engedélyezi, és egyszeri bejelentkezés konfigurálása Facebook-alkalmazás által a munkahelyen.</span><span class="sxs-lookup"><span data-stu-id="43e70-139">In this section, you enable Azure AD SSO in hello Azure portal, and you configure SSO in your Workplace by Facebook application.</span></span>

1. <span data-ttu-id="43e70-140">Az Azure portál, a hello hello **által Facebook munkahelyi** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="43e70-140">In hello Azure portal, on hello **Workplace by Facebook** application integration page, select **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="43e70-142">A hello **egyszeri bejelentkezés** párbeszédpanelen jelölje ki **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="43e70-142">In hello **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on** tooenable SSO.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="43e70-144">A hello **Facebook-tartomány és az URL-címek munkahelyi** területen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="43e70-144">In hello **Workplace by Facebook Domain and URLs** section, do hello following:</span></span>

    <span data-ttu-id="43e70-145">a.</span><span class="sxs-lookup"><span data-stu-id="43e70-145">a.</span></span> <span data-ttu-id="43e70-146">A hello **bejelentkezési URL-cím** szövegmezőbe írja be egy URL-címet, amely a következő mintát hello használja:`https://<company subdomain>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="43e70-146">In hello **Sign-on URL** text box, type a URL that uses hello following pattern: `https://<company subdomain>.facebook.com`</span></span>

    <span data-ttu-id="43e70-147">b.</span><span class="sxs-lookup"><span data-stu-id="43e70-147">b.</span></span> <span data-ttu-id="43e70-148">A hello **azonosító** szövegmezőbe írja be egy URL-címet, amely a következő mintát hello használja:`https://www.facebook.com/company/<scim company id>`</span><span class="sxs-lookup"><span data-stu-id="43e70-148">In hello **Identifier** text box, type a URL that uses hello following pattern: `https://www.facebook.com/company/<scim company id>`</span></span>

    > [!NOTE]
    > <span data-ttu-id="43e70-149">Ezek az értékek csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="43e70-149">These values are an example only.</span></span> <span data-ttu-id="43e70-150">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="43e70-150">Update these values with hello actual sign-on URL and identifier.</span></span> <span data-ttu-id="43e70-151">Kapcsolattartási hello [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="43e70-151">Contact hello [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) tooget these values.</span></span> 

4. <span data-ttu-id="43e70-152">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **tanúsítvány (Base64)**, majd mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="43e70-152">In hello **SAML Signing Certificate** section, select **Certificate (Base64)**, and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="43e70-154">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="43e70-154">Select **Save**.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="43e70-156">A hello **Facebook konfigurációja munkahelyi** szakaszban jelölje be **munkahelyi konfigurálása által a Facebook** tooopen hello **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="43e70-156">In hello **Workplace by Facebook Configuration** section, select **Configure Workplace by Facebook** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="43e70-157">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="43e70-157">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference** section.</span></span>

    ![Munkahelyi Facebook-konfigurációja](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. <span data-ttu-id="43e70-159">Egy másik webes böngészőablakban jelentkezzen be munkahelyi Facebook vállalati hely tooyour rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="43e70-159">In a different web browser window, sign in tooyour Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="43e70-160">Hello SAML-alapú hitelesítési folyamat részeként munkahelyi too2.5 kilobájt be a lekérdezési karakterláncok használhatja rendelés toopass paraméterek tooAzure AD mérete.</span><span class="sxs-lookup"><span data-stu-id="43e70-160">As part of hello SAML authentication process, Workplace can use query strings of up too2.5 kilobytes in size in order toopass parameters tooAzure AD.</span></span>

8. <span data-ttu-id="43e70-161">A hello **vállalati irányítópult**, nyissa meg toohello **hitelesítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="43e70-161">In hello **Company Dashboard**, go toohello **Authentication** tab.</span></span>

9. <span data-ttu-id="43e70-162">A **SAML-alapú hitelesítés**, jelölje be **SSO csak** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="43e70-162">Under **SAML Authentication**, select **SSO Only** from hello drop-down list.</span></span>

10. <span data-ttu-id="43e70-163">Hello átmásolva hello értékeket **Facebook konfigurációja munkahelyi** hello Azure-portálon hello megfelelő mezőkbe szakaszában:</span><span class="sxs-lookup"><span data-stu-id="43e70-163">Enter hello values copied from hello **Workplace by Facebook Configuration** section of hello Azure portal into hello corresponding fields:</span></span>

    *   <span data-ttu-id="43e70-164">Az a **SAML-alapú URL-cím** szövegmezőben, Beillesztés hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon hello másolta.</span><span class="sxs-lookup"><span data-stu-id="43e70-164">In the **SAML URL** text box, paste hello value of **Single Sign-On Service URL**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="43e70-165">Az a **SAML kiállítójának URL-címe** szövegmezőben, Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon hello másolta.</span><span class="sxs-lookup"><span data-stu-id="43e70-165">In the **SAML Issuer URL** text box, paste hello value of **SAML Entity ID**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="43e70-166">A **SAML kijelentkezési átirányítási (nem kötelező)**, illessze be a hello értékének **Sign-Out URL-cím**, amely az Azure-portálon hello másolta.</span><span class="sxs-lookup"><span data-stu-id="43e70-166">In **SAML Logout Redirect (optional)**, paste hello value of **Sign-Out URL**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="43e70-167">Nyissa meg a **base-64 kódolású tanúsítvány** a Jegyzettömbben le: hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="43e70-167">Open your **base-64 encoded certificate** in Notepad, downloaded from hello Azure portal.</span></span> <span data-ttu-id="43e70-168">Hello tartalmát, másolja a vágólapra, és illessze be azt toothe **SAML tanúsítvány** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="43e70-168">Copy hello content of it into your clipboard, and then paste it toothe **SAML Certificate** text box.</span></span>

11. <span data-ttu-id="43e70-169">Előfordulhat, hogy tooenter hello célközönség URL-címe, címzett URL-címet, és az ACS (helyességi feltétel fogyasztói szolgáltatás) URL-cím alatt hello feltüntetve **SAML-alapú konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="43e70-169">You might need tooenter hello audience URL, recipient URL, and ACS (Assertion Consumer Service) URL, listed under hello **SAML Configuration** section.</span></span>

12. <span data-ttu-id="43e70-170">Görgessen hello szakasz toohello aljára, és válassza ki **teszt SSO**.</span><span class="sxs-lookup"><span data-stu-id="43e70-170">Scroll toohello bottom of hello section, and select **Test SSO**.</span></span> <span data-ttu-id="43e70-171">Egy előugró ablak jelenik meg, a hello Azure AD bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="43e70-171">A pop-up window appears, with hello Azure AD sign-in page.</span></span> <span data-ttu-id="43e70-172">tooauthenticate, adja meg a hitelesítő adatokat a szokásos módon.</span><span class="sxs-lookup"><span data-stu-id="43e70-172">tooauthenticate, enter your credentials as normal.</span></span> <span data-ttu-id="43e70-173">Győződjön meg arról, hello e-mail címet adott vissza az Azure AD vissza az hello megegyeznek a hello munkahelyi fiókkal jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="43e70-173">Ensure hello email address returned back from Azure AD is hello same as hello Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="43e70-174">Ha hello teszt sikeresen befejeződött, görgessen toohello alsó hello lap, és válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="43e70-174">If hello test has finished successfully, scroll toohello bottom of hello page and select **Save**.</span></span>

14. <span data-ttu-id="43e70-175">Munkahelyi felhasználója számára most jelenik meg az Azure AD bejelentkezési oldalára hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="43e70-175">Anyone using Workplace is now presented with Azure AD sign-in page for authentication.</span></span>

<span data-ttu-id="43e70-176">Választhat tooconfigure egy URL-címet, amely lehet az Azure AD hello kijelentkezési oldalon használt toopoint SAML kijelentkezett.</span><span class="sxs-lookup"><span data-stu-id="43e70-176">You can choose tooconfigure a SAML sign out URL, which can be used toopoint at hello Azure AD sign-out page.</span></span> <span data-ttu-id="43e70-177">Ha ezt a beállítást engedélyezve és konfigurálva van, hello felhasználó már nem irányított toohello munkahelyi kijelentkezési lap.</span><span class="sxs-lookup"><span data-stu-id="43e70-177">When this setting is enabled and configured, hello user is no longer directed toohello Workplace sign-out page.</span></span> <span data-ttu-id="43e70-178">Ehelyett a hello felhasználó hozzá lett adva a hello SAML kijelentkezési átirányítási beállítás átirányított toohello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="43e70-178">Instead, hello user is redirected toohello URL that was added in hello SAML sign-out redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="43e70-179">Ezek az utasítások belül hello tömör verziója most el tudja olvasni [Azure-portálon](https://portal.azure.com), míg a hello app beállításakor.</span><span class="sxs-lookup"><span data-stu-id="43e70-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app.</span></span> <span data-ttu-id="43e70-180">Ez az alkalmazás hozzáadása a hello után **Active Directory** > **vállalati alkalmazások** szakaszban, egyszerűen jelölje be a hello **egyszeri bejelentkezés** fülre, és hozzáférést hello beágyazott keresztül hello dokumentáció **konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="43e70-180">After adding this app from hello **Active Directory** > **Enterprise Applications** section, simply select hello **Single Sign-On** tab, and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="43e70-181">További szolgáltatásáról hello embedded dokumentációjából hello [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="43e70-181">You can read more about hello embedded documentation feature in hello [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="configure-reauthentication-frequency"></a><span data-ttu-id="43e70-182">Újrahitelesítés gyakoriságának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43e70-182">Configure reauthentication frequency</span></span>

<span data-ttu-id="43e70-183">Beállíthatja a munkahelyi tooprompt egy SAML-ellenőrzés céljából minden nap, három nap, egy hét, a két hét, egy hónap, vagy egyáltalán nem.</span><span class="sxs-lookup"><span data-stu-id="43e70-183">You can configure Workplace tooprompt for a SAML check every day, three days, one week, two weeks, one month, or never.</span></span>

> [!NOTE] 
><span data-ttu-id="43e70-184">hello minimális hello SAML-ellenőrzése mobilalkalmazás értéke tooone hét.</span><span class="sxs-lookup"><span data-stu-id="43e70-184">hello minimum value for hello SAML check on mobile applications is set tooone week.</span></span>

<span data-ttu-id="43e70-185">A SAML alaphelyzetbe állítja az összes felhasználó számára is kényszeríthető.</span><span class="sxs-lookup"><span data-stu-id="43e70-185">You can also force a SAML reset for all users.</span></span> <span data-ttu-id="43e70-186">toodo a, használjon hello **szükséges SAML-alapú hitelesítés az összes felhasználó most** gombra.</span><span class="sxs-lookup"><span data-stu-id="43e70-186">toodo this, use hello **Require SAML authentication for all users now** button.</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="43e70-187">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="43e70-187">Create an Azure AD test user</span></span>
<span data-ttu-id="43e70-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="43e70-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

1. <span data-ttu-id="43e70-190">A hello **Azure-portálon**, a hello bal oldali panelen, jelölje ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="43e70-190">In hello **Azure portal**, in hello left pane, select **Azure Active Directory**.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="43e70-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="43e70-192">toodisplay hello list of users, go too**Users and groups**, and select **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="43e70-194">tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="43e70-194">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="43e70-196">A hello **felhasználói** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="43e70-196">In hello **User** dialog box, do hello following:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="43e70-198">a.</span><span class="sxs-lookup"><span data-stu-id="43e70-198">a.</span></span> <span data-ttu-id="43e70-199">A hello **neve** szövegmezőben **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43e70-199">In hello **Name** text box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="43e70-200">b.</span><span class="sxs-lookup"><span data-stu-id="43e70-200">b.</span></span> <span data-ttu-id="43e70-201">A hello **felhasználónév** , a típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="43e70-201">In hello **User name** text box, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="43e70-202">c.</span><span class="sxs-lookup"><span data-stu-id="43e70-202">c.</span></span> <span data-ttu-id="43e70-203">Válassza ki **megjelenítése jelszó**, és írja le.</span><span class="sxs-lookup"><span data-stu-id="43e70-203">Select **Show Password**, and write it down.</span></span>

    <span data-ttu-id="43e70-204">d.</span><span class="sxs-lookup"><span data-stu-id="43e70-204">d.</span></span> <span data-ttu-id="43e70-205">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="43e70-205">Select **Create**.</span></span>
 
### <a name="create-a-workplace-by-facebook-test-user"></a><span data-ttu-id="43e70-206">Hozzon létre egy munkahelyi Facebook teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="43e70-206">Create a Workplace by Facebook test user</span></span>

<span data-ttu-id="43e70-207">Ebben a szakaszban Britta Simon nevű felhasználó létrehozta a munkaterületen Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="43e70-207">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="43e70-208">Munkahelyi Facebook által támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="43e70-208">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="43e70-209">Nincs olyan művelet, ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="43e70-209">There is no action for you in this section.</span></span> <span data-ttu-id="43e70-210">Ha a felhasználó nem létezik a munkahely által Facebook, egy új hozta létre tooaccess munkahelyi tett kísérlet során Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="43e70-210">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt tooaccess Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="43e70-211">Ha egy felhasználó toocreate manuálisan kell, lépjen kapcsolatba a hello [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/).</span><span class="sxs-lookup"><span data-stu-id="43e70-211">If you need toocreate a user manually, contact hello [Workplace by Facebook Client support team](https://workplace.fb.com/faq/).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="43e70-212">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="43e70-212">Assign hello Azure AD test user</span></span>

<span data-ttu-id="43e70-213">Ebben a szakaszban Britta Simon toouse Azure egyszeri Bejelentkezéses hozzáférést tooWorkplace Facebook által biztosított engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="43e70-213">In this section, you enable Britta Simon toouse Azure SSO by granting access tooWorkplace by Facebook.</span></span>

![Felhasználó hozzárendelése][200] 

1. <span data-ttu-id="43e70-215">Hello Azure portál, nyissa meg hello alkalmazások megtekintése.</span><span class="sxs-lookup"><span data-stu-id="43e70-215">In hello Azure portal, open hello applications view.</span></span> <span data-ttu-id="43e70-216">Nyissa meg a könyvtár nézet toohello, nyissa meg túl**vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="43e70-216">Go toohello directory view, go too**Enterprise applications**, and then select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="43e70-218">Hello alkalmazások listában válassza ki a **által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="43e70-218">In hello applications list, select **Workplace by Facebook**.</span></span>

    ![hello munkahelyi hello alkalmazások listáját a Facebook-kapcsolat](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="43e70-220">Hello hello bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="43e70-220">In hello menu on hello left, select **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="43e70-222">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="43e70-222">Select **Add**.</span></span> <span data-ttu-id="43e70-223">Ezt követően a hello **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="43e70-223">Then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="43e70-225">A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="43e70-225">In hello **Users and groups** dialog box, select **Britta Simon** in hello users list.</span></span>

6. <span data-ttu-id="43e70-226">A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="43e70-226">In hello **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="43e70-227">A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="43e70-227">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="43e70-228">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="43e70-228">Test single sign-on</span></span>

<span data-ttu-id="43e70-229">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="43e70-229">If you want tootest your SSO settings, open hello Access Panel.</span></span>
<span data-ttu-id="43e70-230">További információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="43e70-230">For more information, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="43e70-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43e70-231">Next steps</span></span>

* <span data-ttu-id="43e70-232">Lásd: hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="43e70-232">See hello [list of tutorials on how toointegrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md).</span></span>
* <span data-ttu-id="43e70-233">Olvasási [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43e70-233">Read [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>
* <span data-ttu-id="43e70-234">További tudnivalók túl[konfigurálja, a felhasználók átadása](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="43e70-234">Learn more about how too[configure user provisioning](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span></span>



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

