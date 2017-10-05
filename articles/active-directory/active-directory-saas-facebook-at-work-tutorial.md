---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
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
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="57bf7-103">Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="57bf7-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="57bf7-104">Ebben az oktatóanyagban elsajátíthatja által Facebook munkahelyi integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="57bf7-104">In this tutorial, you learn how to integrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="57bf7-105">Munkahelyi által Facebook integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="57bf7-105">Integrating Workplace by Facebook with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="57bf7-106">Az Azure AD, aki hozzáféréssel rendelkezik a munkahelyi Facebook által szabályozható.</span><span class="sxs-lookup"><span data-stu-id="57bf7-106">You can control in Azure AD who has access to Workplace by Facebook.</span></span>
- <span data-ttu-id="57bf7-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni munkahelyi által aláírt Facebook (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="57bf7-107">You can enable your users to automatically get signed on to Workplace by Facebook (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="57bf7-108">A fiók egyetlen központi helyen kezelheti: az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="57bf7-108">You can manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="57bf7-109">Szoftver egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatos további tudnivalókért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="57bf7-109">For more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57bf7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57bf7-110">Prerequisites</span></span>

<span data-ttu-id="57bf7-111">Konfigurálása az Azure AD-integrációs munkahelyi által Facebook, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="57bf7-111">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="57bf7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="57bf7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="57bf7-113">A munkahelyi Facebook egyszeri bejelentkezés (SSO) által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="57bf7-113">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="57bf7-114">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="57bf7-114">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="57bf7-115">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="57bf7-115">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="57bf7-116">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="57bf7-116">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="57bf7-117">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="57bf7-117">Scenario description</span></span>
<span data-ttu-id="57bf7-118">Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni.</span><span class="sxs-lookup"><span data-stu-id="57bf7-118">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="57bf7-119">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="57bf7-119">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="57bf7-120">Adja hozzá a munkahelyi Facebook által a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="57bf7-120">Add Workplace by Facebook from the gallery.</span></span>
2. <span data-ttu-id="57bf7-121">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="57bf7-121">Configure and test Azure AD single sign-on.</span></span>

## <a name="add-workplace-by-facebook-from-the-gallery"></a><span data-ttu-id="57bf7-122">Adja hozzá a munkahelyi Facebook által a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="57bf7-122">Add Workplace by Facebook from the gallery</span></span>
<span data-ttu-id="57bf7-123">Konfigurálja az Azure AD integrálása a munkahely által Facebook, vegye fel munkahelyi Facebook által a gyűjteményből a kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="57bf7-123">To configure the integration of Workplace by Facebook into Azure AD, add Workplace by Facebook from the gallery to your list of managed SaaS apps.</span></span>

1. <span data-ttu-id="57bf7-124">Az a [Azure-portálon](https://portal.azure.com), a bal oldali panelen válassza ki a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-124">In the [Azure portal](https://portal.azure.com), in the left pane, select **Azure Active Directory**.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="57bf7-126">Keresse meg a **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-126">Browse to **Enterprise applications** > **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="57bf7-128">Az új alkalmazás hozzáadásához válassza **új alkalmazás** a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="57bf7-128">To add the new application, select **New application** on the top of the dialog box.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="57bf7-130">Írja be a keresőmezőbe, **által Facebook munkahelyi**, és válassza ki **által Facebook munkahelyi** eredménybe.</span><span class="sxs-lookup"><span data-stu-id="57bf7-130">In the search box, type **Workplace by Facebook**, and select **Workplace by Facebook** from results.</span></span> <span data-ttu-id="57bf7-131">Válassza ki **Hozzáadás**, hogy vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="57bf7-131">Then select **Add**, to add the application.</span></span>

    ![Munkahelyi Facebook által az eredménylistában](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="57bf7-133">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57bf7-133">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="57bf7-134">Ebben a szakaszban konfigurálása és tesztelése a Facebook, a "Britta Simon." nevű tesztfelhasználó alapján által munkahelyi Azure AD egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="57bf7-134">In this section, you configure and test Azure AD SSO with Workplace by Facebook, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="57bf7-135">Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, a partner felhasználó által Facebook munkahelyi Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="57bf7-135">For SSO to work, Azure AD needs to know what the counterpart user in Workplace by Facebook is to a user in Azure AD.</span></span> <span data-ttu-id="57bf7-136">Más szóval közötti egy Azure AD-felhasználó és a kapcsolódó felhasználó a munkahelyi Facebook által hivatkozott kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="57bf7-136">In other words, a linked relationship between an Azure AD user and the related user in Workplace by Facebook should be established.</span></span>

<span data-ttu-id="57bf7-137">Ez kapcsolatot létesíteni a értékének hozzárendelésével a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** munkahelyi Facebook által.</span><span class="sxs-lookup"><span data-stu-id="57bf7-137">Establish this relationship by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workplace by Facebook.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="57bf7-138">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57bf7-138">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="57bf7-139">Ebben a szakaszban Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és egyszeri bejelentkezés konfigurálása Facebook-alkalmazás által a munkahelyen.</span><span class="sxs-lookup"><span data-stu-id="57bf7-139">In this section, you enable Azure AD SSO in the Azure portal, and you configure SSO in your Workplace by Facebook application.</span></span>

1. <span data-ttu-id="57bf7-140">Az Azure portálon a a **által Facebook munkahelyi** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-140">In the Azure portal, on the **Workplace by Facebook** application integration page, select **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="57bf7-142">Az a **egyszeri bejelentkezés** párbeszédpanelen jelölje ki **mód** , **SAML-alapú bejelentkezés** SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="57bf7-142">In the **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on** to enable SSO.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="57bf7-144">Az a **Facebook-tartomány és az URL-címek munkahelyi** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="57bf7-144">In the **Workplace by Facebook Domain and URLs** section, do the following:</span></span>

    <span data-ttu-id="57bf7-145">a.</span><span class="sxs-lookup"><span data-stu-id="57bf7-145">a.</span></span> <span data-ttu-id="57bf7-146">Az a **bejelentkezési URL-cím** szövegmezőbe írja be egy URL-címet, amely a következő mintát használ:`https://<company subdomain>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="57bf7-146">In the **Sign-on URL** text box, type a URL that uses the following pattern: `https://<company subdomain>.facebook.com`</span></span>

    <span data-ttu-id="57bf7-147">b.</span><span class="sxs-lookup"><span data-stu-id="57bf7-147">b.</span></span> <span data-ttu-id="57bf7-148">Az a **azonosító** szövegmezőbe írja be egy URL-címet, amely a következő mintát használ:`https://www.facebook.com/company/<scim company id>`</span><span class="sxs-lookup"><span data-stu-id="57bf7-148">In the **Identifier** text box, type a URL that uses the following pattern: `https://www.facebook.com/company/<scim company id>`</span></span>

    > [!NOTE]
    > <span data-ttu-id="57bf7-149">Ezek az értékek csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="57bf7-149">These values are an example only.</span></span> <span data-ttu-id="57bf7-150">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="57bf7-150">Update these values with the actual sign-on URL and identifier.</span></span> <span data-ttu-id="57bf7-151">Lépjen kapcsolatba a [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="57bf7-151">Contact the [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) to get these values.</span></span> 

4. <span data-ttu-id="57bf7-152">A a **SAML-aláíró tanúsítványa** szakaszban jelölje be **tanúsítvány (Base64)**, majd mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="57bf7-152">In the **SAML Signing Certificate** section, select **Certificate (Base64)**, and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="57bf7-154">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="57bf7-154">Select **Save**.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="57bf7-156">Az a **Facebook konfigurációja munkahelyi** szakaszban jelölje be **munkahelyi konfigurálása által a Facebook** megnyitásához a **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="57bf7-156">In the **Workplace by Facebook Configuration** section, select **Configure Workplace by Facebook** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="57bf7-157">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="57bf7-157">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference** section.</span></span>

    ![Munkahelyi Facebook-konfigurációja](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. <span data-ttu-id="57bf7-159">Egy másik webes böngészőablakban jelentkezzen be a munkahelyi Facebook vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="57bf7-159">In a different web browser window, sign in to your Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="57bf7-160">A SAML-hitelesítési folyamat részeként munkahelyi használhatja lekérdezési karakterláncok legfeljebb 2,5 kilobájt méretű ahhoz, hogy az Azure AD-paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="57bf7-160">As part of the SAML authentication process, Workplace can use query strings of up to 2.5 kilobytes in size in order to pass parameters to Azure AD.</span></span>

8. <span data-ttu-id="57bf7-161">Az a **vállalati irányítópult**, navigáljon a **hitelesítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="57bf7-161">In the **Company Dashboard**, go to the **Authentication** tab.</span></span>

9. <span data-ttu-id="57bf7-162">A **SAML-alapú hitelesítés**, jelölje be **SSO csak** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="57bf7-162">Under **SAML Authentication**, select **SSO Only** from the drop-down list.</span></span>

10. <span data-ttu-id="57bf7-163">Adja meg az értékeket, átmásolva a **Facebook konfigurációja munkahelyi** szakasza a megfelelő mezőkbe az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="57bf7-163">Enter the values copied from the **Workplace by Facebook Configuration** section of the Azure portal into the corresponding fields:</span></span>

    *   <span data-ttu-id="57bf7-164">Az a **SAML-alapú URL-cím** szöveg mezőbe illessze be az értékét **egyszeri bejelentkezési URL-címe**, amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="57bf7-164">In the **SAML URL** text box, paste the value of **Single Sign-On Service URL**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="57bf7-165">Az a **SAML kiállítójának URL-címe** szöveg mezőbe illessze be az értékét **SAML Entitásazonosító**, amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="57bf7-165">In the **SAML Issuer URL** text box, paste the value of **SAML Entity ID**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="57bf7-166">A **SAML kijelentkezési átirányítási (nem kötelező)**, illessze be az értékét **Sign-Out URL-cím**, amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="57bf7-166">In **SAML Logout Redirect (optional)**, paste the value of **Sign-Out URL**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="57bf7-167">Nyissa meg a **base-64 kódolású tanúsítvány** Azure-portálról letöltött a Jegyzettömbben.</span><span class="sxs-lookup"><span data-stu-id="57bf7-167">Open your **base-64 encoded certificate** in Notepad, downloaded from the Azure portal.</span></span> <span data-ttu-id="57bf7-168">A tartalmának másolása a vágólapra és illessze be azt a **SAML tanúsítvány** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="57bf7-168">Copy the content of it into your clipboard, and then paste it to the **SAML Certificate** text box.</span></span>

11. <span data-ttu-id="57bf7-169">Szükség lehet a célközönség URL-címet, címzett alatt felsorolva, és ACS (helyességi feltétel fogyasztói szolgáltatás) URL-címe, a **SAML-alapú konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="57bf7-169">You might need to enter the audience URL, recipient URL, and ACS (Assertion Consumer Service) URL, listed under the **SAML Configuration** section.</span></span>

12. <span data-ttu-id="57bf7-170">A szakasz alján görgessen, és válassza ki **teszt SSO**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-170">Scroll to the bottom of the section, and select **Test SSO**.</span></span> <span data-ttu-id="57bf7-171">Egy előugró ablak jelenik meg, az az Azure AD bejelentkezési oldal.</span><span class="sxs-lookup"><span data-stu-id="57bf7-171">A pop-up window appears, with the Azure AD sign-in page.</span></span> <span data-ttu-id="57bf7-172">A hitelesítéshez adja meg a hitelesítő adatok szokásos módon.</span><span class="sxs-lookup"><span data-stu-id="57bf7-172">To authenticate, enter your credentials as normal.</span></span> <span data-ttu-id="57bf7-173">Győződjön meg arról a címre küldi vissza az Azure AD vissza a ugyanaz a munkahelyi fiókkal jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="57bf7-173">Ensure the email address returned back from Azure AD is the same as the Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="57bf7-174">Ha a vizsgálat sikeresen befejeződött, görgessen a lap, és válasszon alján **mentése**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-174">If the test has finished successfully, scroll to the bottom of the page and select **Save**.</span></span>

14. <span data-ttu-id="57bf7-175">Munkahelyi felhasználója számára most jelenik meg az Azure AD bejelentkezési oldalára hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="57bf7-175">Anyone using Workplace is now presented with Azure AD sign-in page for authentication.</span></span>

<span data-ttu-id="57bf7-176">Kiválaszthatja, konfigurálhatja az egy URL-cím, az Azure AD kijelentkezési oldalon ponthoz használható SAML kijelentkezett.</span><span class="sxs-lookup"><span data-stu-id="57bf7-176">You can choose to configure a SAML sign out URL, which can be used to point at the Azure AD sign-out page.</span></span> <span data-ttu-id="57bf7-177">Ha ezt a beállítást engedélyezve és konfigurálva van, a felhasználó már nem irányítja a rendszer a munkahelyi kijelentkezési lapra.</span><span class="sxs-lookup"><span data-stu-id="57bf7-177">When this setting is enabled and configured, the user is no longer directed to the Workplace sign-out page.</span></span> <span data-ttu-id="57bf7-178">Ehelyett a felhasználó átirányítási URL-címet, amely az SAML kijelentkezési átirányítási beállítás lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="57bf7-178">Instead, the user is redirected to the URL that was added in the SAML sign-out redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="57bf7-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="57bf7-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app.</span></span> <span data-ttu-id="57bf7-180">Ez az alkalmazás a hozzáadása után a **Active Directory** > **vállalati alkalmazások** szakaszban egyszerűen jelölje be a **egyszeri bejelentkezés** lapra, és elérheti a beágyazott keresztül dokumentáció a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="57bf7-180">After adding this app from the **Active Directory** > **Enterprise Applications** section, simply select the **Single Sign-On** tab, and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="57bf7-181">További embedded dokumentációjából szolgáltatásáról a [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="57bf7-181">You can read more about the embedded documentation feature in the [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="configure-reauthentication-frequency"></a><span data-ttu-id="57bf7-182">Újrahitelesítés gyakoriságának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57bf7-182">Configure reauthentication frequency</span></span>

<span data-ttu-id="57bf7-183">Beállíthatja a munkahelyi bekéri a egy SAML-ellenőrzést minden nap, három nap, egy hét, a két hét, egy hónap, vagy egyáltalán nem.</span><span class="sxs-lookup"><span data-stu-id="57bf7-183">You can configure Workplace to prompt for a SAML check every day, three days, one week, two weeks, one month, or never.</span></span>

> [!NOTE] 
><span data-ttu-id="57bf7-184">A mobilalkalmazás az SAML-ellenőrzését minimális értéke egy hét.</span><span class="sxs-lookup"><span data-stu-id="57bf7-184">The minimum value for the SAML check on mobile applications is set to one week.</span></span>

<span data-ttu-id="57bf7-185">A SAML alaphelyzetbe állítja az összes felhasználó számára is kényszeríthető.</span><span class="sxs-lookup"><span data-stu-id="57bf7-185">You can also force a SAML reset for all users.</span></span> <span data-ttu-id="57bf7-186">Ehhez használja a **szükséges SAML-alapú hitelesítés az összes felhasználó most** gombra.</span><span class="sxs-lookup"><span data-stu-id="57bf7-186">To do this, use the **Require SAML authentication for all users now** button.</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="57bf7-187">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="57bf7-187">Create an Azure AD test user</span></span>
<span data-ttu-id="57bf7-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="57bf7-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

1. <span data-ttu-id="57bf7-190">Az a **Azure-portálon**, a bal oldali panelen válassza ki a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-190">In the **Azure portal**, in the left pane, select **Azure Active Directory**.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="57bf7-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-192">To display the list of users, go to **Users and groups**, and select **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="57bf7-194">Lehetőségre a **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-194">To open the **User** dialog box, select **Add**.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="57bf7-196">Az a **felhasználói** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="57bf7-196">In the **User** dialog box, do the following:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="57bf7-198">a.</span><span class="sxs-lookup"><span data-stu-id="57bf7-198">a.</span></span> <span data-ttu-id="57bf7-199">Az a **neve** szövegmezőben **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-199">In the **Name** text box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="57bf7-200">b.</span><span class="sxs-lookup"><span data-stu-id="57bf7-200">b.</span></span> <span data-ttu-id="57bf7-201">Az a **felhasználónév** mezőbe, írja be a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="57bf7-201">In the **User name** text box, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="57bf7-202">c.</span><span class="sxs-lookup"><span data-stu-id="57bf7-202">c.</span></span> <span data-ttu-id="57bf7-203">Válassza ki **megjelenítése jelszó**, és írja le.</span><span class="sxs-lookup"><span data-stu-id="57bf7-203">Select **Show Password**, and write it down.</span></span>

    <span data-ttu-id="57bf7-204">d.</span><span class="sxs-lookup"><span data-stu-id="57bf7-204">d.</span></span> <span data-ttu-id="57bf7-205">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="57bf7-205">Select **Create**.</span></span>
 
### <a name="create-a-workplace-by-facebook-test-user"></a><span data-ttu-id="57bf7-206">Hozzon létre egy munkahelyi Facebook teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="57bf7-206">Create a Workplace by Facebook test user</span></span>

<span data-ttu-id="57bf7-207">Ebben a szakaszban Britta Simon nevű felhasználó létrehozta a munkaterületen Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="57bf7-207">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="57bf7-208">Munkahelyi Facebook által támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="57bf7-208">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="57bf7-209">Nincs olyan művelet, ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="57bf7-209">There is no action for you in this section.</span></span> <span data-ttu-id="57bf7-210">Ha a felhasználó nem létezik a munkahely által Facebook, egy új hozta létre munkahelyi elérésére tett kísérlet során Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="57bf7-210">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt to access Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="57bf7-211">Ha a felhasználó manuális létrehozása, forduljon a [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/).</span><span class="sxs-lookup"><span data-stu-id="57bf7-211">If you need to create a user manually, contact the [Workplace by Facebook Client support team](https://workplace.fb.com/faq/).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="57bf7-212">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="57bf7-212">Assign the Azure AD test user</span></span>

<span data-ttu-id="57bf7-213">Ebben a szakaszban Azure SSO nyújtó munkahelyi Facebook által használandó Britta Simon engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="57bf7-213">In this section, you enable Britta Simon to use Azure SSO by granting access to Workplace by Facebook.</span></span>

![Felhasználó hozzárendelése][200] 

1. <span data-ttu-id="57bf7-215">Nyissa meg az alkalmazások megtekintése az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="57bf7-215">In the Azure portal, open the applications view.</span></span> <span data-ttu-id="57bf7-216">A könyvtár nézet megnyitása, írja be a **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-216">Go to the directory view, go to **Enterprise applications**, and then select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="57bf7-218">Az alkalmazások listában válassza ki a **által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-218">In the applications list, select **Workplace by Facebook**.</span></span>

    ![A munkahely által az alkalmazások listáját a Facebook-hivatkozás](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="57bf7-220">A bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-220">In the menu on the left, select **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. <span data-ttu-id="57bf7-222">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="57bf7-222">Select **Add**.</span></span> <span data-ttu-id="57bf7-223">Ezt követően a a **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-223">Then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="57bf7-225">Az a **felhasználók és csoportok** párbeszédpanelen jelölje ki **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="57bf7-225">In the **Users and groups** dialog box, select **Britta Simon** in the users list.</span></span>

6. <span data-ttu-id="57bf7-226">Az a **felhasználók és csoportok** párbeszédpanelen jelölje ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-226">In the **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="57bf7-227">Az a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="57bf7-227">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="57bf7-228">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="57bf7-228">Test single sign-on</span></span>

<span data-ttu-id="57bf7-229">Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="57bf7-229">If you want to test your SSO settings, open the Access Panel.</span></span>
<span data-ttu-id="57bf7-230">További információkért lásd: [Bevezetés a Hozzáférési panel használatába](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="57bf7-230">For more information, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="57bf7-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57bf7-231">Next steps</span></span>

* <span data-ttu-id="57bf7-232">Tekintse meg a [SaaS-alkalmazásokhoz az Azure Active Directoryval integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="57bf7-232">See the [list of tutorials on how to integrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md).</span></span>
* <span data-ttu-id="57bf7-233">Olvasási [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="57bf7-233">Read [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>
* <span data-ttu-id="57bf7-234">További tudnivalók a [konfigurálja, a felhasználók átadása](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="57bf7-234">Learn more about how to [configure user provisioning](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span></span>



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

