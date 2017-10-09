---
title: "Oktatóanyag: Azure Active Directoryval integrált TalentLMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TalentLMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 25538086602e58fbaab0fbf223f5b03908a74922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a><span data-ttu-id="19452-103">Oktatóanyag: Azure Active Directoryval integrált TalentLMS</span><span class="sxs-lookup"><span data-stu-id="19452-103">Tutorial: Azure Active Directory integration with TalentLMS</span></span>

<span data-ttu-id="19452-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TalentLMS az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="19452-104">In this tutorial, you learn how toointegrate TalentLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="19452-105">TalentLMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="19452-105">Integrating TalentLMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="19452-106">Megadhatja a hozzáférés tooTalentLMS rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="19452-106">You can control in Azure AD who has access tooTalentLMS</span></span>
- <span data-ttu-id="19452-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTalentLMS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="19452-107">You can enable your users tooautomatically get signed-on tooTalentLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="19452-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="19452-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="19452-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="19452-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19452-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="19452-110">Prerequisites</span></span>

<span data-ttu-id="19452-111">az Azure AD integrálása TalentLMS tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="19452-111">tooconfigure Azure AD integration with TalentLMS, you need hello following items:</span></span>

- <span data-ttu-id="19452-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="19452-112">An Azure AD subscription</span></span>
- <span data-ttu-id="19452-113">Egy TalentLMS egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="19452-113">A TalentLMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="19452-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="19452-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="19452-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="19452-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="19452-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="19452-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="19452-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="19452-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="19452-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="19452-118">Scenario description</span></span>
<span data-ttu-id="19452-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="19452-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="19452-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="19452-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="19452-121">Hello gyűjteményből TalentLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="19452-121">Adding TalentLMS from hello gallery</span></span>
2. <span data-ttu-id="19452-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="19452-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-talentlms-from-hello-gallery"></a><span data-ttu-id="19452-123">Hello gyűjteményből TalentLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="19452-123">Adding TalentLMS from hello gallery</span></span>
<span data-ttu-id="19452-124">tooconfigure hello integrációja TalentLMS az Azure AD-be, meg kell tooadd TalentLMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="19452-124">tooconfigure hello integration of TalentLMS into Azure AD, you need tooadd TalentLMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="19452-125">**tooadd TalentLMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="19452-125">**tooadd TalentLMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="19452-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="19452-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="19452-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="19452-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="19452-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="19452-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="19452-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="19452-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="19452-133">Hello keresési mezőbe, írja be a **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="19452-133">In hello search box, type **TalentLMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. <span data-ttu-id="19452-135">A hello eredmények panelen válassza ki a **TalentLMS**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="19452-135">In hello results panel, select **TalentLMS**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="19452-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="19452-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="19452-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján TalentLMS</span><span class="sxs-lookup"><span data-stu-id="19452-138">In this section, you configure and test Azure AD single sign-on with TalentLMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="19452-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TalentLMS tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="19452-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TalentLMS is tooa user in Azure AD.</span></span> <span data-ttu-id="19452-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TalentLMS közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="19452-140">In other words, a link relationship between an Azure AD user and hello related user in TalentLMS needs toobe established.</span></span>

<span data-ttu-id="19452-141">TalentLMS, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="19452-141">In TalentLMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="19452-142">tooconfigure és az Azure AD az egyszeri bejelentkezés TalentLMS-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="19452-142">tooconfigure and test Azure AD single sign-on with TalentLMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="19452-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="19452-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="19452-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="19452-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="19452-145">**[TalentLMS tesztfelhasználó létrehozása](#creating-a-talentlms-test-user)**  -toohave egy megfelelője a Britta Simon a TalentLMS, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="19452-145">**[Creating a TalentLMS test user](#creating-a-talentlms-test-user)** - toohave a counterpart of Britta Simon in TalentLMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="19452-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="19452-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="19452-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="19452-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="19452-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="19452-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="19452-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TalentLMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="19452-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TalentLMS application.</span></span>

<span data-ttu-id="19452-150">**az Azure AD tooconfigure egyszeri bejelentkezést a TalentLMS, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="19452-150">**tooconfigure Azure AD single sign-on with TalentLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="19452-151">Az Azure portál, a hello hello **TalentLMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="19452-151">In hello Azure portal, on hello **TalentLMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="19452-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="19452-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. <span data-ttu-id="19452-155">A hello **TalentLMS tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="19452-155">On hello **TalentLMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    <span data-ttu-id="19452-157">a.</span><span class="sxs-lookup"><span data-stu-id="19452-157">a.</span></span> <span data-ttu-id="19452-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.TalentLMSapp.com`</span><span class="sxs-lookup"><span data-stu-id="19452-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.TalentLMSapp.com`</span></span>

    <span data-ttu-id="19452-159">b.</span><span class="sxs-lookup"><span data-stu-id="19452-159">b.</span></span> <span data-ttu-id="19452-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<tenant-name>.talentlms.com`</span><span class="sxs-lookup"><span data-stu-id="19452-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://<tenant-name>.talentlms.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="19452-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="19452-161">These values are not real.</span></span> <span data-ttu-id="19452-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="19452-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="19452-163">Ügyfél [TalentLMS ügyfél-támogatási csoport](https://www.talentlms.com/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="19452-163">Contact [TalentLMS Client support team](https://www.talentlms.com/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="19452-164">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** hello tanúsítvány közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="19452-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. <span data-ttu-id="19452-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="19452-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="19452-168">A hello **TalentLMS konfigurációs** kattintson **konfigurálása TalentLMS** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="19452-168">On hello **TalentLMS Configuration** section, click **Configure TalentLMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="19452-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="19452-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. <span data-ttu-id="19452-171">Egy másik webes böngészőablakban jelentkezzen tooyour TalentLMS vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="19452-171">In a different web browser window, log in tooyour TalentLMS company site as an administrator.</span></span>

8. <span data-ttu-id="19452-172">A hello **fiók & beállítások** területen kattintson a hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="19452-172">In hello **Account & Settings** section, click hello **Users** tab.</span></span>
   
    <span data-ttu-id="19452-173">![Fiók & beállítások](./media/active-directory-saas-talentlms-tutorial/IC777296.png "fiók & beállítások")</span><span class="sxs-lookup"><span data-stu-id="19452-173">![Account & Settings](./media/active-directory-saas-talentlms-tutorial/IC777296.png "Account & Settings")</span></span>

9. <span data-ttu-id="19452-174">Kattintson a **egyszeri bejelentkezés (SSO)**,</span><span class="sxs-lookup"><span data-stu-id="19452-174">Click **Single Sign-On (SSO)**,</span></span>

10. <span data-ttu-id="19452-175">Az egyszeri bejelentkezés szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="19452-175">In hello Single Sign-On section, perform hello following steps:</span></span>
   
    <span data-ttu-id="19452-176">![Egyszeri bejelentkezés](./media/active-directory-saas-talentlms-tutorial/IC777297.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="19452-176">![Single Sign-On](./media/active-directory-saas-talentlms-tutorial/IC777297.png "Single Sign-On")</span></span>   

    <span data-ttu-id="19452-177">a.</span><span class="sxs-lookup"><span data-stu-id="19452-177">a.</span></span> <span data-ttu-id="19452-178">A hello **SSO integrációs típus** listáról válassza ki **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="19452-178">From hello **SSO integration type** list, select **SAML 2.0**.</span></span>

    <span data-ttu-id="19452-179">b.</span><span class="sxs-lookup"><span data-stu-id="19452-179">b.</span></span> <span data-ttu-id="19452-180">A hello **identitásszolgáltató (IDP)** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="19452-180">In hello **Identity provider (IDP)** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="19452-181">c.</span><span class="sxs-lookup"><span data-stu-id="19452-181">c.</span></span> <span data-ttu-id="19452-182">Beillesztés hello **ujjlenyomat** hello az Azure portálról érték **tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="19452-182">Paste hello **Thumbprint** value from Azure portal into hello **Certificate fingerprint** textbox.</span></span>    

    <span data-ttu-id="19452-183">d.</span><span class="sxs-lookup"><span data-stu-id="19452-183">d.</span></span>  <span data-ttu-id="19452-184">A hello **távoli bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="19452-184">In hello **Remote sign-in URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="19452-185">e.</span><span class="sxs-lookup"><span data-stu-id="19452-185">e.</span></span> <span data-ttu-id="19452-186">A hello **távoli kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="19452-186">In hello **Remote sign-out URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="19452-187">f.</span><span class="sxs-lookup"><span data-stu-id="19452-187">f.</span></span> <span data-ttu-id="19452-188">Töltse ki hello a következőket:</span><span class="sxs-lookup"><span data-stu-id="19452-188">Fill in hello following:</span></span> 

    * <span data-ttu-id="19452-189">A hello **TargetedID** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span><span class="sxs-lookup"><span data-stu-id="19452-189">In hello **TargetedID** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span></span>
     
    * <span data-ttu-id="19452-190">A hello **Keresztnév** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span><span class="sxs-lookup"><span data-stu-id="19452-190">In hello **First name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span></span>
    
    * <span data-ttu-id="19452-191">A hello **Vezetéknév** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span><span class="sxs-lookup"><span data-stu-id="19452-191">In hello **Last name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span></span>
    
    * <span data-ttu-id="19452-192">A hello **E-mail** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="19452-192">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>
    
11. <span data-ttu-id="19452-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="19452-193">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="19452-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="19452-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="19452-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="19452-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="19452-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="19452-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="19452-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="19452-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="19452-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="19452-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="19452-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="19452-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="19452-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="19452-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="19452-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="19452-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="19452-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="19452-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="19452-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="19452-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="19452-209">a.</span><span class="sxs-lookup"><span data-stu-id="19452-209">a.</span></span> <span data-ttu-id="19452-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="19452-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="19452-211">b.</span><span class="sxs-lookup"><span data-stu-id="19452-211">b.</span></span> <span data-ttu-id="19452-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="19452-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="19452-213">c.</span><span class="sxs-lookup"><span data-stu-id="19452-213">c.</span></span> <span data-ttu-id="19452-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="19452-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="19452-215">d.</span><span class="sxs-lookup"><span data-stu-id="19452-215">d.</span></span> <span data-ttu-id="19452-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="19452-216">Click **Create**.</span></span>
 
### <a name="creating-a-talentlms-test-user"></a><span data-ttu-id="19452-217">TalentLMS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="19452-217">Creating a TalentLMS test user</span></span>

<span data-ttu-id="19452-218">az Azure AD tooenable felhasználók toolog a tooTalentLMS, akkor ki kell építenie TalentLMS be.</span><span class="sxs-lookup"><span data-stu-id="19452-218">tooenable Azure AD users toolog in tooTalentLMS, they must be provisioned into TalentLMS.</span></span> <span data-ttu-id="19452-219">TalentLMS hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="19452-219">In hello case of TalentLMS, provisioning is a manual task.</span></span>

<span data-ttu-id="19452-220">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="19452-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="19452-221">Jelentkezzen be tooyour **TalentLMS** bérlő.</span><span class="sxs-lookup"><span data-stu-id="19452-221">Log in tooyour **TalentLMS** tenant.</span></span>

2. <span data-ttu-id="19452-222">Kattintson a **felhasználók**, és kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="19452-222">Click **Users**, and then click **Add User**.</span></span>

3. <span data-ttu-id="19452-223">A hello **felhasználó hozzáadása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="19452-223">On hello **Add user** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="19452-224">![Felhasználó hozzáadása](./media/active-directory-saas-talentlms-tutorial/IC777299.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="19452-224">![Add User](./media/active-directory-saas-talentlms-tutorial/IC777299.png "Add User")</span></span>  

    <span data-ttu-id="19452-225">a.</span><span class="sxs-lookup"><span data-stu-id="19452-225">a.</span></span> <span data-ttu-id="19452-226">A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="19452-226">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="19452-227">b.</span><span class="sxs-lookup"><span data-stu-id="19452-227">b.</span></span> <span data-ttu-id="19452-228">A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="19452-228">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="19452-229">c.</span><span class="sxs-lookup"><span data-stu-id="19452-229">c.</span></span> <span data-ttu-id="19452-230">A hello **E-mail cím** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="19452-230">In hello **Email address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="19452-231">d.</span><span class="sxs-lookup"><span data-stu-id="19452-231">d.</span></span> <span data-ttu-id="19452-232">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="19452-232">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="19452-233">Bármely más TalentLMS felhasználói fiók létrehozása eszközök vagy TalentLMS tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="19452-233">You can use any other TalentLMS user account creation tools or APIs provided by TalentLMS tooprovision AAD user accounts.</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="19452-234">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="19452-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="19452-235">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTalentLMS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="19452-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTalentLMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="19452-237">**tooassign Britta Simon tooTalentLMS, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="19452-237">**tooassign Britta Simon tooTalentLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="19452-238">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="19452-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="19452-240">Hello alkalmazások listában válassza ki a **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="19452-240">In hello applications list, select **TalentLMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. <span data-ttu-id="19452-242">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="19452-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="19452-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="19452-244">Click **Add** button.</span></span> <span data-ttu-id="19452-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="19452-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="19452-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="19452-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="19452-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="19452-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="19452-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="19452-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="19452-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="19452-250">Testing single sign-on</span></span>

<span data-ttu-id="19452-251">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="19452-251">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="19452-252">Hello TalentLMS hello hozzáférési Panel csempére kattintva szerezheti be automatikusan bejelentkezett tooyour TalentLMS alkalmazás</span><span class="sxs-lookup"><span data-stu-id="19452-252">When you click hello TalentLMS tile in hello Access Panel, you should get automatically signed-on tooyour TalentLMS application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="19452-253">További források</span><span class="sxs-lookup"><span data-stu-id="19452-253">Additional resources</span></span>

* [<span data-ttu-id="19452-254">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="19452-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="19452-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="19452-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

