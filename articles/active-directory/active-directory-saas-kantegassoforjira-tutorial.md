---
title: "Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a JIRA |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a JIRA Kantega SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 67894cc55ef91d0991c62e0e4f1be712723cb474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a><span data-ttu-id="7edd0-103">Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a JIRA</span><span class="sxs-lookup"><span data-stu-id="7edd0-103">Tutorial: Azure Active Directory integration with Kantega SSO for JIRA</span></span>

<span data-ttu-id="7edd0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kantega egyszeri bejelentkezés az Azure Active Directory (Azure AD) JIRA.</span><span class="sxs-lookup"><span data-stu-id="7edd0-104">In this tutorial, you learn how toointegrate Kantega SSO for JIRA with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7edd0-105">A JIRA Kantega SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7edd0-105">Integrating Kantega SSO for JIRA with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7edd0-106">Megadhatja a hozzáférés tooKantega SSO rendelkező JIRA az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7edd0-106">You can control in Azure AD who has access tooKantega SSO for JIRA</span></span>
- <span data-ttu-id="7edd0-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKantega SSO vonatkozóan JIRA (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7edd0-107">You can enable your users tooautomatically get signed-on tooKantega SSO for JIRA (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7edd0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7edd0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7edd0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7edd0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7edd0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7edd0-110">Prerequisites</span></span>

<span data-ttu-id="7edd0-111">tooconfigure Kantega SSO JIRA az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="7edd0-111">tooconfigure Azure AD integration with Kantega SSO for JIRA, you need hello following items:</span></span>

- <span data-ttu-id="7edd0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7edd0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7edd0-113">Egy Kantega SSO JIRA egyszeri bejelentkezést az előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="7edd0-113">A Kantega SSO for JIRA single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7edd0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7edd0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7edd0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7edd0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7edd0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7edd0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7edd0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7edd0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7edd0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7edd0-118">Scenario description</span></span>
<span data-ttu-id="7edd0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7edd0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7edd0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7edd0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7edd0-121">A JIRA Kantega SSO hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7edd0-121">Adding Kantega SSO for JIRA from hello gallery</span></span>
2. <span data-ttu-id="7edd0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7edd0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-jira-from-hello-gallery"></a><span data-ttu-id="7edd0-123">A JIRA Kantega SSO hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7edd0-123">Adding Kantega SSO for JIRA from hello gallery</span></span>
<span data-ttu-id="7edd0-124">tooconfigure hello integrációja a JIRA Kantega SSO az Azure AD-be, szükség van tooadd Kantega SSO JIRA hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7edd0-124">tooconfigure hello integration of Kantega SSO for JIRA into Azure AD, you need tooadd Kantega SSO for JIRA from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7edd0-125">**tooadd Kantega SSO hello gyűjteményből JIRA a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7edd0-125">**tooadd Kantega SSO for JIRA from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7edd0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7edd0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7edd0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7edd0-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7edd0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7edd0-133">Hello keresési mezőbe, írja be a **Kantega SSO a JIRA**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-133">In hello search box, type **Kantega SSO for JIRA**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_search.png)

5. <span data-ttu-id="7edd0-135">A hello eredmények panelen válassza a **Kantega SSO JIRA a**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-135">In hello results panel, select **Kantega SSO for JIRA**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7edd0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7edd0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7edd0-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel, az úgynevezett "Britta Simon" tesztfelhasználó alapján JIRA.</span><span class="sxs-lookup"><span data-stu-id="7edd0-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for JIRA based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7edd0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kantega SSO JIRA a tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7edd0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kantega SSO for JIRA is tooa user in Azure AD.</span></span> <span data-ttu-id="7edd0-140">Ez azt jelenti hello Kantega SSO JIRA a kapcsolódó felhasználó és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7edd0-140">In other words, a link relationship between an Azure AD user and hello related user in Kantega SSO for JIRA needs toobe established.</span></span>

<span data-ttu-id="7edd0-141">JIRA Kantega egyszeri Bejelentkezést, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7edd0-141">In Kantega SSO for JIRA, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7edd0-142">tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel JIRA, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7edd0-142">tooconfigure and test Azure AD single sign-on with Kantega SSO for JIRA, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7edd0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7edd0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7edd0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7edd0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7edd0-145">**[Egy Kantega SSO JIRA teszt felhasználó létrehozása](#creating-a-kantega-sso-for-jira-test-user)**  -toohave egy megfelelője a Britta Simon a Kantega SSO a JIRA, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7edd0-145">**[Creating a Kantega SSO for JIRA test user](#creating-a-kantega-sso-for-jira-test-user)** - toohave a counterpart of Britta Simon in Kantega SSO for JIRA that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7edd0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7edd0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7edd0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7edd0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7edd0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7edd0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7edd0-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Kantega SSO JIRA alkalmazáshoz az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7edd0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kantega SSO for JIRA application.</span></span>

<span data-ttu-id="7edd0-150">**Kantega egyszeri bejelentkezési modellel JIRA, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7edd0-150">**tooconfigure Azure AD single sign-on with Kantega SSO for JIRA, perform hello following steps:**</span></span>

1. <span data-ttu-id="7edd0-151">Az Azure portál, a hello hello **Kantega SSO a JIRA** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-151">In hello Azure portal, on hello **Kantega SSO for JIRA** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7edd0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7edd0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_samlbase.png)

3. <span data-ttu-id="7edd0-155">A **IDP** mód, a hello kezdeményezett **Kantega SSO JIRA tartomány és az URL-címek** szakasz hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="7edd0-155">In **IDP** initiated mode, on hello **Kantega SSO for JIRA Domain and URLs** section perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_url1.png)

    <span data-ttu-id="7edd0-157">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-157">a.</span></span> <span data-ttu-id="7edd0-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="7edd0-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="7edd0-159">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-159">b.</span></span> <span data-ttu-id="7edd0-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="7edd0-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="7edd0-161">A **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="7edd0-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_url2.png)

    <span data-ttu-id="7edd0-163">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="7edd0-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7edd0-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7edd0-164">These values are not real.</span></span> <span data-ttu-id="7edd0-165">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="7edd0-165">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="7edd0-166">Ezek az értékek fogadásának Jira beépülő modul, hello oktatóanyag későbbi részében ismertetett hello konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="7edd0-166">These values are received during hello configuration of Jira plugin, which is explained later in hello tutorial.</span></span>

5. <span data-ttu-id="7edd0-167">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7edd0-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_certificate.png) 

6. <span data-ttu-id="7edd0-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="7edd0-171">Egy másik webes böngészőablakban jelentkezzen be a helyi kiszolgálón JIRA tooyour rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7edd0-171">In a different web browser window, log in tooyour JIRA on premise server as an administrator.</span></span>

8. <span data-ttu-id="7edd0-172">Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-172">Hover on cog and click hello **Add-ons**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon1.png)

9. <span data-ttu-id="7edd0-174">A bővítmények lap szakaszban kattintson **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-174">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="7edd0-175">Keresési **Kantega SSO JIRA (SAML & Kerberos) a** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="7edd0-175">Search **Kantega SSO for JIRA (SAML & Kerberos)** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon2.png)

10. <span data-ttu-id="7edd0-177">hello beépülő modul telepítésének megkezdése.</span><span class="sxs-lookup"><span data-stu-id="7edd0-177">hello plugin installation starts.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon3.png)

11. <span data-ttu-id="7edd0-179">Hello telepítés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="7edd0-179">Once hello installation is complete.</span></span> <span data-ttu-id="7edd0-180">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-180">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon33.png)

12. <span data-ttu-id="7edd0-182">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-182">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon34.png)
    
13. <span data-ttu-id="7edd0-184">Új beépülő modul megtalálható-e **integrációja**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-184">New plugin is listed under **INTEGRATIONS**.</span></span> <span data-ttu-id="7edd0-185">Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="7edd0-185">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon35.png)

14. <span data-ttu-id="7edd0-187">A hello **SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="7edd0-187">In hello **SAML** section.</span></span> <span data-ttu-id="7edd0-188">Válassza ki **Azure Active Directory (Azure AD)** a hello **Hozzáadás identitásszolgáltató** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="7edd0-188">Select **Azure Active Directory (Azure AD)** from hello **Add identity provider** dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon4.png)

15. <span data-ttu-id="7edd0-190">Válassza ki az előfizetési szinten **alapvető**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-190">Select subscription level as **Basic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon5.png)     

16. <span data-ttu-id="7edd0-192">A hello **alkalmazás tulajdonságainak** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7edd0-192">On hello **App properties** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon6.png)

    <span data-ttu-id="7edd0-194">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-194">a.</span></span> <span data-ttu-id="7edd0-195">Másolás hello **App ID URI** értékét, és használja azt **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím** a hello **Kantega SSO JIRA tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7edd0-195">Copy hello **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on hello **Kantega SSO for JIRA Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="7edd0-196">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-196">b.</span></span> <span data-ttu-id="7edd0-197">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-197">Click **Next**.</span></span>

17. <span data-ttu-id="7edd0-198">A hello **metaadatok importálása** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7edd0-198">On hello **Metadata import** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon7.png)

    <span data-ttu-id="7edd0-200">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-200">a.</span></span> <span data-ttu-id="7edd0-201">Válassza ki **metaadat-fájlt a számítógépen lévő**, és az Azure-portálról letöltött feltöltés metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="7edd0-201">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="7edd0-202">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-202">b.</span></span> <span data-ttu-id="7edd0-203">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-203">Click **Next**.</span></span>

18. <span data-ttu-id="7edd0-204">A hello **nevét és az egyszeri bejelentkezés hely** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7edd0-204">On hello **Name and SSO location** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon8.png)
    
    <span data-ttu-id="7edd0-206">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-206">a.</span></span> <span data-ttu-id="7edd0-207">Adja hozzá az identitásszolgáltató hello nevét a **identitás szolgáltatónevet** szövegmező (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7edd0-207">Add Name of hello Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="7edd0-208">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-208">b.</span></span> <span data-ttu-id="7edd0-209">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-209">Click **Next**.</span></span>

19. <span data-ttu-id="7edd0-210">Ellenőrizze a hello aláíró tanúsítvánnyal, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-210">Verify hello Signing certificate and click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon9.png)

20. <span data-ttu-id="7edd0-212">A hello **JIRA felhasználói fiókok** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7edd0-212">On hello **JIRA user accounts** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon10.png)

    <span data-ttu-id="7edd0-214">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-214">a.</span></span> <span data-ttu-id="7edd0-215">Válassza ki **felhasználók JIRA tartozó belső könyvtárban szükség esetén hozzon létre** , és írja be a hello megfelelő hello csoport felhasználói neve (lehet több nem.</span><span class="sxs-lookup"><span data-stu-id="7edd0-215">Select **Create users in JIRA's internal Directory if needed** and enter hello appropriate name of hello group for users (can be multiple no.</span></span> <span data-ttu-id="7edd0-216">vesszővel elválasztott csoportok).</span><span class="sxs-lookup"><span data-stu-id="7edd0-216">of groups separated by comma).</span></span>

    <span data-ttu-id="7edd0-217">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-217">b.</span></span> <span data-ttu-id="7edd0-218">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-218">Click **Next**.</span></span>

21. <span data-ttu-id="7edd0-219">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-219">Click **Finish**.</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon11.png)

22. <span data-ttu-id="7edd0-221">A hello **tartományok ismert az Azure AD** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7edd0-221">On hello **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon12.png)

    <span data-ttu-id="7edd0-223">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-223">a.</span></span> <span data-ttu-id="7edd0-224">Válassza ki **tartományok ismert** hello lap hello bal oldali panelről.</span><span class="sxs-lookup"><span data-stu-id="7edd0-224">Select **Known domains** from hello left panel of hello page.</span></span>

    <span data-ttu-id="7edd0-225">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-225">b.</span></span> <span data-ttu-id="7edd0-226">Adja meg hello **tartományok ismert** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7edd0-226">Enter domain name in hello **Known domains** textbox.</span></span>

    <span data-ttu-id="7edd0-227">c.</span><span class="sxs-lookup"><span data-stu-id="7edd0-227">c.</span></span> <span data-ttu-id="7edd0-228">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-228">Click **Save**.</span></span> 

> [!TIP]
> <span data-ttu-id="7edd0-229">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7edd0-229">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7edd0-230">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7edd0-230">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7edd0-231">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7edd0-231">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7edd0-232">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7edd0-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="7edd0-233">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7edd0-233">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7edd0-235">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7edd0-235">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7edd0-236">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-236">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7edd0-238">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-238">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7edd0-240">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7edd0-240">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7edd0-242">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7edd0-242">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7edd0-244">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-244">a.</span></span> <span data-ttu-id="7edd0-245">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-245">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7edd0-246">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-246">b.</span></span> <span data-ttu-id="7edd0-247">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7edd0-247">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7edd0-248">c.</span><span class="sxs-lookup"><span data-stu-id="7edd0-248">c.</span></span> <span data-ttu-id="7edd0-249">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-249">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7edd0-250">d.</span><span class="sxs-lookup"><span data-stu-id="7edd0-250">d.</span></span> <span data-ttu-id="7edd0-251">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-251">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-jira-test-user"></a><span data-ttu-id="7edd0-252">Egy Kantega SSO a JIRA tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7edd0-252">Creating a Kantega SSO for JIRA test user</span></span>

<span data-ttu-id="7edd0-253">az Azure AD tooenable felhasználók toolog a tooJIRA, akkor ki kell építenie JIRA be.</span><span class="sxs-lookup"><span data-stu-id="7edd0-253">tooenable Azure AD users toolog in tooJIRA, they must be provisioned into JIRA.</span></span> <span data-ttu-id="7edd0-254">A JIRA Kantega egyszeri Bejelentkezést egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7edd0-254">In Kantega SSO for JIRA, provisioning is a manual task.</span></span>

<span data-ttu-id="7edd0-255">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7edd0-255">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="7edd0-256">Jelentkezzen be a helyi kiszolgálón JIRA tooyour rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7edd0-256">Log in tooyour JIRA on premise server as an administrator.</span></span>

2. <span data-ttu-id="7edd0-257">Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-257">Hover on cog and click hello **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforjira-tutorial/user1.png) 

3. <span data-ttu-id="7edd0-259">A **felhasználókezelés** szakasz lapra, majd **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-259">Under **User management** tab section, click **Create user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforjira-tutorial/user2.png) 

4. <span data-ttu-id="7edd0-261">A hello **"Új felhasználó létrehozása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7edd0-261">On hello **“Create new user”** dialog page, perform hello following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforjira-tutorial/user3.png) 

    <span data-ttu-id="7edd0-263">a.</span><span class="sxs-lookup"><span data-stu-id="7edd0-263">a.</span></span> <span data-ttu-id="7edd0-264">A hello **E-mail cím** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="7edd0-264">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="7edd0-265">b.</span><span class="sxs-lookup"><span data-stu-id="7edd0-265">b.</span></span> <span data-ttu-id="7edd0-266">A hello **teljes nevét** szövegmezőhöz hello felhasználó például Britta Simon típus teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="7edd0-266">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>

    <span data-ttu-id="7edd0-267">c.</span><span class="sxs-lookup"><span data-stu-id="7edd0-267">c.</span></span> <span data-ttu-id="7edd0-268">A hello **felhasználónév** szövegmezőhöz: hello e-mail felhasználó például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="7edd0-268">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="7edd0-269">d.</span><span class="sxs-lookup"><span data-stu-id="7edd0-269">d.</span></span> <span data-ttu-id="7edd0-270">A hello **jelszó** szövegmezőhöz típus hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="7edd0-270">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="7edd0-271">e.</span><span class="sxs-lookup"><span data-stu-id="7edd0-271">e.</span></span> <span data-ttu-id="7edd0-272">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-272">Click **Create user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7edd0-273">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7edd0-273">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7edd0-274">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse azáltal, hogy hozzáférés tooKantega egyszeri Bejelentkezéssel biztosítja a JIRA engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="7edd0-274">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKantega SSO for JIRA.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7edd0-276">**tooassign Britta Simon tooKantega JIRA, az egyszeri bejelentkezés hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7edd0-276">**tooassign Britta Simon tooKantega SSO for JIRA, perform hello following steps:**</span></span>

1. <span data-ttu-id="7edd0-277">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-277">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7edd0-279">Hello alkalmazások listában válassza ki a **Kantega SSO a JIRA**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-279">In hello applications list, select **Kantega SSO for JIRA**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_app.png) 

3. <span data-ttu-id="7edd0-281">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7edd0-281">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7edd0-283">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7edd0-283">Click **Add** button.</span></span> <span data-ttu-id="7edd0-284">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7edd0-284">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7edd0-286">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7edd0-286">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7edd0-287">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7edd0-287">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7edd0-288">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7edd0-288">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7edd0-289">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7edd0-289">Testing single sign-on</span></span>

<span data-ttu-id="7edd0-290">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="7edd0-290">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7edd0-291">Hello Kantega SSO JIRA csempe a hozzáférési Panel hello gombra JIRA alkalmazás kapja meg automatikusan bejelentkezett tooyour Kantega egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7edd0-291">When you click hello Kantega SSO for JIRA tile in hello Access Panel, you should get automatically signed-on tooyour Kantega SSO for JIRA application.</span></span>
<span data-ttu-id="7edd0-292">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7edd0-292">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7edd0-293">További források</span><span class="sxs-lookup"><span data-stu-id="7edd0-293">Additional resources</span></span>

* [<span data-ttu-id="7edd0-294">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7edd0-294">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7edd0-295">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7edd0-295">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_203.png

