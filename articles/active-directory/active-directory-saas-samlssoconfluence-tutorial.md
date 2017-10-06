---
title: "Oktatóanyag: Azure Active Directory-integráció való összefolyás felett az SAML-alapú egyszeri felbontása GmbH |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felbontása GmbH való összefolyás felett SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: fe50636709857ec49023e24bdc8c6cd8c58e3c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a><span data-ttu-id="23c92-103">Oktatóanyag: Azure Active Directory-integráció való összefolyás felett az SAML-alapú egyszeri felbontása GmbH</span><span class="sxs-lookup"><span data-stu-id="23c92-103">Tutorial: Azure Active Directory integration with SAML SSO for Confluence by resolution GmbH</span></span>

<span data-ttu-id="23c92-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate való összefolyás felett a SAML SSO felbontása GmbH az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="23c92-104">In this tutorial, you learn how toointegrate SAML SSO for Confluence by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="23c92-105">SAML SSO való összefolyás felett a felbontása GmbH az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="23c92-105">Integrating SAML SSO for Confluence by resolution GmbH with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="23c92-106">Megadhatja a hozzáférés tooSAML való összefolyás felett az SSO felbontása GmbH rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="23c92-106">You can control in Azure AD who has access tooSAML SSO for Confluence by resolution GmbH</span></span>
- <span data-ttu-id="23c92-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAML való összefolyás felett az SSO felbontása GmbH (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="23c92-107">You can enable your users tooautomatically get signed-on tooSAML SSO for Confluence by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="23c92-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="23c92-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="23c92-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="23c92-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23c92-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="23c92-110">Prerequisites</span></span>

<span data-ttu-id="23c92-111">tooconfigure felbontása GmbH SAML-alapú egyszeri való összefolyás felett az Azure AD-integrációs, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="23c92-111">tooconfigure Azure AD integration with SAML SSO for Confluence by resolution GmbH, you need hello following items:</span></span>

- <span data-ttu-id="23c92-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="23c92-112">An Azure AD subscription</span></span>
- <span data-ttu-id="23c92-113">A SAML SSO a felbontása GmbH egyszeri bejelentkezés engedélyezve van az előfizetésben való összefolyás felett</span><span class="sxs-lookup"><span data-stu-id="23c92-113">A SAML SSO for Confluence by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="23c92-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="23c92-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="23c92-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="23c92-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="23c92-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="23c92-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="23c92-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23c92-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="23c92-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="23c92-118">Scenario description</span></span>
<span data-ttu-id="23c92-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="23c92-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="23c92-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="23c92-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="23c92-121">Megoldási GmbH való összefolyás felett a SAML SSO hozzáadását hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="23c92-121">Adding SAML SSO for Confluence by resolution GmbH from hello gallery</span></span>
2. <span data-ttu-id="23c92-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="23c92-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-hello-gallery"></a><span data-ttu-id="23c92-123">Megoldási GmbH való összefolyás felett a SAML SSO hozzáadását hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="23c92-123">Adding SAML SSO for Confluence by resolution GmbH from hello gallery</span></span>

<span data-ttu-id="23c92-124">tooconfigure hello integrációja való összefolyás felett a SAML SSO felbontása GmbH az Azure AD-be, szükség van tooadd SAML SSO való összefolyás felett felbontása GmbH hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="23c92-124">tooconfigure hello integration of SAML SSO for Confluence by resolution GmbH into Azure AD, you need tooadd SAML SSO for Confluence by resolution GmbH from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="23c92-125">**tooadd való összefolyás felett a SAML SSO felbontása GmbH hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="23c92-125">**tooadd SAML SSO for Confluence by resolution GmbH from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="23c92-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="23c92-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="23c92-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="23c92-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="23c92-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="23c92-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="23c92-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="23c92-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="23c92-133">Hello keresési mezőbe, írja be a **felbontása GmbH való összefolyás felett a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="23c92-133">In hello search box, type **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. <span data-ttu-id="23c92-135">A hello eredmények panelen válassza a **felbontása GmbH való összefolyás felett a SAML SSO**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-135">In hello results panel, select **SAML SSO for Confluence by resolution GmbH**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="23c92-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="23c92-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="23c92-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez való összefolyás felett az SAML-alapú egyszeri által GmbH alapuló "Britta Simon." nevű tesztfelhasználó felbontás</span><span class="sxs-lookup"><span data-stu-id="23c92-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="23c92-139">Egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználót a SAML SSO való összefolyás felett a feloldási GmbH tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="23c92-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAML SSO for Confluence by resolution GmbH is tooa user in Azure AD.</span></span> <span data-ttu-id="23c92-140">Ez azt jelenti az Azure AD-felhasználó és hello kapcsolódó felhasználót a SAML SSO való összefolyás felett a feloldási hivatkozás kapcsolatát GmbH kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="23c92-140">In other words, a link relationship between an Azure AD user and hello related user in SAML SSO for Confluence by resolution GmbH needs toobe established.</span></span>

<span data-ttu-id="23c92-141">A SAML SSO a felbontása GmbH való összefolyás felett, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="23c92-141">In SAML SSO for Confluence by resolution GmbH, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="23c92-142">tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri való összefolyás felett felbontása GmbH, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="23c92-142">tooconfigure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="23c92-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="23c92-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="23c92-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23c92-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="23c92-145">**[A SAML SSO a feloldási GmbH teszt felhasználó való összefolyás felett létrehozása](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  -toohave Britta Simon a SAML SSO való összefolyás felett a egy megfelelője a felbontása GmbH, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="23c92-145">**[Creating a SAML SSO for Confluence by resolution GmbH test user](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)** - toohave a counterpart of Britta Simon in SAML SSO for Confluence by resolution GmbH that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="23c92-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="23c92-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="23c92-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="23c92-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="23c92-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23c92-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="23c92-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, majd állítsa egyszeri bejelentkezéshez a SAML SSO való összefolyás felett a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="23c92-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAML SSO for Confluence by resolution GmbH application.</span></span>

<span data-ttu-id="23c92-150">**tooconfigure az Azure AD egyszeri bejelentkezéshez a SAML SSO való összefolyás felett a felbontása GmbH, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="23c92-150">**tooconfigure Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="23c92-151">Az Azure portál, a hello hello **felbontása GmbH való összefolyás felett a SAML SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="23c92-151">In hello Azure portal, on hello **SAML SSO for Confluence by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="23c92-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="23c92-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. <span data-ttu-id="23c92-155">A hello **SAML SSO való összefolyás felett felbontása GmbH tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="23c92-155">On hello **SAML SSO for Confluence by resolution GmbH Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    <span data-ttu-id="23c92-157">a.</span><span class="sxs-lookup"><span data-stu-id="23c92-157">a.</span></span> <span data-ttu-id="23c92-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="23c92-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="23c92-159">b.</span><span class="sxs-lookup"><span data-stu-id="23c92-159">b.</span></span> <span data-ttu-id="23c92-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="23c92-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="23c92-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="23c92-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="23c92-162">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="23c92-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    <span data-ttu-id="23c92-164">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="23c92-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="23c92-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="23c92-165">These values are not real.</span></span> <span data-ttu-id="23c92-166">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="23c92-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="23c92-167">Ügyfél [való összefolyás felett feloldási GmbH ügyfél által a SAML SSO támogatási csoport](https://www.resolution.de/go/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="23c92-167">Contact [SAML SSO for Confluence by resolution GmbH Client support team](https://www.resolution.de/go/support) tooget these values.</span></span> 

5. <span data-ttu-id="23c92-168">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="23c92-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. <span data-ttu-id="23c92-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. <span data-ttu-id="23c92-172">Egy másik webes böngészőablakban, jelentkezzen be tooyour **való összefolyás felett feloldási GmbH felügyeleti portál a SAML SSO** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="23c92-172">In a different web browser window, log in tooyour **SAML SSO for Confluence by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="23c92-173">Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="23c92-173">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. <span data-ttu-id="23c92-175">Biztosan átirányított tooAdministrator lapot.</span><span class="sxs-lookup"><span data-stu-id="23c92-175">You are redirected tooAdministrator Access page.</span></span> <span data-ttu-id="23c92-176">Megadja a hello jelszót, majd **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-176">Enter hello password and click **Confirm** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. <span data-ttu-id="23c92-178">A **ATLASSIAN piactér** lapra, majd **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="23c92-178">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. <span data-ttu-id="23c92-180">Keresési **SAML-alapú egyszeri bejelentkezés (SSO) való összefolyás felett a** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="23c92-180">Search **SAML Single Sign On (SSO) for Confluence** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. <span data-ttu-id="23c92-182">hello beépülő modul telepítése elindul.</span><span class="sxs-lookup"><span data-stu-id="23c92-182">hello plugin installation will start.</span></span> <span data-ttu-id="23c92-183">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-183">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. <span data-ttu-id="23c92-186">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-186">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. <span data-ttu-id="23c92-188">Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="23c92-188">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. <span data-ttu-id="23c92-190">Az új beépülő modult is található **felhasználók és biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="23c92-190">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. <span data-ttu-id="23c92-192">A **SAML SingleSignOn beépülő modul konfigurációs** kattintson **adja hozzá a további identitásszolgáltató** az identitásszolgáltató tooconfigure hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-192">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button tooconfigure hello settings of Identity Provider.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. <span data-ttu-id="23c92-194">Hajtsa végre az ezen a lapon a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="23c92-194">Perform following steps on this page:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    <span data-ttu-id="23c92-196">a.</span><span class="sxs-lookup"><span data-stu-id="23c92-196">a.</span></span> <span data-ttu-id="23c92-197">Adja hozzá **neve** az identitásszolgáltató (például az Azure AD) hello.</span><span class="sxs-lookup"><span data-stu-id="23c92-197">Add **Name** of hello Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="23c92-198">b.</span><span class="sxs-lookup"><span data-stu-id="23c92-198">b.</span></span> <span data-ttu-id="23c92-199">Adja hozzá **leírás** az identitásszolgáltató (például az Azure AD) hello.</span><span class="sxs-lookup"><span data-stu-id="23c92-199">Add **Description** of hello Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="23c92-200">c.</span><span class="sxs-lookup"><span data-stu-id="23c92-200">c.</span></span> <span data-ttu-id="23c92-201">Kattintson a **XML** és select hello **metaadatok** Azure portálról letöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="23c92-201">Click **XML** and select hello **Metadata** file that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="23c92-202">d.</span><span class="sxs-lookup"><span data-stu-id="23c92-202">d.</span></span> <span data-ttu-id="23c92-203">Kattintson a **terhelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-203">Click **Load** button.</span></span>

    <span data-ttu-id="23c92-204">e.</span><span class="sxs-lookup"><span data-stu-id="23c92-204">e.</span></span> <span data-ttu-id="23c92-205">Hello IdP metaadatok olvas, és kiemelve hello képernyőfelvétel a hello mezők tölti fel.</span><span class="sxs-lookup"><span data-stu-id="23c92-205">It reads hello IdP metadata and populates hello fields as highlighted in hello screenshot.</span></span>   
18. <span data-ttu-id="23c92-206">Kattintson a **beállítások mentése** toosave hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-206">Click **Save settings** button toosave hello settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="23c92-208">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="23c92-208">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="23c92-209">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="23c92-209">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="23c92-210">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="23c92-210">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="23c92-211">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="23c92-211">Creating an Azure AD test user</span></span>
<span data-ttu-id="23c92-212">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="23c92-212">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="23c92-214">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="23c92-214">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="23c92-215">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="23c92-215">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="23c92-217">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="23c92-217">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="23c92-219">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23c92-219">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="23c92-221">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="23c92-221">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="23c92-223">a.</span><span class="sxs-lookup"><span data-stu-id="23c92-223">a.</span></span> <span data-ttu-id="23c92-224">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="23c92-224">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="23c92-225">b.</span><span class="sxs-lookup"><span data-stu-id="23c92-225">b.</span></span> <span data-ttu-id="23c92-226">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="23c92-226">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="23c92-227">c.</span><span class="sxs-lookup"><span data-stu-id="23c92-227">c.</span></span> <span data-ttu-id="23c92-228">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="23c92-228">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="23c92-229">d.</span><span class="sxs-lookup"><span data-stu-id="23c92-229">d.</span></span> <span data-ttu-id="23c92-230">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-230">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a><span data-ttu-id="23c92-231">A SAML SSO a való összefolyás felett feloldási GmbH teszt felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="23c92-231">Creating a SAML SSO for Confluence by resolution GmbH test user</span></span>

<span data-ttu-id="23c92-232">az Azure AD tooenable felhasználók toolog tooSAML való összefolyás felett az SSO felbontása GmbH, a azok kell üzembe való összefolyás felett a SAML SSO be névfeloldási GmbH által.</span><span class="sxs-lookup"><span data-stu-id="23c92-232">tooenable Azure AD users toolog in tooSAML SSO for Confluence by resolution GmbH, they must be provisioned into SAML SSO for Confluence by resolution GmbH.</span></span>  
<span data-ttu-id="23c92-233">A SAML SSO a felbontása GmbH való összefolyás felett egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="23c92-233">In SAML SSO for Confluence by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="23c92-234">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="23c92-234">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="23c92-235">Jelentkezzen be rendszergazdaként való összefolyás felett feloldási GmbH vállalati hely által a SAML SSO tooyour.</span><span class="sxs-lookup"><span data-stu-id="23c92-235">Log in tooyour SAML SSO for Confluence by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="23c92-236">Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="23c92-236">Hover on cog and click hello **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. <span data-ttu-id="23c92-238">A felhasználók szakaszban kattintson **felhasználók hozzáadása az** fülre. A hello **"A felhasználó hozzáadása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="23c92-238">Under Users section, click **Add users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    <span data-ttu-id="23c92-240">a.</span><span class="sxs-lookup"><span data-stu-id="23c92-240">a.</span></span> <span data-ttu-id="23c92-241">A hello **felhasználónév** szövegmezőhöz: hello e-mail Britta Simon például felhasználó.</span><span class="sxs-lookup"><span data-stu-id="23c92-241">In hello **Username** textbox, type hello email of user like Britta Simon.</span></span>

    <span data-ttu-id="23c92-242">b.</span><span class="sxs-lookup"><span data-stu-id="23c92-242">b.</span></span> <span data-ttu-id="23c92-243">A hello **teljes nevét** szövegmezőhöz hello teljes típusnév Britta Simon például felhasználó.</span><span class="sxs-lookup"><span data-stu-id="23c92-243">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="23c92-244">c.</span><span class="sxs-lookup"><span data-stu-id="23c92-244">c.</span></span> <span data-ttu-id="23c92-245">A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="23c92-245">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="23c92-246">d.</span><span class="sxs-lookup"><span data-stu-id="23c92-246">d.</span></span> <span data-ttu-id="23c92-247">A hello **jelszó** szövegmezőhöz Britta Simon típus hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="23c92-247">In hello **Password** textbox, type hello password for Britta Simon.</span></span>

    <span data-ttu-id="23c92-248">e.</span><span class="sxs-lookup"><span data-stu-id="23c92-248">e.</span></span> <span data-ttu-id="23c92-249">Kattintson a **jelszó megerősítése** írja be újból hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="23c92-249">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="23c92-250">f.</span><span class="sxs-lookup"><span data-stu-id="23c92-250">f.</span></span> <span data-ttu-id="23c92-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-251">Click **Add** button.</span></span>    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="23c92-252">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="23c92-252">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="23c92-253">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAML való összefolyás felett az SSO felbontása GmbH megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="23c92-253">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAML SSO for Confluence by resolution GmbH.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="23c92-255">**tooassign Britta Simon tooSAML való összefolyás felett az SSO felbontása GmbH, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="23c92-255">**tooassign Britta Simon tooSAML SSO for Confluence by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="23c92-256">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="23c92-256">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="23c92-258">Hello alkalmazások listában válassza ki a **felbontása GmbH való összefolyás felett a SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="23c92-258">In hello applications list, select **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. <span data-ttu-id="23c92-260">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="23c92-260">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="23c92-262">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="23c92-262">Click **Add** button.</span></span> <span data-ttu-id="23c92-263">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23c92-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="23c92-265">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="23c92-265">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="23c92-266">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23c92-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="23c92-267">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23c92-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="23c92-268">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="23c92-268">Testing single sign-on</span></span>

<span data-ttu-id="23c92-269">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="23c92-269">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="23c92-270">Hello való összefolyás felett a SAML SSO által feloldási GmbH csempe a hozzáférési Panel hello kattintáskor szerezheti be automatikusan bejelentkezett tooyour SAML SSO való összefolyás felett a felbontása GmbH alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="23c92-270">When you click hello SAML SSO for Confluence by resolution GmbH tile in hello Access Panel, you should get automatically signed-on tooyour SAML SSO for Confluence by resolution GmbH application.</span></span>
<span data-ttu-id="23c92-271">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="23c92-271">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="23c92-272">További források</span><span class="sxs-lookup"><span data-stu-id="23c92-272">Additional resources</span></span>

* [<span data-ttu-id="23c92-273">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="23c92-273">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="23c92-274">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="23c92-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

