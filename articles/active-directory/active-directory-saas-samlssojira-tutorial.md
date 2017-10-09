---
title: "Oktatóanyag: Azure Active Directory-integráció SAML-alapú egyszeri a Jira felbontása GmbH |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felbontása GmbH Jira SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: a3436a9aa25640e931a61b5ba4a62611e6e07890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a><span data-ttu-id="30a17-103">Oktatóanyag: Azure Active Directory-integráció SAML-alapú egyszeri a Jira felbontása GmbH</span><span class="sxs-lookup"><span data-stu-id="30a17-103">Tutorial: Azure Active Directory integration with SAML SSO for Jira by resolution GmbH</span></span>

<span data-ttu-id="30a17-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jira a SAML SSO felbontása GmbH az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="30a17-104">In this tutorial, you learn how toointegrate SAML SSO for Jira by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="30a17-105">SAML SSO Jira a felbontása GmbH az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="30a17-105">Integrating SAML SSO for Jira by resolution GmbH with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="30a17-106">Szabályozhatja az Azure AD hozzáférési tooSAML SSO rendelkező Jira a felbontása GmbH</span><span class="sxs-lookup"><span data-stu-id="30a17-106">You can control in Azure AD who has access tooSAML SSO for Jira by resolution GmbH</span></span>
- <span data-ttu-id="30a17-107">Engedélyezheti a Jira a felhasználók tooautomatically get bejelentkezett tooSAML SSO felbontást GmbH (egyszeri bejelentkezés) az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="30a17-107">You can enable your users tooautomatically get signed-on tooSAML SSO for Jira by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="30a17-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="30a17-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="30a17-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="30a17-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30a17-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="30a17-110">Prerequisites</span></span>

<span data-ttu-id="30a17-111">tooconfigure felbontása GmbH SAML-alapú egyszeri Jira az Azure AD-integrációs, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="30a17-111">tooconfigure Azure AD integration with SAML SSO for Jira by resolution GmbH, you need hello following items:</span></span>

- <span data-ttu-id="30a17-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="30a17-112">An Azure AD subscription</span></span>
- <span data-ttu-id="30a17-113">A SAML SSO a Jira felbontása GmbH egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="30a17-113">A SAML SSO for Jira by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="30a17-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="30a17-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="30a17-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="30a17-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="30a17-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="30a17-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="30a17-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30a17-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="30a17-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="30a17-118">Scenario description</span></span>
<span data-ttu-id="30a17-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="30a17-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="30a17-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="30a17-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="30a17-121">Megoldási GmbH Jira a SAML SSO hozzáadását hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="30a17-121">Adding SAML SSO for Jira by resolution GmbH from hello gallery</span></span>
2. <span data-ttu-id="30a17-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="30a17-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-hello-gallery"></a><span data-ttu-id="30a17-123">Megoldási GmbH Jira a SAML SSO hozzáadását hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="30a17-123">Adding SAML SSO for Jira by resolution GmbH from hello gallery</span></span>
<span data-ttu-id="30a17-124">tooconfigure hello integrációja Jira a SAML SSO felbontása GmbH az Azure AD-be, szükség van tooadd SAML SSO Jira felbontása GmbH hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="30a17-124">tooconfigure hello integration of SAML SSO for Jira by resolution GmbH into Azure AD, you need tooadd SAML SSO for Jira by resolution GmbH from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="30a17-125">**tooadd Jira a SAML SSO felbontása GmbH hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="30a17-125">**tooadd SAML SSO for Jira by resolution GmbH from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="30a17-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="30a17-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="30a17-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="30a17-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="30a17-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="30a17-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="30a17-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="30a17-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="30a17-133">Hello keresési mezőbe, írja be a **Jira felbontása GmbH a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="30a17-133">In hello search box, type **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. <span data-ttu-id="30a17-135">A hello eredmények panelen válassza ki a **Jira felbontása GmbH a SAML SSO**, és kattintson a **hozzáadása** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-135">In hello results panel, select **SAML SSO for Jira by resolution GmbH**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="30a17-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="30a17-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="30a17-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri a Jira GmbH alapján "Britta Simon." nevű tesztfelhasználó felbontása</span><span class="sxs-lookup"><span data-stu-id="30a17-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="30a17-139">Egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználót a SAML SSO Jira a feloldási GmbH tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="30a17-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAML SSO for Jira by resolution GmbH is tooa user in Azure AD.</span></span> <span data-ttu-id="30a17-140">Ez azt jelenti az Azure AD-felhasználó és hello kapcsolódó felhasználót a SAML SSO Jira a feloldási hivatkozás kapcsolatát GmbH kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="30a17-140">In other words, a link relationship between an Azure AD user and hello related user in SAML SSO for Jira by resolution GmbH needs toobe established.</span></span>

<span data-ttu-id="30a17-141">A SAML SSO a felbontása GmbH Jira, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="30a17-141">In SAML SSO for Jira by resolution GmbH, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="30a17-142">tooconfigure és ellenőrzéséhez az Azure AD egyszeri bejelentkezéshez a SAML SSO Jira felbontása GmbH, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="30a17-142">tooconfigure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="30a17-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="30a17-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="30a17-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30a17-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="30a17-145">**[A SAML SSO a feloldási GmbH teszt felhasználó Jira létrehozása](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  -toohave Britta Simon a SAML SSO a Jira egy megfelelője a felbontása GmbH, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="30a17-145">**[Creating a SAML SSO for Jira by resolution GmbH test user](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)** - toohave a counterpart of Britta Simon in SAML SSO for Jira by resolution GmbH that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="30a17-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="30a17-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="30a17-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="30a17-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="30a17-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="30a17-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="30a17-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, majd állítsa egyszeri bejelentkezéshez a SAML SSO Jira a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="30a17-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAML SSO for Jira by resolution GmbH application.</span></span>

<span data-ttu-id="30a17-150">**tooconfigure az Azure AD egyszeri bejelentkezéshez a SAML SSO Jira a felbontása GmbH, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="30a17-150">**tooconfigure Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="30a17-151">Az Azure portál, a hello hello **Jira felbontása GmbH a SAML SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="30a17-151">In hello Azure portal, on hello **SAML SSO for Jira by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="30a17-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="30a17-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. <span data-ttu-id="30a17-155">A hello **SAML SSO Jira felbontása GmbH tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="30a17-155">On hello **SAML SSO for Jira by resolution GmbH Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    <span data-ttu-id="30a17-157">a.</span><span class="sxs-lookup"><span data-stu-id="30a17-157">a.</span></span> <span data-ttu-id="30a17-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="30a17-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="30a17-159">b.</span><span class="sxs-lookup"><span data-stu-id="30a17-159">b.</span></span> <span data-ttu-id="30a17-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="30a17-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="30a17-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="30a17-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="30a17-162">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="30a17-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    <span data-ttu-id="30a17-164">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="30a17-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="30a17-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="30a17-165">These values are not real.</span></span> <span data-ttu-id="30a17-166">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="30a17-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="30a17-167">Ügyfél [Jira feloldási GmbH ügyfél által a SAML SSO támogatási csoport](https://www.resolution.de/go/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="30a17-167">Contact [SAML SSO for Jira by resolution GmbH Client support team](https://www.resolution.de/go/support) tooget these values.</span></span> 

5. <span data-ttu-id="30a17-168">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="30a17-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. <span data-ttu-id="30a17-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="30a17-172">Egy másik webes böngészőablakban, jelentkezzen be tooyour **Jira feloldási GmbH felügyeleti portál a SAML SSO** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="30a17-172">In a different web browser window, log in tooyour **SAML SSO for Jira by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="30a17-173">Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="30a17-173">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. <span data-ttu-id="30a17-175">Biztosan átirányított tooAdministrator lapot.</span><span class="sxs-lookup"><span data-stu-id="30a17-175">You are redirected tooAdministrator Access page.</span></span> <span data-ttu-id="30a17-176">Adja meg a hello **jelszó** kattintson **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-176">Enter hello **Password** and click **Confirm** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. <span data-ttu-id="30a17-178">A bővítmények lap szakaszban kattintson **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="30a17-178">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="30a17-179">Keresési **SAML-alapú egyszeri bejelentkezés (SSO) a JIRA** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="30a17-179">Search **SAML Single Sign On (SSO) for JIRA** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. <span data-ttu-id="30a17-181">hello beépülő modul telepítése elindul.</span><span class="sxs-lookup"><span data-stu-id="30a17-181">hello plugin installation will start.</span></span> <span data-ttu-id="30a17-182">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-182">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. <span data-ttu-id="30a17-185">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-185">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. <span data-ttu-id="30a17-187">Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="30a17-187">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. <span data-ttu-id="30a17-189">A **SAML SingleSignOn beépülő modul konfigurációs** kattintson **adja hozzá a további identitásszolgáltató** az identitásszolgáltató tooconfigure hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-189">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button tooconfigure hello settings of Identity Provider.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. <span data-ttu-id="30a17-191">Hajtsa végre az ezen a lapon a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="30a17-191">Perform following steps on this page:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon5.png)
 
    <span data-ttu-id="30a17-193">a.</span><span class="sxs-lookup"><span data-stu-id="30a17-193">a.</span></span> <span data-ttu-id="30a17-194">Adja hozzá **neve** az identitásszolgáltató (például az Azure AD) hello.</span><span class="sxs-lookup"><span data-stu-id="30a17-194">Add **Name** of hello Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="30a17-195">b.</span><span class="sxs-lookup"><span data-stu-id="30a17-195">b.</span></span> <span data-ttu-id="30a17-196">Adja hozzá **leírás** az identitásszolgáltató (például az Azure AD) hello.</span><span class="sxs-lookup"><span data-stu-id="30a17-196">Add **Description** of hello Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="30a17-197">c.</span><span class="sxs-lookup"><span data-stu-id="30a17-197">c.</span></span> <span data-ttu-id="30a17-198">Kattintson a **XML** és select hello **metaadatok** fájlt, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="30a17-198">Click **XML** and select hello **Metadata** file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="30a17-199">d.</span><span class="sxs-lookup"><span data-stu-id="30a17-199">d.</span></span> <span data-ttu-id="30a17-200">Kattintson a **terhelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-200">Click **Load** button.</span></span>

    <span data-ttu-id="30a17-201">e.</span><span class="sxs-lookup"><span data-stu-id="30a17-201">e.</span></span> <span data-ttu-id="30a17-202">Hello IdP metaadatok olvas, és kiemelve hello képernyőfelvétel a hello mezők tölti fel.</span><span class="sxs-lookup"><span data-stu-id="30a17-202">It reads hello IdP metadata and populates hello fields as highlighted in hello screenshot.</span></span>   

16. <span data-ttu-id="30a17-203">Kattintson a **beállítások mentése** toosave hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-203">Click **Save settings** button toosave hello settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="30a17-205">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="30a17-205">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="30a17-206">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="30a17-206">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="30a17-207">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="30a17-207">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="30a17-208">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="30a17-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="30a17-209">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="30a17-209">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="30a17-211">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="30a17-211">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="30a17-212">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="30a17-212">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="30a17-214">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="30a17-214">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="30a17-216">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30a17-216">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="30a17-218">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="30a17-218">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="30a17-220">a.</span><span class="sxs-lookup"><span data-stu-id="30a17-220">a.</span></span> <span data-ttu-id="30a17-221">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="30a17-221">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="30a17-222">b.</span><span class="sxs-lookup"><span data-stu-id="30a17-222">b.</span></span> <span data-ttu-id="30a17-223">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="30a17-223">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="30a17-224">c.</span><span class="sxs-lookup"><span data-stu-id="30a17-224">c.</span></span> <span data-ttu-id="30a17-225">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="30a17-225">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="30a17-226">d.</span><span class="sxs-lookup"><span data-stu-id="30a17-226">d.</span></span> <span data-ttu-id="30a17-227">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-227">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a><span data-ttu-id="30a17-228">A SAML SSO a Jira feloldási GmbH teszt felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="30a17-228">Creating a SAML SSO for Jira by resolution GmbH test user</span></span>

<span data-ttu-id="30a17-229">az Azure AD tooenable felhasználók toolog tooSAML Jira az SSO felbontása GmbH, a azok kell üzembe Jira a SAML SSO be névfeloldási GmbH által.</span><span class="sxs-lookup"><span data-stu-id="30a17-229">tooenable Azure AD users toolog in tooSAML SSO for Jira by resolution GmbH, they must be provisioned into SAML SSO for Jira by resolution GmbH.</span></span>  
<span data-ttu-id="30a17-230">A SAML SSO a felbontása GmbH Jira kézi tevékenység kiépítése.</span><span class="sxs-lookup"><span data-stu-id="30a17-230">In SAML SSO for Jira by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="30a17-231">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="30a17-231">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="30a17-232">Jelentkezzen be rendszergazdaként a feloldási GmbH vállalati hely Jira SAML SSO tooyour.</span><span class="sxs-lookup"><span data-stu-id="30a17-232">Log in tooyour SAML SSO for Jira by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="30a17-233">Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="30a17-233">Hover on cog and click hello **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. <span data-ttu-id="30a17-235">Átirányított tooAdministrator hozzáférés lap tooenter áll **jelszó** kattintson **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-235">You are redirected tooAdministrator Access page tooenter **Password** and click **Confirm** button.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. <span data-ttu-id="30a17-237">A **felhasználókezelés** szakasz lapra, majd **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="30a17-237">Under **User management** tab section, click **create user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. <span data-ttu-id="30a17-239">A hello **"Új felhasználó létrehozása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="30a17-239">On hello **“Create new user”** dialog page, perform hello following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    <span data-ttu-id="30a17-241">a.</span><span class="sxs-lookup"><span data-stu-id="30a17-241">a.</span></span> <span data-ttu-id="30a17-242">A hello **E-mail cím** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="30a17-242">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="30a17-243">b.</span><span class="sxs-lookup"><span data-stu-id="30a17-243">b.</span></span> <span data-ttu-id="30a17-244">A hello **teljes nevét** szövegmezőhöz hello felhasználó például Britta Simon típus teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="30a17-244">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>

    <span data-ttu-id="30a17-245">c.</span><span class="sxs-lookup"><span data-stu-id="30a17-245">c.</span></span> <span data-ttu-id="30a17-246">A hello **felhasználónév** szövegmezőhöz: hello e-mail felhasználó például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="30a17-246">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="30a17-247">d.</span><span class="sxs-lookup"><span data-stu-id="30a17-247">d.</span></span> <span data-ttu-id="30a17-248">A hello **jelszó** szövegmezőhöz típus hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="30a17-248">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="30a17-249">e.</span><span class="sxs-lookup"><span data-stu-id="30a17-249">e.</span></span> <span data-ttu-id="30a17-250">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="30a17-250">Click **Create user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="30a17-251">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="30a17-251">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="30a17-252">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse azáltal, hogy egyszeri Bejelentkezéses hozzáférést tooSAML biztosítja a Jira felbontása GmbH engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="30a17-252">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAML SSO for Jira by resolution GmbH.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="30a17-254">**tooassign Britta Simon tooSAML Jira az SSO felbontása GmbH, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="30a17-254">**tooassign Britta Simon tooSAML SSO for Jira by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="30a17-255">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="30a17-255">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="30a17-257">Hello alkalmazások listában válassza ki a **Jira felbontása GmbH a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="30a17-257">In hello applications list, select **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. <span data-ttu-id="30a17-259">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="30a17-259">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="30a17-261">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="30a17-261">Click **Add** button.</span></span> <span data-ttu-id="30a17-262">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30a17-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="30a17-264">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="30a17-264">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="30a17-265">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30a17-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="30a17-266">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30a17-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="30a17-267">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="30a17-267">Testing single sign-on</span></span>

<span data-ttu-id="30a17-268">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="30a17-268">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="30a17-269">Hello Jira a SAML SSO által feloldási GmbH csempe a hozzáférési Panel hello kattintáskor szerezheti be automatikusan bejelentkezett tooyour SAML SSO Jira a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="30a17-269">When you click hello SAML SSO for Jira by resolution GmbH tile in hello Access Panel, you should get automatically signed-on tooyour SAML SSO for Jira by resolution GmbH application.</span></span>
<span data-ttu-id="30a17-270">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="30a17-270">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="30a17-271">További források</span><span class="sxs-lookup"><span data-stu-id="30a17-271">Additional resources</span></span>

* [<span data-ttu-id="30a17-272">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="30a17-272">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="30a17-273">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="30a17-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

