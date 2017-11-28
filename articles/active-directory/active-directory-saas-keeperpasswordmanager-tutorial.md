---
title: "Oktatóanyag: Azure Active Directoryval integrált üzemeltetőjének jelszókezelő & digitális tároló |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és üzemeltetőjének jelszókezelő & digitális tároló között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 01e59004e6bdccfca08094f9da6f82270d5028d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a><span data-ttu-id="85d6a-103">Oktatóanyag: Azure Active Directoryval integrált üzemeltetőjének jelszókezelő & digitális tároló</span><span class="sxs-lookup"><span data-stu-id="85d6a-103">Tutorial: Azure Active Directory integration with Keeper Password Manager & Digital Vault</span></span>

<span data-ttu-id="85d6a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate üzemeltetőjének jelszókezelő & digitális tárolóban az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="85d6a-104">In this tutorial, you learn how toointegrate Keeper Password Manager & Digital Vault with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="85d6a-105">Jelszókezelő üzemeltetőjének & digitális tároló integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="85d6a-105">Integrating Keeper Password Manager & Digital Vault with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="85d6a-106">Szabályozhatja az Azure AD, aki rendelkezik tooKeeper jelszókezelő & digitális tároló hozzáférés</span><span class="sxs-lookup"><span data-stu-id="85d6a-106">You can control in Azure AD who has access tooKeeper Password Manager & Digital Vault</span></span>
- <span data-ttu-id="85d6a-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKeeper jelszókezelő & digitális tároló (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="85d6a-107">You can enable your users tooautomatically get signed-on tooKeeper Password Manager & Digital Vault (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="85d6a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="85d6a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="85d6a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="85d6a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="85d6a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="85d6a-110">Prerequisites</span></span>

<span data-ttu-id="85d6a-111">tooconfigure üzemeltetőjének jelszókezelő & digitális tárolóban az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="85d6a-111">tooconfigure Azure AD integration with Keeper Password Manager & Digital Vault, you need hello following items:</span></span>

- <span data-ttu-id="85d6a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="85d6a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="85d6a-113">A jelszókezelő üzemeltetőjének & digitális tároló egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="85d6a-113">A Keeper Password Manager & Digital Vault single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="85d6a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="85d6a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="85d6a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="85d6a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="85d6a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="85d6a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="85d6a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="85d6a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="85d6a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="85d6a-118">Scenario description</span></span>
<span data-ttu-id="85d6a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="85d6a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="85d6a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="85d6a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="85d6a-121">Hello gyűjteményből üzemeltetőjének jelszókezelő & digitális tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="85d6a-121">Adding Keeper Password Manager & Digital Vault from hello gallery</span></span>
2. <span data-ttu-id="85d6a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="85d6a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-keeper-password-manager--digital-vault-from-hello-gallery"></a><span data-ttu-id="85d6a-123">Hello gyűjteményből üzemeltetőjének jelszókezelő & digitális tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="85d6a-123">Adding Keeper Password Manager & Digital Vault from hello gallery</span></span>
<span data-ttu-id="85d6a-124">tooconfigure hello integrációs üzemeltetőjének jelszókezelő & digitális tárolóban, az Azure AD-be, szükség tooadd üzemeltetőjének jelszókezelő & digitális tároló a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="85d6a-124">tooconfigure hello integration of Keeper Password Manager & Digital Vault into Azure AD, you need tooadd Keeper Password Manager & Digital Vault from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="85d6a-125">**tooadd üzemeltetőjének jelszókezelő & digitális tároló hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="85d6a-125">**tooadd Keeper Password Manager & Digital Vault from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="85d6a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="85d6a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="85d6a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="85d6a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="85d6a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="85d6a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="85d6a-133">Hello keresési mezőbe, írja be a **üzemeltetőjének jelszókezelő & digitális tároló**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-133">In hello search box, type **Keeper Password Manager & Digital Vault**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. <span data-ttu-id="85d6a-135">A hello eredmények panelen válassza a **üzemeltetőjének jelszókezelő & digitális tároló**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="85d6a-135">In hello results panel, select **Keeper Password Manager & Digital Vault**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="85d6a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="85d6a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="85d6a-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés üzemeltetőjének jelszókezelő & "Britta Simon." nevű tesztfelhasználó alapuló digitális tároló</span><span class="sxs-lookup"><span data-stu-id="85d6a-138">In this section, you configure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="85d6a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói üzemeltetőjének jelszókezelő & digitális tároló tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="85d6a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Keeper Password Manager & Digital Vault is tooa user in Azure AD.</span></span> <span data-ttu-id="85d6a-140">Ez azt jelenti hello kapcsolódó felhasználói üzemeltetőjének jelszókezelő & digitális tároló és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="85d6a-140">In other words, a link relationship between an Azure AD user and hello related user in Keeper Password Manager & Digital Vault needs toobe established.</span></span>

<span data-ttu-id="85d6a-141">Jelszókezelő üzemeltetőjének & digitális tárolóban, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="85d6a-141">In Keeper Password Manager & Digital Vault, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="85d6a-142">tooconfigure és üzemeltetőjének jelszókezelő & digitális tárolóban az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="85d6a-142">tooconfigure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="85d6a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="85d6a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="85d6a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="85d6a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="85d6a-145">**[Jelszókezelő üzemeltetőjének & digitális tároló tesztfelhasználó létrehozása](#creating-a-keeperpasswordmanager-test-user)**  -toohave egy megfelelője a Britta Simon üzemeltetőjének jelszókezelő & digitális tárolóban, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="85d6a-145">**[Creating a Keeper Password Manager & Digital Vault test user](#creating-a-keeperpasswordmanager-test-user)** - toohave a counterpart of Britta Simon in Keeper Password Manager & Digital Vault that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="85d6a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="85d6a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="85d6a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="85d6a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="85d6a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85d6a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="85d6a-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a üzemeltetőjének jelszókezelő & digitális tároló kérelemben egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="85d6a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Keeper Password Manager & Digital Vault application.</span></span>

<span data-ttu-id="85d6a-150">**üzemeltetőjének jelszókezelő & digitális tárolóban, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="85d6a-150">**tooconfigure Azure AD single sign-on with Keeper Password Manager & Digital Vault, perform hello following steps:**</span></span>

1. <span data-ttu-id="85d6a-151">Az Azure portál, a hello hello **üzemeltetőjének jelszókezelő & digitális tároló** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-151">In hello Azure portal, on hello **Keeper Password Manager & Digital Vault** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="85d6a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="85d6a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. <span data-ttu-id="85d6a-155">A hello **üzemeltetőjének jelszókezelő & digitális tároló tartomány és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="85d6a-155">On hello **Keeper Password Manager & Digital Vault Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    <span data-ttu-id="85d6a-157">a.</span><span class="sxs-lookup"><span data-stu-id="85d6a-157">a.</span></span> <span data-ttu-id="85d6a-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span><span class="sxs-lookup"><span data-stu-id="85d6a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span></span>

    <span data-ttu-id="85d6a-159">b.</span><span class="sxs-lookup"><span data-stu-id="85d6a-159">b.</span></span> <span data-ttu-id="85d6a-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="85d6a-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span></span>

    <span data-ttu-id="85d6a-161">c.</span><span class="sxs-lookup"><span data-stu-id="85d6a-161">c.</span></span> <span data-ttu-id="85d6a-162">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://{SSO CONNECT SERVER}/sso-connect`</span><span class="sxs-lookup"><span data-stu-id="85d6a-162">In hello **Identifier** textbox, type a URL using hello following pattern: `https://{SSO CONNECT SERVER}/sso-connect`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="85d6a-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="85d6a-163">These values are not real.</span></span> <span data-ttu-id="85d6a-164">Frissítheti ezeket az értékeket hello tényleges válasz URL-CÍMEN, és a bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="85d6a-164">Update these values with hello actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="85d6a-165">Ügyfél [üzemeltetőjének jelszókezelő & digitális tároló ügyfél-támogatási csoport](https://keepersecurity.com/contact.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="85d6a-165">Contact [Keeper Password Manager & Digital Vault Client support team](https://keepersecurity.com/contact.html) tooget these values.</span></span> 

4. <span data-ttu-id="85d6a-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="85d6a-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. <span data-ttu-id="85d6a-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="85d6a-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="85d6a-170">A hello **üzemeltetőjének jelszókezelő & digitális tároló konfigurációs** kattintson **konfigurálása üzemeltetőjének jelszókezelő & digitális tároló** tooopen **bejelentkezéskonfigurálása**ablak.</span><span class="sxs-lookup"><span data-stu-id="85d6a-170">On hello **Keeper Password Manager & Digital Vault Configuration** section, click **Configure Keeper Password Manager & Digital Vault** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="85d6a-171">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="85d6a-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. <span data-ttu-id="85d6a-173">tooconfigure egyszeri bejelentkezést a **üzemeltetőjének jelszókezelő & digitális tároló konfigurációs** oldalán, regisztrációkor hello útmutatást követve [üzemeltetőjének támogatási útmutató](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span><span class="sxs-lookup"><span data-stu-id="85d6a-173">tooconfigure single sign-on on **Keeper Password Manager & Digital Vault Configuration** side, follow hello guidelines given at [Keeper Support Guide](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span></span>

> [!TIP]
> <span data-ttu-id="85d6a-174">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="85d6a-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="85d6a-175">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="85d6a-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="85d6a-176">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="85d6a-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="85d6a-177">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="85d6a-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="85d6a-178">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="85d6a-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="85d6a-180">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="85d6a-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="85d6a-181">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="85d6a-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="85d6a-183">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="85d6a-185">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85d6a-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="85d6a-187">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="85d6a-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="85d6a-189">a.</span><span class="sxs-lookup"><span data-stu-id="85d6a-189">a.</span></span> <span data-ttu-id="85d6a-190">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="85d6a-191">b.</span><span class="sxs-lookup"><span data-stu-id="85d6a-191">b.</span></span> <span data-ttu-id="85d6a-192">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="85d6a-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="85d6a-193">c.</span><span class="sxs-lookup"><span data-stu-id="85d6a-193">c.</span></span> <span data-ttu-id="85d6a-194">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="85d6a-195">d.</span><span class="sxs-lookup"><span data-stu-id="85d6a-195">d.</span></span> <span data-ttu-id="85d6a-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="85d6a-196">Click **Create**.</span></span>
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a><span data-ttu-id="85d6a-197">Jelszókezelő üzemeltetőjének & digitális tároló tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="85d6a-197">Creating a Keeper Password Manager & Digital Vault test user</span></span>

<span data-ttu-id="85d6a-198">az Azure AD tooenable felhasználók toolog tooKeeper jelszókezelő & digitális tárolóban, azok ki kell építenie üzemeltetőjének jelszókezelő & digitális tároló be.</span><span class="sxs-lookup"><span data-stu-id="85d6a-198">tooenable Azure AD users toolog in tooKeeper Password Manager & Digital Vault, they must be provisioned into Keeper Password Manager & Digital Vault.</span></span> <span data-ttu-id="85d6a-199">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="85d6a-199">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="85d6a-200">Felveheti a kapcsolatot [üzemeltetőjének támogatási](https://keepersecurity.com/contact.html), ha manuálisan szeretné toosetup felhasználók.</span><span class="sxs-lookup"><span data-stu-id="85d6a-200">You can contact [Keeper Support](https://keepersecurity.com/contact.html), if you want toosetup users manually.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="85d6a-201">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="85d6a-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="85d6a-202">Ebben a szakaszban megadásával engedélyeznie Britta Simon toouse Azure egyszeri bejelentkezés tooKeeper jelszókezelő & digitális tároló eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="85d6a-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKeeper Password Manager & Digital Vault.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="85d6a-204">**tooassign Britta Simon tooKeeper jelszókezelő & digitális tárolóban, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="85d6a-204">**tooassign Britta Simon tooKeeper Password Manager & Digital Vault, perform hello following steps:**</span></span>

1. <span data-ttu-id="85d6a-205">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="85d6a-207">Hello alkalmazások listában válassza ki a **üzemeltetőjének jelszókezelő & digitális tároló**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-207">In hello applications list, select **Keeper Password Manager & Digital Vault**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. <span data-ttu-id="85d6a-209">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="85d6a-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="85d6a-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="85d6a-211">Click **Add** button.</span></span> <span data-ttu-id="85d6a-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85d6a-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="85d6a-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="85d6a-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="85d6a-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85d6a-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="85d6a-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85d6a-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="85d6a-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="85d6a-217">Testing single sign-on</span></span>

<span data-ttu-id="85d6a-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="85d6a-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="85d6a-219">Hello üzemeltetőjének jelszókezelő & digitális tároló csempe a hozzáférési Panel hello kattintáskor üzemeltetőjének jelszókezelő & digitális tároló alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="85d6a-219">When you click hello Keeper Password Manager & Digital Vault tile in hello Access Panel, you should get login page of Keeper Password Manager & Digital Vault application.</span></span> <span data-ttu-id="85d6a-220">Sikeres hitelesítés után szerezheti be hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="85d6a-220">Upon successful authentication, you should get into hello application.</span></span> <span data-ttu-id="85d6a-221">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="85d6a-221">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="85d6a-222">További források</span><span class="sxs-lookup"><span data-stu-id="85d6a-222">Additional resources</span></span>

* [<span data-ttu-id="85d6a-223">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="85d6a-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="85d6a-224">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="85d6a-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_203.png

