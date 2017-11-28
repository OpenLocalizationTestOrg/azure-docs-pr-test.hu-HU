---
title: "Oktatóanyag: Azure Active Directory-integráció a IBM Kenexa felmérés vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az IBM Kenexa felmérés vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: cf7ed886b4418ac396ca7056827ee10fd7a19ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a><span data-ttu-id="71a2f-103">Oktatóanyag: Azure Active Directory-integráció a IBM Kenexa felmérés vállalati</span><span class="sxs-lookup"><span data-stu-id="71a2f-103">Tutorial: Azure Active Directory integration with IBM Kenexa Survey Enterprise</span></span>

<span data-ttu-id="71a2f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate IBM Kenexa felmérés vállalati az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="71a2f-104">In this tutorial, you learn how toointegrate IBM Kenexa Survey Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="71a2f-105">IBM Kenexa felmérés vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="71a2f-105">Integrating IBM Kenexa Survey Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="71a2f-106">Az Azure AD hozzáférési tooIBM Kenexa felmérés vállalati rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="71a2f-106">You can control in Azure AD who has access tooIBM Kenexa Survey Enterprise.</span></span>
- <span data-ttu-id="71a2f-107">A felhasználók tooautomatically bejelentkezés tooIBM Kenexa felmérés vállalati engedélyezheti az egyszeri bejelentkezés (SSO) használata az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="71a2f-107">You can enable your users tooautomatically sign in tooIBM Kenexa Survey Enterprise by using single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="71a2f-108">A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="71a2f-108">You can manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="71a2f-109">Ha tooknow szoftverrel kapcsolatos további, az Azure AD egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="71a2f-109">If you want tooknow more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71a2f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="71a2f-110">Prerequisites</span></span>

<span data-ttu-id="71a2f-111">az Azure AD integrálása a IBM Kenexa felmérés vállalati tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="71a2f-111">tooconfigure Azure AD integration with IBM Kenexa Survey Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="71a2f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="71a2f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="71a2f-113">Az IBM Kenexa felmérés vállalati SSO-kompatibilis előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="71a2f-113">An IBM Kenexa Survey Enterprise SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="71a2f-114">Ebben az oktatóanyagban hello lépéseket tesztelésekor, azt javasoljuk, hogy nem használ egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="71a2f-114">When you test hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="71a2f-115">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="71a2f-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="71a2f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="71a2f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="71a2f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="71a2f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="71a2f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="71a2f-118">Scenario description</span></span>
<span data-ttu-id="71a2f-119">Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni.</span><span class="sxs-lookup"><span data-stu-id="71a2f-119">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="71a2f-120">hello az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="71a2f-120">hello scenario outlined in hello tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="71a2f-121">IBM Kenexa felmérés vállalati hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="71a2f-121">Adding IBM Kenexa Survey Enterprise from hello gallery</span></span>
* <span data-ttu-id="71a2f-122">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="71a2f-122">Configuring and testing Azure AD SSO</span></span>

## <a name="add-ibm-kenexa-survey-enterprise-from-hello-gallery"></a><span data-ttu-id="71a2f-123">Adja hozzá az IBM Kenexa felmérés vállalati hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="71a2f-123">Add IBM Kenexa Survey Enterprise from hello gallery</span></span>
<span data-ttu-id="71a2f-124">az Azure AD-be IBM Kenexa felmérés vállalati tooconfigure hello integrációja IBM Kenexa felmérés vállalati hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="71a2f-124">tooconfigure hello integration of IBM Kenexa Survey Enterprise into Azure AD, add IBM Kenexa Survey Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="71a2f-125">IBM Kenexa felmérés vállalati hello gyűjteményből tooadd hello a következő:</span><span class="sxs-lookup"><span data-stu-id="71a2f-125">tooadd IBM Kenexa Survey Enterprise from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="71a2f-126">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-126">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Active Directory** button.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="71a2f-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="71a2f-130">egy alkalmazáskészletet, tooadd kattintson hello **új alkalmazás** gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-130">tooadd an application, click hello **New application** button.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="71a2f-132">Hello keresési mezőbe, írja be a **IBM Kenexa felmérés vállalati**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-132">In hello search box, type **IBM Kenexa Survey Enterprise**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. <span data-ttu-id="71a2f-134">Hello eredmények listájában válassza **IBM Kenexa felmérés vállalati**, majd kattintson a hello **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-134">In hello results list, select **IBM Kenexa Survey Enterprise**, and then click hello **Add** button tooadd hello application.</span></span>

    ![IBM Kenexa felmérés vállalati hello eredmények listájában](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="71a2f-136">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="71a2f-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="71a2f-137">Ebben a szakaszban konfigurálása és tesztelése az Azure AD SSO IBM Kenexa felmérés vállalati "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="71a2f-137">In this section, you configure and test Azure AD SSO with IBM Kenexa Survey Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="71a2f-138">Az SSO toowork az Azure AD tooidentify hello IBM Kenexa felmérés vállalati felhasználó megfelelőjére kell az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="71a2f-138">For SSO toowork, Azure AD needs tooidentify hello IBM Kenexa Survey Enterprise user counterpart in Azure AD.</span></span> <span data-ttu-id="71a2f-139">Ez azt jelenti az Azure AD kapcsolatot kell létesítenie egy hivatkozás egy Azure AD-felhasználó és a kapcsolódó felhasználó között IBM Kenexa felmérés vállalati.</span><span class="sxs-lookup"><span data-stu-id="71a2f-139">In other words, Azure AD must establish a link relationship between an Azure AD user and a related user in IBM Kenexa Survey Enterprise.</span></span>

<span data-ttu-id="71a2f-140">tooestablish hello hivatkozás kapcsolat hozzárendelése hello értékének hello **felhasználónév** IBM Kenexa felmérés vállalati hello hello értékeként **felhasználónév** az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="71a2f-140">tooestablish hello link relationship, assign hello value of hello **user name** in IBM Kenexa Survey Enterprise as hello value of hello **Username** in Azure AD.</span></span>

<span data-ttu-id="71a2f-141">tooconfigure és tesztelési Azure AD SSO IBM Kenexa felmérés vállalati, a következő két szakasz hello teljes hello építőelemeit.</span><span class="sxs-lookup"><span data-stu-id="71a2f-141">tooconfigure and test Azure AD SSO with IBM Kenexa Survey Enterprise, complete hello building blocks in hello next two sections.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="71a2f-142">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="71a2f-142">Configure Azure AD SSO</span></span>

<span data-ttu-id="71a2f-143">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést a hello Azure-portálon, és egyszeri bejelentkezés konfigurálása az IBM Kenexa felmérés vállalati alkalmazás hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="71a2f-143">In this section, you enable Azure AD SSO in hello Azure portal and configure SSO in your IBM Kenexa Survey Enterprise application by doing hello following:</span></span>

1. <span data-ttu-id="71a2f-144">Az Azure portál, a hello hello **IBM Kenexa felmérés vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-144">In hello Azure portal, on hello **IBM Kenexa Survey Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![IBM Kenexa felmérés vállalati konfigurálása egyszeri bejelentkezés hivatkozás][4]

2. <span data-ttu-id="71a2f-146">A hello **egyszeri bejelentkezés** párbeszédpanel hello **mód** mezőben válassza **SAML-alapú bejelentkezés** tooenable egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="71a2f-146">In hello **Single sign-on** dialog box, in hello **Mode** box, select **SAML-based Sign-on** tooenable SSO.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. <span data-ttu-id="71a2f-148">A hello **IBM Kenexa felmérés vállalati tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="71a2f-148">In hello **IBM Kenexa Survey Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![IBM Kenexa felmérés vállalati tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    <span data-ttu-id="71a2f-150">a.</span><span class="sxs-lookup"><span data-stu-id="71a2f-150">a.</span></span> <span data-ttu-id="71a2f-151">A hello **azonosító** szövegmezőhöz URL-címet adja meg a következő mintát hello:`https://surveys.kenexa.com/<companycode>`</span><span class="sxs-lookup"><span data-stu-id="71a2f-151">In hello **Identifier** textbox, type a URL with hello following pattern: `https://surveys.kenexa.com/<companycode>`</span></span>

    <span data-ttu-id="71a2f-152">b.</span><span class="sxs-lookup"><span data-stu-id="71a2f-152">b.</span></span> <span data-ttu-id="71a2f-153">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet adja meg a következő mintát hello:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span><span class="sxs-lookup"><span data-stu-id="71a2f-153">In hello **Reply URL** textbox, type a URL with hello following pattern: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="71a2f-154">hello előző értékei nem valódi.</span><span class="sxs-lookup"><span data-stu-id="71a2f-154">hello preceding values are not real.</span></span> <span data-ttu-id="71a2f-155">Hello tényleges azonosítójú frissítheti, illetve válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="71a2f-155">Update them with hello actual identifier and reply URL.</span></span> <span data-ttu-id="71a2f-156">tooobtain hello tényleges értékek, a kapcsolattartási hello [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="71a2f-156">tooobtain hello actual values, contact hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

4. <span data-ttu-id="71a2f-157">A **SAML-aláíró tanúsítványa**, kattintson a **tanúsítvány (Base64)**, és mentse a hello tanúsítvány fájl tooyour számítógép.</span><span class="sxs-lookup"><span data-stu-id="71a2f-157">Under **SAML Signing Certificate**, click **Certificate (Base64)**, and then save hello certificate file tooyour computer.</span></span>

    ![hello (Base64) tanúsítvány letöltési hivatkozását](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    <span data-ttu-id="71a2f-159">IBM Kenexa felmérés vállalati alkalmazás hello tooreceive hello biztonsági helyességi feltételek Markup Language (SAML) helyességi feltételek egy meghatározott formátumban, amely akkor tooadd egyéni attribútum hozzárendelések toohello-konfigurációt igényli az SAML-jogkivonat attribútumok vár.</span><span class="sxs-lookup"><span data-stu-id="71a2f-159">hello IBM Kenexa Survey Enterprise application expects tooreceive hello Security Assertions Markup Language (SAML) assertions in a specific format, which requires you tooadd custom attribute mappings toohello configuration of your SAML token attributes.</span></span> <span data-ttu-id="71a2f-160">hello hello válaszul hello felhasználóazonosítót jogcím értékét meg kell egyeznie hello hello Kenexa rendszerben konfigurált egyszeri bejelentkezési azonosítója.</span><span class="sxs-lookup"><span data-stu-id="71a2f-160">hello value of hello user-identifier claim in hello response must match hello SSO ID that's configured in hello Kenexa system.</span></span> <span data-ttu-id="71a2f-161">toomap hello a szervezetében, egyszeri Bejelentkezéses Internet Datagram Protocol (IDP) megfelelő felhasználói azonosítót, hello együttműködve [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="71a2f-161">toomap hello appropriate user identifier in your organization as SSO Internet Datagram Protocol (IDP), work with hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> 

    <span data-ttu-id="71a2f-162">Alapértelmezés szerint az Azure AD hello felhasználói azonosító hello felhasználó egyszerű felhasználónév (UPN) értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="71a2f-162">By default, Azure AD sets hello user identifier as hello user principal name (UPN) value.</span></span> <span data-ttu-id="71a2f-163">Módosíthatja ezt az értéket a hello **attribútum** lap, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="71a2f-163">You can change this value on hello **Attribute** tab, as shown in hello following screenshot.</span></span> <span data-ttu-id="71a2f-164">hello integráció csak az megfelelően leképezési hello befejezése után működik.</span><span class="sxs-lookup"><span data-stu-id="71a2f-164">hello integration works only after you've completed hello mapping correctly.</span></span>
    
    ![hello felhasználói attribútumok párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png) 

5. <span data-ttu-id="71a2f-166">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-166">Click **Save**.</span></span>

    ![hello konfigurálása egyszeri bejelentkezéshez mentési gomb](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="71a2f-168">tooopen hello **bejelentkezés konfigurálása** ablakban, a **IBM Kenexa felmérés vállalati konfiguráció**, kattintson a **konfigurálása IBM Kenexa felmérés vállalati**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-168">tooopen hello **Configure sign-on** window, under **IBM Kenexa Survey Enterprise Configuration**, click **Configure IBM Kenexa Survey Enterprise**.</span></span> 
 
    ![hello konfigurálása IBM Kenexa felmérés vállalati hivatkozás](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. <span data-ttu-id="71a2f-170">Másolás hello **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** hello értékeinek **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="71a2f-170">Copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values from hello **Quick Reference** section.</span></span>

8. <span data-ttu-id="71a2f-171">A hello **bejelentkezés konfigurálása** ablakban, a **rövid összefoglaló**, másolása hello **Sign-Out URL-cím**, **SAML Entitásazonosító**, és  **SAML-alapú egyszeri bejelentkezési URL-címe** értékeket.</span><span class="sxs-lookup"><span data-stu-id="71a2f-171">In hello **Configure sign-on** window, under **Quick Reference**, copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values.</span></span>

9. <span data-ttu-id="71a2f-172">a hello SSO tooconfigure **IBM Kenexa felmérés vállalati** oldalán, letöltött hello küldése **tanúsítvány (Base64)**, **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** toohello értékek [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="71a2f-172">tooconfigure SSO on hello **IBM Kenexa Survey Enterprise** side, send hello downloaded **Certificate (Base64)**, **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values toohello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

> [!TIP]
> <span data-ttu-id="71a2f-173">Olvassa el ezeket az utasításokat a hello tömör verziójának tooa [Azure-portálon](https://portal.azure.com) közben állítja be az hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="71a2f-173">You can refer tooa concise version of these instructions in hello [Azure portal](https://portal.azure.com) while you are setting up hello app.</span></span> <span data-ttu-id="71a2f-174">A hello hello alkalmazás hozzáadása után **Active Directory** > **vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapot, és hozzáférhet hello beágyazott keresztül hello dokumentáció **konfigurációs** szakasz hello végén.</span><span class="sxs-lookup"><span data-stu-id="71a2f-174">After you add hello app from hello **Active Directory** > **Enterprise Applications** section, simply click hello **single sign-on** tab, and then access hello embedded documentation through hello **Configuration** section at hello end.</span></span> <span data-ttu-id="71a2f-175">toolearn hello embedded dokumentációjából funkció, bővebben lásd: [az Azure AD dokumentációjában beágyazott](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="71a2f-175">toolearn more about hello embedded documentation feature, see [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="71a2f-176">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="71a2f-176">Create an Azure AD test user</span></span>
<span data-ttu-id="71a2f-177">Ebben a szakaszban Britta Simon tesztfelhasználó hello Azure-portálon létrehozhat hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="71a2f-177">In this section, you create test user Britta Simon in hello Azure portal by doing hello following:</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

1. <span data-ttu-id="71a2f-179">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-179">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="71a2f-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-181">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="71a2f-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="71a2f-183">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="71a2f-185">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="71a2f-185">In hello **User** dialog box, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="71a2f-187">a.</span><span class="sxs-lookup"><span data-stu-id="71a2f-187">a.</span></span> <span data-ttu-id="71a2f-188">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-188">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="71a2f-189">b.</span><span class="sxs-lookup"><span data-stu-id="71a2f-189">b.</span></span> <span data-ttu-id="71a2f-190">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="71a2f-190">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="71a2f-191">c.</span><span class="sxs-lookup"><span data-stu-id="71a2f-191">c.</span></span> <span data-ttu-id="71a2f-192">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="71a2f-192">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="71a2f-193">d.</span><span class="sxs-lookup"><span data-stu-id="71a2f-193">d.</span></span> <span data-ttu-id="71a2f-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-194">Click **Create**.</span></span>
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a><span data-ttu-id="71a2f-195">Az IBM Kenexa felmérés vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="71a2f-195">Create an IBM Kenexa Survey Enterprise test user</span></span>

<span data-ttu-id="71a2f-196">Ebben a szakaszban Britta Simon meghívta IBM Kenexa felmérés vállalati felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="71a2f-196">In this section, you create a user called Britta Simon in IBM Kenexa Survey Enterprise.</span></span> 

<span data-ttu-id="71a2f-197">toocreate felhasználók hello IBM Kenexa felmérés Enterprise rendszer és a térkép hello egyszeri bejelentkezési azonosító számukra, dolgozhat hello [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="71a2f-197">toocreate users in hello IBM Kenexa Survey Enterprise system and map hello SSO ID for them, you can work with hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> <span data-ttu-id="71a2f-198">Ezt az egyszeri bejelentkezési azonosító értéket kell toohello felhasználói azonosító értéket az Azure AD leképezve.</span><span class="sxs-lookup"><span data-stu-id="71a2f-198">This SSO ID value should also be mapped toohello user identifier value from Azure AD.</span></span> <span data-ttu-id="71a2f-199">Módosíthatja az alapértelmezett beállítás a hello **attribútum** fülre.</span><span class="sxs-lookup"><span data-stu-id="71a2f-199">You can change this default setting on hello **Attribute** tab.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="71a2f-200">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="71a2f-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="71a2f-201">Ebben a szakaszban a Britta Simon toouse Azure egyszeri Bejelentkezéses felhasználói hozzáférés tooIBM Kenexa felmérés vállalati megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="71a2f-201">In this section, you enable user Britta Simon toouse Azure SSO by granting access tooIBM Kenexa Survey Enterprise.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="71a2f-203">tooassign felhasználói Britta Simon tooIBM Kenexa felmérés vállalati, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="71a2f-203">tooassign user Britta Simon tooIBM Kenexa Survey Enterprise, do hello following:</span></span>

1. <span data-ttu-id="71a2f-204">Az Azure-portálon hello, nyissa meg a hello **alkalmazások** toohello megtekintheti **Directory** nézetben jelölje ki **vállalati alkalmazások**, és kattintson a **összes alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-204">In hello Azure portal, open hello **Applications** view, go toohello **Directory** view, select **Enterprise applications**, and then click **All applications**.</span></span>

    ![hello "vállalati alkalmazások" és "Összes alkalmazás" hivatkozások][201] 

2. <span data-ttu-id="71a2f-206">A hello **alkalmazások** listáról válassza ki **IBM Kenexa felmérés vállalati**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-206">In hello **Applications** list, select **IBM Kenexa Survey Enterprise**.</span></span>

    ![hello IBM Kenexa felmérés vállalati hivatkozásra hello alkalmazások listája](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. <span data-ttu-id="71a2f-208">Hello bal oldali ablaktáblában kattintson **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-208">In hello left pane, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="71a2f-210">Hello kattintson **Hozzáadás** gombra, majd a hello **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-210">Click hello **Add** button and then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="71a2f-212">A hello **felhasználók és csoportok** párbeszédpanel hello **felhasználók** listáról válassza ki **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="71a2f-212">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="71a2f-213">A hello **felhasználók és csoportok** párbeszédpanel hello kattintson **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-213">In hello **Users and groups** dialog box, click hello **Select** button.</span></span>

7. <span data-ttu-id="71a2f-214">A hello **hozzáadása hozzárendelés** párbeszédpanel hello kattintson **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="71a2f-214">In hello **Add Assignment** dialog box, click hello **Assign** button.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="71a2f-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="71a2f-215">Test single sign-on</span></span>

<span data-ttu-id="71a2f-216">Ebben a szakaszban az Azure AD SSO konfigurációs hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="71a2f-216">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="71a2f-217">Amikor rákattint hello **IBM Kenexa felmérés vállalati** csempe az hello hozzáférési panelre, akkor érdemes automatikusan megtörténik a tooyour IBM Kenexa felmérés vállalati alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="71a2f-217">When you click hello **IBM Kenexa Survey Enterprise** tile in hello Access Panel, you should be automatically signed in tooyour IBM Kenexa Survey Enterprise application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71a2f-218">További források</span><span class="sxs-lookup"><span data-stu-id="71a2f-218">Additional resources</span></span>

* [<span data-ttu-id="71a2f-219">Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="71a2f-219">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="71a2f-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="71a2f-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
