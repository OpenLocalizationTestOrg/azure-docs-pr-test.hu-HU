---
title: "Oktatóanyag: Azure Active Directoryval integrált felismerése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és felismerése között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: f33fc3959f72f875b8c5c4f0abd4e9b6737ca615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a><span data-ttu-id="8efa8-103">Oktatóanyag: Azure Active Directoryval integrált felismerése</span><span class="sxs-lookup"><span data-stu-id="8efa8-103">Tutorial: Azure Active Directory integration with Recognize</span></span>

<span data-ttu-id="8efa8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ismeri fel az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8efa8-104">In this tutorial, you learn how toointegrate Recognize with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8efa8-105">Felismerése integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8efa8-105">Integrating Recognize with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8efa8-106">Megadhatja a hozzáférés tooRecognize rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8efa8-106">You can control in Azure AD who has access tooRecognize</span></span>
- <span data-ttu-id="8efa8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRecognize (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8efa8-107">You can enable your users tooautomatically get signed-on tooRecognize (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8efa8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8efa8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8efa8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8efa8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8efa8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8efa8-110">Prerequisites</span></span>

<span data-ttu-id="8efa8-111">tooconfigure az Azure AD-integrációs felismerése, az a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="8efa8-111">tooconfigure Azure AD integration with Recognize, you need hello following items:</span></span>

- <span data-ttu-id="8efa8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8efa8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8efa8-113">Egy felismerése egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8efa8-113">A Recognize single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8efa8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8efa8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8efa8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8efa8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8efa8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8efa8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8efa8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8efa8-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8efa8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8efa8-118">Scenario description</span></span>
<span data-ttu-id="8efa8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8efa8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8efa8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8efa8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8efa8-121">Hello gyűjteményből felismerése hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8efa8-121">Adding Recognize from hello gallery</span></span>
2. <span data-ttu-id="8efa8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8efa8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-recognize-from-hello-gallery"></a><span data-ttu-id="8efa8-123">Hello gyűjteményből felismerése hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8efa8-123">Adding Recognize from hello gallery</span></span>
<span data-ttu-id="8efa8-124">tooconfigure hello integrációja felismerése az Azure AD-be, meg kell tooadd felismerése hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8efa8-124">tooconfigure hello integration of Recognize into Azure AD, you need tooadd Recognize from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8efa8-125">**tooadd felismerése hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8efa8-125">**tooadd Recognize from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8efa8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8efa8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8efa8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8efa8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8efa8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8efa8-133">Hello keresési mezőbe, írja be a **felismerése**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-133">In hello search box, type **Recognize**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. <span data-ttu-id="8efa8-135">A hello eredmények panelen válassza ki a **felismerése**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-135">In hello results panel, select **Recognize**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8efa8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8efa8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8efa8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján felismerése.</span><span class="sxs-lookup"><span data-stu-id="8efa8-138">In this section, you configure and test Azure AD single sign-on with Recognize based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8efa8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói felismerése a tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8efa8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Recognize is tooa user in Azure AD.</span></span> <span data-ttu-id="8efa8-140">Ez azt jelenti hello kapcsolódó felhasználó a felismerése és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8efa8-140">In other words, a link relationship between an Azure AD user and hello related user in Recognize needs toobe established.</span></span>

<span data-ttu-id="8efa8-141">Felismerése, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8efa8-141">In Recognize, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8efa8-142">tooconfigure és tesztelése az Azure AD-egyszeri bejelentkezés az felismerése, a következő építőelemeket toocomplete hello kell:</span><span class="sxs-lookup"><span data-stu-id="8efa8-142">tooconfigure and test Azure AD single sign-on with Recognize, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8efa8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8efa8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8efa8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8efa8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8efa8-145">**[Felismerése tesztfelhasználó létrehozása](#creating-a-recognize-test-user)**  -toohave egy megfelelője a Britta Simon a felismerése, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8efa8-145">**[Creating a Recognize test user](#creating-a-recognize-test-user)** - toohave a counterpart of Britta Simon in Recognize that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8efa8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8efa8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8efa8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8efa8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8efa8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8efa8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8efa8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az felismerése alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8efa8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Recognize application.</span></span>

<span data-ttu-id="8efa8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a felismerése, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8efa8-150">**tooconfigure Azure AD single sign-on with Recognize, perform hello following steps:**</span></span>

1. <span data-ttu-id="8efa8-151">Az Azure portál, a hello hello **felismerése** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-151">In hello Azure portal, on hello **Recognize** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8efa8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8efa8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. <span data-ttu-id="8efa8-155">A hello **ismeri fel a tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8efa8-155">On hello **Recognize Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    <span data-ttu-id="8efa8-157">a.</span><span class="sxs-lookup"><span data-stu-id="8efa8-157">a.</span></span> <span data-ttu-id="8efa8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://recognizeapp.com/<your-domain>/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="8efa8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://recognizeapp.com/<your-domain>/saml/sso`</span></span>

    <span data-ttu-id="8efa8-159">b.</span><span class="sxs-lookup"><span data-stu-id="8efa8-159">b.</span></span> <span data-ttu-id="8efa8-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://recognizeapp.com/<your-domain>`</span><span class="sxs-lookup"><span data-stu-id="8efa8-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://recognizeapp.com/<your-domain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8efa8-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8efa8-161">These values are not real.</span></span> <span data-ttu-id="8efa8-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="8efa8-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8efa8-163">Ügyfél [ismeri fel az ügyfél-támogatási csoport](mailto:support@recognizeapp.com) bejelentkezési URL-cím, és akkor beszerezheti azonosító értéket hello szolgáltatás szolgáltató metaadatainak URL-CÍMÉT az egyszeri bejelentkezési beállítások szakaszban hello oktatóanyag későbbi részében kifejtett hello megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="8efa8-163">Contact [Recognize Client support team](mailto:support@recognizeapp.com) to get Sign-On URL and you can get Identifier value by opening hello Service Provider Metadata URL from hello SSO Settings section that is explained later in hello tutorial.</span></span> <span data-ttu-id="8efa8-164">.</span><span class="sxs-lookup"><span data-stu-id="8efa8-164">.</span></span> 
 
4. <span data-ttu-id="8efa8-165">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8efa8-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. <span data-ttu-id="8efa8-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8efa8-169">A hello **ismeri fel a konfigurációs** kattintson **konfigurálása ismeri fel** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8efa8-169">On hello **Recognize Configuration** section, click **Configure Recognize** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8efa8-170">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8efa8-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. <span data-ttu-id="8efa8-172">Egy másik webes böngészőablakban, bejelentkezés tooyour felismerése Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="8efa8-172">In a different web browser window, sign-on tooyour Recognize tenant as an administrator.</span></span>

8. <span data-ttu-id="8efa8-173">A hello jobb felső sarokban, kattintson **menü**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-173">On hello upper right corner, click **Menu**.</span></span> <span data-ttu-id="8efa8-174">Nyissa meg túl**vállalati rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-174">Go too**Company Admin**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. <span data-ttu-id="8efa8-176">A hello bal oldali navigációs ablaktábláján kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-176">On hello left navigation pane, click **Settings**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. <span data-ttu-id="8efa8-178">Hajtsa végre a következő lépéseket hello **egyszeri bejelentkezési beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8efa8-178">Perform hello following steps on **SSO Settings** section.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    <span data-ttu-id="8efa8-180">a.</span><span class="sxs-lookup"><span data-stu-id="8efa8-180">a.</span></span> <span data-ttu-id="8efa8-181">Mint **SSO engedélyezése**, jelölje be **ON**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-181">As **Enable SSO**, select **ON**.</span></span>

    <span data-ttu-id="8efa8-182">b.</span><span class="sxs-lookup"><span data-stu-id="8efa8-182">b.</span></span> <span data-ttu-id="8efa8-183">A hello **IDP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8efa8-183">In hello **IDP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="8efa8-184">c.</span><span class="sxs-lookup"><span data-stu-id="8efa8-184">c.</span></span> <span data-ttu-id="8efa8-185">A hello **Sso célként megadott url** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8efa8-185">In hello **Sso target url** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="8efa8-186">d.</span><span class="sxs-lookup"><span data-stu-id="8efa8-186">d.</span></span> <span data-ttu-id="8efa8-187">A hello **Slo-cél URL-címe** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8efa8-187">In hello **Slo target url** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span> 
    
    <span data-ttu-id="8efa8-188">e.</span><span class="sxs-lookup"><span data-stu-id="8efa8-188">e.</span></span> <span data-ttu-id="8efa8-189">Nyissa meg a letöltött **tanúsítvány (Base64)** fájlt a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8efa8-189">Open your downloaded **Certificate (Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** textbox.</span></span>
    
    <span data-ttu-id="8efa8-190">f.</span><span class="sxs-lookup"><span data-stu-id="8efa8-190">f.</span></span> <span data-ttu-id="8efa8-191">Kattintson a hello **beállítások mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-191">Click hello **Save settings** button.</span></span> 

11. <span data-ttu-id="8efa8-192">Hello mellett **egyszeri bejelentkezési beállítások** területen alatt hello URL-Címének másolása **Service Provider metaadatainak URL-címét**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-192">Beside hello **SSO Settings** section, copy hello URL under **Service Provider Metadata url**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. <span data-ttu-id="8efa8-194">Nyissa meg hello **metaadatainak URL-CÍMÉT hivatkozás** egy üres böngésző toodownload hello metaadat-dokumentum szerint.</span><span class="sxs-lookup"><span data-stu-id="8efa8-194">Open hello **Metadata URL link** under a blank browser toodownload hello metadata document.</span></span> <span data-ttu-id="8efa8-195">Majd hello EntityDescriptor value(entityID) hello fájlt másolja és illessze be **azonosító** textbox **ismeri fel a tartomány és az URL-címek szakasz** Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="8efa8-195">Then copy hello EntityDescriptor value(entityID) from hello file and paste it in **Identifier** textbox in **Recognize Domain and URLs section** on Azure portal.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> <span data-ttu-id="8efa8-197">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8efa8-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8efa8-198">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8efa8-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8efa8-199">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8efa8-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8efa8-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8efa8-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="8efa8-201">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8efa8-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8efa8-203">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8efa8-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8efa8-204">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8efa8-206">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8efa8-208">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8efa8-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8efa8-210">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8efa8-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8efa8-212">a.</span><span class="sxs-lookup"><span data-stu-id="8efa8-212">a.</span></span> <span data-ttu-id="8efa8-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8efa8-214">b.</span><span class="sxs-lookup"><span data-stu-id="8efa8-214">b.</span></span> <span data-ttu-id="8efa8-215">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8efa8-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8efa8-216">c.</span><span class="sxs-lookup"><span data-stu-id="8efa8-216">c.</span></span> <span data-ttu-id="8efa8-217">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8efa8-218">d.</span><span class="sxs-lookup"><span data-stu-id="8efa8-218">d.</span></span> <span data-ttu-id="8efa8-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-219">Click **Create**.</span></span>
 
### <a name="creating-a-recognize-test-user"></a><span data-ttu-id="8efa8-220">Felismerése tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8efa8-220">Creating a Recognize test user</span></span>

<span data-ttu-id="8efa8-221">A sorrend tooenable az Azure AD felhasználók toolog felismerése be akkor kell üzembe felismerése a.</span><span class="sxs-lookup"><span data-stu-id="8efa8-221">In order tooenable Azure AD users toolog into Recognize, they must be provisioned into Recognize.</span></span> <span data-ttu-id="8efa8-222">Felismerése hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8efa8-222">In hello case of Recognize, provisioning is a manual task.</span></span>

<span data-ttu-id="8efa8-223">Az alkalmazás nem támogatja a SCIM kiépítés, de van egy másik felhasználó szinkronizálási, hogy a felhasználók kiépítését.</span><span class="sxs-lookup"><span data-stu-id="8efa8-223">This app doesn't support SCIM provisioning but has an alternate user sync that provisions users.</span></span> 

<span data-ttu-id="8efa8-224">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8efa8-224">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8efa8-225">Jelentkezzen be a felismerése vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8efa8-225">Log into your Recognize company site as an administrator.</span></span>

2. <span data-ttu-id="8efa8-226">A hello jobb felső sarokban, kattintson **menü**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-226">On hello upper right corner, click **Menu**.</span></span> <span data-ttu-id="8efa8-227">Nyissa meg túl**vállalati rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-227">Go too**Company Admin**.</span></span>

3. <span data-ttu-id="8efa8-228">A hello bal oldali navigációs ablaktábláján kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-228">On hello left navigation pane, click **Settings**.</span></span>

4. <span data-ttu-id="8efa8-229">Hajtsa végre a következő lépéseket hello **felhasználói szinkronizálási** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8efa8-229">Perform hello following steps on **User Sync** section.</span></span>
   
   <span data-ttu-id="8efa8-230">![Új felhasználó](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="8efa8-230">![New User](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "New User")</span></span>
   
   <span data-ttu-id="8efa8-231">a.</span><span class="sxs-lookup"><span data-stu-id="8efa8-231">a.</span></span> <span data-ttu-id="8efa8-232">Mint **engedélyezve szinkronizálása**, jelölje be **ON**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-232">As **Sync Enabled**, select **ON**.</span></span>
   
   <span data-ttu-id="8efa8-233">b.</span><span class="sxs-lookup"><span data-stu-id="8efa8-233">b.</span></span> <span data-ttu-id="8efa8-234">Mint **választható sync szolgáltató**, jelölje be **Microsoft / Office 365**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-234">As **Choose sync provider**, select **Microsoft / Office 365**.</span></span>
   
   <span data-ttu-id="8efa8-235">c.</span><span class="sxs-lookup"><span data-stu-id="8efa8-235">c.</span></span> <span data-ttu-id="8efa8-236">Kattintson a **felhasználói szinkronizálás futtatása**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-236">Click **Run User Sync**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8efa8-237">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8efa8-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8efa8-238">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRecognize megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8efa8-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRecognize.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8efa8-240">**tooassign Britta Simon tooRecognize, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8efa8-240">**tooassign Britta Simon tooRecognize, perform hello following steps:**</span></span>

1. <span data-ttu-id="8efa8-241">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8efa8-243">Hello alkalmazások listában válassza ki a **felismerése**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-243">In hello applications list, select **Recognize**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. <span data-ttu-id="8efa8-245">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8efa8-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8efa8-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8efa8-247">Click **Add** button.</span></span> <span data-ttu-id="8efa8-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8efa8-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8efa8-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8efa8-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8efa8-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8efa8-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8efa8-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8efa8-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8efa8-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8efa8-253">Testing single sign-on</span></span>

<span data-ttu-id="8efa8-254">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="8efa8-254">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8efa8-255">Hello felismerése csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour felismerése alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="8efa8-255">When you click hello Recognize tile in hello Access Panel, you should get automatically signed-on tooyour Recognize application.</span></span> <span data-ttu-id="8efa8-256">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8efa8-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8efa8-257">További források</span><span class="sxs-lookup"><span data-stu-id="8efa8-257">Additional resources</span></span>

* [<span data-ttu-id="8efa8-258">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8efa8-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8efa8-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8efa8-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

