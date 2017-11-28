---
title: "Oktatóanyag: Azure Active Directoryval integrált JIRA SAML SSO Microsoft |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Microsoft által JIRA SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0dc847b9-eec4-4c31-845e-0144ddedc4a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 178c4c040d9939bca271ac185ca5c2feb14f1247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a><span data-ttu-id="8e20e-103">Oktatóanyag: Azure Active Directoryval integrált JIRA SAML SSO Microsoft</span><span class="sxs-lookup"><span data-stu-id="8e20e-103">Tutorial: Azure Active Directory integration with JIRA SAML SSO by Microsoft</span></span>

<span data-ttu-id="8e20e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate JIRA SAML SSO Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8e20e-104">In this tutorial, you learn how toointegrate JIRA SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8e20e-105">Microsoft JIRA SAML SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8e20e-105">Integrating JIRA SAML SSO by Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8e20e-106">Megadhatja a SAML SSO Microsoft access tooJIRA rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8e20e-106">You can control in Azure AD who has access tooJIRA SAML SSO by Microsoft</span></span>
- <span data-ttu-id="8e20e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJIRA SAML SSO (egyszeri bejelentkezés) a Microsoft által a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8e20e-107">You can enable your users tooautomatically get signed-on tooJIRA SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8e20e-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8e20e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8e20e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8e20e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e20e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8e20e-110">Prerequisites</span></span>

<span data-ttu-id="8e20e-111">tooconfigure JIRA SAML SSO Microsoft Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="8e20e-111">tooconfigure Azure AD integration with JIRA SAML SSO by Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="8e20e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8e20e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8e20e-113">JIRA kiszolgáló alkalmazás telepítve van egy 64 bites Windows server (a helyszínen vagy a hello felhőalapú infrastruktúra-szolgáltatási infrastruktúra)</span><span class="sxs-lookup"><span data-stu-id="8e20e-113">JIRA server application installed on a Windows 64-bit server (on premise or on hello cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="8e20e-114">JIRA kiszolgáló HTTPS-kompatibilis</span><span class="sxs-lookup"><span data-stu-id="8e20e-114">JIRA server is HTTPS enabled</span></span>
- <span data-ttu-id="8e20e-115">Megjegyzés: hello támogatott verziók JIRA beépülő modul szakasz alatt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="8e20e-115">Note hello supported versions for JIRA Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="8e20e-116">JIRA kiszolgálóhoz lehet csatlakozni az internethez különösen tooAzure AD bejelentkezési lapon a hitelesítéshez és kell tudni tooreceive hello Azure ad-token</span><span class="sxs-lookup"><span data-stu-id="8e20e-116">JIRA server is reachable on internet particularly tooAzure AD Login page for authentication and should able tooreceive hello token from Azure AD</span></span>
- <span data-ttu-id="8e20e-117">Rendszergazdai hitelesítő adataival JIRA beállítása</span><span class="sxs-lookup"><span data-stu-id="8e20e-117">Admin credentials are set up in JIRA</span></span>
- <span data-ttu-id="8e20e-118">WebSudo JIRA le van tiltva</span><span class="sxs-lookup"><span data-stu-id="8e20e-118">WebSudo is disabled in JIRA</span></span>
- <span data-ttu-id="8e20e-119">Teszt felhasználó hello JIRA kiszolgálói alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e20e-119">Test user created in hello JIRA server application</span></span>

> [!NOTE]
> <span data-ttu-id="8e20e-120">Ez az oktatóanyag lépéseit tootest hello, nem javasoljuk JIRA az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8e20e-120">tootest hello steps in this tutorial, we do not recommend using a production environment of JIRA.</span></span> <span data-ttu-id="8e20e-121">Vizsgálat hello integrációs először fejlesztési vagy átmeneti környezet hello alkalmazás, majd használja hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8e20e-121">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="8e20e-122">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8e20e-122">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8e20e-123">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8e20e-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8e20e-124">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8e20e-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-jira"></a><span data-ttu-id="8e20e-125">JIRA támogatott verziói</span><span class="sxs-lookup"><span data-stu-id="8e20e-125">Supported versions of JIRA</span></span> 

<span data-ttu-id="8e20e-126">Mostantól JIRA következő verziói támogatottak:</span><span class="sxs-lookup"><span data-stu-id="8e20e-126">As of now, following versions of JIRA are supported:</span></span>

- <span data-ttu-id="8e20e-127">JIRA Core és a szoftver: 6.0 too7.2.0</span><span class="sxs-lookup"><span data-stu-id="8e20e-127">JIRA Core and Software: 6.0 too7.2.0</span></span>
- <span data-ttu-id="8e20e-128">JIRA ügyfélszolgálatához: 3.0-s too3.2</span><span class="sxs-lookup"><span data-stu-id="8e20e-128">JIRA Service Desk: 3.0 too3.2</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8e20e-129">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8e20e-129">Scenario description</span></span>
<span data-ttu-id="8e20e-130">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8e20e-130">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8e20e-131">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8e20e-131">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8e20e-132">Hozzáadás JIRA SAML SSO Microsoft hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8e20e-132">Adding JIRA SAML SSO by Microsoft from hello gallery</span></span>
2. <span data-ttu-id="8e20e-133">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8e20e-133">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jira-saml-sso-by-microsoft-from-hello-gallery"></a><span data-ttu-id="8e20e-134">Hozzáadás JIRA SAML SSO Microsoft hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8e20e-134">Adding JIRA SAML SSO by Microsoft from hello gallery</span></span>
<span data-ttu-id="8e20e-135">tooconfigure hello integrációja JIRA SAML SSO Microsoft által az Azure AD-be, meg kell tooadd JIRA SAML SSO Microsoft hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8e20e-135">tooconfigure hello integration of JIRA SAML SSO by Microsoft into Azure AD, you need tooadd JIRA SAML SSO by Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8e20e-136">**tooadd JIRA SAML SSO Microsoft hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8e20e-136">**tooadd JIRA SAML SSO by Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e20e-137">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-137">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8e20e-139">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-139">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8e20e-140">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-140">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8e20e-142">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8e20e-142">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8e20e-144">Hello keresési mezőbe, írja be a **JIRA SAML SSO Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-144">In hello search box, type **JIRA SAML SSO by Microsoft**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_search.png)

5. <span data-ttu-id="8e20e-146">A hello eredmények panelen válassza a **JIRA SAML SSO Microsoft**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-146">In hello results panel, select **JIRA SAML SSO by Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8e20e-148">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8e20e-148">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8e20e-149">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés JIRA SAML-alapú egyszeri "Britta Simon." nevű tesztfelhasználó alapján a Microsoft által</span><span class="sxs-lookup"><span data-stu-id="8e20e-149">In this section, you configure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8e20e-150">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen JIRA SAML SSO Microsoft hello megfelelőjére felhasználó tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8e20e-150">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in JIRA SAML SSO by Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="8e20e-151">Ez azt jelenti hello kapcsolódó felhasználó a Microsoft által JIRA SAML SSO és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8e20e-151">In other words, a link relationship between an Azure AD user and hello related user in JIRA SAML SSO by Microsoft needs toobe established.</span></span>

<span data-ttu-id="8e20e-152">A Microsoft által JIRA a SAML SSO, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8e20e-152">In JIRA SAML SSO by Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8e20e-153">tooconfigure és JIRA SAML-alapú egyszeri Microsoft Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8e20e-153">tooconfigure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8e20e-154">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8e20e-154">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8e20e-155">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8e20e-155">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8e20e-156">**[Egy JIRA SAML SSO Microsoft teszt felhasználó létrehozása](#creating-a-jira-saml-sso-by-microsoft-test-user)**  -toohave Britta Simon JIRA SAML SSO felhasználói csatolt toohello az Azure AD-ábrázolása, Microsoft által a valami.</span><span class="sxs-lookup"><span data-stu-id="8e20e-156">**[Creating a JIRA SAML SSO by Microsoft test user](#creating-a-jira-saml-sso-by-microsoft-test-user)** - toohave a counterpart of Britta Simon in JIRA SAML SSO by Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8e20e-157">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8e20e-157">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8e20e-158">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8e20e-158">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8e20e-159">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e20e-159">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8e20e-160">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a JIRA SAML SSO konfigurálása egyszeri bejelentkezéshez Microsoft-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8e20e-160">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your JIRA SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="8e20e-161">**JIRA SAML-alapú egyszeri Microsoft, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8e20e-161">**tooconfigure Azure AD single sign-on with JIRA SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e20e-162">Az Azure portál, a hello hello **JIRA SAML SSO Microsoft** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-162">In hello Azure portal, on hello **JIRA SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8e20e-164">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8e20e-164">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_samlbase.png)

3. <span data-ttu-id="8e20e-166">A hello **JIRA SAML SSO Microsoft Domain és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8e20e-166">On hello **JIRA SAML SSO by Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_url.png)

    <span data-ttu-id="8e20e-168">a.</span><span class="sxs-lookup"><span data-stu-id="8e20e-168">a.</span></span> <span data-ttu-id="8e20e-169">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="8e20e-169">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="8e20e-170">b.</span><span class="sxs-lookup"><span data-stu-id="8e20e-170">b.</span></span> <span data-ttu-id="8e20e-171">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="8e20e-171">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="8e20e-172">c.</span><span class="sxs-lookup"><span data-stu-id="8e20e-172">c.</span></span> <span data-ttu-id="8e20e-173">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="8e20e-173">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8e20e-174">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8e20e-174">These values are not real.</span></span> <span data-ttu-id="8e20e-175">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="8e20e-175">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="8e20e-176">Port megadása nem kötelező, abban az esetben, ha egy elnevezett URL-címet.</span><span class="sxs-lookup"><span data-stu-id="8e20e-176">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="8e20e-177">Ezek az értékek fogadásának Jira beépülő modul, hello oktatóanyag későbbi részében ismertetett hello konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="8e20e-177">These values are received during hello configuration of Jira plugin, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="8e20e-178">toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8e20e-178">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="8e20e-179">a.</span><span class="sxs-lookup"><span data-stu-id="8e20e-179">a.</span></span> <span data-ttu-id="8e20e-180">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-180">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="8e20e-182">b.</span><span class="sxs-lookup"><span data-stu-id="8e20e-182">b.</span></span> <span data-ttu-id="8e20e-183">Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="8e20e-183">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="8e20e-185">c.</span><span class="sxs-lookup"><span data-stu-id="8e20e-185">c.</span></span> <span data-ttu-id="8e20e-186">Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="8e20e-186">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="8e20e-188">d.</span><span class="sxs-lookup"><span data-stu-id="8e20e-188">d.</span></span> <span data-ttu-id="8e20e-189">Most lépjen tulajdonságlapján toohello **JIRA SAML SSO Microsoft** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="8e20e-189">Now go toohello property page of **JIRA SAML SSO by Microsoft** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/appid.png)

    <span data-ttu-id="8e20e-191">e.</span><span class="sxs-lookup"><span data-stu-id="8e20e-191">e.</span></span> <span data-ttu-id="8e20e-192">Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` , és másolja ezt az értéket a Jegyzettömbben, a rendszer később hello beépülő modul hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="8e20e-192">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for hello configuration of hello plugin.</span></span>

5. <span data-ttu-id="8e20e-193">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-193">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8e20e-195">Ügyfél [Microsoft](mailto:waadpartners@microsoft.com) a következő hello JIRA beépülő modul az információ hello.</span><span class="sxs-lookup"><span data-stu-id="8e20e-195">Contact [Microsoft](mailto:waadpartners@microsoft.com) with hello following information for hello JIRA plugin.</span></span>
    
    *   <span data-ttu-id="8e20e-196">Ügyfél neve:</span><span class="sxs-lookup"><span data-stu-id="8e20e-196">Customer Name:</span></span>
    *   <span data-ttu-id="8e20e-197">Elsődleges tartománynév:</span><span class="sxs-lookup"><span data-stu-id="8e20e-197">Primary domain name:</span></span>
    *   <span data-ttu-id="8e20e-198">Prémium szintű Azure AD: Igen/nem (beépülő modul lesz elérhető tooall hello vevő ingyenes, a Basic és a Premium Termékváltozat)</span><span class="sxs-lookup"><span data-stu-id="8e20e-198">Azure AD Premium: Yes/No (Plugin will be available tooall     hello customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="8e20e-199">Ez az integráció használó felhasználók száma:</span><span class="sxs-lookup"><span data-stu-id="8e20e-199">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="8e20e-200">JIRA verzió:</span><span class="sxs-lookup"><span data-stu-id="8e20e-200">JIRA Version:</span></span>
    *   <span data-ttu-id="8e20e-201">Megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="8e20e-201">Comments:</span></span>

7. <span data-ttu-id="8e20e-202">Egy másik webes böngészőablakban jelentkezzen be tooyour JIRA példány rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8e20e-202">In a different web browser window, log in tooyour JIRA instance as an administrator.</span></span>

8. <span data-ttu-id="8e20e-203">Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-203">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="8e20e-205">A bővítmények lap szakaszban kattintson **bővítmények kezelése**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-205">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/addon7.png)

10. <span data-ttu-id="8e20e-207">Manuális feltöltéséhez hello beépülő modul a Microsoft biztosítja.</span><span class="sxs-lookup"><span data-stu-id="8e20e-207">Manually upload hello plugin provided by Microsoft.</span></span> <span data-ttu-id="8e20e-208">Hello beépülő modul telepítése után megjelenik **felhasználó telepített** bővítmények szakasza **kezelése bővítmény** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e20e-208">Once hello plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="8e20e-209">Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="8e20e-209">Click **Configure** tooconfigure hello new plugin.</span></span>

12. <span data-ttu-id="8e20e-210">Hajtsa végre a következő lépéseket a konfiguráció lapon:</span><span class="sxs-lookup"><span data-stu-id="8e20e-210">Perform following steps on configuration page:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="8e20e-212">a.</span><span class="sxs-lookup"><span data-stu-id="8e20e-212">a.</span></span> <span data-ttu-id="8e20e-213">A **metaadatainak URL-CÍMÉT** illessze be a hello **metaadatainak URL-CÍMÉT** jön létre az Azure AD-ből, majd kattintson a hello **megoldásához** gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-213">In **Metadata URL** paste hello **Metadata URL** generated from Azure AD and click hello **Resolve** button.</span></span> <span data-ttu-id="8e20e-214">Beolvassa a hello IdP metaadatainak URL-CÍMÉT és az összes hello mezők adatai tölti fel.</span><span class="sxs-lookup"><span data-stu-id="8e20e-214">It reads hello IdP metadata URL and populates all hello fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="8e20e-215">Alapértelmezett SAML Felhasználóazonosító helye azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8e20e-215">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="8e20e-216">Tooan attribútum beállításaival, és adja meg a hello megfelelő attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="8e20e-216">You can change this tooan attribute option and enter hello appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="8e20e-217">Győződjön meg arról, hogy van-e rendelve hello alkalmazást úgy, hogy nem történt hiba elhárításához hello metaadatok csak egy tanúsítványra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-217">Ensure that there is only one certificate mapped against hello app so that there is no error in resolving hello metadata.</span></span> <span data-ttu-id="8e20e-218">Ha több tanúsítvány hello metaadatok feloldása után a rendszergazda hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="8e20e-218">If there are multiple certificates, upon resolving hello metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="8e20e-219">b.</span><span class="sxs-lookup"><span data-stu-id="8e20e-219">b.</span></span> <span data-ttu-id="8e20e-220">Másolás hello **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-címen** értéket, majd illessze be őket a **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-címen** szövegmezőből, illetve a **JIRA SAML SSO Microsoft Domain és URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="8e20e-220">Copy hello **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **JIRA SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="8e20e-221">c.</span><span class="sxs-lookup"><span data-stu-id="8e20e-221">c.</span></span> <span data-ttu-id="8e20e-222">A **bejelentkezési gomb neve** hello típusnév gomb a szervezet által hello felhasználók toosee a bejelentkezési képernyőn.</span><span class="sxs-lookup"><span data-stu-id="8e20e-222">In **Login Button Name** type hello name of button your organization wants hello users toosee on login screen.</span></span>

    <span data-ttu-id="8e20e-223">d.</span><span class="sxs-lookup"><span data-stu-id="8e20e-223">d.</span></span> <span data-ttu-id="8e20e-224">A **SAML felhasználói azonosító helyek** válassza **hello NameIdentifier elemében hello tulajdonos utasítás alkalmazás felhasználói Azonosítóját** vagy **felhasználói azonosító attribútum elem van**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-224">In **SAML User ID Locations** select either **User ID is in hello NameIdentifier element of hello Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="8e20e-225">Ezt az Azonosítót toobe hello JIRA felhasználói azonosítóval rendelkezik. Ha hello felhasználói azonosító nem egyezik, majd rendszer nem engedélyezi a felhasználók toolog.</span><span class="sxs-lookup"><span data-stu-id="8e20e-225">This ID has toobe hello JIRA user id. If hello user id is not matched, then system will not allow users toolog in.</span></span> 
    
    <span data-ttu-id="8e20e-226">e.</span><span class="sxs-lookup"><span data-stu-id="8e20e-226">e.</span></span> <span data-ttu-id="8e20e-227">Ha **felhasználói azonosító attribútum elem van** beállítás, ezt a **attribútum neve** hello típusnév szövegmező felhasználói azonosítót várt hello attribútum.</span><span class="sxs-lookup"><span data-stu-id="8e20e-227">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type hello name of hello attribute where User Id is expected.</span></span> 

    <span data-ttu-id="8e20e-228">f.</span><span class="sxs-lookup"><span data-stu-id="8e20e-228">f.</span></span> <span data-ttu-id="8e20e-229">Ha az Azure ad-val hello összevont tartományt (például az AD FS stb.) használ, majd kattintson a hello **engedélyezése Hitelesítőtartományának feltárási** lehetőséget, majd hello konfigurálása **tartománynév**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-229">If you are using hello federated domain (like ADFS etc.) with Azure AD, then click on hello **Enable Home Realm Discovery** option and configure hello **Domain Name**.</span></span>
    
    <span data-ttu-id="8e20e-230">g.</span><span class="sxs-lookup"><span data-stu-id="8e20e-230">g.</span></span> <span data-ttu-id="8e20e-231">A **tartománynév** hello tartomány neve itt hello az AD FS-alapú bejelentkezés esetén.</span><span class="sxs-lookup"><span data-stu-id="8e20e-231">In **Domain Name** type hello domain name here in case of hello ADFS-based login.</span></span>

    <span data-ttu-id="8e20e-232">h.</span><span class="sxs-lookup"><span data-stu-id="8e20e-232">h.</span></span> <span data-ttu-id="8e20e-233">Ellenőrizze **engedélyezése egyszeri bejelentkezéshez kimenő** Ha toolog ki akarja-e az Azure AD, ha a JIRA kijelentkezik a felhasználói.</span><span class="sxs-lookup"><span data-stu-id="8e20e-233">Check **Enable Single Sign out** if you wish toolog out from Azure AD when a user logs out from JIRA.</span></span> 

    <span data-ttu-id="8e20e-234">i.</span><span class="sxs-lookup"><span data-stu-id="8e20e-234">i.</span></span> <span data-ttu-id="8e20e-235">Kattintson a **mentése** toosave hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-235">Click **Save** button toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="8e20e-236">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8e20e-236">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8e20e-237">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8e20e-237">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8e20e-238">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8e20e-238">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8e20e-239">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e20e-239">Creating an Azure AD test user</span></span>
<span data-ttu-id="8e20e-240">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8e20e-240">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8e20e-242">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8e20e-242">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e20e-243">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-243">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8e20e-245">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-245">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8e20e-247">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8e20e-247">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8e20e-249">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8e20e-249">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8e20e-251">a.</span><span class="sxs-lookup"><span data-stu-id="8e20e-251">a.</span></span> <span data-ttu-id="8e20e-252">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-252">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8e20e-253">b.</span><span class="sxs-lookup"><span data-stu-id="8e20e-253">b.</span></span> <span data-ttu-id="8e20e-254">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8e20e-254">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8e20e-255">c.</span><span class="sxs-lookup"><span data-stu-id="8e20e-255">c.</span></span> <span data-ttu-id="8e20e-256">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-256">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8e20e-257">d.</span><span class="sxs-lookup"><span data-stu-id="8e20e-257">d.</span></span> <span data-ttu-id="8e20e-258">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-258">Click **Create**.</span></span>
 
### <a name="creating-a-jira-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="8e20e-259">Egy JIRA SAML SSO által Microsoft tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e20e-259">Creating a JIRA SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="8e20e-260">az Azure AD tooenable felhasználók toolog tooJIRA a helyi kiszolgálón, akkor kell üzembe JIRA SAML SSO be a Microsoft által.</span><span class="sxs-lookup"><span data-stu-id="8e20e-260">tooenable Azure AD users toolog in tooJIRA on-premise server, they must be provisioned into JIRA SAML SSO by Microsoft.</span></span> <span data-ttu-id="8e20e-261">A Microsoft által JIRA a SAML SSO egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8e20e-261">For JIRA SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="8e20e-262">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8e20e-262">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e20e-263">Jelentkezzen be tooyour JIRA a helyi kiszolgáló rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8e20e-263">Log in tooyour JIRA on-premise server as an administrator.</span></span>

2. <span data-ttu-id="8e20e-264">Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-264">Hover on cog and click hello **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-jiramicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="8e20e-266">Átirányított tooAdministrator hozzáférés lap tooenter áll **jelszó** kattintson **megerősítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-266">You are redirected tooAdministrator Access page tooenter **Password** and click **Confirm** button.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-jiramicrosoft-tutorial/user2.png) 

4. <span data-ttu-id="8e20e-268">A **felhasználókezelés** szakasz lapra, majd **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-268">Under **User management** tab section, click **create user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-jiramicrosoft-tutorial/user3.png) 

5. <span data-ttu-id="8e20e-270">A hello **"Új felhasználó létrehozása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8e20e-270">On hello **“Create new user”** dialog page, perform hello following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-jiramicrosoft-tutorial/user4.png) 

    <span data-ttu-id="8e20e-272">a.</span><span class="sxs-lookup"><span data-stu-id="8e20e-272">a.</span></span> <span data-ttu-id="8e20e-273">A hello **E-mail cím** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="8e20e-273">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="8e20e-274">b.</span><span class="sxs-lookup"><span data-stu-id="8e20e-274">b.</span></span> <span data-ttu-id="8e20e-275">A hello **teljes nevét** szövegmezőhöz hello felhasználó például Britta Simon típus teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="8e20e-275">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>

    <span data-ttu-id="8e20e-276">c.</span><span class="sxs-lookup"><span data-stu-id="8e20e-276">c.</span></span> <span data-ttu-id="8e20e-277">A hello **felhasználónév** szövegmezőhöz: hello e-mail felhasználó például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="8e20e-277">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="8e20e-278">d.</span><span class="sxs-lookup"><span data-stu-id="8e20e-278">d.</span></span> <span data-ttu-id="8e20e-279">A hello **jelszó** szövegmezőhöz típus hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="8e20e-279">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="8e20e-280">e.</span><span class="sxs-lookup"><span data-stu-id="8e20e-280">e.</span></span> <span data-ttu-id="8e20e-281">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-281">Click **Create user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8e20e-282">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8e20e-282">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8e20e-283">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés SAML SSO Microsoft access tooJIRA megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8e20e-283">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJIRA SAML SSO by Microsoft.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8e20e-285">**tooassign Britta Simon tooJIRA SAML SSO Microsoft, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8e20e-285">**tooassign Britta Simon tooJIRA SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e20e-286">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-286">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8e20e-288">Hello alkalmazások listában válassza ki a **JIRA SAML SSO Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-288">In hello applications list, select **JIRA SAML SSO by Microsoft**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_app.png) 

3. <span data-ttu-id="8e20e-290">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8e20e-290">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8e20e-292">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8e20e-292">Click **Add** button.</span></span> <span data-ttu-id="8e20e-293">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8e20e-293">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8e20e-295">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8e20e-295">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8e20e-296">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8e20e-296">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8e20e-297">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8e20e-297">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8e20e-298">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8e20e-298">Testing single sign-on</span></span>

<span data-ttu-id="8e20e-299">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8e20e-299">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8e20e-300">Hello JIRA SAML SSO által Microsoft csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour JIRA SAML SSO Microsoft alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="8e20e-300">When you click hello JIRA SAML SSO by Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour JIRA SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="8e20e-301">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8e20e-301">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e20e-302">További források</span><span class="sxs-lookup"><span data-stu-id="8e20e-302">Additional resources</span></span>

* [<span data-ttu-id="8e20e-303">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8e20e-303">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8e20e-304">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8e20e-304">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_203.png

