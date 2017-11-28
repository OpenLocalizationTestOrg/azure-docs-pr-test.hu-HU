---
title: "Oktatóanyag: Azure Active Directoryval integrált MOVEit átadás - Azure AD-integrációs |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és MOVEit átadás - integráció az Azure AD között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 5bbe4f2d952bd45c4d58d55ffc3467b4eb871fd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a><span data-ttu-id="02166-103">Oktatóanyag: Azure Active Directoryval integrált MOVEit átadás - Azure AD-integrációs</span><span class="sxs-lookup"><span data-stu-id="02166-103">Tutorial: Azure Active Directory integration with MOVEit Transfer - Azure AD integration</span></span>

<span data-ttu-id="02166-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate MOVEit átadás - Azure AD integrálása az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="02166-104">In this tutorial, you learn how toointegrate MOVEit Transfer - Azure AD integration with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="02166-105">MOVEit átadás - Azure AD integrálása az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="02166-105">Integrating MOVEit Transfer - Azure AD integration with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="02166-106">Az Azure AD hozzáférési tooMOVEit átadás - Azure AD-integrációs rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="02166-106">You can control in Azure AD who has access tooMOVEit Transfer - Azure AD integration.</span></span>
- <span data-ttu-id="02166-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMOVEit átadás - az Azure AD integrálása (egyszeri bejelentkezés) az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="02166-107">You can enable your users tooautomatically get signed-on tooMOVEit Transfer - Azure AD integration (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="02166-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="02166-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="02166-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="02166-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02166-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="02166-110">Prerequisites</span></span>

<span data-ttu-id="02166-111">a következő elemek hello kell tooconfigure MOVEit átadás - Azure AD integrálása az Azure AD integrálása:</span><span class="sxs-lookup"><span data-stu-id="02166-111">tooconfigure Azure AD integration with MOVEit Transfer - Azure AD integration, you need hello following items:</span></span>

- <span data-ttu-id="02166-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="02166-112">An Azure AD subscription</span></span>
- <span data-ttu-id="02166-113">A MOVEit átvitel – az Azure AD integrálása egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="02166-113">A MOVEit Transfer - Azure AD integration single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="02166-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="02166-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="02166-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="02166-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="02166-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="02166-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="02166-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="02166-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="02166-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="02166-118">Scenario description</span></span>
<span data-ttu-id="02166-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="02166-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="02166-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="02166-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="02166-121">MOVEit átadás - hello gyűjteményből az Azure AD-integrációs hozzáadása</span><span class="sxs-lookup"><span data-stu-id="02166-121">Adding MOVEit Transfer - Azure AD integration from hello gallery</span></span>
2. <span data-ttu-id="02166-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="02166-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moveit-transfer---azure-ad-integration-from-hello-gallery"></a><span data-ttu-id="02166-123">MOVEit átadás - hello gyűjteményből az Azure AD-integrációs hozzáadása</span><span class="sxs-lookup"><span data-stu-id="02166-123">Adding MOVEit Transfer - Azure AD integration from hello gallery</span></span>
<span data-ttu-id="02166-124">tooconfigure hello integrációs MOVEit átadás - Azure AD integrálása az Azure AD-be kell tooadd MOVEit átadás - felügyelt SaaS-alkalmazásokhoz az Azure AD-integrációs hello gyűjtemény tooyour listából.</span><span class="sxs-lookup"><span data-stu-id="02166-124">tooconfigure hello integration of MOVEit Transfer - Azure AD integration into Azure AD, you need tooadd MOVEit Transfer - Azure AD integration from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="02166-125">**tooadd MOVEit átadás - hello gyűjteményből, az Azure AD-integrációs hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="02166-125">**tooadd MOVEit Transfer - Azure AD integration from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="02166-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="02166-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="02166-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="02166-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="02166-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="02166-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="02166-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="02166-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="02166-133">Hello keresési mezőbe, írja be a **MOVEit átadás - Azure AD-integrációs**, jelölje be **MOVEit átadás - Azure AD-integrációs** eredmény panelen kattintson a **Hozzáadás** gomb tooadd hello az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="02166-133">In hello search box, type **MOVEit Transfer - Azure AD integration**, select **MOVEit Transfer - Azure AD integration** from result panel then click **Add** button tooadd hello application.</span></span>

    ![MOVEit átadás - hello eredménylistában az Azure AD-integráció](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="02166-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="02166-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="02166-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a MOVEit átadás - Azure AD-integrációs "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="02166-136">In this section, you configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="02166-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói az MOVEit átviteli – az Azure AD-integrációs tooa felhasználói Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="02166-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MOVEit Transfer - Azure AD integration is tooa user in Azure AD.</span></span> <span data-ttu-id="02166-138">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello MOVEit átadás - hivatkozás kapcsolata az Azure AD-integrációs kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="02166-138">In other words, a link relationship between an Azure AD user and hello related user in MOVEit Transfer - Azure AD integration needs toobe established.</span></span>

<span data-ttu-id="02166-139">MOVEit átadás - Azure AD-integrációs, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="02166-139">In MOVEit Transfer - Azure AD integration, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="02166-140">tooconfigure és MOVEit átadás - Azure AD integrálása az Azure AD az egyszeri bejelentkezés-teszthez van szüksége a következő építőelemeket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="02166-140">tooconfigure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="02166-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="02166-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="02166-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="02166-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="02166-143">**[Hozzon létre egy MOVEit átadás - az Azure AD-integrációs tesztfelhasználó](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - toohave egy megfelelője a Britta Simon az MOVEit átviteli - Azure AD-integrációs, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="02166-143">**[Create a MOVEit Transfer - Azure AD integration test user](#create-a-moveit-transfer---azure-ad-integration-test-user)** - toohave a counterpart of Britta Simon in MOVEit Transfer - Azure AD integration that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="02166-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="02166-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="02166-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="02166-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="02166-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="02166-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="02166-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a MOVEit átadás - Azure AD-integrációs alkalmazást az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="02166-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MOVEit Transfer - Azure AD integration application.</span></span>

<span data-ttu-id="02166-148">**tooconfigure az Azure AD egyszeri bejelentkezést a MOVEit átadás - Azure AD-integrációs, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="02166-148">**tooconfigure Azure AD single sign-on with MOVEit Transfer - Azure AD integration, perform hello following steps:**</span></span>

1. <span data-ttu-id="02166-149">Az Azure portál, a hello hello **MOVEit átadás - Azure AD-integrációs** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="02166-149">In hello Azure portal, on hello **MOVEit Transfer - Azure AD integration** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="02166-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="02166-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. <span data-ttu-id="02166-153">A hello **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="02166-153">On hello **MOVEit Transfer - Azure AD integration Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    <span data-ttu-id="02166-155">a.</span><span class="sxs-lookup"><span data-stu-id="02166-155">a.</span></span> <span data-ttu-id="02166-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://contoso.com`</span><span class="sxs-lookup"><span data-stu-id="02166-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://contoso.com`</span></span>

    <span data-ttu-id="02166-157">b.</span><span class="sxs-lookup"><span data-stu-id="02166-157">b.</span></span> <span data-ttu-id="02166-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://contoso.com/<tenatid>`</span><span class="sxs-lookup"><span data-stu-id="02166-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://contoso.com/<tenatid>`</span></span>

    <span data-ttu-id="02166-159">c.</span><span class="sxs-lookup"><span data-stu-id="02166-159">c.</span></span> <span data-ttu-id="02166-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span><span class="sxs-lookup"><span data-stu-id="02166-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="02166-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="02166-161">These values are not real.</span></span> <span data-ttu-id="02166-162">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="02166-162">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="02166-163">Olvassa el ezeket az értékeket később a **szolgáltató metaadatainak URL-címe** szakaszban, vagy forduljon a [MOVEit átadás - az Azure AD integrálása ügyfél támogatási csoport](https://community.ipswitch.com/s/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="02166-163">You can refer these values later in **Service Provider Metadata URL** section or contact [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support) tooget these values.</span></span>

4. <span data-ttu-id="02166-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="02166-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. <span data-ttu-id="02166-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="02166-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="02166-168">Bejelentkezés tooyour MOVEit átviteli Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="02166-168">Sign on tooyour MOVEit Transfer tenant as an administrator.</span></span>

7. <span data-ttu-id="02166-169">A hello bal oldali navigációs ablaktábláján kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="02166-169">On hello left navigation pane, click **Settings**.</span></span>

    ![Beállítások szakaszban az alkalmazás ügyféloldali](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. <span data-ttu-id="02166-171">Kattintson a **egyszeri bejelentkezés** hivatkozás, amely alatt **biztonsági házirendek -> felhasználói hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="02166-171">Click **Single Signon** link, which is under **Security Policies -> User Auth**.</span></span>

    ![Biztonsági házirendek az alkalmazás ügyféloldali](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. <span data-ttu-id="02166-173">Kattintson a hello metaadatainak URL-CÍMÉT, hivatkozás toodownload hello metaadat-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="02166-173">Click hello Metadata URL link toodownload hello metadata document.</span></span>

    ![Szolgáltató metaadatainak URL-címe](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * <span data-ttu-id="02166-175">Győződjön meg arról **entityid beállítást** megfelelő **azonosító** a hello **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="02166-175">Verify **entityID** matches **Identifier** in hello **MOVEit Transfer - Azure AD integration Domain and URLs** section .</span></span>
    * <span data-ttu-id="02166-176">Győződjön meg arról **AssertionConsumerService** hely URL-címe megegyezik **válasz URL-CÍMEN** a hello **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="02166-176">Verify **AssertionConsumerService** Location URL matches **REPLY URL** in hello **MOVEit Transfer - Azure AD integration Domain and URLs** section.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. <span data-ttu-id="02166-178">Kattintson a **identitásszolgáltató hozzáadása** tooadd egy új Összevont identitásszolgáltató gombra.</span><span class="sxs-lookup"><span data-stu-id="02166-178">Click **Add Identity Provider** button tooadd a new Federated Identity Provider.</span></span>

    ![Identitás-szolgáltató felvétele](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. <span data-ttu-id="02166-180">Kattintson a **Tallózás...**  tooselect hello Azure portálról letöltött metaadat-fájlt, majd kattintson az **identitásszolgáltató hozzáadása** tooupload hello letöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="02166-180">Click **Browse...** tooselect hello metadata file which you downloaded from Azure portal, then click **Add Identity Provider** tooupload hello downloaded file.</span></span>

    ![SAML-Identitásszolgáltatóként](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. <span data-ttu-id="02166-182">Jelölje ki "**Igen**" mint **engedélyezve** a hello **összevont identitás szolgáltató beállításainak szerkesztése...**  lapot, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="02166-182">Select "**Yes**" as **Enabled** in hello **Edit Federated Identity Provider Settings...** page and click **Save**.</span></span>

    ![Összevont identitás-szolgáltató beállításai](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. <span data-ttu-id="02166-184">A hello **összevont identitás szolgáltató felhasználói beállítások szerkesztése** lapon, hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="02166-184">In hello **Edit Federated Identity Provider User Settings** page, perform hello following actions:</span></span>
    
    ![Összevont identitás Szolgáltatóbeállítások szerkesztése](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    <span data-ttu-id="02166-186">a.</span><span class="sxs-lookup"><span data-stu-id="02166-186">a.</span></span> <span data-ttu-id="02166-187">Válassza ki **SAML NameID** , **bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="02166-187">Select **SAML NameID** as **Login name**.</span></span>
    
    <span data-ttu-id="02166-188">b.</span><span class="sxs-lookup"><span data-stu-id="02166-188">b.</span></span> <span data-ttu-id="02166-189">Válassza ki **más** , **teljes nevét** és hello **attribútumnév** szövegmezőbe írja be hello értéket: `http://schemas.microsoft.com/identity/claims/displayname`.</span><span class="sxs-lookup"><span data-stu-id="02166-189">Select **Other** as **Full name** and in hello **Attribute name** textbox put hello value: `http://schemas.microsoft.com/identity/claims/displayname`.</span></span>
    
    <span data-ttu-id="02166-190">c.</span><span class="sxs-lookup"><span data-stu-id="02166-190">c.</span></span> <span data-ttu-id="02166-191">Válassza ki **más** , **E-mail** és hello **attribútumnév** szövegmezőbe írja be hello értéket: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="02166-191">Select **Other** as **Email** and in hello **Attribute name** textbox put hello value: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="02166-192">d.</span><span class="sxs-lookup"><span data-stu-id="02166-192">d.</span></span> <span data-ttu-id="02166-193">Válassza ki **Igen** , **fiók automatikus létrehozása frissítsen**.</span><span class="sxs-lookup"><span data-stu-id="02166-193">Select **Yes** as **Auto-create account on signon**.</span></span>
    
    <span data-ttu-id="02166-194">e.</span><span class="sxs-lookup"><span data-stu-id="02166-194">e.</span></span> <span data-ttu-id="02166-195">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="02166-195">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="02166-196">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="02166-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="02166-197">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="02166-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="02166-198">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="02166-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="02166-199">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="02166-199">Create an Azure AD test user</span></span>

<span data-ttu-id="02166-200">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="02166-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="02166-202">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="02166-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="02166-203">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="02166-203">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="02166-205">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="02166-205">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="02166-207">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="02166-207">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="02166-209">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="02166-209">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    <span data-ttu-id="02166-211">a.</span><span class="sxs-lookup"><span data-stu-id="02166-211">a.</span></span> <span data-ttu-id="02166-212">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="02166-212">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="02166-213">b.</span><span class="sxs-lookup"><span data-stu-id="02166-213">b.</span></span> <span data-ttu-id="02166-214">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="02166-214">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="02166-215">c.</span><span class="sxs-lookup"><span data-stu-id="02166-215">c.</span></span> <span data-ttu-id="02166-216">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="02166-216">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="02166-217">d.</span><span class="sxs-lookup"><span data-stu-id="02166-217">d.</span></span> <span data-ttu-id="02166-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="02166-218">Click **Create**.</span></span>
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a><span data-ttu-id="02166-219">Hozzon létre egy MOVEit átadás - Azure AD-integrációs teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="02166-219">Create a MOVEit Transfer - Azure AD integration test user</span></span>

<span data-ttu-id="02166-220">hello ebben a szakaszban célja toocreate MOVEit átadás - Azure AD-integrációs Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="02166-220">hello objective of this section is toocreate a user called Britta Simon in MOVEit Transfer - Azure AD integration.</span></span> <span data-ttu-id="02166-221">MOVEit átadás - Azure AD-integrációs támogatja közvetlenül az időponthoz kötött kiosztást, amelyhez engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="02166-221">MOVEit Transfer - Azure AD integration supports just-in-time provisioning, which you have enabled.</span></span> <span data-ttu-id="02166-222">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="02166-222">There is no action item for you in this section.</span></span> <span data-ttu-id="02166-223">Új felhasználó jön létre, egy kísérlet tooaccess MOVEit átviteli – Ha még nem létezik az Azure AD-integrációs során.</span><span class="sxs-lookup"><span data-stu-id="02166-223">A new user is created during an attempt tooaccess MOVEit Transfer - Azure AD integration if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="02166-224">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [MOVEit átadás - az Azure AD integrálása ügyfél támogatási csoport](https://community.ipswitch.com/s/support).</span><span class="sxs-lookup"><span data-stu-id="02166-224">If you need toocreate a user manually, you need toocontact hello [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="02166-225">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="02166-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="02166-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMOVEit átadás - Azure AD-integrációs megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="02166-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMOVEit Transfer - Azure AD integration.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="02166-228">**tooassign Britta Simon tooMOVEit átadás - Azure AD-integrációs, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="02166-228">**tooassign Britta Simon tooMOVEit Transfer - Azure AD integration, perform hello following steps:**</span></span>

1. <span data-ttu-id="02166-229">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="02166-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="02166-231">Hello alkalmazások listában válassza ki a **MOVEit átadás - Azure AD-integrációs**.</span><span class="sxs-lookup"><span data-stu-id="02166-231">In hello applications list, select **MOVEit Transfer - Azure AD integration**.</span></span>

    ![hello MOVEit átadás - hello alkalmazások listáját hivatkozásra az Azure AD-integrációs](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. <span data-ttu-id="02166-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="02166-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="02166-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="02166-235">Click **Add** button.</span></span> <span data-ttu-id="02166-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="02166-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="02166-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="02166-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="02166-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="02166-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="02166-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="02166-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="02166-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="02166-241">Test single sign-on</span></span>

<span data-ttu-id="02166-242">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="02166-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="02166-243">Ha hello MOVEit átadás - Azure AD-integrációs csempéjére kattint, a hozzáférési Panel hello, kapja meg automatikusan bejelentkezett tooyour MOVEit átadás - Azure AD-integrációs alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="02166-243">When you click hello MOVEit Transfer - Azure AD integration tile in hello Access Panel, you should get automatically signed-on tooyour MOVEit Transfer - Azure AD integration application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="02166-244">További források</span><span class="sxs-lookup"><span data-stu-id="02166-244">Additional resources</span></span>

* [<span data-ttu-id="02166-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="02166-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="02166-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="02166-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

