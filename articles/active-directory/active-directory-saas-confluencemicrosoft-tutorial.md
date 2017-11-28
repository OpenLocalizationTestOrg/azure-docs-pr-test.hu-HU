---
title: "Oktatóanyag: Azure Active Directoryval integrált való összefolyás felett SAML SSO Microsoft |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Microsoft által való összefolyás felett SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ace23800e3908c8125052b4a2edcaae71f19935d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a><span data-ttu-id="e96b8-103">Oktatóanyag: Azure Active Directoryval integrált való összefolyás felett SAML SSO Microsoft</span><span class="sxs-lookup"><span data-stu-id="e96b8-103">Tutorial: Azure Active Directory integration with Confluence SAML SSO by Microsoft</span></span>

<span data-ttu-id="e96b8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate való összefolyás felett SAML SSO Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e96b8-104">In this tutorial, you learn how toointegrate Confluence SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e96b8-105">Microsoft való összefolyás felett SAML SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e96b8-105">Integrating Confluence SAML SSO by Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e96b8-106">Megadhatja a SAML SSO Microsoft access tooConfluence rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e96b8-106">You can control in Azure AD who has access tooConfluence SAML SSO by Microsoft</span></span>
- <span data-ttu-id="e96b8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooConfluence SAML SSO (egyszeri bejelentkezés) a Microsoft által a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e96b8-107">You can enable your users tooautomatically get signed-on tooConfluence SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e96b8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e96b8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e96b8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e96b8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e96b8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e96b8-110">Prerequisites</span></span>

<span data-ttu-id="e96b8-111">tooconfigure való összefolyás felett SAML SSO Microsoft Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="e96b8-111">tooconfigure Azure AD integration with Confluence SAML SSO by Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="e96b8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e96b8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e96b8-113">Való összefolyás felett kiszolgáló alkalmazás telepítve van egy 64 bites Windows server (a helyszínen vagy a hello felhőalapú infrastruktúra-szolgáltatási infrastruktúra)</span><span class="sxs-lookup"><span data-stu-id="e96b8-113">Confluence server application installed on a Windows 64-bit server (on premise or on hello cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="e96b8-114">A kiszolgálóhoz való összefolyás felett HTTPS-kompatibilis</span><span class="sxs-lookup"><span data-stu-id="e96b8-114">Confluence server is HTTPS enabled</span></span>
- <span data-ttu-id="e96b8-115">Megjegyzés: hello támogatott verziók való összefolyás felett beépülő modul szakasz alatt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="e96b8-115">Note hello supported versions for Confluence Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="e96b8-116">Való összefolyás felett kiszolgálóhoz lehet csatlakozni az internethez különösen tooAzure AD bejelentkezési lapon a hitelesítéshez és kell tudni tooreceive hello Azure ad-token</span><span class="sxs-lookup"><span data-stu-id="e96b8-116">Confluence server is reachable on internet particularly tooAzure AD Login page for authentication and should able tooreceive hello token from Azure AD</span></span>
- <span data-ttu-id="e96b8-117">Rendszergazdai hitelesítő adataival való összefolyás felett beállítása</span><span class="sxs-lookup"><span data-stu-id="e96b8-117">Admin credentials are set up in Confluence</span></span>
- <span data-ttu-id="e96b8-118">WebSudo le van tiltva, az való összefolyás felett</span><span class="sxs-lookup"><span data-stu-id="e96b8-118">WebSudo is disabled in Confluence</span></span>
- <span data-ttu-id="e96b8-119">Teszt felhasználó hello való összefolyás felett kiszolgálói alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e96b8-119">Test user created in hello Confluence server application</span></span>

> [!NOTE]
> <span data-ttu-id="e96b8-120">Ez az oktatóanyag lépéseit tootest hello, nem javasoljuk való összefolyás felett az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e96b8-120">tootest hello steps in this tutorial, we do not recommend using a production environment of Confluence.</span></span> <span data-ttu-id="e96b8-121">Vizsgálat hello integrációs először fejlesztési vagy átmeneti környezet hello alkalmazás, majd használja hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e96b8-121">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="e96b8-122">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e96b8-122">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e96b8-123">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e96b8-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e96b8-124">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e96b8-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-confluence"></a><span data-ttu-id="e96b8-125">Való összefolyás felett támogatott verziói</span><span class="sxs-lookup"><span data-stu-id="e96b8-125">Supported versions of Confluence</span></span> 

<span data-ttu-id="e96b8-126">Mostantól való összefolyás felett következő verziói támogatottak:</span><span class="sxs-lookup"><span data-stu-id="e96b8-126">As of now, following versions of Confluence are supported:</span></span>

- <span data-ttu-id="e96b8-127">Való összefolyás felett: 5.0-s too5.10</span><span class="sxs-lookup"><span data-stu-id="e96b8-127">Confluence: 5.0 too5.10</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e96b8-128">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e96b8-128">Scenario description</span></span>
<span data-ttu-id="e96b8-129">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e96b8-129">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e96b8-130">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e96b8-130">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e96b8-131">Hello galériából való összefolyás felett SAML SSO Microsoft hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e96b8-131">Adding Confluence SAML SSO by Microsoft from hello gallery</span></span>
2. <span data-ttu-id="e96b8-132">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e96b8-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-confluence-saml-sso-by-microsoft-from-hello-gallery"></a><span data-ttu-id="e96b8-133">Hello galériából való összefolyás felett SAML SSO Microsoft hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e96b8-133">Adding Confluence SAML SSO by Microsoft from hello gallery</span></span>
<span data-ttu-id="e96b8-134">tooconfigure hello integrációja való összefolyás felett SAML SSO Microsoft által az Azure AD-be, meg kell tooadd való összefolyás felett SAML SSO Microsoft hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e96b8-134">tooconfigure hello integration of Confluence SAML SSO by Microsoft into Azure AD, you need tooadd Confluence SAML SSO by Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e96b8-135">**tooadd való összefolyás felett SAML SSO Microsoft hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e96b8-135">**tooadd Confluence SAML SSO by Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e96b8-136">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-136">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e96b8-138">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-138">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e96b8-139">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-139">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e96b8-141">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e96b8-141">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e96b8-143">Hello keresési mezőbe, írja be a **való összefolyás felett SAML SSO Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-143">In hello search box, type **Confluence SAML SSO by Microsoft**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. <span data-ttu-id="e96b8-145">A hello eredmények panelen válassza a **való összefolyás felett SAML SSO Microsoft**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-145">In hello results panel, select **Confluence SAML SSO by Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e96b8-147">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e96b8-147">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e96b8-148">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez való összefolyás felett SAML-alapú egyszeri "Britta Simon" nevű tesztfelhasználó alapján a Microsoft által.</span><span class="sxs-lookup"><span data-stu-id="e96b8-148">In this section, you configure and test Azure AD single sign-on with Confluence SAML SSO by Microsoft based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e96b8-149">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen való összefolyás felett SAML SSO Microsoft hello megfelelőjére felhasználó tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e96b8-149">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Confluence SAML SSO by Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="e96b8-150">Ez azt jelenti hello kapcsolódó felhasználó a Microsoft által való összefolyás felett SAML SSO és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e96b8-150">In other words, a link relationship between an Azure AD user and hello related user in Confluence SAML SSO by Microsoft needs toobe established.</span></span>

<span data-ttu-id="e96b8-151">A Microsoft által való összefolyás felett a SAML SSO, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e96b8-151">In Confluence SAML SSO by Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e96b8-152">tooconfigure és való összefolyás felett SAML-alapú egyszeri Microsoft Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e96b8-152">tooconfigure and test Azure AD single sign-on with Confluence SAML SSO by Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e96b8-153">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e96b8-153">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e96b8-154">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e96b8-154">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e96b8-155">**[Egy való összefolyás felett SAML SSO Microsoft teszt felhasználó létrehozása](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  -toohave Britta Simon való összefolyás felett SAML SSO felhasználói csatolt toohello az Azure AD-ábrázolása, Microsoft által a valami.</span><span class="sxs-lookup"><span data-stu-id="e96b8-155">**[Creating a Confluence SAML SSO by Microsoft test user](#creating-a-confluence-saml-sso-by-microsoft-test-user)** - toohave a counterpart of Britta Simon in Confluence SAML SSO by Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e96b8-156">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e96b8-156">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e96b8-157">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e96b8-157">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e96b8-158">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e96b8-158">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e96b8-159">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a való összefolyás felett SAML SSO konfigurálása egyszeri bejelentkezéshez Microsoft-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e96b8-159">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Confluence SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="e96b8-160">**való összefolyás felett SAML-alapú egyszeri Microsoft, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e96b8-160">**tooconfigure Azure AD single sign-on with Confluence SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="e96b8-161">Az Azure portál, a hello hello **való összefolyás felett SAML SSO Microsoft** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-161">In hello Azure portal, on hello **Confluence SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e96b8-163">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e96b8-163">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. <span data-ttu-id="e96b8-165">A hello **való összefolyás felett SAML SSO Microsoft Domain és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e96b8-165">On hello **Confluence SAML SSO by Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    <span data-ttu-id="e96b8-167">a.</span><span class="sxs-lookup"><span data-stu-id="e96b8-167">a.</span></span> <span data-ttu-id="e96b8-168">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="e96b8-168">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="e96b8-169">b.</span><span class="sxs-lookup"><span data-stu-id="e96b8-169">b.</span></span> <span data-ttu-id="e96b8-170">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="e96b8-170">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="e96b8-171">c.</span><span class="sxs-lookup"><span data-stu-id="e96b8-171">c.</span></span> <span data-ttu-id="e96b8-172">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="e96b8-172">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e96b8-173">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e96b8-173">These values are not real.</span></span> <span data-ttu-id="e96b8-174">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="e96b8-174">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="e96b8-175">Port megadása nem kötelező, abban az esetben, ha egy elnevezett URL-címet.</span><span class="sxs-lookup"><span data-stu-id="e96b8-175">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="e96b8-176">Ezek az értékek fogadásának való összefolyás felett beépülő modul, hello oktatóanyag későbbi részében ismertetett hello konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="e96b8-176">These values are received during hello configuration of Confluence plugin, which is explained later in hello tutorial.</span></span>
 

4. <span data-ttu-id="e96b8-177">toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e96b8-177">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="e96b8-178">a.</span><span class="sxs-lookup"><span data-stu-id="e96b8-178">a.</span></span> <span data-ttu-id="e96b8-179">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-179">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="e96b8-181">b.</span><span class="sxs-lookup"><span data-stu-id="e96b8-181">b.</span></span> <span data-ttu-id="e96b8-182">Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e96b8-182">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="e96b8-184">c.</span><span class="sxs-lookup"><span data-stu-id="e96b8-184">c.</span></span> <span data-ttu-id="e96b8-185">Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="e96b8-185">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="e96b8-187">d.</span><span class="sxs-lookup"><span data-stu-id="e96b8-187">d.</span></span> <span data-ttu-id="e96b8-188">Most lépjen tulajdonságlapján toohello **való összefolyás felett SAML SSO Microsoft** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="e96b8-188">Now go toohello property page of **Confluence SAML SSO by Microsoft** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    <span data-ttu-id="e96b8-190">e.</span><span class="sxs-lookup"><span data-stu-id="e96b8-190">e.</span></span> <span data-ttu-id="e96b8-191">Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` , és másolja ezt az értéket a Jegyzettömbben, a rendszer később hello beépülő modul hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="e96b8-191">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for hello configuration of hello plugin.</span></span>

5. <span data-ttu-id="e96b8-192">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-192">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e96b8-194">Ügyfél [Microsoft](mailto:waadpartners@microsoft.com) a következő hello való összefolyás felett beépülő modul az információ hello.</span><span class="sxs-lookup"><span data-stu-id="e96b8-194">Contact [Microsoft](mailto:waadpartners@microsoft.com) with hello following information for hello Confluence plugin.</span></span>
    
    *   <span data-ttu-id="e96b8-195">Ügyfél neve:</span><span class="sxs-lookup"><span data-stu-id="e96b8-195">Customer Name:</span></span>
    *   <span data-ttu-id="e96b8-196">Elsődleges tartománynév:</span><span class="sxs-lookup"><span data-stu-id="e96b8-196">Primary domain name:</span></span>
    *   <span data-ttu-id="e96b8-197">Prémium szintű Azure AD: Igen/nem (beépülő modul lesz elérhető tooall hello vevő ingyenes, a Basic és a Premium Termékváltozat)</span><span class="sxs-lookup"><span data-stu-id="e96b8-197">Azure AD Premium: Yes/No (Plugin will be available tooall     hello customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="e96b8-198">Ez az integráció használó felhasználók száma:</span><span class="sxs-lookup"><span data-stu-id="e96b8-198">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="e96b8-199">Való összefolyás felett verzió:</span><span class="sxs-lookup"><span data-stu-id="e96b8-199">Confluence Version:</span></span>
    *   <span data-ttu-id="e96b8-200">Megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="e96b8-200">Comments:</span></span>

7. <span data-ttu-id="e96b8-201">Egy másik webes böngészőablakban jelentkezzen be tooyour való összefolyás felett példány rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e96b8-201">In a different web browser window, log in tooyour Confluence instance as an administrator.</span></span>

8. <span data-ttu-id="e96b8-202">Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-202">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="e96b8-204">A bővítmények lap szakaszban kattintson **bővítmények kezelése**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-204">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon72.png)

10. <span data-ttu-id="e96b8-206">Manuális feltöltéséhez hello beépülő modul a Microsoft biztosítja.</span><span class="sxs-lookup"><span data-stu-id="e96b8-206">Manually upload hello plugin provided by Microsoft.</span></span> <span data-ttu-id="e96b8-207">Hello beépülő modul telepítése után megjelenik **felhasználó telepített** bővítmények szakasza **kezelése bővítmény** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e96b8-207">Once hello plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="e96b8-208">Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="e96b8-208">Click **Configure** tooconfigure hello new plugin.</span></span>

12. <span data-ttu-id="e96b8-209">Hajtsa végre a következő lépéseket a konfiguráció lapon:</span><span class="sxs-lookup"><span data-stu-id="e96b8-209">Perform following steps on configuration page:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="e96b8-211">a.</span><span class="sxs-lookup"><span data-stu-id="e96b8-211">a.</span></span> <span data-ttu-id="e96b8-212">A **metaadatainak URL-CÍMÉT** illessze be a hello **metaadatainak URL-CÍMÉT** jön létre az Azure AD-ből, majd kattintson a hello **megoldásához** gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-212">In **Metadata URL** paste hello **Metadata URL** generated from Azure AD and click hello **Resolve** button.</span></span> <span data-ttu-id="e96b8-213">Beolvassa a hello IdP metaadatainak URL-CÍMÉT és az összes hello mezők adatai tölti fel.</span><span class="sxs-lookup"><span data-stu-id="e96b8-213">It reads hello IdP metadata URL and populates all hello fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="e96b8-214">Alapértelmezett SAML Felhasználóazonosító helye azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e96b8-214">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="e96b8-215">Tooan attribútum beállításaival, és adja meg a hello megfelelő attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="e96b8-215">You can change this tooan attribute option and enter hello appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="e96b8-216">Győződjön meg arról, hogy van-e rendelve hello alkalmazást úgy, hogy nem történt hiba elhárításához hello metaadatok csak egy tanúsítványra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-216">Ensure that there is only one certificate mapped against hello app so that there is no error in resolving hello metadata.</span></span> <span data-ttu-id="e96b8-217">Ha több tanúsítvány hello metaadatok feloldása után a rendszergazda hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="e96b8-217">If there are multiple certificates, upon resolving hello metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="e96b8-218">b.</span><span class="sxs-lookup"><span data-stu-id="e96b8-218">b.</span></span> <span data-ttu-id="e96b8-219">Másolás hello **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-címen** értéket, majd illessze be őket a **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-címen** szövegmezőből, illetve a **való összefolyás felett SAML SSO Microsoft Domain és URL-címek**  Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="e96b8-219">Copy hello **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **Confluence SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="e96b8-220">c.</span><span class="sxs-lookup"><span data-stu-id="e96b8-220">c.</span></span> <span data-ttu-id="e96b8-221">A **bejelentkezési gomb neve** hello típusnév gomb a szervezet által hello felhasználók toosee a bejelentkezési képernyőn.</span><span class="sxs-lookup"><span data-stu-id="e96b8-221">In **Login Button Name** type hello name of button your organization wants hello users toosee on login screen.</span></span>

    <span data-ttu-id="e96b8-222">d.</span><span class="sxs-lookup"><span data-stu-id="e96b8-222">d.</span></span> <span data-ttu-id="e96b8-223">A **SAML felhasználói azonosító helyek** válassza **hello NameIdentifier elemében hello tulajdonos utasítás alkalmazás felhasználói Azonosítóját** vagy **felhasználói azonosító attribútum elem van**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-223">In **SAML User ID Locations** select either **User ID is in hello NameIdentifier element of hello Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="e96b8-224">Ezt az Azonosítót toobe hello való összefolyás felett felhasználói azonosítóval rendelkezik. Ha hello felhasználói azonosító nem egyezik, majd rendszer nem engedélyezi a felhasználók toolog.</span><span class="sxs-lookup"><span data-stu-id="e96b8-224">This ID has toobe hello Confluence user id. If hello user id is not matched, then system will not allow users toolog in.</span></span> 
    
    <span data-ttu-id="e96b8-225">e.</span><span class="sxs-lookup"><span data-stu-id="e96b8-225">e.</span></span> <span data-ttu-id="e96b8-226">Ha **felhasználói azonosító attribútum elem van** beállítás, ezt a **attribútum neve** hello típusnév szövegmező felhasználói azonosítót várt hello attribútum.</span><span class="sxs-lookup"><span data-stu-id="e96b8-226">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type hello name of hello attribute where User Id is expected.</span></span> 

    <span data-ttu-id="e96b8-227">f.</span><span class="sxs-lookup"><span data-stu-id="e96b8-227">f.</span></span> <span data-ttu-id="e96b8-228">Ha az Azure ad-val hello összevont tartományt (például az AD FS stb.) használ, majd kattintson a hello **engedélyezése Hitelesítőtartományának feltárási** lehetőséget, majd hello konfigurálása **tartománynév**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-228">If you are using hello federated domain (like ADFS etc.) with Azure AD, then click on hello **Enable Home Realm Discovery** option and configure hello **Domain Name**.</span></span>
    
    <span data-ttu-id="e96b8-229">g.</span><span class="sxs-lookup"><span data-stu-id="e96b8-229">g.</span></span> <span data-ttu-id="e96b8-230">A **tartománynév** hello tartomány neve itt hello az AD FS-alapú bejelentkezés esetén.</span><span class="sxs-lookup"><span data-stu-id="e96b8-230">In **Domain Name** type hello domain name here in case of hello ADFS-based login.</span></span>

    <span data-ttu-id="e96b8-231">h.</span><span class="sxs-lookup"><span data-stu-id="e96b8-231">h.</span></span> <span data-ttu-id="e96b8-232">Ellenőrizze **engedélyezése egyszeri bejelentkezéshez kimenő** Ha toolog ki akarja-e az Azure AD, ha a való összefolyás felett kijelentkezik a felhasználói.</span><span class="sxs-lookup"><span data-stu-id="e96b8-232">Check **Enable Single Sign out** if you wish toolog out from Azure AD when a user logs out from Confluence.</span></span> 

    <span data-ttu-id="e96b8-233">i.</span><span class="sxs-lookup"><span data-stu-id="e96b8-233">i.</span></span> <span data-ttu-id="e96b8-234">Kattintson a **mentése** toosave hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-234">Click **Save** button toosave hello settings.</span></span>


> [!TIP]
> <span data-ttu-id="e96b8-235">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e96b8-235">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e96b8-236">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e96b8-236">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e96b8-237">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e96b8-237">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e96b8-238">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e96b8-238">Creating an Azure AD test user</span></span>
<span data-ttu-id="e96b8-239">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e96b8-239">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e96b8-241">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e96b8-241">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e96b8-242">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-242">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e96b8-244">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-244">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e96b8-246">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e96b8-246">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e96b8-248">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e96b8-248">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e96b8-250">a.</span><span class="sxs-lookup"><span data-stu-id="e96b8-250">a.</span></span> <span data-ttu-id="e96b8-251">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-251">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e96b8-252">b.</span><span class="sxs-lookup"><span data-stu-id="e96b8-252">b.</span></span> <span data-ttu-id="e96b8-253">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e96b8-253">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e96b8-254">c.</span><span class="sxs-lookup"><span data-stu-id="e96b8-254">c.</span></span> <span data-ttu-id="e96b8-255">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-255">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e96b8-256">d.</span><span class="sxs-lookup"><span data-stu-id="e96b8-256">d.</span></span> <span data-ttu-id="e96b8-257">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-257">Click **Create**.</span></span>
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="e96b8-258">Egy való összefolyás felett SAML SSO által Microsoft tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e96b8-258">Creating a Confluence SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="e96b8-259">az Azure AD tooenable felhasználók toolog tooConfluence a helyi kiszolgálón, a azok ki kell építenie való összefolyás felett SAML SSO a Microsoft által.</span><span class="sxs-lookup"><span data-stu-id="e96b8-259">tooenable Azure AD users toolog in tooConfluence on premise server, they must be provisioned into Confluence SAML SSO by Microsoft.</span></span> <span data-ttu-id="e96b8-260">A Microsoft által való összefolyás felett a SAML SSO egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e96b8-260">For Confluence SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="e96b8-261">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e96b8-261">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="e96b8-262">Jelentkezzen be a helyi kiszolgálón való összefolyás felett tooyour rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e96b8-262">Log in tooyour Confluence on premise server as an administrator.</span></span>

2. <span data-ttu-id="e96b8-263">Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-263">Hover on cog and click hello **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="e96b8-265">A felhasználók szakaszban kattintson **felhasználók hozzáadása az** fülre. A hello **"A felhasználó hozzáadása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e96b8-265">Under Users section, click **Add users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    <span data-ttu-id="e96b8-267">a.</span><span class="sxs-lookup"><span data-stu-id="e96b8-267">a.</span></span> <span data-ttu-id="e96b8-268">A hello **felhasználónév** szövegmezőhöz: hello e-mail Britta Simon például felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e96b8-268">In hello **Username** textbox, type hello email of user like Britta Simon.</span></span>

    <span data-ttu-id="e96b8-269">b.</span><span class="sxs-lookup"><span data-stu-id="e96b8-269">b.</span></span> <span data-ttu-id="e96b8-270">A hello **teljes nevét** szövegmezőhöz hello teljes típusnév Britta Simon például felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e96b8-270">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="e96b8-271">c.</span><span class="sxs-lookup"><span data-stu-id="e96b8-271">c.</span></span> <span data-ttu-id="e96b8-272">A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="e96b8-272">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="e96b8-273">d.</span><span class="sxs-lookup"><span data-stu-id="e96b8-273">d.</span></span> <span data-ttu-id="e96b8-274">A hello **jelszó** szövegmezőhöz Britta Simon típus hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e96b8-274">In hello **Password** textbox, type hello password for Britta Simon.</span></span>

    <span data-ttu-id="e96b8-275">e.</span><span class="sxs-lookup"><span data-stu-id="e96b8-275">e.</span></span> <span data-ttu-id="e96b8-276">Kattintson a **jelszó megerősítése** írja be újból hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="e96b8-276">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="e96b8-277">f.</span><span class="sxs-lookup"><span data-stu-id="e96b8-277">f.</span></span> <span data-ttu-id="e96b8-278">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-278">Click **Add** button.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e96b8-279">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e96b8-279">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e96b8-280">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés SAML SSO Microsoft access tooConfluence megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e96b8-280">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooConfluence SAML SSO by Microsoft.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e96b8-282">**tooassign Britta Simon tooConfluence SAML SSO Microsoft, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e96b8-282">**tooassign Britta Simon tooConfluence SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="e96b8-283">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-283">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e96b8-285">Hello alkalmazások listában válassza ki a **való összefolyás felett SAML SSO Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-285">In hello applications list, select **Confluence SAML SSO by Microsoft**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. <span data-ttu-id="e96b8-287">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e96b8-287">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e96b8-289">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e96b8-289">Click **Add** button.</span></span> <span data-ttu-id="e96b8-290">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e96b8-290">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e96b8-292">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e96b8-292">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e96b8-293">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e96b8-293">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e96b8-294">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e96b8-294">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e96b8-295">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e96b8-295">Testing single sign-on</span></span>

<span data-ttu-id="e96b8-296">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e96b8-296">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e96b8-297">Hello való összefolyás felett SAML SSO által Microsoft csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour való összefolyás felett SAML SSO Microsoft alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e96b8-297">When you click hello Confluence SAML SSO by Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour Confluence SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="e96b8-298">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e96b8-298">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e96b8-299">További források</span><span class="sxs-lookup"><span data-stu-id="e96b8-299">Additional resources</span></span>

* [<span data-ttu-id="e96b8-300">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e96b8-300">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e96b8-301">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e96b8-301">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

