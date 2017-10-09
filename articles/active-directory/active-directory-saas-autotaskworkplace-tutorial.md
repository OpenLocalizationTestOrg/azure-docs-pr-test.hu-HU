---
title: "Oktatóanyag: Azure Active Directoryval integrált Autotask munkahelyi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Autotask munkahelyi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7f820f24e8e9493fa2e1c075f2ef61d7eaa84f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a><span data-ttu-id="910ff-103">Oktatóanyag: Azure Active Directoryval integrált Autotask munkahelyi</span><span class="sxs-lookup"><span data-stu-id="910ff-103">Tutorial: Azure Active Directory integration with Autotask Workplace</span></span>

<span data-ttu-id="910ff-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Autotask munkahelyi Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="910ff-104">In this tutorial, you learn how toointegrate Autotask Workplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="910ff-105">Autotask munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="910ff-105">Integrating Autotask Workplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="910ff-106">Megadhatja a munkahelyi hozzáférés tooAutotask rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="910ff-106">You can control in Azure AD who has access tooAutotask Workplace</span></span>
- <span data-ttu-id="910ff-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAutotask munkahelyi (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="910ff-107">You can enable your users tooautomatically get signed-on tooAutotask Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="910ff-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="910ff-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="910ff-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="910ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="910ff-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="910ff-110">Prerequisites</span></span>

<span data-ttu-id="910ff-111">az Azure AD integrálása Autotask munkahelyi tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="910ff-111">tooconfigure Azure AD integration with Autotask Workplace, you need hello following items:</span></span>

- <span data-ttu-id="910ff-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="910ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="910ff-113">Egy Autotask munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="910ff-113">An Autotask Workplace single-sign on enabled subscription</span></span>
- <span data-ttu-id="910ff-114">Egy rendszergazda vagy a munkahelyi felügyelői rendszergazda kell lennie.</span><span class="sxs-lookup"><span data-stu-id="910ff-114">You must be an administrator or super administrator in Workplace.</span></span>
- <span data-ttu-id="910ff-115">A hello Azure AD rendszergazdai fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="910ff-115">You must have an administrator account in hello Azure AD.</span></span>
- <span data-ttu-id="910ff-116">fogja ezt a szolgáltatást használó felhasználók hello munkahelyi és az Azure AD hello fiókokat kell rendelkeznie, és az e-mail-címét egyaránt meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="910ff-116">hello users that will utilize this feature must have accounts within Workplace and hello Azure AD, and their email addresses for both must match.</span></span>

> [!NOTE]
> <span data-ttu-id="910ff-117">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="910ff-117">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="910ff-118">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="910ff-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="910ff-119">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="910ff-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="910ff-120">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="910ff-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="910ff-121">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="910ff-121">Scenario description</span></span>
<span data-ttu-id="910ff-122">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="910ff-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="910ff-123">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="910ff-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="910ff-124">Hello gyűjteményből Autotask munkahelyi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="910ff-124">Adding Autotask Workplace from hello gallery</span></span>
2. <span data-ttu-id="910ff-125">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="910ff-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-autotask-workplace-from-hello-gallery"></a><span data-ttu-id="910ff-126">Hello gyűjteményből Autotask munkahelyi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="910ff-126">Adding Autotask Workplace from hello gallery</span></span>
<span data-ttu-id="910ff-127">tooconfigure hello integrációs Autotask munkahely, az Azure AD-be, meg kell tooadd Autotask munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="910ff-127">tooconfigure hello integration of Autotask Workplace into Azure AD, you need tooadd Autotask Workplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="910ff-128">**tooadd Autotask munkahelyi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="910ff-128">**tooadd Autotask Workplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="910ff-129">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="910ff-129">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="910ff-131">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="910ff-131">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="910ff-132">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="910ff-132">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="910ff-134">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="910ff-134">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="910ff-136">Hello keresési mezőbe, írja be a **Autotask munkahelyi**, jelölje be **Autotask munkahelyi** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="910ff-136">In hello search box, type **Autotask Workplace**, select  **Autotask Workplace**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Autotask munkahelyi hello az eredmények listájában](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="910ff-138">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="910ff-138">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="910ff-139">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Autotask munkahelyi "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="910ff-139">In this section, you configure and test Azure AD single sign-on with Autotask Workplace based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="910ff-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Autotask munkahelyi tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="910ff-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Autotask Workplace is tooa user in Azure AD.</span></span> <span data-ttu-id="910ff-141">Ez azt jelenti hello kapcsolódó felhasználói Autotask munkahelyi és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="910ff-141">In other words, a link relationship between an Azure AD user and hello related user in Autotask Workplace needs toobe established.</span></span>

<span data-ttu-id="910ff-142">Autotask munkahelyi rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="910ff-142">In Autotask Workplace, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="910ff-143">tooconfigure és az Azure AD az egyszeri bejelentkezés Autotask munkahelyi-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="910ff-143">tooconfigure and test Azure AD single sign-on with Autotask Workplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="910ff-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="910ff-144">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="910ff-145">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="910ff-145">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="910ff-146">**[Hozzon létre egy Autotask munkahelyi tesztfelhasználó](#create-an-autotask-workplace-test-user)**  -toohave egy megfelelője a Britta Simon Autotask munkahelyi, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="910ff-146">**[Create an Autotask Workplace test user](#create-an-autotask-workplace-test-user)** - toohave a counterpart of Britta Simon in Autotask Workplace that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="910ff-147">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="910ff-147">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="910ff-148">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="910ff-148">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="910ff-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="910ff-149">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="910ff-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Autotask munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="910ff-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Autotask Workplace application.</span></span>

<span data-ttu-id="910ff-151">**tooconfigure az Azure AD egyszeri bejelentkezést a Autotask munkahelyi, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="910ff-151">**tooconfigure Azure AD single sign-on with Autotask Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="910ff-152">Az Azure portál, a hello hello **Autotask munkahelyi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="910ff-152">In hello Azure portal, on hello **Autotask Workplace** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="910ff-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="910ff-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. <span data-ttu-id="910ff-156">Ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód, hajtsa végre a következő lépéseket a hello hello **Autotask munkahelyi tartomány és az URL-címek** szakasz:</span><span class="sxs-lookup"><span data-stu-id="910ff-156">If you wish tooconfigure hello application in **IDP** initiated mode, perform hello following steps on hello **Autotask Workplace Domain and URLs** section:</span></span>

    ![Autotask munkahelyi tartomány URL-címek egyetlen bejelentkezési adatai és IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    <span data-ttu-id="910ff-158">a.</span><span class="sxs-lookup"><span data-stu-id="910ff-158">a.</span></span> <span data-ttu-id="910ff-159">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="910ff-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span></span>

    <span data-ttu-id="910ff-160">b.</span><span class="sxs-lookup"><span data-stu-id="910ff-160">b.</span></span> <span data-ttu-id="910ff-161">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="910ff-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span></span>

4. <span data-ttu-id="910ff-162">Ha tooconfigure hello alkalmazás **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** , és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="910ff-162">If you wish tooconfigure hello application in **SP** initiated mode, check **Show advanced URL settings** and perform hello following steps:</span></span>

    ![Autotask munkahelyi tartomány URL-címek egyetlen bejelentkezési adatai és SP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    <span data-ttu-id="910ff-164">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awp.autotask.net/loginsso`</span><span class="sxs-lookup"><span data-stu-id="910ff-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/loginsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="910ff-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="910ff-165">These values are not real.</span></span> <span data-ttu-id="910ff-166">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="910ff-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="910ff-167">Ügyfél [Autotask munkahelyi ügyfél-támogatási csoport](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="910ff-167">Contact [Autotask Workplace Client support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget these values.</span></span> 

5. <span data-ttu-id="910ff-168">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="910ff-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. <span data-ttu-id="910ff-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="910ff-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="910ff-172">Egy másik webes böngészőablakban tooWorkplace Online használatával bejelentkezés hello rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="910ff-172">In a different web browser window, Log in tooWorkplace Online using hello administrator credentials.</span></span>

    >[!Note]
    ><span data-ttu-id="910ff-173">Hello IdP konfigurálásakor altartomány megadott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="910ff-173">When configuring hello IdP, a subdomain will need toobe specified.</span></span> <span data-ttu-id="910ff-174">tooconfirm hello megfelelő altartomány, bejelentkezési tooWorkplace Online.</span><span class="sxs-lookup"><span data-stu-id="910ff-174">tooconfirm hello correct subdomain, login tooWorkplace Online.</span></span> <span data-ttu-id="910ff-175">Miután bejelentkezett, ellenőrizze a Megjegyzés toohello altartomány hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="910ff-175">Once logged in, make note toohello subdomain in hello URL.</span></span>
    ><span data-ttu-id="910ff-176">hello altartomány közötti hello "https://" és ".awp.autotask.net/" hello része, és velünk, Európa, ca vagy au kell lennie.</span><span class="sxs-lookup"><span data-stu-id="910ff-176">hello subdomain is hello part between hello “https://“ and “.awp.autotask.net/“ and should be us, eu, ca, or au.</span></span>

8. <span data-ttu-id="910ff-177">Nyissa meg túl**konfigurációs** > **egyszeri bejelentkezés** , és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="910ff-177">Go too**Configuration** > **Single Sign-On** and perform hello following steps:</span></span>

    ![Autotask egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    <span data-ttu-id="910ff-179">a.</span><span class="sxs-lookup"><span data-stu-id="910ff-179">a.</span></span> <span data-ttu-id="910ff-180">Jelölje be hello **XML metaadatfájl** lehetőséget, és majd feltölteni hello **metaadatainak XML-kódja** Azure portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="910ff-180">Select hello **XML Metadata File** option, and then upload hello **Metadata XML** downloaded from Azure portal.</span></span>

    <span data-ttu-id="910ff-181">b.</span><span class="sxs-lookup"><span data-stu-id="910ff-181">b.</span></span> <span data-ttu-id="910ff-182">Kattintson a **engedélyezze az egyszeri Bejelentkezést**.</span><span class="sxs-lookup"><span data-stu-id="910ff-182">Click **Enable SSO**.</span></span>
    
    ![Konfiguráció jóváhagyásához Autotask egyszeri bejelentkezés](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    <span data-ttu-id="910ff-184">c.</span><span class="sxs-lookup"><span data-stu-id="910ff-184">c.</span></span> <span data-ttu-id="910ff-185">Jelölje be hello **megerősítem, ez az információ helyességéről, valamint a kiállító terjesztési hely megbízható** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="910ff-185">Select hello **I confirm this information is correct and I trust this IdP** check box.</span></span>

    <span data-ttu-id="910ff-186">d.</span><span class="sxs-lookup"><span data-stu-id="910ff-186">d.</span></span> <span data-ttu-id="910ff-187">Kattintson a **jóváhagyása**.</span><span class="sxs-lookup"><span data-stu-id="910ff-187">Click **Approve**.</span></span>
     
>[!Note]
><span data-ttu-id="910ff-188">Ha Autotask munkahelyi konfigurálásánál segítségre van szüksége, tekintse át [ezen a lapon](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget segítséget a munkahelyi fiókjával.</span><span class="sxs-lookup"><span data-stu-id="910ff-188">If you require assistance with configuring Autotask Workplace, please see [this page](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget assistance with your Workplace account.</span></span>

> [!TIP]
> <span data-ttu-id="910ff-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="910ff-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="910ff-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="910ff-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="910ff-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="910ff-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="910ff-192">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="910ff-192">Create an Azure AD test user</span></span>

<span data-ttu-id="910ff-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="910ff-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="910ff-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="910ff-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="910ff-196">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="910ff-196">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="910ff-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="910ff-198">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="910ff-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="910ff-200">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="910ff-202">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="910ff-202">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="910ff-204">a.</span><span class="sxs-lookup"><span data-stu-id="910ff-204">a.</span></span> <span data-ttu-id="910ff-205">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="910ff-205">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="910ff-206">b.</span><span class="sxs-lookup"><span data-stu-id="910ff-206">b.</span></span> <span data-ttu-id="910ff-207">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="910ff-207">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="910ff-208">c.</span><span class="sxs-lookup"><span data-stu-id="910ff-208">c.</span></span> <span data-ttu-id="910ff-209">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="910ff-209">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="910ff-210">d.</span><span class="sxs-lookup"><span data-stu-id="910ff-210">d.</span></span> <span data-ttu-id="910ff-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="910ff-211">Click **Create**.</span></span>

### <a name="create-an-autotask-workplace-test-user"></a><span data-ttu-id="910ff-212">Hozzon létre egy Autotask munkahelyi tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="910ff-212">Create an Autotask Workplace test user</span></span>

<span data-ttu-id="910ff-213">Ebben a szakaszban egy Autotask Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="910ff-213">In this section, you create a user called Britta Simon in Autotask.</span></span> <span data-ttu-id="910ff-214">Adjon együttműködve [Autotask munkahelyi támogatási csoport](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello felhasználók hello Autotask munkahelyi platform.</span><span class="sxs-lookup"><span data-stu-id="910ff-214">Please work with [Autotask Workplace support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello users in hello Autotask Workplace platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="910ff-215">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="910ff-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="910ff-216">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés munkahelyi hozzáférés tooAutotask megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="910ff-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAutotask Workplace.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="910ff-218">**tooassign Britta Simon tooAutotask munkahelyi, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="910ff-218">**tooassign Britta Simon tooAutotask Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="910ff-219">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="910ff-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="910ff-221">Hello alkalmazások listában válassza ki a **Autotask munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="910ff-221">In hello applications list, select **Autotask Workplace**.</span></span>

    ![hello Autotask munkahelyi hivatkozásra hello alkalmazások listája](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. <span data-ttu-id="910ff-223">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="910ff-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="910ff-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="910ff-225">Click **Add** button.</span></span> <span data-ttu-id="910ff-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="910ff-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="910ff-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="910ff-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="910ff-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="910ff-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="910ff-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="910ff-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="910ff-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="910ff-231">Test single sign-on</span></span>

<span data-ttu-id="910ff-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="910ff-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="910ff-233">Hello Autotask munkahelyi hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Autotask munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="910ff-233">When you click hello Autotask Workplace tile in hello Access Panel, you should get automatically signed-on tooyour Autotask Workplace application.</span></span>
<span data-ttu-id="910ff-234">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="910ff-234">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="910ff-235">További források</span><span class="sxs-lookup"><span data-stu-id="910ff-235">Additional resources</span></span>

* [<span data-ttu-id="910ff-236">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="910ff-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="910ff-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="910ff-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

