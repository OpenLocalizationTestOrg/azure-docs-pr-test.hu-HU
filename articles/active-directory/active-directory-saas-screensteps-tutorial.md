---
title: "Oktatóanyag: Azure Active Directoryval integrált ScreenSteps |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ScreenSteps között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: fd041e5fe4552727eeda2dabc1773d8043d410f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a><span data-ttu-id="e32b7-103">Oktatóanyag: Azure Active Directoryval integrált ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="e32b7-103">Tutorial: Azure Active Directory integration with ScreenSteps</span></span>

<span data-ttu-id="e32b7-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ScreenSteps az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e32b7-104">In this tutorial, you learn how toointegrate ScreenSteps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e32b7-105">ScreenSteps integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e32b7-105">Integrating ScreenSteps with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e32b7-106">Az Azure AD hozzáférési tooScreenSteps rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e32b7-106">You can control in Azure AD who has access tooScreenSteps.</span></span>
- <span data-ttu-id="e32b7-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooScreenSteps (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="e32b7-107">You can enable your users tooautomatically get signed-on tooScreenSteps (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e32b7-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="e32b7-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="e32b7-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e32b7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e32b7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e32b7-110">Prerequisites</span></span>

<span data-ttu-id="e32b7-111">az Azure AD integrálása ScreenSteps tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e32b7-111">tooconfigure Azure AD integration with ScreenSteps, you need hello following items:</span></span>

- <span data-ttu-id="e32b7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e32b7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e32b7-113">Egy ScreenSteps egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e32b7-113">A ScreenSteps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e32b7-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e32b7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e32b7-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e32b7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e32b7-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e32b7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e32b7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e32b7-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e32b7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e32b7-118">Scenario description</span></span>
<span data-ttu-id="e32b7-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e32b7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e32b7-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e32b7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e32b7-121">Hello gyűjteményből ScreenSteps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e32b7-121">Adding ScreenSteps from hello gallery</span></span>
2. <span data-ttu-id="e32b7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e32b7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-screensteps-from-hello-gallery"></a><span data-ttu-id="e32b7-123">Hello gyűjteményből ScreenSteps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e32b7-123">Adding ScreenSteps from hello gallery</span></span>
<span data-ttu-id="e32b7-124">tooconfigure hello integrációja ScreenSteps az Azure AD-be, meg kell tooadd ScreenSteps hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e32b7-124">tooconfigure hello integration of ScreenSteps into Azure AD, you need tooadd ScreenSteps from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e32b7-125">**tooadd ScreenSteps hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e32b7-125">**tooadd ScreenSteps from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e32b7-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="e32b7-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e32b7-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="e32b7-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e32b7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="e32b7-133">Hello keresési mezőbe, írja be a **ScreenSteps**, jelölje be **ScreenSteps** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-133">In hello search box, type **ScreenSteps**, select **ScreenSteps** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e32b7-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e32b7-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e32b7-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="e32b7-136">In this section, you configure and test Azure AD single sign-on with ScreenSteps based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e32b7-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ScreenSteps tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e32b7-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ScreenSteps is tooa user in Azure AD.</span></span> <span data-ttu-id="e32b7-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ScreenSteps közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e32b7-138">In other words, a link relationship between an Azure AD user and hello related user in ScreenSteps needs toobe established.</span></span>

<span data-ttu-id="e32b7-139">ScreenSteps, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e32b7-139">In ScreenSteps, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e32b7-140">tooconfigure és az Azure AD az egyszeri bejelentkezés ScreenSteps-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e32b7-140">tooconfigure and test Azure AD single sign-on with ScreenSteps, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e32b7-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e32b7-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e32b7-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e32b7-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e32b7-143">**[ScreenSteps tesztfelhasználó létrehozása](#create-a-screensteps-test-user)**  -toohave egy megfelelője a Britta Simon a ScreenSteps, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e32b7-143">**[Create a ScreenSteps test user](#create-a-screensteps-test-user)** - toohave a counterpart of Britta Simon in ScreenSteps that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e32b7-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e32b7-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e32b7-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e32b7-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e32b7-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e32b7-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e32b7-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ScreenSteps alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e32b7-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ScreenSteps application.</span></span>

<span data-ttu-id="e32b7-148">**az Azure AD tooconfigure egyszeri bejelentkezést a ScreenSteps, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e32b7-148">**tooconfigure Azure AD single sign-on with ScreenSteps, perform hello following steps:**</span></span>

1. <span data-ttu-id="e32b7-149">Az Azure portál, a hello hello **ScreenSteps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-149">In hello Azure portal, on hello **ScreenSteps** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="e32b7-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e32b7-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. <span data-ttu-id="e32b7-153">A hello **ScreenSteps tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e32b7-153">On hello **ScreenSteps Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk ScreenSteps tartomány és az URL-címek](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    <span data-ttu-id="e32b7-155">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.ScreenSteps.com`</span><span class="sxs-lookup"><span data-stu-id="e32b7-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.ScreenSteps.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e32b7-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e32b7-156">This value is not real.</span></span> <span data-ttu-id="e32b7-157">Frissítse ezt az értéket hello tényleges bejelentkezési URL-cím, az oktatóanyag későbbi részében ismertetett.</span><span class="sxs-lookup"><span data-stu-id="e32b7-157">Update this value with hello actual Sign-On URL, which is explained later in this tutorial.</span></span> 

4. <span data-ttu-id="e32b7-158">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e32b7-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. <span data-ttu-id="e32b7-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e32b7-162">A hello **ScreenSteps konfigurációs** kattintson **konfigurálása ScreenSteps** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e32b7-162">On hello **ScreenSteps Configuration** section, click **Configure ScreenSteps** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e32b7-163">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e32b7-163">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![ScreenSteps konfiguráció](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. <span data-ttu-id="e32b7-165">Egy másik webes böngészőablakban jelentkezzen be a ScreenSteps vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e32b7-165">In a different web browser window, log into your ScreenSteps company site as an administrator.</span></span>

8. <span data-ttu-id="e32b7-166">Kattintson a **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-166">Click **Account Settings**.</span></span>

    <span data-ttu-id="e32b7-167">![Fiókkezelés](./media/active-directory-saas-screensteps-tutorial/ic778523.png "fiókkezelés")</span><span class="sxs-lookup"><span data-stu-id="e32b7-167">![Account management](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Account management")</span></span>

9. <span data-ttu-id="e32b7-168">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-168">Click **Single Sign-on**.</span></span>

    <span data-ttu-id="e32b7-169">![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778524.png "távoli hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="e32b7-169">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span></span>

10. <span data-ttu-id="e32b7-170">Kattintson a **egyszeri bejelentkezési végpont létrehozásához**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-170">Click **Create Single Sign-on Endpoint**.</span></span>

    <span data-ttu-id="e32b7-171">![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778525.png "távoli hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="e32b7-171">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span></span>

11. <span data-ttu-id="e32b7-172">A hello **létrehozása egyszeri bejelentkezés végpont** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e32b7-172">In hello **Create Single Sign-on Endpoint** section, perform hello following steps:</span></span>

    <span data-ttu-id="e32b7-173">![Hozzon létre egy hitelesítési végpontot](./media/active-directory-saas-screensteps-tutorial/ic778526.png "hozzon létre egy hitelesítési végpontot")</span><span class="sxs-lookup"><span data-stu-id="e32b7-173">![Create an authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778526.png "Create an authentication endpoint")</span></span>
    
    <span data-ttu-id="e32b7-174">a.</span><span class="sxs-lookup"><span data-stu-id="e32b7-174">a.</span></span> <span data-ttu-id="e32b7-175">A hello **cím** szövegmezőhöz címét adja meg.</span><span class="sxs-lookup"><span data-stu-id="e32b7-175">In hello **Title** textbox, type a title.</span></span>
    
    <span data-ttu-id="e32b7-176">b.</span><span class="sxs-lookup"><span data-stu-id="e32b7-176">b.</span></span> <span data-ttu-id="e32b7-177">A hello **mód** listáról válassza ki **SAML**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-177">From hello **Mode** list, select **SAML**.</span></span>
    
    <span data-ttu-id="e32b7-178">c.</span><span class="sxs-lookup"><span data-stu-id="e32b7-178">c.</span></span> <span data-ttu-id="e32b7-179">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-179">Click **Create**.</span></span>

12. <span data-ttu-id="e32b7-180">**Szerkesztés** hello új végpont.</span><span class="sxs-lookup"><span data-stu-id="e32b7-180">**Edit** hello new endpoint.</span></span>

    <span data-ttu-id="e32b7-181">![Szerkessze a végpont](./media/active-directory-saas-screensteps-tutorial/ic778528.png "végpont szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="e32b7-181">![Edit endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Edit endpoint")</span></span>

13. <span data-ttu-id="e32b7-182">A hello **szerkesztése egyszeri bejelentkezés végpont** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e32b7-182">In hello **Edit Single Sign-on Endpoint** section, perform hello following steps:</span></span>

    <span data-ttu-id="e32b7-183">![Távoli hitelesítési végpont](./media/active-directory-saas-screensteps-tutorial/ic778527.png "távoli hitelesítési végpontot")</span><span class="sxs-lookup"><span data-stu-id="e32b7-183">![Remote authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication endpoint")</span></span>

    <span data-ttu-id="e32b7-184">a.</span><span class="sxs-lookup"><span data-stu-id="e32b7-184">a.</span></span> <span data-ttu-id="e32b7-185">Kattintson a **feltöltés új SAML tanúsítványfájl**, és ezután feltöltés hello tanúsítványt, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="e32b7-185">Click **Upload new SAML Certificate file**, and then upload hello certificate, which you have downloaded from Azure portal.</span></span>
    
    <span data-ttu-id="e32b7-186">b.</span><span class="sxs-lookup"><span data-stu-id="e32b7-186">b.</span></span> <span data-ttu-id="e32b7-187">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **távoli bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e32b7-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **Remote Login URL** textbox.</span></span>
    
    <span data-ttu-id="e32b7-188">c.</span><span class="sxs-lookup"><span data-stu-id="e32b7-188">c.</span></span> <span data-ttu-id="e32b7-189">Beillesztés **Sign-Out URL-cím** értéket, amely akkor másolta, az Azure-portálon hello hello **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e32b7-189">Paste **Sign-Out URL** value, which you have copied from hello Azure portal into hello **Log out URL** textbox.</span></span>
    
    <span data-ttu-id="e32b7-190">d.</span><span class="sxs-lookup"><span data-stu-id="e32b7-190">d.</span></span> <span data-ttu-id="e32b7-191">Válassza ki a **csoport** tooassign felhasználók toowhen kiépítésüket.</span><span class="sxs-lookup"><span data-stu-id="e32b7-191">Select a **Group** tooassign users toowhen they are provisioned.</span></span>
    
    <span data-ttu-id="e32b7-192">e.</span><span class="sxs-lookup"><span data-stu-id="e32b7-192">e.</span></span> <span data-ttu-id="e32b7-193">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-193">Click **Update**.</span></span>

    <span data-ttu-id="e32b7-194">f.</span><span class="sxs-lookup"><span data-stu-id="e32b7-194">f.</span></span> <span data-ttu-id="e32b7-195">Másolás hello **SAML-alapú ügyfél URL-címe** toohello vágólapra és illessze be a toohello **bejelentkezési URL-cím** textbox **ScreenSteps tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e32b7-195">Copy hello **SAML Consumer URL** toohello clipboard and paste in toohello **Sign-on URL** textbox in **ScreenSteps Domain and URLs** section.</span></span>
    
    <span data-ttu-id="e32b7-196">g.</span><span class="sxs-lookup"><span data-stu-id="e32b7-196">g.</span></span> <span data-ttu-id="e32b7-197">Térjen vissza a toohello **szerkesztése egyszeri bejelentkezés végpont**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-197">Return toohello **Edit Single Sign-on Endpoint**.</span></span>
    
    <span data-ttu-id="e32b7-198">h.</span><span class="sxs-lookup"><span data-stu-id="e32b7-198">h.</span></span> <span data-ttu-id="e32b7-199">Kattintson a hello **alapértelmezett fiók** toouse gombra a végpont összes felhasználó számára ScreenSteps be tudjon jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="e32b7-199">Click hello **Make default for account** button toouse this endpoint for all users who log into ScreenSteps.</span></span> <span data-ttu-id="e32b7-200">Másik lehetőségként kattinthat hello **tooSite hozzáadása** gomb toouse ehhez a végponthoz tartozó adott helynél **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-200">Alternatively you can click hello **Add tooSite** button toouse this endpoint for specific sites in **ScreenSteps**.</span></span>

> [!TIP]
> <span data-ttu-id="e32b7-201">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e32b7-201">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e32b7-202">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e32b7-202">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e32b7-203">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e32b7-203">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e32b7-204">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e32b7-204">Create an Azure AD test user</span></span>

<span data-ttu-id="e32b7-205">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e32b7-205">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="e32b7-207">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e32b7-207">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e32b7-208">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-208">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e32b7-210">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-210">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e32b7-212">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e32b7-212">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e32b7-214">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e32b7-214">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e32b7-216">a.</span><span class="sxs-lookup"><span data-stu-id="e32b7-216">a.</span></span> <span data-ttu-id="e32b7-217">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-217">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e32b7-218">b.</span><span class="sxs-lookup"><span data-stu-id="e32b7-218">b.</span></span> <span data-ttu-id="e32b7-219">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="e32b7-219">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="e32b7-220">c.</span><span class="sxs-lookup"><span data-stu-id="e32b7-220">c.</span></span> <span data-ttu-id="e32b7-221">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e32b7-221">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="e32b7-222">d.</span><span class="sxs-lookup"><span data-stu-id="e32b7-222">d.</span></span> <span data-ttu-id="e32b7-223">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-223">Click **Create**.</span></span>
 
### <a name="create-a-screensteps-test-user"></a><span data-ttu-id="e32b7-224">ScreenSteps tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e32b7-224">Create a ScreenSteps test user</span></span>

<span data-ttu-id="e32b7-225">Ebben a szakaszban egy ScreenSteps Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e32b7-225">In this section, you create a user called Britta Simon in ScreenSteps.</span></span> <span data-ttu-id="e32b7-226">Együttműködve [ScreenSteps ügyfél-támogatási csoport](https://www.screensteps.com/contact) felhasználót is hozzáadhat hello hello ScreenSteps platform.</span><span class="sxs-lookup"><span data-stu-id="e32b7-226">Work with [ScreenSteps Client support team](https://www.screensteps.com/contact) to add hello users in hello ScreenSteps platform.</span></span> <span data-ttu-id="e32b7-227">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="e32b7-227">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e32b7-228">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e32b7-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e32b7-229">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooScreenSteps megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e32b7-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooScreenSteps.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="e32b7-231">**tooassign Britta Simon tooScreenSteps, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e32b7-231">**tooassign Britta Simon tooScreenSteps, perform hello following steps:**</span></span>

1. <span data-ttu-id="e32b7-232">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e32b7-234">Hello alkalmazások listában válassza ki a **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-234">In hello applications list, select **ScreenSteps**.</span></span>

    ![hello ScreenSteps hivatkozásra hello alkalmazások listája](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. <span data-ttu-id="e32b7-236">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e32b7-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="e32b7-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e32b7-238">Click **Add** button.</span></span> <span data-ttu-id="e32b7-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e32b7-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="e32b7-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e32b7-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e32b7-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e32b7-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e32b7-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e32b7-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e32b7-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e32b7-244">Test single sign-on</span></span>

<span data-ttu-id="e32b7-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e32b7-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e32b7-246">Hello ScreenSteps hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ScreenSteps alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e32b7-246">When you click hello ScreenSteps tile in hello Access Panel, you should get automatically signed-on tooyour ScreenSteps application.</span></span>
<span data-ttu-id="e32b7-247">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e32b7-247">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e32b7-248">További források</span><span class="sxs-lookup"><span data-stu-id="e32b7-248">Additional resources</span></span>

* [<span data-ttu-id="e32b7-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e32b7-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e32b7-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e32b7-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

