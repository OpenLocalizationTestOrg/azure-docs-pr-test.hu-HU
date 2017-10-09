---
title: "Oktatóanyag: Azure Active Directoryval integrált TOPdesk - nyilvános |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TOPdesk - nyilvános között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ef0dd06157ecc3b33814590039f5cbae64e8c916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a><span data-ttu-id="b7545-103">Oktatóanyag: Azure Active Directoryval integrált TOPdesk - nyilvános</span><span class="sxs-lookup"><span data-stu-id="b7545-103">Tutorial: Azure Active Directory integration with TOPdesk - Public</span></span>

<span data-ttu-id="b7545-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TOPdesk - nyilvános Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b7545-104">In this tutorial, you learn how toointegrate TOPdesk - Public with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7545-105">TOPdesk - nyilvános az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-105">Integrating TOPdesk - Public with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b7545-106">Az Azure AD hozzáférési tooTOPdesk - rendelkező nyilvános szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="b7545-106">You can control in Azure AD who has access tooTOPdesk - Public.</span></span>
- <span data-ttu-id="b7545-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTOPdesk - nyilvános (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="b7545-107">You can enable your users tooautomatically get signed-on tooTOPdesk - Public (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b7545-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="b7545-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="b7545-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7545-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7545-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b7545-110">Prerequisites</span></span>

<span data-ttu-id="b7545-111">az Azure AD integrálása TOPdesk - tooconfigure nyilvános, a következő elemek hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="b7545-111">tooconfigure Azure AD integration with TOPdesk - Public, you need hello following items:</span></span>

- <span data-ttu-id="b7545-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b7545-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7545-113">Egy TOPdesk - nyilvános egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b7545-113">A TOPdesk - Public single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7545-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b7545-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7545-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b7545-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7545-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b7545-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7545-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7545-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7545-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b7545-118">Scenario description</span></span>
<span data-ttu-id="b7545-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b7545-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7545-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b7545-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7545-121">Hozzáadás TOPdesk - nyilvános hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b7545-121">Adding TOPdesk - Public from hello gallery</span></span>
2. <span data-ttu-id="b7545-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b7545-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-topdesk---public-from-hello-gallery"></a><span data-ttu-id="b7545-123">Hozzáadás TOPdesk - nyilvános hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b7545-123">Adding TOPdesk - Public from hello gallery</span></span>
<span data-ttu-id="b7545-124">tooconfigure hello integrációja TOPdesk - nyilvános az Azure AD-be kell tooadd TOPdesk - nyilvános hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b7545-124">tooconfigure hello integration of TOPdesk - Public into Azure AD, you need tooadd TOPdesk - Public from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b7545-125">**tooadd TOPdesk - nyilvános hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b7545-125">**tooadd TOPdesk - Public from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7545-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b7545-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="b7545-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b7545-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b7545-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7545-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="b7545-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b7545-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="b7545-133">Hello keresési mezőbe, írja be a **TOPdesk - nyilvános**, jelölje be **TOPdesk - nyilvános** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-133">In hello search box, type **TOPdesk - Public**, select **TOPdesk - Public** from result panel then click **Add** button tooadd hello application.</span></span>

    ![TOPdesk - hello eredménylistában nyilvános](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b7545-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7545-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b7545-136">Ebben a szakaszban az Azure AD egyszeri bejelentkezést a TOPdesk tesztelése és konfigurálása – nyilvános "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="b7545-136">In this section, you configure and test Azure AD single sign-on with TOPdesk - Public based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7545-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TOPdesk - nyilvános tooa felhasználói Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b7545-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TOPdesk - Public is tooa user in Azure AD.</span></span> <span data-ttu-id="b7545-138">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TOPdesk - hivatkozás kapcsolatának nyilvános kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="b7545-138">In other words, a link relationship between an Azure AD user and hello related user in TOPdesk - Public needs toobe established.</span></span>

<span data-ttu-id="b7545-139">A TOPdesk - nyilvános hozzárendelése hello értékének hello **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b7545-139">In TOPdesk - Public, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b7545-140">tooconfigure és az Azure AD egyszeri bejelentkezést a TOPdesk - nyilvános tesztelése, a következő építőelemeket toocomplete hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="b7545-140">tooconfigure and test Azure AD single sign-on with TOPdesk - Public, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b7545-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b7545-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b7545-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7545-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7545-143">**[Hozzon létre egy TOPdesk - nyilvános tesztfelhasználó](#create-a-topdesk---public-test-user)**  - toohave egy megfelelője a Britta Simon a TOPdesk - nyilvános, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b7545-143">**[Create a TOPdesk - Public test user](#create-a-topdesk---public-test-user)** - toohave a counterpart of Britta Simon in TOPdesk - Public that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7545-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b7545-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7545-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b7545-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b7545-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7545-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b7545-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez a TOPdesk - nyilvános alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7545-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TOPdesk - Public application.</span></span>

<span data-ttu-id="b7545-148">**az Azure AD egyszeri bejelentkezést a TOPdesk - tooconfigure nyilvános, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="b7545-148">**tooconfigure Azure AD single sign-on with TOPdesk - Public, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7545-149">Az Azure portál, a hello hello **TOPdesk - nyilvános** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b7545-149">In hello Azure portal, on hello **TOPdesk - Public** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="b7545-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b7545-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. <span data-ttu-id="b7545-153">A hello **TOPdesk - nyilvános tartományi és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-153">On hello **TOPdesk - Public Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk TOPdesk - nyilvános tartományi és URL-címek](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    <span data-ttu-id="b7545-155">a.</span><span class="sxs-lookup"><span data-stu-id="b7545-155">a.</span></span> <span data-ttu-id="b7545-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.topdesk.net`</span><span class="sxs-lookup"><span data-stu-id="b7545-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net`</span></span>
    
    <span data-ttu-id="b7545-157">b.</span><span class="sxs-lookup"><span data-stu-id="b7545-157">b.</span></span> <span data-ttu-id="b7545-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.topdesk.net/tas/public/login/verify`</span><span class="sxs-lookup"><span data-stu-id="b7545-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net/tas/public/login/verify`</span></span>

    <span data-ttu-id="b7545-159">c.</span><span class="sxs-lookup"><span data-stu-id="b7545-159">c.</span></span> <span data-ttu-id="b7545-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.topdesk.net/tas/public/login/saml`</span><span class="sxs-lookup"><span data-stu-id="b7545-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net/tas/public/login/saml`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="b7545-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b7545-161">These values are not real.</span></span> <span data-ttu-id="b7545-162">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="b7545-162">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="b7545-163">Válasz URL-CÍMEN explaned az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="b7545-163">Reply URL is explaned later in tutorial.</span></span> <span data-ttu-id="b7545-164">Ügyfél [TOPdesk - nyilvános ügyfél-támogatási csoport](https://help.topdesk.com/saas/enterprise/user/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b7545-164">Contact [TOPdesk - Public Client support team](https://help.topdesk.com/saas/enterprise/user/) tooget these values.</span></span>  

4. <span data-ttu-id="b7545-165">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b7545-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. <span data-ttu-id="b7545-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="b7545-169">A hello **TOPdesk - nyilvános konfigurációs** kattintson **konfigurálása TOPdesk - nyilvános** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b7545-169">On hello **TOPdesk - Public Configuration** section, click **Configure TOPdesk - Public** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b7545-170">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b7545-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TOPdesk - nyilvános konfigurációja](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. <span data-ttu-id="b7545-172">Jelentkezzen be tooyour **TOPdesk - nyilvános** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b7545-172">Sign on tooyour **TOPdesk - Public** company site as an administrator.</span></span>

8. <span data-ttu-id="b7545-173">A hello **TOPdesk** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b7545-173">In hello **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="b7545-174">![Beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="b7545-174">![Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Settings")</span></span>

9. <span data-ttu-id="b7545-175">Kattintson a **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b7545-175">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="b7545-176">![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="b7545-176">![Login Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Login Settings")</span></span>

10. <span data-ttu-id="b7545-177">Bontsa ki a hello **bejelentkezési beállítások** menüben, majd kattintson **általános**.</span><span class="sxs-lookup"><span data-stu-id="b7545-177">Expand hello **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="b7545-178">![Általános](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "általános")</span><span class="sxs-lookup"><span data-stu-id="b7545-178">![General](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "General")</span></span>

11. <span data-ttu-id="b7545-179">A hello **nyilvános** hello szakasza **SAML bejelentkezési** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-179">In hello **Public** section of hello **SAML login** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b7545-180">![Műszaki beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "műszaki beállításai")</span><span class="sxs-lookup"><span data-stu-id="b7545-180">![Technical Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technical Settings")</span></span>
   
    <span data-ttu-id="b7545-181">a.</span><span class="sxs-lookup"><span data-stu-id="b7545-181">a.</span></span> <span data-ttu-id="b7545-182">Kattintson a **letöltése** toodownload hello nyilvános metaadat-fájlt, és mentse helyileg a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b7545-182">Click **Download** toodownload hello public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="b7545-183">b.</span><span class="sxs-lookup"><span data-stu-id="b7545-183">b.</span></span> <span data-ttu-id="b7545-184">Nyissa meg a letöltött hello metaadat-fájlt, és keresse a hello **AssertionConsumerService** csomópont.</span><span class="sxs-lookup"><span data-stu-id="b7545-184">Open hello downloaded metadata file, and then locate hello **AssertionConsumerService** node.</span></span>

    <span data-ttu-id="b7545-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span><span class="sxs-lookup"><span data-stu-id="b7545-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span></span>
   
    <span data-ttu-id="b7545-186">c.</span><span class="sxs-lookup"><span data-stu-id="b7545-186">c.</span></span> <span data-ttu-id="b7545-187">Másolás hello **AssertionConsumerService** értékét, illessze be ezt az értéket a hello **válasz URL-CÍMEN** textbox **TOPdesk - nyilvános tartományi és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b7545-187">Copy hello **AssertionConsumerService** value, paste this value in hello **Reply URL** textbox in **TOPdesk - Public Domain and URLs** section.</span></span>      
   
12. <span data-ttu-id="b7545-188">toocreate a tanúsítványfájlt, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-188">toocreate a certificate file, perform hello following steps:</span></span>
    
    <span data-ttu-id="b7545-189">![Tanúsítvány](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "tanúsítvány")</span><span class="sxs-lookup"><span data-stu-id="b7545-189">![Certificate](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Certificate")</span></span>
    
    <span data-ttu-id="b7545-190">a.</span><span class="sxs-lookup"><span data-stu-id="b7545-190">a.</span></span> <span data-ttu-id="b7545-191">Nyissa meg hello Azure portálról letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="b7545-191">Open hello downloaded metadata file from Azure portal.</span></span>
    
    <span data-ttu-id="b7545-192">b.</span><span class="sxs-lookup"><span data-stu-id="b7545-192">b.</span></span> <span data-ttu-id="b7545-193">Bontsa ki a hello **RoleDescriptor** csomópont egy **xsi: type** a **táplált: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="b7545-193">Expand hello **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    
    <span data-ttu-id="b7545-194">c.</span><span class="sxs-lookup"><span data-stu-id="b7545-194">c.</span></span> <span data-ttu-id="b7545-195">Másolja a hello hello értékének **x.509** csomópont.</span><span class="sxs-lookup"><span data-stu-id="b7545-195">Copy hello value of hello **X509Certificate** node.</span></span>
    
    <span data-ttu-id="b7545-196">d.</span><span class="sxs-lookup"><span data-stu-id="b7545-196">d.</span></span> <span data-ttu-id="b7545-197">Mentés hello másolt **x.509** érték helyileg a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b7545-197">Save hello copied **X509Certificate** value locally on your computer in a file.</span></span>

13. <span data-ttu-id="b7545-198">A hello **nyilvános** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b7545-198">In hello **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="b7545-199">![SAML bejelentkezési](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="b7545-199">![SAML Login](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML Login")</span></span>

14. <span data-ttu-id="b7545-200">A hello **SAML-alapú konfigurációs Segéd** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-200">On hello **SAML configuration assistant** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="b7545-201">![SAML-alapú konfigurációs Segéd](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML-alapú konfigurációs Segéd")</span><span class="sxs-lookup"><span data-stu-id="b7545-201">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="b7545-202">a.</span><span class="sxs-lookup"><span data-stu-id="b7545-202">a.</span></span> <span data-ttu-id="b7545-203">tooupload a letöltött metaadatok fájl Azure-portálon, a **összevonási metaadatok**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="b7545-203">tooupload your downloaded metadata file from Azure portal, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="b7545-204">b.</span><span class="sxs-lookup"><span data-stu-id="b7545-204">b.</span></span> <span data-ttu-id="b7545-205">a tanúsítvány a fájl tooupload **tanúsítvány (RSA)**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="b7545-205">tooupload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="b7545-206">c.</span><span class="sxs-lookup"><span data-stu-id="b7545-206">c.</span></span> <span data-ttu-id="b7545-207">során kapott azonosítóértékeket hello TOPdesk támogatási csoport, a fájl tooupload hello **embléma ikon**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="b7545-207">tooupload hello logo file you got from hello TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="b7545-208">d.</span><span class="sxs-lookup"><span data-stu-id="b7545-208">d.</span></span> <span data-ttu-id="b7545-209">A hello **felhasználói név attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="b7545-209">In hello **User name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="b7545-210">e.</span><span class="sxs-lookup"><span data-stu-id="b7545-210">e.</span></span> <span data-ttu-id="b7545-211">A hello **megjelenített név** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="b7545-211">In hello **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="b7545-212">f.</span><span class="sxs-lookup"><span data-stu-id="b7545-212">f.</span></span> <span data-ttu-id="b7545-213">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="b7545-214">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b7545-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b7545-215">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b7545-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b7545-216">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7545-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b7545-217">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b7545-217">Create an Azure AD test user</span></span>

<span data-ttu-id="b7545-218">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b7545-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="b7545-220">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b7545-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7545-221">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-221">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b7545-223">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b7545-223">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b7545-225">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="b7545-225">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b7545-227">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-227">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    <span data-ttu-id="b7545-229">a.</span><span class="sxs-lookup"><span data-stu-id="b7545-229">a.</span></span> <span data-ttu-id="b7545-230">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7545-230">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7545-231">b.</span><span class="sxs-lookup"><span data-stu-id="b7545-231">b.</span></span> <span data-ttu-id="b7545-232">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="b7545-232">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="b7545-233">c.</span><span class="sxs-lookup"><span data-stu-id="b7545-233">c.</span></span> <span data-ttu-id="b7545-234">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b7545-234">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="b7545-235">d.</span><span class="sxs-lookup"><span data-stu-id="b7545-235">d.</span></span> <span data-ttu-id="b7545-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-236">Click **Create**.</span></span>
 
### <a name="create-a-topdesk---public-test-user"></a><span data-ttu-id="b7545-237">Hozzon létre egy TOPdesk - nyilvános tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="b7545-237">Create a TOPdesk - Public test user</span></span>

<span data-ttu-id="b7545-238">A sorrend tooenable az Azure AD felhasználók toolog TOPdesk - be nyilvános, TOPdesk - nyilvános be kell kiépítve.</span><span class="sxs-lookup"><span data-stu-id="b7545-238">In order tooenable Azure AD users toolog into TOPdesk - Public, they must be provisioned into TOPdesk - Public.</span></span>  
<span data-ttu-id="b7545-239">Hello esetében TOPdesk - nyilvános, kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b7545-239">In hello case of TOPdesk - Public, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="b7545-240">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-240">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="b7545-241">Jelentkezzen be tooyour **TOPdesk - nyilvános** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b7545-241">Sign on tooyour **TOPdesk - Public** company site as administrator.</span></span>

2. <span data-ttu-id="b7545-242">Hello hello felső menüben kattintson a **TOPdesk \> új \> támogatófájljait \> személy**.</span><span class="sxs-lookup"><span data-stu-id="b7545-242">In hello menu on hello top, click **TOPdesk \> New \> Support Files \> Person**.</span></span>
   
    <span data-ttu-id="b7545-243">![Személy](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "személy")</span><span class="sxs-lookup"><span data-stu-id="b7545-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span></span>

3. <span data-ttu-id="b7545-244">Hello új személy párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b7545-244">On hello New Person dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="b7545-245">![Új személy](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "új személy")</span><span class="sxs-lookup"><span data-stu-id="b7545-245">![New Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "New Person")</span></span>
   
    <span data-ttu-id="b7545-246">a.</span><span class="sxs-lookup"><span data-stu-id="b7545-246">a.</span></span> <span data-ttu-id="b7545-247">Kattintson a hello Általános fülre.</span><span class="sxs-lookup"><span data-stu-id="b7545-247">Click hello General tab.</span></span>

    <span data-ttu-id="b7545-248">b.</span><span class="sxs-lookup"><span data-stu-id="b7545-248">b.</span></span> <span data-ttu-id="b7545-249">A hello **vezetékneve** szövegmező, írja be például a Simon hello felhasználó vezetékneve</span><span class="sxs-lookup"><span data-stu-id="b7545-249">In hello **Surname** textbox, type Surname of hello user like Simon</span></span>
 
    <span data-ttu-id="b7545-250">c.</span><span class="sxs-lookup"><span data-stu-id="b7545-250">c.</span></span> <span data-ttu-id="b7545-251">Válassza ki a **hely** hello fiók.</span><span class="sxs-lookup"><span data-stu-id="b7545-251">Select a **Site** for hello account.</span></span>
 
    <span data-ttu-id="b7545-252">d.</span><span class="sxs-lookup"><span data-stu-id="b7545-252">d.</span></span> <span data-ttu-id="b7545-253">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-253">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="b7545-254">TOPdesk - nyilvános felhasználói fiók létrehozása eszközök vagy TOPdesk - nyilvános tooprovision az Azure AD felhasználói fiókok által nyújtott API-kat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b7545-254">You can use any other TOPdesk - Public user account creation tools or APIs provided by TOPdesk - Public tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b7545-255">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="b7545-255">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b7545-256">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTOPdesk - nyilvános megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b7545-256">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTOPdesk - Public.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="b7545-258">**tooassign Britta Simon tooTOPdesk - nyilvános, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="b7545-258">**tooassign Britta Simon tooTOPdesk - Public, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7545-259">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7545-259">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b7545-261">Hello alkalmazások listában válassza ki a **TOPdesk - nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="b7545-261">In hello applications list, select **TOPdesk - Public**.</span></span>

    ![hello TOPdesk - hello alkalmazások listáját hivatkozásra nyilvános](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. <span data-ttu-id="b7545-263">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b7545-263">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="b7545-265">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7545-265">Click **Add** button.</span></span> <span data-ttu-id="b7545-266">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7545-266">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="b7545-268">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b7545-268">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b7545-269">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7545-269">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7545-270">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7545-270">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b7545-271">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b7545-271">Test single sign-on</span></span>

<span data-ttu-id="b7545-272">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b7545-272">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b7545-273">Hello TOPdesk - kattintva nyilvános hello hozzáférési Panel csempére, automatikusan bejelentkezett tooyour TOPdesk - nyilvános alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="b7545-273">When you click hello TOPdesk - Public tile in hello Access Panel, you should get automatically signed-on tooyour TOPdesk - Public application.</span></span>
<span data-ttu-id="b7545-274">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b7545-274">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b7545-275">További források</span><span class="sxs-lookup"><span data-stu-id="b7545-275">Additional resources</span></span>

* [<span data-ttu-id="b7545-276">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b7545-276">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7545-277">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b7545-277">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

