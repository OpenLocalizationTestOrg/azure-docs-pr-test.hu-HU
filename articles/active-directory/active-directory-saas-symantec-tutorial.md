---
title: "Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Symantec webes biztonsági szolgáltatás (VSS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9f02b3d4ce2073110c55af4b567b0e3b5a88404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="87a94-103">Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS)</span><span class="sxs-lookup"><span data-stu-id="87a94-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="87a94-104">Ebből az oktatóanyagból megtudhatja, hogyan toointegrate a Symantec webes biztonsági szolgáltatás (VSS) rendelkező fiók az Azure Active Directory (Azure AD-) fiók, hogy WSS hitelesíthető hello Azure AD kiépítve a felhasználó SAML-alapú hitelesítést használ, és kényszeríteni a felhasználót vagy csoport-szintű szabályzat előírásainak.</span><span class="sxs-lookup"><span data-stu-id="87a94-104">In this tutorial, you will learn how toointegrate your Symantec Web Security Service (WSS) account with your Azure Active Directory (Azure AD) account so that WSS can authenticate an end user provisioned in hello Azure AD using SAML authentication and enforce user or group level policy rules.</span></span>

<span data-ttu-id="87a94-105">Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="87a94-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="87a94-106">Kezelheti az összes hello felhasználók és csoportok az Azure AD portálon WSS fiókját használják.</span><span class="sxs-lookup"><span data-stu-id="87a94-106">Manage all of hello end users and groups used by your WSS account from your Azure AD portal.</span></span> 

- <span data-ttu-id="87a94-107">Engedélyezi a hello befejező felhasználóknak tooauthenticate magukat WSS használata az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="87a94-107">Allow hello end users tooauthenticate themselves in WSS using their Azure AD credentials.</span></span>

- <span data-ttu-id="87a94-108">Engedélyezze a felhasználó hello kényszerítési, és csoportosítsa WSS fiókjához megadott szintű szabályzat szabályok.</span><span class="sxs-lookup"><span data-stu-id="87a94-108">Enable hello enforcement of user and group level policy rules defined in your WSS account.</span></span>

<span data-ttu-id="87a94-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87a94-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87a94-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="87a94-110">Prerequisites</span></span>

<span data-ttu-id="87a94-111">tooconfigure az Azure AD-integrációs Symantec webes biztonsági szolgáltatás (VSS), a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="87a94-111">tooconfigure Azure AD integration with Symantec Web Security Service (WSS), you need hello following items:</span></span>

- <span data-ttu-id="87a94-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="87a94-112">An Azure AD subscription</span></span>
- <span data-ttu-id="87a94-113">A Symantec webes biztonsági szolgáltatás (VSS) fiók</span><span class="sxs-lookup"><span data-stu-id="87a94-113">A Symantec Web Security Service (WSS) account</span></span>

> [!NOTE]
> <span data-ttu-id="87a94-114">Ez az oktatóanyag lépéseit tootest hello, ne a WSS fiókkal, amely jelenleg használatban van az üzemi célra.</span><span class="sxs-lookup"><span data-stu-id="87a94-114">tootest hello steps in this tutorial, we do not recommend using a WSS account that is currently being used for production purpose.</span></span>

<span data-ttu-id="87a94-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="87a94-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87a94-116">Ne használja a WSS-fiókot, amely jelenleg használja a teszteléshez üzemi célra nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="87a94-116">Do not use your WSS account that is currently being used for production purpose for this test unless it is necessary.</span></span>
- <span data-ttu-id="87a94-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87a94-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87a94-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="87a94-118">Scenario description</span></span>
<span data-ttu-id="87a94-119">Ebben az oktatóanyagban konfigurálhatja az Azure AD tooenable egyetlen bejelentkezés tooWSS definiálva az Azure AD-fiókot hello felhasználó hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="87a94-119">In this tutorial, you will configure your Azure AD tooenable single sign-on tooWSS using hello end user credentials defined in your Azure AD account.</span></span>
<span data-ttu-id="87a94-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="87a94-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87a94-121">Hello gyűjteményből hello Symantec webes biztonsági szolgáltatás (VSS) alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87a94-121">Adding hello Symantec Web Security Service (WSS) app from hello gallery</span></span>
2. <span data-ttu-id="87a94-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="87a94-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a><span data-ttu-id="87a94-123">Symantec webes biztonsági szolgáltatás (VSS) hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="87a94-123">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
<span data-ttu-id="87a94-124">tooconfigure hello integrációs Symantec webes biztonsági szolgáltatás (VSS) az Azure AD-be, szükség Symantec webes biztonsági szolgáltatás (VSS) tooadd hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="87a94-124">tooconfigure hello integration of Symantec Web Security Service (WSS) into Azure AD, you need tooadd Symantec Web Security Service (WSS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="87a94-125">**tooadd Symantec webes biztonsági szolgáltatás (VSS) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="87a94-125">**tooadd Symantec Web Security Service (WSS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="87a94-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="87a94-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="87a94-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="87a94-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="87a94-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="87a94-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="87a94-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="87a94-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="87a94-133">Hello keresési mezőbe, írja be a **Symantec webes biztonsági szolgáltatás (VSS)**, jelölje be **Symantec webes biztonsági szolgáltatás (VSS)** eredmény panelen kattintson a **Hozzáadás** gomb tooadd hello az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="87a94-133">In hello search box, type **Symantec Web Security Service (WSS)**, select **Symantec Web Security Service (WSS)** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Symantec webes biztonsági szolgáltatás (VSS) hello eredmények listájában](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="87a94-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="87a94-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="87a94-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Symantec webes biztonsági szolgáltatás (WSS) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="87a94-136">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="87a94-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Symantec webes biztonsági szolgáltatás (VSS) a tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="87a94-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Symantec Web Security Service (WSS) is tooa user in Azure AD.</span></span> <span data-ttu-id="87a94-138">Ez azt jelenti hello kapcsolódó felhasználó a Symantec webes biztonsági szolgáltatás (VSS) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="87a94-138">In other words, a link relationship between an Azure AD user and hello related user in Symantec Web Security Service (WSS) needs toobe established.</span></span>

<span data-ttu-id="87a94-139">A Symantec webes biztonsági szolgáltatás (VSS), rendeljen hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="87a94-139">In Symantec Web Security Service (WSS), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="87a94-140">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Symantec webes biztonsági szolgáltatás (VSS), a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="87a94-140">tooconfigure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="87a94-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="87a94-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="87a94-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87a94-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="87a94-143">**[Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása](#create-a-symantec-web-security-service-wss-test-user)**  -toohave egy megfelelője a Britta Simon a Symantec webes biztonsági szolgáltatás (WSS), amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="87a94-143">**[Create a Symantec Web Security Service (WSS) test user](#create-a-symantec-web-security-service-wss-test-user)** - toohave a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="87a94-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="87a94-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="87a94-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="87a94-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="87a94-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="87a94-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="87a94-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Symantec webes biztonsági szolgáltatás (VSS) alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="87a94-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="87a94-148">**az Azure AD az egyszeri bejelentkezés tooconfigure Symantec webes biztonsági szolgáltatás (VSS) a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="87a94-148">**tooconfigure Azure AD single sign-on with Symantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="87a94-149">Az Azure portál, a hello hello **Symantec webes biztonsági szolgáltatás (VSS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="87a94-149">In hello Azure portal, on hello **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="87a94-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="87a94-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="87a94-153">A hello **Symantec webes biztonsági szolgáltatás (VSS) tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="87a94-153">On hello **Symantec Web Security Service (WSS) Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Symantec webes biztonsági szolgáltatás (VSS) tartományhoz és URL-címek](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="87a94-155">a.</span><span class="sxs-lookup"><span data-stu-id="87a94-155">a.</span></span> <span data-ttu-id="87a94-156">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="87a94-156">In hello **Identifier** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    <span data-ttu-id="87a94-157">b.</span><span class="sxs-lookup"><span data-stu-id="87a94-157">b.</span></span> <span data-ttu-id="87a94-158">A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello URL-címe:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span><span class="sxs-lookup"><span data-stu-id="87a94-158">In hello **Reply URL** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span></span>

    > [!NOTE]
    > <span data-ttu-id="87a94-159">Lépjen kapcsolatba a hello [Symantec webes biztonsági szolgáltatás (VSS) ügyfél-támogatási csoport](https://www.symantec.com/contact-us) Ha hello értékei hello **azonosító** és **válasz URL-CÍMEN** valamilyen okból kifolyólag nem működik.</span><span class="sxs-lookup"><span data-stu-id="87a94-159">Please contact hello [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) if hello values for hello **Identifier** and **Reply URL** are not working for some reason.</span></span>

4. <span data-ttu-id="87a94-160">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="87a94-160">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="87a94-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="87a94-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="87a94-164">tooconfigure egyszeri bejelentkezést a hello Symantec webes biztonsági szolgáltatás (VSS) oldalon, tekintse meg a toohello WSS online dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="87a94-164">tooconfigure single sign-on on hello Symantec Web Security Service (WSS) side, refer toohello WSS online documentation.</span></span> <span data-ttu-id="87a94-165">letöltött hello **metaadatainak XML-kódja** fájl toobe hello WSS portálon importálni kell.</span><span class="sxs-lookup"><span data-stu-id="87a94-165">hello downloaded **Metadata XML** file will need toobe imported into hello WSS portal.</span></span> <span data-ttu-id="87a94-166">Kapcsolattartási hello [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us) hello konfigurációval hello WSS Portal segítségért.</span><span class="sxs-lookup"><span data-stu-id="87a94-166">Contact hello [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) if you need assistance with hello configuration on hello WSS portal.</span></span>

> [!TIP]
> <span data-ttu-id="87a94-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="87a94-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="87a94-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="87a94-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="87a94-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87a94-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="87a94-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="87a94-170">Create an Azure AD test user</span></span>

<span data-ttu-id="87a94-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="87a94-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="87a94-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="87a94-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="87a94-174">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="87a94-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="87a94-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="87a94-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="87a94-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="87a94-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="87a94-180">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="87a94-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    <span data-ttu-id="87a94-182">a.</span><span class="sxs-lookup"><span data-stu-id="87a94-182">a.</span></span> <span data-ttu-id="87a94-183">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87a94-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87a94-184">b.</span><span class="sxs-lookup"><span data-stu-id="87a94-184">b.</span></span> <span data-ttu-id="87a94-185">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="87a94-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="87a94-186">c.</span><span class="sxs-lookup"><span data-stu-id="87a94-186">c.</span></span> <span data-ttu-id="87a94-187">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="87a94-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="87a94-188">d.</span><span class="sxs-lookup"><span data-stu-id="87a94-188">d.</span></span> <span data-ttu-id="87a94-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="87a94-189">Click **Create**.</span></span>
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="87a94-190">Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="87a94-190">Create a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="87a94-191">Ebben a szakaszban nevű Britta Simon Symantec webes biztonsági szolgáltatás (VSS) a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="87a94-191">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="87a94-192">hello megfelelő befejezési felhasználónév manuálisan létrehozott hello WSS portálon, vagy megvárhatja hello felhasználók/csoportok hello Azure AD szinkronizálása toobe toohello WSS portál üzembe helyezve (~ 15 perc). néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="87a94-192">hello corresponding end username can be manually created in hello WSS portal or you can wait for hello users/groups provisioned in hello Azure AD toobe synchronized toohello WSS portal after a few minutes (~15 minutes).</span></span> <span data-ttu-id="87a94-193">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="87a94-193">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="87a94-194">hello nyilvános IP-cím lesz használt toobrowse webhelyek hello végfelhasználói gép is kell toobe hello Symantec webes biztonsági szolgáltatás (VSS) portálon kiépítve.</span><span class="sxs-lookup"><span data-stu-id="87a94-194">hello public IP address of hello end user machine, which will be used toobrowse websites also need toobe provisioned in hello Symantec Web Security Service (WSS) portal.</span></span>

> [!NOTE]
> <span data-ttu-id="87a94-195">Adjon [ide](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget a géphez tartozó nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="87a94-195">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget your machine's public IPaddress.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="87a94-196">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="87a94-196">Assign hello Azure AD test user</span></span>

<span data-ttu-id="87a94-197">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooSymantec webes biztonsági szolgáltatás (VSS) megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="87a94-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSymantec Web Security Service (WSS).</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="87a94-199">**tooassign Britta Simon tooSymantec webes biztonsági szolgáltatás (VSS), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="87a94-199">**tooassign Britta Simon tooSymantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="87a94-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="87a94-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="87a94-202">Hello alkalmazások listában válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**.</span><span class="sxs-lookup"><span data-stu-id="87a94-202">In hello applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![hello Symantec webes biztonsági szolgáltatás (VSS) hivatkozásra hello alkalmazások listája](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. <span data-ttu-id="87a94-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="87a94-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="87a94-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="87a94-206">Click **Add** button.</span></span> <span data-ttu-id="87a94-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87a94-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="87a94-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="87a94-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="87a94-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87a94-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87a94-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87a94-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="87a94-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="87a94-212">Test single sign-on</span></span>

<span data-ttu-id="87a94-213">Ez a szakasz azt tesztelni fogja hello az egyszeri bejelentkezés funkció most, hogy a WSS-fiók toouse konfigurálása az Azure ad-val SAML-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="87a94-213">In this section, you'll test hello single sign-on functionality now that you've configured your WSS account toouse your Azure AD for SAML authentication.</span></span>

<span data-ttu-id="87a94-214">Miután konfigurálta a webes böngésző tooproxy forgalom tooWSS, nyissa meg a webböngészőt, és próbálja meg toobrowse tooa hely, akkor lesz az Azure-bejelentkezés átirányított toohello lap.</span><span class="sxs-lookup"><span data-stu-id="87a94-214">After you have configured your web browser tooproxy traffic tooWSS, when you open your web browser and try toobrowse tooa site then you'll be redirected toohello Azure sign-on page.</span></span> <span data-ttu-id="87a94-215">Adja meg hello tesztelési célból felhasználó van kiépítve hello hitelesítő adatait, hello Azure AD (Ez azt jelenti, hogy BrittaSimon) és társított jelszót.</span><span class="sxs-lookup"><span data-stu-id="87a94-215">Enter hello credentials of hello test end user that has been provisioned in hello Azure AD (that is, BrittaSimon) and associated password.</span></span> <span data-ttu-id="87a94-216">Hitelesítését követően be fog tudni toobrowse toohello webhelyet, ahol a kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="87a94-216">Once authenticated, you'll be able toobrowse toohello website that you chose.</span></span> <span data-ttu-id="87a94-217">Kell hoz létre házirendszabály hello WSS ügyféloldali tooblock BrittaSimon tallózással tooa adott hely, akkor kell megjelennie hello WSS blokk lap toobrowse toothat hely BrittaSimon felhasználóként tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="87a94-217">Should you create a policy rule on hello WSS side tooblock BrittaSimon from browsing tooa particular site then you should see hello WSS block page when you attempt toobrowse toothat site as user BrittaSimon.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87a94-218">További források</span><span class="sxs-lookup"><span data-stu-id="87a94-218">Additional resources</span></span>

* [<span data-ttu-id="87a94-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="87a94-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87a94-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="87a94-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

