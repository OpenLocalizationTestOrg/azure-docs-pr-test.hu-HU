---
title: "Oktatóanyag: Azure Active Directoryval integrált Tangoe parancs prémium Mobile |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Tangoe parancs prémium Mobile között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7bd1cd6afade58a4a47a173b249f92eb84e42112
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a><span data-ttu-id="6e688-103">Oktatóanyag: Azure Active Directoryval integrált Tangoe parancs prémium Mobile</span><span class="sxs-lookup"><span data-stu-id="6e688-103">Tutorial: Azure Active Directory integration with Tangoe Command Premium Mobile</span></span>

<span data-ttu-id="6e688-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Tangoe parancs prémium Mobile az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e688-104">In this tutorial, you learn how toointegrate Tangoe Command Premium Mobile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6e688-105">Tangoe parancs prémium Mobile integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6e688-105">Integrating Tangoe Command Premium Mobile with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6e688-106">Megadhatja a hozzáférés tooTangoe parancs prémium Mobile rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6e688-106">You can control in Azure AD who has access tooTangoe Command Premium Mobile</span></span>
- <span data-ttu-id="6e688-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTangoe parancs prémium Mobile (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="6e688-107">You can enable your users tooautomatically get signed-on tooTangoe Command Premium Mobile (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6e688-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6e688-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6e688-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6e688-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e688-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6e688-110">Prerequisites</span></span>

<span data-ttu-id="6e688-111">tooconfigure az Azure AD integrálása Tangoe parancs prémium Mobile, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="6e688-111">tooconfigure Azure AD integration with Tangoe Command Premium Mobile, you need hello following items:</span></span>

- <span data-ttu-id="6e688-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6e688-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6e688-113">Egy Tangoe parancs prémium Mobile egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6e688-113">A Tangoe Command Premium Mobile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6e688-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6e688-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6e688-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6e688-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6e688-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6e688-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6e688-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6e688-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6e688-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6e688-118">Scenario description</span></span>
<span data-ttu-id="6e688-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6e688-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6e688-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6e688-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6e688-121">Adja hozzá a Tangoe parancs prémium Mobile hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6e688-121">Add Tangoe Command Premium Mobile from hello gallery</span></span>
2. <span data-ttu-id="6e688-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e688-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tangoe-command-premium-mobile-from-hello-gallery"></a><span data-ttu-id="6e688-123">Adja hozzá a Tangoe parancs prémium Mobile hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6e688-123">Add Tangoe Command Premium Mobile from hello gallery</span></span>
<span data-ttu-id="6e688-124">tooconfigure hello integrációs Tangoe parancs prémium mobil az Azure AD-be, meg kell tooadd Tangoe parancs prémium Mobile hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6e688-124">tooconfigure hello integration of Tangoe Command Premium Mobile into Azure AD, you need tooadd Tangoe Command Premium Mobile from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6e688-125">**tooadd Tangoe parancs prémium Mobile hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6e688-125">**tooadd Tangoe Command Premium Mobile from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e688-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6e688-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6e688-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6e688-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6e688-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6e688-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6e688-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6e688-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6e688-133">Hello keresési mezőbe, írja be a **Tangoe parancs prémium Mobile**, jelölje be **Tangoe parancs prémium Mobile** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6e688-133">In hello search box, type **Tangoe Command Premium Mobile**, select **Tangoe Command Premium Mobile** from result panel then click **Add** button tooadd hello application.</span></span>

    ![<span data-ttu-id="6e688-134">Adja hozzá a Tangoe parancs prémium mobil gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6e688-134">Add Tangoe Command Premium Mobile from gallery</span></span> ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6e688-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e688-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="6e688-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Tangoe parancs prémium Mobile "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="6e688-136">In this section, you configure and test Azure AD single sign-on with Tangoe Command Premium Mobile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6e688-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Tangoe parancs prémium Mobile tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6e688-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tangoe Command Premium Mobile is tooa user in Azure AD.</span></span> <span data-ttu-id="6e688-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Tangoe parancs prémium Mobile közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6e688-138">In other words, a link relationship between an Azure AD user and hello related user in Tangoe Command Premium Mobile needs toobe established.</span></span>

<span data-ttu-id="6e688-139">A Tangoe parancs prémium Mobile, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6e688-139">In Tangoe Command Premium Mobile, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6e688-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Tangoe parancs prémium Mobile-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6e688-140">tooconfigure and test Azure AD single sign-on with Tangoe Command Premium Mobile, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6e688-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6e688-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6e688-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e688-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6e688-143">**[Tangoe parancs prémium Mobile tesztfelhasználó létrehozása](#create-a-tangoe-command-premium-mobile-test-user)**  -toohave egy megfelelője a Britta Simon a Tangoe parancs prémium Mobile felhasználói az Azure AD csatolt toohello ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6e688-143">**[Create a Tangoe Command Premium Mobile test user](#create-a-tangoe-command-premium-mobile-test-user)** - toohave a counterpart of Britta Simon in Tangoe Command Premium Mobile that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6e688-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6e688-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6e688-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6e688-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6e688-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e688-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6e688-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Tangoe parancs prémium Mobile alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="6e688-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tangoe Command Premium Mobile application.</span></span>

<span data-ttu-id="6e688-148">**tooconfigure az Azure AD egyszeri bejelentkezést és a Tangoe parancs prémium Mobile, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6e688-148">**tooconfigure Azure AD single sign-on with Tangoe Command Premium Mobile, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e688-149">Az Azure portál, a hello hello **Tangoe parancs prémium Mobile** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6e688-149">In hello Azure portal, on hello **Tangoe Command Premium Mobile** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6e688-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6e688-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. <span data-ttu-id="6e688-153">A hello **Tangoe parancs prémium Mobile tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6e688-153">On hello **Tangoe Command Premium Mobile Domain and URLs** section, perform hello following steps:</span></span>

    ![Tangoe parancs prémium mobil tartomány és az URL-címek](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    <span data-ttu-id="6e688-155">a.</span><span class="sxs-lookup"><span data-stu-id="6e688-155">a.</span></span> <span data-ttu-id="6e688-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span><span class="sxs-lookup"><span data-stu-id="6e688-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span></span>

    <span data-ttu-id="6e688-157">b.</span><span class="sxs-lookup"><span data-stu-id="6e688-157">b.</span></span> <span data-ttu-id="6e688-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.tangoe.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="6e688-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://sso.tangoe.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6e688-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6e688-159">These values are not real.</span></span> <span data-ttu-id="6e688-160">Ezek az értékek frissítése hello tényleges válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="6e688-160">Update these values with hello actual  Reply URL and Sign-On URL.</span></span> <span data-ttu-id="6e688-161">Ügyfél [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6e688-161">Contact [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) tooget these values.</span></span> 

4. <span data-ttu-id="6e688-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6e688-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. <span data-ttu-id="6e688-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6e688-164">Click **Save** button.</span></span>

    ![Mentés gombja](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="6e688-166">A hello **Tangoe prémium mobilalkalmazás konfigurálása** kattintson **Tangoe parancsot konfigurálása prémium szintű Mobile** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6e688-166">On hello **Tangoe Command Premium Mobile Configuration** section, click **Configure Tangoe Command Premium Mobile** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6e688-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6e688-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Tangoe parancs prémium Mobile konfigurációs szakasz](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. <span data-ttu-id="6e688-169">az alkalmazáshoz konfigurált SSO tooget, forduljon a [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) , és adja meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6e688-169">tooget SSO configured for your application, contact your [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) and provide hello following:</span></span>

   - <span data-ttu-id="6e688-170">hello letöltött metaadatait tartalmazó fájl</span><span class="sxs-lookup"><span data-stu-id="6e688-170">hello downloaded metadata file</span></span>
   - <span data-ttu-id="6e688-171">Hello **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="6e688-171">hello **SAML Entity ID**</span></span>
   - <span data-ttu-id="6e688-172">Hello **SAML-alapú egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="6e688-172">hello **SAML Single Sign-On Service URL**</span></span>
   - <span data-ttu-id="6e688-173">Hello **Sign-Out URL-címe**</span><span class="sxs-lookup"><span data-stu-id="6e688-173">hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="6e688-174">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="6e688-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6e688-175">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="6e688-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6e688-176">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6e688-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6e688-177">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="6e688-177">Create an Azure AD test user</span></span>
<span data-ttu-id="6e688-178">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e688-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6e688-180">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6e688-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e688-181">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6e688-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6e688-183">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6e688-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6e688-185">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e688-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Felhasználó hozzáadása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6e688-187">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6e688-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6e688-189">a.</span><span class="sxs-lookup"><span data-stu-id="6e688-189">a.</span></span> <span data-ttu-id="6e688-190">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6e688-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6e688-191">b.</span><span class="sxs-lookup"><span data-stu-id="6e688-191">b.</span></span> <span data-ttu-id="6e688-192">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6e688-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6e688-193">c.</span><span class="sxs-lookup"><span data-stu-id="6e688-193">c.</span></span> <span data-ttu-id="6e688-194">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6e688-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6e688-195">d.</span><span class="sxs-lookup"><span data-stu-id="6e688-195">d.</span></span> <span data-ttu-id="6e688-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6e688-196">Click **Create**.</span></span>
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a><span data-ttu-id="6e688-197">Tangoe parancs prémium Mobile tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e688-197">Create a Tangoe Command Premium Mobile test user</span></span>

<span data-ttu-id="6e688-198">Ebben a szakaszban Tangoe parancs prémium Mobile Britta Simon nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6e688-198">In this section, you create a user called Britta Simon in Tangoe Command Premium Mobile.</span></span> 

<span data-ttu-id="6e688-199">Tangoe parancs prémium Mobile alkalmazást kell minden hello felhasználók toobe kiépítve hello alkalmazás egyszeri bejelentkezés végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="6e688-199">Tangoe Command Premium Mobile application needs all hello users toobe provisioned in hello application before doing Single Sign On.</span></span> <span data-ttu-id="6e688-200">Ezért kérjük, munkahelyi rendelkező hello [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) tooprovision felhasználóira hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="6e688-200">So please work with hello [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) tooprovision all these users into hello application.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="6e688-201">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="6e688-201">Assign hello Azure AD test user</span></span>

<span data-ttu-id="6e688-202">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTangoe parancs prémium Mobile megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6e688-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTangoe Command Premium Mobile.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6e688-204">**tooassign Britta Simon tooTangoe parancs prémium Mobile, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6e688-204">**tooassign Britta Simon tooTangoe Command Premium Mobile, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e688-205">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6e688-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6e688-207">Hello alkalmazások listában válassza ki a **Tangoe parancs prémium Mobile**.</span><span class="sxs-lookup"><span data-stu-id="6e688-207">In hello applications list, select **Tangoe Command Premium Mobile**.</span></span>

    ![Tangoe parancs prémium Mobile alkalmazáslistában](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. <span data-ttu-id="6e688-209">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6e688-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6e688-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6e688-211">Click **Add** button.</span></span> <span data-ttu-id="6e688-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e688-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6e688-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6e688-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6e688-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e688-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6e688-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6e688-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6e688-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6e688-217">Test single sign-on</span></span>

<span data-ttu-id="6e688-218">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="6e688-218">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6e688-219">Ha a hozzáférési Panel hello hello Tangoe parancs prémium Mobile csempe gombra kattint, automatikusan bejelentkezett tooyour Tangoe parancs prémium Mobile alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="6e688-219">When you click hello Tangoe Command Premium Mobile tile in hello Access Panel, you should get automatically signed-on tooyour Tangoe Command Premium Mobile application.</span></span> <span data-ttu-id="6e688-220">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6e688-220">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6e688-221">További források</span><span class="sxs-lookup"><span data-stu-id="6e688-221">Additional resources</span></span>

* [<span data-ttu-id="6e688-222">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6e688-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6e688-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6e688-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

