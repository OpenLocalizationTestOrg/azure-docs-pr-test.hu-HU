---
title: "Oktatóanyag: Azure Active Directoryval integrált Veritas vállalati Vault.cloud SSO |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Veritas vállalati Vault.cloud SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 1037e70515686091460ac41e9e5a7951639bb520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a><span data-ttu-id="2cd7c-103">Oktatóanyag: Azure Active Directoryval integrált Veritas vállalati Vault.cloud egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2cd7c-103">Tutorial: Azure Active Directory integration with Veritas Enterprise Vault.cloud SSO</span></span>

<span data-ttu-id="2cd7c-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Veritas vállalati Vault.cloud egyszeri bejelentkezés az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2cd7c-104">In this tutorial, you learn how toointegrate Veritas Enterprise Vault.cloud SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2cd7c-105">Veritas vállalati Vault.cloud SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-105">Integrating Veritas Enterprise Vault.cloud SSO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2cd7c-106">Megadhatja a hozzáférés tooVeritas vállalati Vault.cloud SSO rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2cd7c-106">You can control in Azure AD who has access tooVeritas Enterprise Vault.cloud SSO</span></span>
- <span data-ttu-id="2cd7c-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooVeritas vállalati Vault.cloud egyszeri Bejelentkezést (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="2cd7c-107">You can enable your users tooautomatically get signed-on tooVeritas Enterprise Vault.cloud SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2cd7c-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2cd7c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2cd7c-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2cd7c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cd7c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2cd7c-110">Prerequisites</span></span>

<span data-ttu-id="2cd7c-111">tooconfigure Veritas vállalati Vault.cloud egyszeri bejelentkezés az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-111">tooconfigure Azure AD integration with Veritas Enterprise Vault.cloud SSO, you need hello following items:</span></span>

- <span data-ttu-id="2cd7c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2cd7c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2cd7c-113">Egy Veritas vállalati Vault.cloud SSO egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2cd7c-113">A Veritas Enterprise Vault.cloud SSO single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2cd7c-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2cd7c-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2cd7c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2cd7c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2cd7c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2cd7c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2cd7c-118">Scenario description</span></span>
<span data-ttu-id="2cd7c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2cd7c-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2cd7c-121">Hello gyűjteményből Veritas vállalati Vault.cloud SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2cd7c-121">Adding Veritas Enterprise Vault.cloud SSO from hello gallery</span></span>
2. <span data-ttu-id="2cd7c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2cd7c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-hello-gallery"></a><span data-ttu-id="2cd7c-123">Hello gyűjteményből Veritas vállalati Vault.cloud SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2cd7c-123">Adding Veritas Enterprise Vault.cloud SSO from hello gallery</span></span>
<span data-ttu-id="2cd7c-124">tooconfigure hello integrációja Veritas vállalati Vault.cloud egyszeri bejelentkezés az Azure AD-be, meg kell tooadd Veritas vállalati Vault.cloud SSO hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-124">tooconfigure hello integration of Veritas Enterprise Vault.cloud SSO into Azure AD, you need tooadd Veritas Enterprise Vault.cloud SSO from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2cd7c-125">**tooadd Veritas vállalati Vault.cloud SSO hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2cd7c-125">**tooadd Veritas Enterprise Vault.cloud SSO from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cd7c-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2cd7c-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2cd7c-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2cd7c-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2cd7c-133">Hello keresési mezőbe, írja be a **Veritas vállalati Vault.cloud SSO**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-133">In hello search box, type **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. <span data-ttu-id="2cd7c-135">A hello eredmények panelen válassza a **Veritas vállalati Vault.cloud SSO**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-135">In hello results panel, select **Veritas Enterprise Vault.cloud SSO**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2cd7c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2cd7c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2cd7c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Veritas vállalati Vault.cloud SSO "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-138">In this section, you configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2cd7c-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Veritas vállalati Vault.cloud SSO tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Veritas Enterprise Vault.cloud SSO is tooa user in Azure AD.</span></span> <span data-ttu-id="2cd7c-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Veritas vállalati Vault.cloud SSO közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-140">In other words, a link relationship between an Azure AD user and hello related user in Veritas Enterprise Vault.cloud SSO needs toobe established.</span></span>

<span data-ttu-id="2cd7c-141">Veritas vállalati Vault.cloud SSO, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-141">In Veritas Enterprise Vault.cloud SSO, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2cd7c-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Veritas vállalati Vault.cloud SSO-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-142">tooconfigure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2cd7c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2cd7c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2cd7c-145">**[Veritas vállalati Vault.cloud SSO tesztfelhasználó létrehozása](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  -toohave egy megfelelője a Britta Simon a Veritas vállalati Vault.cloud SSO csatolt toohello az Azure AD felhasználói ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-145">**[Creating a Veritas Enterprise Vault.cloud SSO test user](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)** - toohave a counterpart of Britta Simon in Veritas Enterprise Vault.cloud SSO that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2cd7c-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2cd7c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2cd7c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2cd7c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2cd7c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Veritas vállalati Vault.cloud SSO-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Veritas Enterprise Vault.cloud SSO application.</span></span>

<span data-ttu-id="2cd7c-150">**Veritas vállalati Vault.cloud egyszeri bejelentkezési modellel, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2cd7c-150">**tooconfigure Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cd7c-151">Az Azure portál, a hello hello **Veritas vállalati Vault.cloud SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-151">In hello Azure portal, on hello **Veritas Enterprise Vault.cloud SSO** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2cd7c-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. <span data-ttu-id="2cd7c-155">A hello **Veritas vállalati Vault.cloud SSO tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-155">On hello **Veritas Enterprise Vault.cloud SSO Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    <span data-ttu-id="2cd7c-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span><span class="sxs-lookup"><span data-stu-id="2cd7c-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="2cd7c-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-158">This value is not real.</span></span> <span data-ttu-id="2cd7c-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="2cd7c-160">Ügyfél [Veritas vállalati Vault.cloud SSO ügyfél-támogatási csoport](https://www.veritas.com/support/.html) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-160">Contact [Veritas Enterprise Vault.cloud SSO Client support team](https://www.veritas.com/support/.html) tooget this value.</span></span> 

4. <span data-ttu-id="2cd7c-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. <span data-ttu-id="2cd7c-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2cd7c-165">A hello **Veritas vállalati Vault.cloud SSO konfigurációs** területén kattintson **konfigurálása Veritas vállalati Vault.cloud SSO** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-165">On hello **Veritas Enterprise Vault.cloud SSO Configuration** section, click **Configure Veritas Enterprise Vault.cloud SSO** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2cd7c-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2cd7c-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. <span data-ttu-id="2cd7c-168">tooconfigure egyszeri bejelentkezést a **Veritas vállalati Vault.cloud SSO** oldalon kell letöltött toosend hello **Certificate(Base64)** és **SAML-alapú egyszeri bejelentkezési URL-címe**túl[Veritas vállalati Vault.cloud SSO támogatási csoport](https://www.veritas.com/support/.html).</span><span class="sxs-lookup"><span data-stu-id="2cd7c-168">tooconfigure single sign-on on **Veritas Enterprise Vault.cloud SSO** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html).</span></span>

> [!TIP]
> <span data-ttu-id="2cd7c-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2cd7c-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2cd7c-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2cd7c-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2cd7c-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2cd7c-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2cd7c-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="2cd7c-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2cd7c-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2cd7c-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cd7c-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2cd7c-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2cd7c-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2cd7c-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2cd7c-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2cd7c-184">a.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-184">a.</span></span> <span data-ttu-id="2cd7c-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2cd7c-186">b.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-186">b.</span></span> <span data-ttu-id="2cd7c-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2cd7c-188">c.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-188">c.</span></span> <span data-ttu-id="2cd7c-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2cd7c-190">d.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-190">d.</span></span> <span data-ttu-id="2cd7c-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-191">Click **Create**.</span></span>
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a><span data-ttu-id="2cd7c-192">Veritas vállalati Vault.cloud SSO tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2cd7c-192">Creating a Veritas Enterprise Vault.cloud SSO test user</span></span>

<span data-ttu-id="2cd7c-193">Ebben a szakaszban egy Britta Simon meghívta vállalati Vault.cloud egyszeri Bejelentkezéses felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-193">In this section, you create a user called Britta Simon in Enterprise Vault.cloud SSO.</span></span> <span data-ttu-id="2cd7c-194">Együttműködve [Veritas vállalati Vault.cloud SSO támogatási csoport](https://www.veritas.com/support/.html) felhasználót is hozzáadhat hello hello vállalati Vault.cloud SSO platform.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-194">Work with [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html) to add hello users in hello Enterprise Vault.cloud SSO platform.</span></span> <span data-ttu-id="2cd7c-195">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-195">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2cd7c-196">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2cd7c-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2cd7c-197">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés vállalati Vault.cloud egyszeri Bejelentkezéses hozzáférést tooVeritas megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVeritas Enterprise Vault.cloud SSO.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2cd7c-199">**tooassign Britta Simon tooVeritas vállalati Vault.cloud SSO, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2cd7c-199">**tooassign Britta Simon tooVeritas Enterprise Vault.cloud SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cd7c-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2cd7c-202">Hello alkalmazások listában válassza ki a **Veritas vállalati Vault.cloud SSO**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-202">In hello applications list, select **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. <span data-ttu-id="2cd7c-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2cd7c-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-206">Click **Add** button.</span></span> <span data-ttu-id="2cd7c-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2cd7c-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2cd7c-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2cd7c-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2cd7c-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2cd7c-212">Testing single sign-on</span></span>

<span data-ttu-id="2cd7c-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2cd7c-214">Ha a hozzáférési Panel hello hello Veritas vállalati Vault.cloud SSO csempe gombra kattint, automatikusan bejelentkezett tooyour Veritas vállalati Vault.cloud egyszeri bejelentkezési alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="2cd7c-214">When you click hello Veritas Enterprise Vault.cloud SSO tile in hello Access Panel, you should get automatically signed-on tooyour Veritas Enterprise Vault.cloud SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2cd7c-215">További források</span><span class="sxs-lookup"><span data-stu-id="2cd7c-215">Additional resources</span></span>

* [<span data-ttu-id="2cd7c-216">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2cd7c-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2cd7c-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2cd7c-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

