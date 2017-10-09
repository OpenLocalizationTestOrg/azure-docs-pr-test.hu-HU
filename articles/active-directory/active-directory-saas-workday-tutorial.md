---
title: "Oktatóanyag: Azure Active Directoryval integrált Workday |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Workday között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: aaa41e372e45f464c4540a70fc79c98dbc06d6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a><span data-ttu-id="e3ae8-103">Oktatóanyag: Azure Active Directoryval integrált munkanap</span><span class="sxs-lookup"><span data-stu-id="e3ae8-103">Tutorial: Azure Active Directory integration with Workday</span></span>

<span data-ttu-id="e3ae8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Workday az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-104">In this tutorial, you learn how toointegrate Workday with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e3ae8-105">Munkanapok integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-105">Integrating Workday with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e3ae8-106">Megadhatja a hozzáférés tooWorkday rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e3ae8-106">You can control in Azure AD who has access tooWorkday</span></span>
- <span data-ttu-id="e3ae8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkday (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e3ae8-107">You can enable your users tooautomatically get signed-on tooWorkday (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e3ae8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e3ae8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e3ae8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3ae8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e3ae8-110">Prerequisites</span></span>

<span data-ttu-id="e3ae8-111">az Azure AD-integráció a Workday tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-111">tooconfigure Azure AD integration with Workday, you need hello following items:</span></span>

- <span data-ttu-id="e3ae8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e3ae8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e3ae8-113">A Workday egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="e3ae8-113">A Workday single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e3ae8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e3ae8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e3ae8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e3ae8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e3ae8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e3ae8-118">Scenario description</span></span>
<span data-ttu-id="e3ae8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e3ae8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e3ae8-121">Hozzáadás a Workday hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e3ae8-121">Adding Workday from hello gallery</span></span>
2. <span data-ttu-id="e3ae8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e3ae8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workday-from-hello-gallery"></a><span data-ttu-id="e3ae8-123">Hozzáadás a Workday hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e3ae8-123">Adding Workday from hello gallery</span></span>
<span data-ttu-id="e3ae8-124">tooconfigure hello integrációs munkanap, az Azure AD-be, szükség tooadd Workday a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-124">tooconfigure hello integration of Workday into Azure AD, you need tooadd Workday from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e3ae8-125">**tooadd Workday hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e3ae8-125">**tooadd Workday from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e3ae8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e3ae8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e3ae8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e3ae8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e3ae8-133">Hello keresési mezőbe, írja be a **Workday**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-133">In hello search box, type **Workday**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. <span data-ttu-id="e3ae8-135">A hello eredmények panelen válassza ki a **Workday**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-135">In hello results panel, select **Workday**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e3ae8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e3ae8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e3ae8-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Workday "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="e3ae8-138">In this section, you configure and test Azure AD single sign-on with Workday based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e3ae8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Workday tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workday is tooa user in Azure AD.</span></span> <span data-ttu-id="e3ae8-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Workday közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-140">In other words, a link relationship between an Azure AD user and hello related user in Workday needs toobe established.</span></span>

<span data-ttu-id="e3ae8-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Workday.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workday.</span></span>

<span data-ttu-id="e3ae8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Workday-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-142">tooconfigure and test Azure AD single sign-on with Workday, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e3ae8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e3ae8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e3ae8-145">**[Munkanapok tesztfelhasználó létrehozása](#creating-a-workday-test-user)**  -toohave egy megfelelője a Britta Simon a Workday, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-145">**[Creating a Workday test user](#creating-a-workday-test-user)** - toohave a counterpart of Britta Simon in Workday that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e3ae8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e3ae8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e3ae8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3ae8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e3ae8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Workday-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workday application.</span></span>

<span data-ttu-id="e3ae8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Workday, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e3ae8-150">**tooconfigure Azure AD single sign-on with Workday, perform hello following steps:**</span></span>

1. <span data-ttu-id="e3ae8-151">Az Azure portál, a hello hello **Workday** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-151">In hello Azure portal, on hello **Workday** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e3ae8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. <span data-ttu-id="e3ae8-155">A hello **Workday tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-155">On hello **Workday Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    <span data-ttu-id="e3ae8-157">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-157">a.</span></span> <span data-ttu-id="e3ae8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://impl.workday.com/<tenant>/login-saml2.htmld`</span><span class="sxs-lookup"><span data-stu-id="e3ae8-158">In hello **Sign-on URL** textbox, type hello value as: `https://impl.workday.com/<tenant>/login-saml2.htmld`</span></span>

    <span data-ttu-id="e3ae8-159">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-159">b.</span></span> <span data-ttu-id="e3ae8-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://impl.workday.com/<tenant>/login-saml.htmld`</span><span class="sxs-lookup"><span data-stu-id="e3ae8-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://impl.workday.com/<tenant>/login-saml.htmld`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e3ae8-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-161">These values are not hello real.</span></span> <span data-ttu-id="e3ae8-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-162">Update these values with hello actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="e3ae8-163">A válasz URL-címe például altartomány kell rendelkeznie: www, wd2, wd3, wd3-impl, wd5, wd5-impl).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-163">Your reply URL must have a subdomain for example: www, wd2, wd3, wd3-impl, wd5, wd5-impl).</span></span> <span data-ttu-id="e3ae8-164">Valami, amit például "*http://www.myworkday.com*" működik, de "*http://myworkday.com*" viszont nem.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-164">Using something like "*http://www.myworkday.com*" works but "*http://myworkday.com*" does not.</span></span> <span data-ttu-id="e3ae8-165">Ügyfél [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-165">Contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) tooget these values.</span></span> 
 

4. <span data-ttu-id="e3ae8-166">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. <span data-ttu-id="e3ae8-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e3ae8-170">A hello **Workday konfigurációs** területén kattintson **konfigurálása Workday** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-170">On hello **Workday Configuration** section, click **Configure Workday** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e3ae8-171">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e3ae8-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="e3ae8-172">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="e3ae8-172">![Configure Single Sign-On](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span></span>
7. <span data-ttu-id="e3ae8-173">Egy másik webes böngészőablakban jelentkezzen tooyour Workday vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-173">In a different web browser window, log in tooyour Workday company site as an administrator.</span></span>

8. <span data-ttu-id="e3ae8-174">Nyissa meg túl**menü \> munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-174">Go too**Menu \> Workbench**.</span></span>
   
    <span data-ttu-id="e3ae8-175">![Munkaterület](./media/active-directory-saas-workday-tutorial/IC782923.png "munkaterület")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span></span>

9. <span data-ttu-id="e3ae8-176">Nyissa meg túl**fiók felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-176">Go too**Account Administration**.</span></span>
   
    <span data-ttu-id="e3ae8-177">![Felügyeleti fiók](./media/active-directory-saas-workday-tutorial/IC782924.png "felügyeleti fiók")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-177">![Account Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")</span></span>

10. <span data-ttu-id="e3ae8-178">Nyissa meg túl**bérlői beállításának szerkesztése – biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-178">Go too**Edit Tenant Setup – Security**.</span></span>
   
    <span data-ttu-id="e3ae8-179">![Bérlői biztonsági szerkesztése](./media/active-directory-saas-workday-tutorial/IC782925.png "bérlői biztonsági szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-179">![Edit Tenant Security](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")</span></span>

11. <span data-ttu-id="e3ae8-180">A hello **átirányítási URL-eket** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-180">In hello **Redirection URLs** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="e3ae8-181">![Átirányítási URL-eket](./media/active-directory-saas-workday-tutorial/IC7829581.png "átirányítási URL-címek")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-181">![Redirection URLs](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")</span></span>
   
    <span data-ttu-id="e3ae8-182">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-182">a.</span></span> <span data-ttu-id="e3ae8-183">Kattintson a **sort ad hozzá a**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-183">Click **Add Row**.</span></span>
   
    <span data-ttu-id="e3ae8-184">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-184">b.</span></span> <span data-ttu-id="e3ae8-185">A hello **bejelentkezési átirányítási URL-cím** szövegmező és hello **Mobile átirányítási URL-cím** szövegmezőhöz típus hello **bejelentkezési URL-cím** hello megadott **Workday tartományi és URL-címek** hello Azure-portálon szakasza.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-185">In hello **Login Redirect URL** textbox and hello **Mobile Redirect URL** textbox, type hello **Sign-on URL** you have entered on hello **Workday Domain and URLs** section of hello Azure portal.</span></span>
   
    <span data-ttu-id="e3ae8-186">c.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-186">c.</span></span> <span data-ttu-id="e3ae8-187">Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **Sign-Out URL-cím**, és illessze be hello **kijelentkezési átirányítási URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-187">In hello Azure portal, on hello **Configure sign-on** window, copy hello **Sign-Out URL**, and then paste it into hello **Logout Redirect URL** textbox.</span></span>
   
    <span data-ttu-id="e3ae8-188">d.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-188">d.</span></span>  <span data-ttu-id="e3ae8-189">A **környezet** szövegmezőhöz hello környezet neve.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-189">In **Environment** textbox, type hello environment name.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="e3ae8-190">hello hello környezet attribútum értékének kötődik toohello értékének hello bérlői URL-cím:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-190">hello value of hello Environment attribute is tied toohello value of hello tenant URL:</span></span>  
    ><span data-ttu-id="e3ae8-191">-Ha hello tartomány neve hello Workday-bérlői URL-cím kezdődik impl például: *https://impl.workday.com/\<bérlői\>/login-saml2.htmld*), hello **környezet** attribútum tooImplementation kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-191">-If hello domain name of hello Workday tenant URL starts with impl for example: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), hello **Environment** attribute must be set tooImplementation.</span></span>  
    ><span data-ttu-id="e3ae8-192">-Ha hello tartománynév kezdődik-e valami mást, szükség van-e toocontact [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello megfelelő **környezet** érték.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-192">-If hello domain name starts with something else, you need toocontact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello matching **Environment** value.</span></span>

12. <span data-ttu-id="e3ae8-193">A hello **SAML-alapú telepítő** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-193">In hello **SAML Setup** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="e3ae8-194">![SAML-alapú telepítő](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-alapú telepítő")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-194">![SAML Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")</span></span>
   
    <span data-ttu-id="e3ae8-195">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-195">a.</span></span>  <span data-ttu-id="e3ae8-196">Válassza ki **SAML-alapú hitelesítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-196">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="e3ae8-197">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-197">b.</span></span>  <span data-ttu-id="e3ae8-198">Kattintson a **sort ad hozzá a**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-198">Click **Add Row**.</span></span>

13. <span data-ttu-id="e3ae8-199">A SAML-alapú identitás-szolgáltatóktól szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-199">In hello SAML Identity Providers section, perform hello following steps:</span></span>
   
    <span data-ttu-id="e3ae8-200">![SAML-alapú identitás-szolgáltatóktól](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-alapú identitás-szolgáltatóktól")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-200">![SAML Identity Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")</span></span>
   
    <span data-ttu-id="e3ae8-201">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-201">a.</span></span> <span data-ttu-id="e3ae8-202">Hello Identity Provider szövegmezőben írja be a szolgáltató nevét (például: *SPInitiatedSSO*).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-202">In hello Identity Provider Name textbox, type a provider name (for example: *SPInitiatedSSO*).</span></span>
   
    <span data-ttu-id="e3ae8-203">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-203">b.</span></span> <span data-ttu-id="e3ae8-204">Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **SAML Entitásazonosító** értékét, és illessze be hello **kibocsátó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-204">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Entity ID** value, and then paste it into hello **Issuer** textbox.</span></span>
   
    <span data-ttu-id="e3ae8-205">c.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-205">c.</span></span> <span data-ttu-id="e3ae8-206">Válassza ki **engedélyezése a Workday kezdeményezett kijelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-206">Select **Enable Workday Initiated Logout**.</span></span>
   
    <span data-ttu-id="e3ae8-207">d.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-207">d.</span></span> <span data-ttu-id="e3ae8-208">Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **Sign-Out URL-cím** értékét, és illessze be hello **kijelentkezési kérelem URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-208">In hello Azure portal, on hello **Configure sign-on** window, copy hello **Sign-Out URL** value, and then paste it into hello **Logout Request URL** textbox.</span></span>

    <span data-ttu-id="e3ae8-209">e.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-209">e.</span></span> <span data-ttu-id="e3ae8-210">Kattintson a **szolgáltató nyilvános kulcs Identitástanúsítvány**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-210">Click **Identity Provider Public Key Certificate**, and then click **Create**.</span></span> 

    <span data-ttu-id="e3ae8-211">![Hozzon létre](./media/active-directory-saas-workday-tutorial/IC782928.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-211">![Create](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")</span></span>

    <span data-ttu-id="e3ae8-212">f.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-212">f.</span></span> <span data-ttu-id="e3ae8-213">Kattintson a **x509 létrehozása nyilvános kulcs**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-213">Click **Create x509 Public Key**.</span></span> 

    <span data-ttu-id="e3ae8-214">![Hozzon létre](./media/active-directory-saas-workday-tutorial/IC782929.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-214">![Create](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")</span></span>


14. <span data-ttu-id="e3ae8-215">A hello **nézet x509 nyilvános kulcs** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-215">In hello **View x509 Public Key** section, perform hello following steps:</span></span> 
   
    <span data-ttu-id="e3ae8-216">![Nézet x509 nyilvános kulcs](./media/active-directory-saas-workday-tutorial/IC782930.png "nézet x509 nyilvános kulcs")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-216">![View x509 Public Key](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key")</span></span> 
   
    <span data-ttu-id="e3ae8-217">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-217">a.</span></span> <span data-ttu-id="e3ae8-218">A hello **neve** szövegmező, írja be a tanúsítvány nevét (például: *PPE\_SP*).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-218">In hello **Name** textbox, type a name for your certificate (for example: *PPE\_SP*).</span></span>
   
    <span data-ttu-id="e3ae8-219">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-219">b.</span></span> <span data-ttu-id="e3ae8-220">A hello **érvényes a** szövegmezőhöz típus hello be a tanúsítványhoz tartozó attribútum értéke érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-220">In hello **Valid From** textbox, type hello valid from attribute value of your certificate.</span></span>
   
    <span data-ttu-id="e3ae8-221">c.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-221">c.</span></span>  <span data-ttu-id="e3ae8-222">A hello **érvényes való** szövegmezőhöz típus hello érvényes tooattribute érték a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-222">In hello **Valid To** textbox, type hello valid tooattribute value of your certificate.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="e3ae8-223">Beszerezni hello érvényes dátum, és kattintson duplán a letöltött hello tanúsítványból érvényes toodate hello.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-223">You can get hello valid from date and hello valid toodate from hello downloaded certificate by double-clicking it.</span></span>  <span data-ttu-id="e3ae8-224">hello dátumok találhatók hello **részletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-224">hello dates are listed under hello **Details** tab.</span></span>
    > 
    >
   
    <span data-ttu-id="e3ae8-225">d.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-225">d.</span></span>  <span data-ttu-id="e3ae8-226">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben, és azt a tartalom másolás hello.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-226">Open your base-64 encoded certificate in notepad, and then copy hello content of it.</span></span>
   
    <span data-ttu-id="e3ae8-227">e.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-227">e.</span></span>  <span data-ttu-id="e3ae8-228">A hello **tanúsítvány** szövegmezőhöz a vágólap tartalmának beillesztése hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-228">In hello **Certificate** textbox, paste hello content of your clipboard.</span></span>
   
    <span data-ttu-id="e3ae8-229">f.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-229">f.</span></span>  <span data-ttu-id="e3ae8-230">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-230">Click **OK**.</span></span>

15. <span data-ttu-id="e3ae8-231">Hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-231">Perform hello following steps:</span></span> 
   
    <span data-ttu-id="e3ae8-232">![Egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-232">![SSO configuration](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO configuration")</span></span>
   
    <span data-ttu-id="e3ae8-233">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-233">a.</span></span>  <span data-ttu-id="e3ae8-234">Hello engedélyezése **x509 titkos kulcspár**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-234">Enable hello **x509 Private Key Pair**.</span></span>
   
    <span data-ttu-id="e3ae8-235">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-235">b.</span></span>  <span data-ttu-id="e3ae8-236">A hello **szolgáltatás Szolgáltatóazonosító** szövegmezőhöz típus **http://www.workday.com**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-236">In hello **Service Provider ID** textbox, type **http://www.workday.com**.</span></span>
   
    <span data-ttu-id="e3ae8-237">c.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-237">c.</span></span>  <span data-ttu-id="e3ae8-238">Válassza ki **engedélyezése SP kezdeményezett SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-238">Select **Enable SP Initiated SAML Authentication**.</span></span>
   
    <span data-ttu-id="e3ae8-239">d.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-239">d.</span></span>  <span data-ttu-id="e3ae8-240">Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **IdP egyszeri bejelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-240">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **IdP SSO Service URL** textbox.</span></span>
   
    <span data-ttu-id="e3ae8-241">e.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-241">e.</span></span> <span data-ttu-id="e3ae8-242">Válassza ki **nem Deflate a Szolgáltató által kezdeményezett hitelesítési kérelem**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-242">Select **Do Not Deflate SP-initiated Authentication Request**.</span></span>
   
    <span data-ttu-id="e3ae8-243">f.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-243">f.</span></span> <span data-ttu-id="e3ae8-244">Mint **hitelesítési kérelmek aláírási metódus**, jelölje be **SHA256**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-244">As **Authentication Request Signature Method**, select **SHA256**.</span></span> 
   
    <span data-ttu-id="e3ae8-245">![Hitelesítési kérelem aláírási metódus](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "hitelesítési kérelmek aláírás módja")</span><span class="sxs-lookup"><span data-stu-id="e3ae8-245">![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "Authentication Request Signature Method")</span></span> 
   
    <span data-ttu-id="e3ae8-246">g.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-246">g.</span></span> <span data-ttu-id="e3ae8-247">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-247">Click **OK**.</span></span> 
   
    <span data-ttu-id="e3ae8-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span><span class="sxs-lookup"><span data-stu-id="e3ae8-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span></span>

> [!TIP]
> <span data-ttu-id="e3ae8-249">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e3ae8-249">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e3ae8-250">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-250">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e3ae8-251">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e3ae8-251">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e3ae8-252">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e3ae8-252">Creating an Azure AD test user</span></span>
<span data-ttu-id="e3ae8-253">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-253">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e3ae8-255">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e3ae8-255">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e3ae8-256">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-256">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e3ae8-258">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-258">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e3ae8-260">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-260">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e3ae8-262">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3ae8-262">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e3ae8-264">a.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-264">a.</span></span> <span data-ttu-id="e3ae8-265">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-265">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e3ae8-266">b.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-266">b.</span></span> <span data-ttu-id="e3ae8-267">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-267">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e3ae8-268">c.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-268">c.</span></span> <span data-ttu-id="e3ae8-269">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-269">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e3ae8-270">d.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-270">d.</span></span> <span data-ttu-id="e3ae8-271">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-271">Click **Create**.</span></span>
 
### <a name="creating-a-workday-test-user"></a><span data-ttu-id="e3ae8-272">Munkanapok tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e3ae8-272">Creating a Workday test user</span></span>

<span data-ttu-id="e3ae8-273">tooget át az Workday tesztfelhasználó, kell toocontact hello [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-273">tooget a test user provisioned into Workday, you need toocontact hello [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html).</span></span>
<span data-ttu-id="e3ae8-274">Hello [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html) létrehozza, hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-274">hello [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) creates hello user for you.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e3ae8-275">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e3ae8-275">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e3ae8-276">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWorkday megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-276">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkday.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e3ae8-278">**tooassign Britta Simon tooWorkday, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e3ae8-278">**tooassign Britta Simon tooWorkday, perform hello following steps:**</span></span>

1. <span data-ttu-id="e3ae8-279">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-279">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e3ae8-281">Hello alkalmazások listában válassza ki a **Workday**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-281">In hello applications list, select **Workday**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. <span data-ttu-id="e3ae8-283">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-283">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e3ae8-285">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-285">Click **Add** button.</span></span> <span data-ttu-id="e3ae8-286">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e3ae8-288">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-288">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e3ae8-289">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e3ae8-290">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e3ae8-291">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e3ae8-291">Testing single sign-on</span></span>

<span data-ttu-id="e3ae8-292">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="e3ae8-292">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="e3ae8-293">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e3ae8-293">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3ae8-294">További források</span><span class="sxs-lookup"><span data-stu-id="e3ae8-294">Additional resources</span></span>

* [<span data-ttu-id="e3ae8-295">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e3ae8-295">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e3ae8-296">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e3ae8-296">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="e3ae8-297">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3ae8-297">Configure User Provisioning</span></span>](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

