---
title: "Oktatóanyag: Azure Active Directoryval integrált Ceridian Dayforce HCM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Ceridian Dayforce HCM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4d72f29b4e5e30ef8881806d789f6676fc541e2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a><span data-ttu-id="fd26f-103">Oktatóanyag: Azure Active Directoryval integrált Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="fd26f-103">Tutorial: Azure Active Directory integration with Ceridian Dayforce HCM</span></span>

<span data-ttu-id="fd26f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Ceridian Dayforce HCM az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fd26f-104">In this tutorial, you learn how toointegrate Ceridian Dayforce HCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fd26f-105">Ceridian Dayforce HCM integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="fd26f-105">Integrating Ceridian Dayforce HCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fd26f-106">Az Azure AD hozzáférési tooCeridian Dayforce HCM rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="fd26f-106">You can control in Azure AD who has access tooCeridian Dayforce HCM.</span></span>
- <span data-ttu-id="fd26f-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCeridian Dayforce HCM (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="fd26f-107">You can enable your users tooautomatically get signed-on tooCeridian Dayforce HCM (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="fd26f-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="fd26f-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="fd26f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fd26f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd26f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fd26f-110">Prerequisites</span></span>

<span data-ttu-id="fd26f-111">tooconfigure Ceridian Dayforce HCM az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="fd26f-111">tooconfigure Azure AD integration with Ceridian Dayforce HCM, you need hello following items:</span></span>

- <span data-ttu-id="fd26f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fd26f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fd26f-113">Egy Ceridian Dayforce HCM egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="fd26f-113">A Ceridian Dayforce HCM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fd26f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="fd26f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fd26f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="fd26f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fd26f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fd26f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fd26f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd26f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fd26f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fd26f-118">Scenario description</span></span>
<span data-ttu-id="fd26f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fd26f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fd26f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fd26f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fd26f-121">Hello gyűjteményből Ceridian Dayforce HCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fd26f-121">Adding Ceridian Dayforce HCM from hello gallery</span></span>
2. <span data-ttu-id="fd26f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fd26f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ceridian-dayforce-hcm-from-hello-gallery"></a><span data-ttu-id="fd26f-123">Hello gyűjteményből Ceridian Dayforce HCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fd26f-123">Adding Ceridian Dayforce HCM from hello gallery</span></span>
<span data-ttu-id="fd26f-124">tooconfigure hello integrációja Ceridian Dayforce HCM az Azure AD-be, meg kell tooadd Ceridian Dayforce HCM hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="fd26f-124">tooconfigure hello integration of Ceridian Dayforce HCM into Azure AD, you need tooadd Ceridian Dayforce HCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fd26f-125">**tooadd Ceridian Dayforce HCM hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="fd26f-125">**tooadd Ceridian Dayforce HCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd26f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="fd26f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fd26f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="fd26f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="fd26f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="fd26f-133">Hello keresési mezőbe, írja be a **Ceridian Dayforce HCM**, jelölje be **Ceridian Dayforce HCM** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-133">In hello search box, type **Ceridian Dayforce HCM**, select **Ceridian Dayforce HCM** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Ceridian Dayforce HCM hello eredmények listájában](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fd26f-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd26f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fd26f-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Ceridian Dayforce HCM "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="fd26f-136">In this section, you configure and test Azure AD single sign-on with Ceridian Dayforce HCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fd26f-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Ceridian Dayforce HCM tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="fd26f-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Ceridian Dayforce HCM is tooa user in Azure AD.</span></span> <span data-ttu-id="fd26f-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Ceridian Dayforce HCM közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="fd26f-138">In other words, a link relationship between an Azure AD user and hello related user in Ceridian Dayforce HCM needs toobe established.</span></span>

<span data-ttu-id="fd26f-139">Ceridian Dayforce HCM, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="fd26f-139">In Ceridian Dayforce HCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fd26f-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Ceridian Dayforce HCM-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="fd26f-140">tooconfigure and test Azure AD single sign-on with Ceridian Dayforce HCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fd26f-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fd26f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fd26f-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd26f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fd26f-143">**[Ceridian Dayforce HCM tesztfelhasználó létrehozása](#create-a-ceridian-dayforce-hcm-test-user)**  -toohave egy megfelelője a Britta Simon a Ceridian Dayforce HCM, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="fd26f-143">**[Create a Ceridian Dayforce HCM test user](#create-a-ceridian-dayforce-hcm-test-user)** - toohave a counterpart of Britta Simon in Ceridian Dayforce HCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fd26f-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="fd26f-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fd26f-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="fd26f-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fd26f-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd26f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fd26f-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Ceridian Dayforce HCM alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="fd26f-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Ceridian Dayforce HCM application.</span></span>

<span data-ttu-id="fd26f-148">**Ceridian Dayforce HCM, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="fd26f-148">**tooconfigure Azure AD single sign-on with Ceridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd26f-149">Az Azure portál, a hello hello **Ceridian Dayforce HCM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-149">In hello Azure portal, on hello **Ceridian Dayforce HCM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="fd26f-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="fd26f-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. <span data-ttu-id="fd26f-153">A hello **Ceridian Dayforce HCM tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fd26f-153">On hello **Ceridian Dayforce HCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    <span data-ttu-id="fd26f-155">a.</span><span class="sxs-lookup"><span data-stu-id="fd26f-155">a.</span></span> <span data-ttu-id="fd26f-156">A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign a tooyour Ceridian Dayforce HCM alkalmazás által használt típus hello URL.</span><span class="sxs-lookup"><span data-stu-id="fd26f-156">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Ceridian Dayforce HCM application.</span></span>
    
    | <span data-ttu-id="fd26f-157">Környezet</span><span class="sxs-lookup"><span data-stu-id="fd26f-157">Environment</span></span> | <span data-ttu-id="fd26f-158">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="fd26f-158">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="fd26f-159">Termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="fd26f-159">For production</span></span> | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | <span data-ttu-id="fd26f-160">A teszt</span><span class="sxs-lookup"><span data-stu-id="fd26f-160">For test</span></span> | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    <span data-ttu-id="fd26f-161">b.</span><span class="sxs-lookup"><span data-stu-id="fd26f-161">b.</span></span> <span data-ttu-id="fd26f-162">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="fd26f-162">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    
    | <span data-ttu-id="fd26f-163">Környezet</span><span class="sxs-lookup"><span data-stu-id="fd26f-163">Environment</span></span> | <span data-ttu-id="fd26f-164">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="fd26f-164">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="fd26f-165">Termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="fd26f-165">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp` |
    | <span data-ttu-id="fd26f-166">A teszt</span><span class="sxs-lookup"><span data-stu-id="fd26f-166">For test</span></span> | `https://fs-test.dayforcehcm.com/sp` |
    
    <span data-ttu-id="fd26f-167">c.</span><span class="sxs-lookup"><span data-stu-id="fd26f-167">c.</span></span> <span data-ttu-id="fd26f-168">A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello URL-CÍMÉT az Azure AD toopost hello válasz használják.</span><span class="sxs-lookup"><span data-stu-id="fd26f-168">In hello **Reply URL** textbox, type hello URL used by Azure AD toopost hello response.</span></span>
    
    | <span data-ttu-id="fd26f-169">Környezet</span><span class="sxs-lookup"><span data-stu-id="fd26f-169">Environment</span></span> | <span data-ttu-id="fd26f-170">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="fd26f-170">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="fd26f-171">Termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="fd26f-171">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | <span data-ttu-id="fd26f-172">A teszt</span><span class="sxs-lookup"><span data-stu-id="fd26f-172">For test</span></span> | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > <span data-ttu-id="fd26f-173">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="fd26f-173">These values are not real.</span></span> <span data-ttu-id="fd26f-174">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="fd26f-174">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="fd26f-175">Ügyfél [Ceridian Dayforce HCM ügyfél-támogatási csoport](https://www.ceridian.com/contact-us/index.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="fd26f-175">Contact [Ceridian Dayforce HCM Client support team](https://www.ceridian.com/contact-us/index.html) tooget these values.</span></span>

4. <span data-ttu-id="fd26f-176">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fd26f-176">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. <span data-ttu-id="fd26f-178">A Ceridian Dayforce HCM alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="fd26f-178">Your Ceridian Dayforce HCM application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="fd26f-179">Együttműködve [Ceridian Dayforce HCM támogatási csoport](https://www.ceridian.com/contact-us/index.html) első tooidentify hello megfelelő felhasználói azonosítót.</span><span class="sxs-lookup"><span data-stu-id="fd26f-179">Work with [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) first tooidentify hello correct user identifier.</span></span> <span data-ttu-id="fd26f-180">A Microsoft azt javasolja, hello segítségével **"name"** attribútum az felhasználói azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="fd26f-180">Microsoft recommends using hello **"name"** attribute as user identifier.</span></span> <span data-ttu-id="fd26f-181">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="fd26f-181">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="fd26f-182">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="fd26f-182">hello following screenshot shows an example for this.</span></span>  

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. <span data-ttu-id="fd26f-184">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fd26f-184">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="fd26f-185">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="fd26f-185">Attribute Name</span></span>  | <span data-ttu-id="fd26f-186">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="fd26f-186">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="fd26f-187">név</span><span class="sxs-lookup"><span data-stu-id="fd26f-187">name</span></span>  | <span data-ttu-id="fd26f-188">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="fd26f-188">user.extensionattribute2</span></span> |    

    <span data-ttu-id="fd26f-189">a.</span><span class="sxs-lookup"><span data-stu-id="fd26f-189">a.</span></span> <span data-ttu-id="fd26f-190">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-190">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="fd26f-193">b.</span><span class="sxs-lookup"><span data-stu-id="fd26f-193">b.</span></span> <span data-ttu-id="fd26f-194">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="fd26f-194">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="fd26f-195">c.</span><span class="sxs-lookup"><span data-stu-id="fd26f-195">c.</span></span> <span data-ttu-id="fd26f-196">A hello **érték** listán, válassza hello felhasználói attribútum a megvalósítás toouse használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="fd26f-196">In hello **Value** list, select hello user attribute you want toouse for your implementation.</span></span>
    <span data-ttu-id="fd26f-197">Ha azt szeretné, hogy az egyedi felhasználói azonosítóval EmployeeID toouse hello és hello ExtensionAttribute2 tárolt hello attribútum értéke, majd válassza ki például **user.extensionattribute2**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-197">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select **user.extensionattribute2**.</span></span>
    
    <span data-ttu-id="fd26f-198">d.</span><span class="sxs-lookup"><span data-stu-id="fd26f-198">d.</span></span> <span data-ttu-id="fd26f-199">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-199">Click **Ok**.</span></span>

7. <span data-ttu-id="fd26f-200">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-200">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="fd26f-202">A hello **Ceridian Dayforce HCM konfigurációs** kattintson **Ceridian Dayforce HCM konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fd26f-202">On hello **Ceridian Dayforce HCM Configuration** section, click **Configure Ceridian Dayforce HCM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fd26f-203">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fd26f-203">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Ceridian Dayforce HCM konfiguráció](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. <span data-ttu-id="fd26f-205">tooconfigure egyszeri bejelentkezést a **Ceridian Dayforce HCM** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[Ceridian Dayforce HCM támogatási csoport](https://www.ceridian.com/contact-us/index.html).</span><span class="sxs-lookup"><span data-stu-id="fd26f-205">tooconfigure single sign-on on **Ceridian Dayforce HCM** side, you need toosend hello downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html).</span></span>

> [!TIP]
> <span data-ttu-id="fd26f-206">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="fd26f-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fd26f-207">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="fd26f-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fd26f-208">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fd26f-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fd26f-209">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fd26f-209">Create an Azure AD test user</span></span>

<span data-ttu-id="fd26f-210">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="fd26f-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="fd26f-212">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="fd26f-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd26f-213">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-213">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="fd26f-215">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-215">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="fd26f-217">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="fd26f-217">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="fd26f-219">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fd26f-219">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    <span data-ttu-id="fd26f-221">a.</span><span class="sxs-lookup"><span data-stu-id="fd26f-221">a.</span></span> <span data-ttu-id="fd26f-222">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-222">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fd26f-223">b.</span><span class="sxs-lookup"><span data-stu-id="fd26f-223">b.</span></span> <span data-ttu-id="fd26f-224">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="fd26f-224">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="fd26f-225">c.</span><span class="sxs-lookup"><span data-stu-id="fd26f-225">c.</span></span> <span data-ttu-id="fd26f-226">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fd26f-226">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="fd26f-227">d.</span><span class="sxs-lookup"><span data-stu-id="fd26f-227">d.</span></span> <span data-ttu-id="fd26f-228">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-228">Click **Create**.</span></span>
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a><span data-ttu-id="fd26f-229">Ceridian Dayforce HCM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd26f-229">Create a Ceridian Dayforce HCM test user</span></span>

<span data-ttu-id="fd26f-230">hello ebben a szakaszban célja toocreate Ceridian Dayforce HCM Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="fd26f-230">hello objective of this section is toocreate a user called Britta Simon in Ceridian Dayforce HCM.</span></span> <span data-ttu-id="fd26f-231">Hello együttműködve [Ceridian Dayforce HCM támogatási csoport](https://www.ceridian.com/contact-us/index.html) tooget felhasználók hello Ceridian Dayforce HCM alkalmazás szerepel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-231">Work with hello [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) tooget users added in hello Ceridian Dayforce HCM application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fd26f-232">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fd26f-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fd26f-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCeridian Dayforce HCM megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="fd26f-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCeridian Dayforce HCM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fd26f-235">**tooassign Britta Simon tooCeridian Dayforce HCM, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="fd26f-235">**tooassign Britta Simon tooCeridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd26f-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fd26f-238">Hello alkalmazások listában válassza ki a **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-238">In hello applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. <span data-ttu-id="fd26f-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fd26f-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-242">Click **Add** button.</span></span> <span data-ttu-id="fd26f-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fd26f-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fd26f-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fd26f-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fd26f-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-247">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="fd26f-248">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="fd26f-248">Assign hello Azure AD test user</span></span>

<span data-ttu-id="fd26f-249">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCeridian Dayforce HCM megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="fd26f-249">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCeridian Dayforce HCM.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="fd26f-251">**tooassign Britta Simon tooCeridian Dayforce HCM, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="fd26f-251">**tooassign Britta Simon tooCeridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd26f-252">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-252">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fd26f-254">Hello alkalmazások listában válassza ki a **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-254">In hello applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![hello Ceridian Dayforce HCM hivatkozásra hello alkalmazások listája](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. <span data-ttu-id="fd26f-256">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fd26f-256">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="fd26f-258">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd26f-258">Click **Add** button.</span></span> <span data-ttu-id="fd26f-259">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="fd26f-261">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fd26f-261">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fd26f-262">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fd26f-263">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fd26f-264">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fd26f-264">Test single sign-on</span></span>

<span data-ttu-id="fd26f-265">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="fd26f-265">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="fd26f-266">Ha a hozzáférési Panel hello hello Ceridian Dayforce HCM csempe gombra kattint, automatikusan bejelentkezett tooyour Ceridian Dayforce HCM alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="fd26f-266">When you click hello Ceridian Dayforce HCM tile in hello Access Panel, you should get automatically signed-on tooyour Ceridian Dayforce HCM application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fd26f-267">További források</span><span class="sxs-lookup"><span data-stu-id="fd26f-267">Additional resources</span></span>

* [<span data-ttu-id="fd26f-268">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="fd26f-268">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fd26f-269">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fd26f-269">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

